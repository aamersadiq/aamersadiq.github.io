---
title:  "SQL Analytics Functions - Windowing Clause Default Rows Behaviour (with FIRST_VALUE, LAST_VALUE example)"
date:   2023-01-27 13:15:43
categories: [sql]
tags: [sql]	
---
Windowing clause of analytical function provide more fine-grained access to the data. It uses ROWS/RANGE that limits the rows within the partition by specifying start and end points within the partitio.

By default, the windowing clause is set to ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW, which specifies that the data to be included starts from the current row and goes up within the partition (if no partition is used, then it includes the entire table).

The following dataset will be used for all SQL examples provided.

<img src="{{ site.baseurl }}/images/blog/sql-window-clause/sql-dataset.png" class="fullsize-image" alt="Image">

Let's take an example to illustrate this by finding the shortest and tallest person in each genger in the dataset. We are using FIRST_VALUE/LAST_VALUE which will retired the first/last value within the column. I have also added the default windowing clause.

To illustrate this, let's consider an example where we want to find the shortest and tallest person for each gender in the dataset. For this purpose, we will use the FIRST_VALUE/LAST_VALUE functions, which retrieve the first/last value within a column. Default windowing clause is included it in the query as well.
<pre><code>
select Name, Height,
FIRST_VALUE(NAME) OVER (PARTITION BY GENDER ORDER BY Height ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) as ShortestPerson,
LAST_VALUE(NAME) OVER (PARTITION BY GENDER ORDER BY Height ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) as TallestPerson
from Person
order by Gender, Height
</code></pre>

<img src="{{ site.baseurl }}/images/blog/sql-window-clause/default.png" class="fullsize-image" alt="Image">

First, let's analyze the selection of the shortest person. By using the FIRST_VALUE function with ascending order, the query will return the name of the first value within the partition. In this case, it will select Eva and Satya as the shortest person in each respective partition, which is the correct result.

Now, let's take a look at the selection of the tallest person. We expected Aaron and Megan to be the tallest person in their respective gender, but that's not the result we are getting. 
If we focus on the male partition and the row with Brian, we can see that the default window clause will include everything from the current row (Brian row) to the top of the partition. The rows included at the Brian row will be Satya, James, and Brian, and these rows will be passed to the LAST_VALUE function. Out of these rows, Brian is the last value and appears as the tallest person, as illustrated below.

<img src="{{ site.baseurl }}/images/blog/sql-window-clause/default-window.png" class="fullsize-image" alt="Image">

However, this is not the desired outcome. To correct this issue, we need to modify the windowing clause from the default to ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING. This will enable us to retrieve all the rows before and after the current row within the partition. Let's apply this change to the query.

<pre><code>
select Name, Height,
FIRST_VALUE(NAME) OVER (PARTITION BY GENDER ORDER BY Height) as ShortestPerson,
LAST_VALUE(NAME) OVER (PARTITION BY GENDER ORDER BY Height ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) as TallestPerson
from Person
where Gender = 'M'
order by Gender, Height
</code></pre>

<img src="{{ site.baseurl }}/images/blog/sql-window-clause/fixed.png" class="fullsize-image" alt="Image">

Again focus on the male partition and the row with Brian again. After updating the windowing clause to ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING, the query will now retrieve all the rows before and after the current row at Brian. At the Brian row, the rows included will be Satya, James, Brian, Andrew, and Aaron, which will then be passed to the LAST_VALUE function. Out of these rows, Simon is the last value and will appear as the tallest person, as illustrated below. This is the correct result that we were expecting.

<img src="{{ site.baseurl }}/images/blog/sql-window-clause/fixed-window.png" class="fullsize-image" alt="Image">

It's worth noting that you can use the window clause ROWS BETWEEN x PRECEDING AND x FOLLOWING to include x number of rows before and after the current row in the calculation.
