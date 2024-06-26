---
title:  "SQL Analytics Functions - ORDER BY Clause (with ROW_NUMBER example)"
date:   2023-01-18 13:15:43
categories: [sql]
tags: [sql]	
---
ORDER BY clause imposes ordering on incoming data. It is required by some analytics functions (ROW_NUMBER(), LEAD(), LAG(),FIRST_VALUE()...) but does not make sense on other (COUNT(), MIN()).

ORDER BY syntax.
<pre><code>
function(...) OVER (...ORDER BY col3, col4)
</code></pre>

The following dataset will be used for all SQL examples provided.

<img src="{{ site.baseurl }}/images/blog/sql-order-by/sql-dataset.png" class="fullsize-image" alt="Image">

Lets consider an example  which uses ORDER BY clause and create an ever increasing integer value starting at 1,  subsequent rows will get next higher value. 
<pre><code>
select per.*,
ROW_NUMBER() OVER (ORDER BY Height) as RowNumber
from Person per 
</code></pre>

<img src="{{ site.baseurl }}/images/blog/sql-order-by/order-by.png" class="fullsize-image" alt="Image">

In the above case, the ORDER BY function is applied to each person's height in ascending order, so that the shortest person is listed first. ROW_NUMBER() function is applied to whole table, meaning that it creates the row number for each person. The resulting table will have an additional column showing the row number for each person.

ROW_NUMBER can also be used with PARTITION BY which will resets it to 1 when crossing partition boundary.
<pre><code>
select per.*,
ROW_NUMBER() OVER (PARTITION BY Gender ORDER BY Gender) as RowNumber
from Person per 
</code></pre>

<img src="{{ site.baseurl }}/images/blog/sql-order-by/order-by-partition.png" class="fullsize-image" alt="Image">

In this case, the ROW_NUMBER() function is partitioned by the Gender column, meaning that it creates the row number separately for each gender. The ORDER BY clause is applied within each partition to order the heighs in ascending order, so that the shortest person in each gender is ranked first. The resulting table will have an additional column showing the row number of each peson within their gender.