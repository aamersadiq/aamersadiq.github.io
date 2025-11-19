---
title: "Supercharging PostgreSQL with TimescaleDB for Time Series Data"
date: 2025-11-05
categories: [database, postgres, timeseries]
tags: [database, postgres, timeseries]
---

In today's data driven world, applications are generating massive amounts of time series data from IoT sensor readings and financial transactions to application metrics and user activity logs. While PostgreSQL is a powerful and versatile relational database, it was not specifically designed to handle the unique challenges of time series data at scale. This is where TimescaleDB comes in.

## Introduction to TimescaleDB

TimescaleDB is an open source time series database built as an extension to PostgreSQL. It provides specialized features for storing, analyzing, and managing time series data while maintaining full SQL compatibility and leveraging the robust ecosystem of PostgreSQL.

In our recent project, we built a financial transaction API using .NET 8, PostgreSQL, and TimescaleDB to handle high volume transaction data efficiently. This blog post explores the key TimescaleDB features we used and how they dramatically improved our application's performance and capabilities. The complete code for our project is available at [https://github.com/aamersadiq/postgres-timescale](https://github.com/aamersadiq/postgres-timescale) if you'd like to see a complete implementation.

## Hypertables: The Foundation of TimescaleDB

### What are Hypertables?

A hypertable is a specialized PostgreSQL table designed for high volume time series data, enabled by the TimescaleDB extension. It works by automatically partitioning data by time into smaller, individual PostgreSQL tables called "chunks". This process is transparent to the user, who interacts with the hypertable as if it were a single, large table.

### How Hypertables Work

The hypertable architecture optimizes a standard PostgreSQL table for time series data without changing the SQL interface:

- **Automatic time partitioning**: A user creates a hypertable from a standard table by specifying a timestamp column and a chunk_time_interval (e.g., one day or one week). As new data is inserted, TimescaleDB automatically creates new chunks, managing the partitions in the background.
- **Performance via chunks**: When a query is run, TimescaleDB's query planner identifies and accesses only the relevant chunks based on the timestamp range, dramatically improving performance.
- **Optimization features**: Hypertables include advanced features to further boost performance for time series workloads:
  - **Chunk skipping**: The query planner can skip entire chunks by inspecting minimum and maximum values for specific columns, including non-partitioning columns, before executing the query.
  - **Data compression**: Older, less frequently accessed chunks can be automatically compressed to reduce storage costs and speed up analytical queries.
  - **Continuous aggregates**: These are incrementally updated, continuously refreshed materialized views that provide up to date aggregates in milliseconds, even for massive datasets.
  - **Flexible storage**: Hypertables can store recent, active data in a row-based format for fast inserts and modifications, while automatically compressing and moving older data to a columnar format for efficient analytics.
  - **Simplified management**: Hypertables include built-in features for automated data retention policies, allowing old data to be dropped automatically with a single command.

### Hypertable vs. Standard PostgreSQL Table

While hypertables appear and behave like standard PostgreSQL tables to the end user, their internal structure offers distinct advantages for time series data:

| Feature           | Hypertable (with TimescaleDB)                                                                                                                            | Standard PostgreSQL table                                                                                                                                       |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Partitioning      | Automatically partitions data by time into smaller, more manageable child tables (chunks).                                                               | Requires complex manual setup and management of native declarative partitioning, with all the associated complexity and risk of human error.                    |
| Indexing          | Automatically creates indexes for the time column. Any other indexes created on the hypertable are automatically applied to all new and existing chunks. | All indexes must be created and maintained manually. For partitioned tables, indexes must be created on each partition.                                         |
| Performance       | Optimized for time series inserts and queries, with significant speed improvements over regular partitioning for large datasets.                         | Can become slow for time series queries as the table grows, especially for queries that span a wide time range.                                                 |
| SQL compatibility | Fully compatible with standard SQL, including standard SELECT, INSERT, ALTER TABLE, and DROP TABLE commands.                                             | Also fully compatible with standard SQL.                                                                                                                        |
| Data management   | Provides automated features for data compression and retention policies to efficiently manage data over its lifecycle.                                   | Does not include native features for time series compression or automated data retention; requires manual implementation with potentially slow DELETE commands. |

### Implementation Example

In our project, we converted our Transactions table to a hypertable using the following code in our TimescaleDB initialization script:

