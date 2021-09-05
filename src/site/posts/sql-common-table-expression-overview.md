---
layout: layouts/post.njk
title: Using Common Table Expressions in SQL to Improve Your ETL Process
subtitle: How and when to use common table expressions in SQL using a common sense approach.
date: 2021-08-05
tags:
    - tutorial
    - sql
    - feature
    - post
---

At the core of any application, reporting suite, analytics platform or integration service are data set(s). Most often, this data set is stored in a database which can be queried via SQL. Even non-traditional data stores can offer some form of SQL syntax that can be used to query stored data. SQL is a powerful language that anyone who is seeking to integrate, analyze or develop applications should consider being well-versed in its fundamentals.

## Extract, Transform and Load (ETL)
ETL is an effective approach to integrating your data into other systems and services. The process is as simple as the following:

- **Extract**: get the data
- **Transform**: shape the data
- **Load**: load the transformed data to the new location

Sounds easy, right? Conceptually it is. The fun is *understanding* the data sufficiently to know what you should be extracting, how you should be transforming and what assumptions the new system will have when you load the data. 

## Using Common Table Expressions (CTE) to simplify your Extract process (and do some Transform too)
Common table expressions allow you to create temporary tables that can be referenced later in your SQL statement. You write a CTE in the same way you'd write a subquery, except you wrap all your CTE's in a single `WITH()` clause and define the new temporary table name. I'm using the table name `new_cte_table` as an example.

``` sql
WITH (
  new_cte_table as (
    -- type your SELECT query here
  )
)

SELECT
  new_cte_table.column1
  -- above is how you'd refer to your CTE table
```

Let's say you have been asked to grab all the customers from a database with a `customer_class` of `'vip'` who purchased something in 2020. Traditionally you'd write the following `SELECT` statement.

``` sql
SELECT
  c.customer_id as customer_id
  , c.email as customer_email
  , sum(sh.total) as customer_spend
FROM customers AS c
JOIN sales_headers AS sh ON sh.customer_id = c.customer_id
WHERE
  c.customer_class = 'vip'
  AND date(sh.completed_at) between '2020-01-01' and '2020-12-31'
;
```

This works just fine for this specific request.

> Uhm, so why would you want to use CTE's instead of using simple `JOIN` statements?

The issue comes when you need to add more complexity. This especially occurs when you need to change the [grain of data](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/grain/).

> Declaring the grain is the pivotal step in a dimensional design. The grain establishes exactly what a single fact table row represents.
<p class="caption"><a href="https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/grain/" title="Kimball Group definition of data grain">Kimball Group</a></p>

When you've been asked to add an additional column that requires an aggregate sum of all the rows for a calculation, you're being asked to change the grain. The `sales_header` grain is order level. The new report that you're being asked to make is aggregating all sales by customer and creating a new measure that shows the percentage of "vip" customer spend in comparison to all customer spend. Ultimately, you're being asked to *flatten* normalized data. This just means you're taking data from different tables and turning it into one table. This is also known as *shaping your data*.

Now we know we need three things:

1. One row per customer (only VIP customers)
2. Sum of total sales per customer (only VIP customers)
3. Total sales of *all* customers, which we'll then use to calculate the percentage of sales each VIP customer is contributing individually.

## Creating the query


``` sql
-- GRAB ALL OF THE SALES DATA FOR 2020
WITH 
sales_header_data as (
  SELECT
    sh.customer_id as customer_id
    , sh.total as order_total
    , sh.completed_at as order_date
  FROM sales_header sh
  WHERE
    date(sh.completed_at) between '2020-01-01' and '2020-12-31'
)
, sales_aggregate as (
  SELECT
    sum(order_total) as all_orders_total
  -- use the table we just created to ensure the right filter/dates are applied
  FROM sales_header_data
)
-- GET ALL OF THE CUSTOMER DATA WITH THE FILTER OF "customer_class = 'VIP'"
, customer_data as (
  SELECT
    c.email as customer_email
    , c.customer_id as customer_id
  FROM customers c
  WHERE
  c.customer_class = 'vip'
)

-- NOW THAT YOU HAVE THE DATA YOU WANT, USE THE SET OF DATA IN THE 'FROM'
-- AND YOU'LL BE WORKING ONLY WITH THE DATA IN THAT DATA TABLE/SET 
SELECT
  customer_data.customer_id as customer_id
  , customer_data.customer_email as customer_email
  , sum(sales_header_data.total) as customer_spend
  , (sum(sales_header_data.total) / sales_aggregate.all_orders_total) as perc_total_customer_spend
FROM sales_header_data
JOIN customer_data ON customer_data.customer_id = sales_header_data.customer_id
GROUP BY
  customer_data.customer_id
  , customer_data.customer_email
ORDER BY
sum(sales_header_data.total) DESC
;
```

## Conclusion
Think of a common table expression as part of your extraction along with any simple transforms that are needed, such as casting or applying calculations. It's also a way to ensure you simplify the data set that you're extracting. You only need to filter your data once and then you can refer to that data set for aggregates and JOINS later. This is done by using CTE's to establish the base data sets which you can extrapolate from later. Then use your main query to help finish off the shape of your data.

I hope this was helpful. It's helped improved my thinking around how I write SQL statements for extraction and transformation.
