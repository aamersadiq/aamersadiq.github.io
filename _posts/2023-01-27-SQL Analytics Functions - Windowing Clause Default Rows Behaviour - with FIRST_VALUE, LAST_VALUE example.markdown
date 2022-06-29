---
title:  "SQL Analytics Functions - Windowing Clause Default Rows Behaviour (with FIRST_VALUE, LAST_VALUE example)"
date:   2023-01-27 13:15:43
categories: [sql]
tags: [sql]	
---
Sample dataset:
image

Windowing clause of analytical function provide more fine-grained access to the data. It uses ROWS/RANGE that limits the rows within the partition by specifying start and end points within the partitio.

The default option for the windowing clause is ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW, which means that the data to be viewed starts from the current row and goes up within the partition (if no partition is used, then it includes the entire table).

Let's take an example to illustrate this by finding the shortest and tallest person in each genger in the dataset. We are using FIRST_VALUE/LAST_VALUE which will retired the first/last value within the column. I have also added the default windowing clause.

code
select *,
FIRST_VALUE(NAME) OVER (PARTITION BY GENDER ORDER BY Height ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) as ShortestPerson,
LAST_VALUE(NAME) OVER (PARTITION BY GENDER ORDER BY Height ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) as TallestPerson
from Person
order by Gender, Height


image
default

Lets first examine the shortest person selection. Shortest person using FIRST_VALUE with ascentding order will return the name of the first value within the partition, which means it will pick Eva and Satya as the shortest person in each respective partition and that is the correct result.

Now lets look at tallest person. What was expected for tallest was Aaron and Megan in their respective gender. But that's the result we are getting. We are getting  incorrect person which is not the the tallest person. 
Focus on males partition and row with Brian. On Brian row, default window clause will see everything from current row to all the way up till the top of partition. The rows seen at Brian row will be Satya, James and Brian and these rows will be passed to LAST_VALUE function and out of  these Brian is the last value and appears as tallest as illustrated below. 

image
default-window

But that not what we want. To fix it we need to change the windowing clause from default to ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING which will allow as get everying before the current row and everything after the current row within the partition. Lets apply the change.

code
select *,
FIRST_VALUE(NAME) OVER (PARTITION BY GENDER ORDER BY Height) as ShortestPerson,
LAST_VALUE(NAME) OVER (PARTITION BY GENDER ORDER BY Height ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) as TallestPerson
from Person
where Gender = 'M'
order by Gender, Height

image
fixed



Again focus on males partition and row with Brian. On Brian row, updated window clause will see everything before and after the current row. The rows seen at Brian row will be Satya, James,Brian,Andrew and Aaron and these rows will be passed to LAST_VALUE function and out of these Simon is the last value and appears as tallest as illustrated below. 
imag
fixed-window

Final note that you can use the window clause ROWS BETWEEN x PRECEDING AND x FOLLOWING to include x back and front from the current row.