```csharp
// Convert transactions table to a hypertable
logger.LogInformation("Converting Transactions table to a hypertable...");
using (var cmd = new NpgsqlCommand(
    "SELECT hypertable_name FROM timescaledb_information.hypertables WHERE hypertable_name = 'Transactions';", conn))
{
    var result = await cmd.ExecuteScalarAsync();
    if (result == null)
    {
        logger.LogInformation("Creating hypertable for Transactions...");
        using var createHypertableCmd = new NpgsqlCommand(
            "SELECT create_hypertable('\"Transactions\"', 'CreatedAt', chunk_time_interval => INTERVAL '1 day');", conn);
        await createHypertableCmd.ExecuteNonQueryAsync();

        // Create indexes for better query performance
        logger.LogInformation("Creating indexes for Transactions...");
        using var createIndexCmd1 = new NpgsqlCommand(
            "CREATE INDEX IF NOT EXISTS idx_transactions_account_id ON \"Transactions\" (\"AccountId\");", conn);
        await createIndexCmd1.ExecuteNonQueryAsync();

        using var createIndexCmd2 = new NpgsqlCommand(
            "CREATE INDEX IF NOT EXISTS idx_transactions_category_id ON \"Transactions\" (\"CategoryId\");", conn);
        await createIndexCmd2.ExecuteNonQueryAsync();

        using var createIndexCmd3 = new NpgsqlCommand(
            "CREATE INDEX IF NOT EXISTS idx_transactions_account_created ON \"Transactions\" (\"AccountId\", \"CreatedAt\" DESC);", conn);
        await createIndexCmd3.ExecuteNonQueryAsync();
    }
}
```

This code checks if the Transactions table is already a hypertable, and if not, converts it using the `create_hypertable` function. We specify 'CreatedAt' as our time column and set a chunk time interval of 1 day. We also create additional indexes to optimize our most common query patterns.

## Continuous Aggregates: Real Time Analytics at Scale

Timescale's continuous aggregates are an enhancement of standard PostgreSQL materialized views, specifically designed for high volume time series data. They solve the performance and maintenance issues of regular materialized views by providing automatic, incremental, and efficient updates of aggregated data.

### How Continuous Aggregates Work

- **Incremental updates**: Unlike a standard materialized view that must be recomputed entirely on each refresh, a continuous aggregate only processes new or changed data. It stores the aggregated results in a special internal hypertable.
- **Automatic refresh policies**: Continuous aggregates use an automated job scheduler to run refreshes in the background at a user-defined interval. This removes the manual maintenance burden of keeping aggregates up to date.
- **Real time aggregation**: The query engine can combine pre aggregated data with the very latest raw data that has not yet been materialized. This provides up to the second results without the performance cost of aggregating the entire dataset on every query.

### Key Benefits for Time Series Workloads

- **Faster query performance**: By querying a smaller, pre aggregated dataset instead of the massive underlying hypertable, dashboards, and analytical queries can return results in milliseconds.
- **Storage savings**: Continuous aggregates enable downsampling. You can keep high granularity data for recent periods and automatically aggregate older data into lower granularity summary tables. You can even set retention policies to drop the original, high resolution data once it's no longer needed, saving significant disk space.
- **Reduced resource consumption**: Incremental refreshing uses far fewer CPU and I/O resources than a full recomputation. The automated background process also prevents performance spikes during peak insert times.
- **Hierarchical aggregation**: For complex analytics, you can create continuous aggregates on top of other continuous aggregates. For example, you can aggregate minute by minute data into hourly views, and then aggregate the hourly data into daily summaries.

### Continuous Aggregates vs. Standard PostgreSQL Materialized Views

| Feature           | Timescale Continuous Aggregates                                                                                                                                     | Standard PostgreSQL Materialized Views                                                                                                                                      |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Refresh method    | Refreshes are incremental and only process new or changed data. This makes them highly efficient for frequently updated time series data.                           | Refreshes are inefficient and computationally expensive, as the entire view is recomputed from scratch every time.                                                          |
| Automatic refresh | Includes built in, configurable policies for automated refreshes at user defined intervals, removing the manual burden.                                             | Does not have built in automation. Requires manual use of REFRESH MATERIALIZED VIEW or integration with an external scheduler like cron or a separate PostgreSQL extension. |
| Real time results | By default, continuous aggregates can provide up to the second results by combining the most recent raw data with the pre aggregated data.                          | Results are not real time. They are a static snapshot of the data at the time of the last refresh, so queries will miss any new data added since then.                      |
| Performance       | Optimized for time series workloads. They can use other TimescaleDB features like hypertables and columnar storage to improve performance and reduce storage costs. | Performance is limited by the full table recomputation on each refresh. This is especially problematic for very large, high volume datasets.                                |

