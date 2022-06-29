---
title:  "SQL Analytics Functions (also known as Window Functions)"
date:   2023-01-05 10:15:43
categories: [sql]
tags: [sql]	
---
Analytic functions are used to calculate an aggregate value based on a group of rows. While aggregate functions return a single value for each group, analytic functions can return multiple rows for each group. They are particularly useful for computing moving averages, running totals, percentages, or top-N results within a group. By using analytic functions, you can gain insights into the distribution and patterns of data within a group, and make more informed decisions based on the results.

Let's briefly examine aggregate functions before delving into analytics functions.

<h4>Aggregate Functions</h4>
Aggregate functions, such as COUNT() and SUM(), operate on a set of values to compute a single result.

The following dataset will be used for all SQL examples provided.
<img src="{{ site.baseurl }}/images/blog/sql-analytic-functions/sql-dataset.png" class="fullsize-image" alt="Image">

Lets use COUNT() as an example which could be used to determine the number of males and females in a given dataset.
<pre><code>
select Gender, Count(*) as GenderCount from Person
GROUP BY Gender
</code></pre>

<img src="{{ site.baseurl }}/images/blog/sql-analytic-functions/sql-aggregate-count.png" class="fullsize-image" alt="Image">

In this case, the GROUP BY clause is specified as GROUP BY Gender. This means that the COUNT() function will be applied separately to each group of gender in the dataset. The resulting table shows the number of person of that gender, for each gender.

Aggregate function results in fewer output rows than input rows, as illustrated in the diagram below:
<img src="{{ site.baseurl }}/images/blog/sql-analytic-functions/aggre-funnel.png" class="fullsize-image" alt="Image">

<h4>Analytics Functions</h4>

Analytics functions on the other hand provide access to data in a non-sequential manner, with data partitioned into chunks and fed into the function. COUNT() and SUM() can also be utilized in an analytical manner, with similar behavior to when they are used in an aggregate manner.. Examples of other analytics functions include LEAD()/LAG() which allow access to a particular row bofore or after current row. Analytic functions are the final operations performed on the results of a SQL query and are not affected by GROUP BY, HAVING, or WHERE clauses.

Analytic function syntax.
<pre><code>
function(...) OVER (PARTITION BY col1, col2,... ORDER BY col3, col4... windowing-clause) As column-name
</code></pre>

Getting COUNT() for each row using window function.
<pre><code>
select *, 
COUNT(*) OVER (PARTITION BY Gender) as GenderCount
from Person
</code></pre>


<img src="{{ site.baseurl }}/images/blog/sql-analytic-functions/sql-analytic-count.png" class="fullsize-image" alt="Image">

In this case, the partition clause is specified as "PARTITION BY Gender". This means that the COUNT()  function will be applied separately to each gender group in the dataset. The resulting table will have additional column showing the count in each gender group.

Although it is possible to obtain the same result without using analytic functions, it would require the use of inner joins involving intermediate tables in the SQL query as per below example.

Getting COUNT() for each row without analytic function.
<pre><code>
select per.*, gencnt.GenderCount
from Person per 
inner join (select Gender, Count(*) as GenderCount from Person
group by Gender) gencnt on per.Gender = gencnt.Gender
</code></pre>


For windows clause, resulting row count is same as incoming as illustrated in the diagram below:

<img src="{{ site.baseurl }}/images/blog/sql-analytic-functions/analy-funnel.png" class="fullsize-image" alt="Image">