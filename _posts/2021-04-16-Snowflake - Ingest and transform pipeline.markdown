---
title:  "Snowflake - Ingest and transform pipeline - Design"
date:   2021-04-16 9:56:12
categories: [data]
tags: [data]	
---
If you're planning to establish a data transformation pipeline that processes raw data and generates analytics-ready data for consumers, the following steps depict the data flow from source to destination.:

<img src="{{ site.baseurl }}/images/blog/snowflake-ingest-and-transform-pipeline/snowflake-pipeline.png" class="fullsize-image" alt="Image">

<h4>1</h4>
Retrieve data from the data source, and store it in an AWS S3 bucket in CSV format. Extracting from the database can be achieved by scheduling a job that moves newly inserted or updated data.

<h4>2</h4>
Snowpipe, which is part of Snowflake, is an event-driven data ingestion pipeline that can be activated by cloud storage event notifications(AWS S3, GCP CS, and Azure Blob). Create a Snowpipe trigger for AWS S3 so it can generate a table per CSV file. Each table will have one column that contains data from each line in the CSV file. This is referred to as the T1 Transit Staging area, where the data will be stored until processing is complete, at which point it will be deleted.

<h4>3</h4>
To automate the process of moving data through different stages of transformation in Snowflake, you can create a scheduled task that triggers a stored procedure. This stored procedure will perform the necessary transformations on the data and move it to the next stage.

<h4>4</h4>
In the first stage of data transformation, the data from the T1 Transit Staging database will be moved to the T2 Persistent Staging database. During this stage, each piece of data will be separated into its own column in the table, and the normalization of the data can be performed. It is important to note that the data in the T2 Persistent Staging area will not be deleted.

<h4>5</h4>
In the second stage of data transformation, the data from the T2 Persistent Staging area database be moved to the T3 Integration database. During this phase of data transformation, the data will be enriched by performing lookups from other data sources.

<h4>6</h4>
In the initial phase of data transformation, the data will be moved from the T3 Integration Database area to the T4 Presentation Database. During this stage, the data will be prepared in a way that makes it easy for the presentation client to consume. It is worth noting that there can be several Presentation Databases per client.

<h4>7</h4>
The presentation client can obtain data to display to users by using database views.