### Implementation Example

In our project, we created a continuous aggregate for daily transaction summaries:

```csharp
// Create a continuous aggregate for daily transaction summaries
using (var cmd = new NpgsqlCommand(
    "SELECT view_name FROM timescaledb_information.continuous_aggregates WHERE view_name = 'daily_transaction_summary';", conn))
{
    var result = await cmd.ExecuteScalarAsync();
    if (result == null)
    {
        logger.LogInformation("Creating continuous aggregate view...");
        using var createViewCmd = new NpgsqlCommand(@"
            CREATE MATERIALIZED VIEW IF NOT EXISTS daily_transaction_summary
            WITH (timescaledb.continuous) AS
            SELECT
                time_bucket('1 day', ""CreatedAt"") AS bucket,
                ""AccountId"" AS account_id,
                count(*) AS transaction_count,
                sum(""Amount"") AS total_amount
            FROM ""Transactions""
            GROUP BY bucket, ""AccountId"";", conn);
        await createViewCmd.ExecuteNonQueryAsync();

        // Set refresh policy (refresh every hour)
        logger.LogInformation("Setting refresh policy for continuous aggregate...");
        using var refreshPolicyCmd = new NpgsqlCommand(@"
            SELECT add_continuous_aggregate_policy('daily_transaction_summary',
                start_offset => INTERVAL '3 days',
                end_offset => INTERVAL '1 hour',
                schedule_interval => INTERVAL '1 hour');", conn);
        await refreshPolicyCmd.ExecuteNonQueryAsync();
    }
}
```

This code creates a materialized view that aggregates transaction data by day and account, calculating the transaction count and total amount. We then set up a refresh policy that automatically updates the aggregate every hour, processing data from 3 days ago up to 1 hour ago.

In our repository layer, we leverage this continuous aggregate for efficient querying:

```csharp
public async Task<IEnumerable<object>> GetDailySummaryAsync(
    DateTime? from = null,
    DateTime? to = null,
    Guid? accountId = null)
{

    var conn = (NpgsqlConnection)_context.Database.GetDbConnection();
    if (conn.State != System.Data.ConnectionState.Open)
        await conn.OpenAsync();

    var sql = @"
        SELECT
            bucket::date as date,
            account_id,
            transaction_count,
            total_amount
        FROM daily_transaction_summary
        WHERE 1=1";

    var parameters = new List<NpgsqlParameter>();

    if (from.HasValue)
    {
        sql += " AND bucket >= @from";
        parameters.Add(new NpgsqlParameter("from", NpgsqlDbType.TimestampTz) { Value = from.Value });
    }

    if (to.HasValue)
    {
        sql += " AND bucket <= @to";
        parameters.Add(new NpgsqlParameter("to", NpgsqlDbType.TimestampTz) { Value = to.Value });
    }

    if (accountId.HasValue)
    {
        sql += " AND account_id = @accountId";
        parameters.Add(new NpgsqlParameter("accountId", NpgsqlDbType.Uuid) { Value = accountId.Value });
    }

    sql += " ORDER BY bucket DESC, account_id";

    // Execute query and return results
    // ...
}
```

## Time Bucketing: Simplified Time Series Analytics

Time bucketing is the process of grouping time series data into fixed, uniform intervals, or "buckets," for aggregation and analysis. It is most commonly performed using TimescaleDB's `time_bucket()` function, a powerful extension of PostgreSQL's `date_trunc()` function, which is designed to simplify and optimize time series queries.

By bucketing data, you can transform high volume raw observations into more manageable, fixed interval summaries. For example, you can calculate the average sensor reading every 5 minutes instead of processing every single data point.

### How time_bucket() Works

The `time_bucket()` function works by taking a time interval and a timestamp column. It returns a timestamp that represents the start time of the bucket for each data point. This bucket timestamp can then be used in a GROUP BY clause with standard aggregate functions like AVG(), SUM(), MAX(), and MIN() to summarize data within each bucket.

Syntax: `time_bucket(bucket_interval, time_column)`

In our continuous aggregate example above, we used `time_bucket()` to group transactions by day:

```sql
time_bucket('1 day', "CreatedAt") AS bucket
```

### Key Features of time_bucket()

- **Arbitrary intervals**: While standard PostgreSQL's `date_trunc()` is limited to predefined units like hour, day, or week, `time_bucket()` allows you to use any time interval (e.g., 5 minutes, 3 days, or 6 hours).
- **Origin alignment**: By default, `time_bucket()` aligns intervals based on a standard origin (the UNIX epoch). However, you can use the origin parameter to align buckets relative to a different start time. For example, weekly buckets can be aligned to start on a Sunday or Monday.
- **Timezone support**: With TIMESTAMPTZ data types, you can specify a timezone to ensure buckets are aligned correctly for a specific region, especially for daily or weekly aggregations.
- **Gap filling**: For datasets with missing data points, the `time_bucket_gapfill()` function can be used to generate rows for missing time intervals. This is especially useful for visualization in tools like Grafana, ensuring continuous data lines instead of jagged gaps.

### time_bucket() vs. date_trunc()

| Feature     | time_bucket() (TimescaleDB)                                                       | date_trunc() (Standard PostgreSQL)                                                                                |
| ----------- | --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Intervals   | Supports arbitrary intervals (e.g., '10 minutes').                                | Limited to fixed units (e.g., hour, week, month).                                                                 |
| Flexibility | Offers more flexible alignment options via the origin and offset parameters.      | Less flexible; alignment is based on the standard calendar definitions.                                           |
| Performance | Optimized for time series data, especially within continuous aggregates.          | Performs adequately for simple date part bucketing but can be less performant on very large time series datasets. |
| Gap filling | Built in functionality with time_bucket_gapfill() for handling missing intervals. | Requires a more manual, complex approach to fill gaps using subqueries or other techniques.                       |

## Compression Policies: Automated Storage Optimization

TimescaleDB compression policies automate the process of compressing old data in a hypertable to save storage space and improve query performance on historical data. A policy can be configured to automatically compress chunks once they reach a certain age, such as after a week or a month.

### How Compression Policies Work

- **Enabling compression**: Before a policy can be added, compression must first be enabled on the hypertable using the ALTER TABLE command.
- **Defining the policy**: The `add_compression_policy()` function schedules a background job that identifies and compresses chunks based on a defined age, specified as an INTERVAL.
- **Columnar storage**: When a policy is triggered, TimescaleDB automatically converts the data in the affected chunks from the standard row based format to a highly optimized columnar format. This process drastically reduces storage requirements and is key to achieving high compression ratios.
- **Optimizing for queries**: For further optimization, you can specify ORDER BY and SEGMENT BY columns when enabling compression.
  - **Segment by**: This groups data in the compressed columns based on a specific column, like a device_id. Queries filtering by this column become very efficient, as the system can retrieve and decompress only the relevant segments.
  - **Order by**: This pre-sorts the data within each compressed segment, which can improve the performance of queries that scan the data in time order.
- **Hybrid storage**: The policy driven approach creates a hybrid storage model within a single hypertable. Recent data, which is more frequently updated and queried, remains in a fast row based format. Older, "colder" data is automatically transformed into the space efficient columnar format. This means you get the best performance for both recent and historical queries without any manual management.

### Key Benefits

- **Massive storage savings**: By converting data to a columnar format and applying specialized compression algorithms, TimescaleDB can achieve storage savings of 90% or more on time series data.
- **Improved query performance**: For analytical and aggregate queries that scan large time ranges, reading from a compressed columnar store is much faster than scanning an uncompressed row based store.
- **Automated management**: A compression policy automates the entire lifecycle of your data. Once set, you don't need to manually run compression jobs or worry about storage management.
- **Optimized for typical workloads**: The hybrid row/columnar approach aligns with common time series workloads, where recent data is written and queried frequently, while older data is mainly used for large scale analytics.

## Conclusion

TimescaleDB extends PostgreSQL with powerful features specifically designed for time series data, making it an excellent choice for applications that need to store and analyze large volumes of time stamped data. In our financial transaction API project, we've seen significant performance improvements and reduced complexity by leveraging hypertables, continuous aggregates, time bucketing, and compression policies.

The best part is that TimescaleDB maintains full compatibility with the PostgreSQL ecosystem, so you can continue using your favorite tools, libraries, and extensions while gaining these specialized time series capabilities.

If you're building an application that deals with time series data, I highly recommend giving TimescaleDB a try.

Happy coding!
