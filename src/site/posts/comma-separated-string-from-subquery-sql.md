---
layout: layouts/post.njk
title: Creating an SQL query where one column contains aggregated data from another table
subtitle: An SQL exercise that goes through a work request, problem and solution.
date: 2020-12-15
tags:
    - tutorial
    - sql
    - feature
    - post
---

Every now and then there can be a need to create a comma separated string that aggregates information from another table. Below is an example scenario. 

## Work request

We have been asked to list all the pick zones for the items in cartons that are `in_distribution`.  Often a carton contains items that are in more than one pick zone.

The following columns are being requested.

1. `cartons.carton_id` the number of the carton/box that the items will be contained in
1. `cartons.status` the status of the carton `['pending', 'in_distribution', 'fulfilled']`
1. `zones.code` in a comma separated string showing each zone where an item is located for the carton
1. `wave.id` the ID of the wave which represents the batch of item picks given to the warehouse floor to be fulfilled
1. Number of items within the carton which will need to be summed from `carton_items.id` which has a unique ID for each item, regardless of `SKU` or `stock_item.id`

## Database Schema

We have the following tables:

1. `cartons` holding information for a carton/box that holds items
1. `carton_items` holding what items are in what cartons/box
1. `waves` holding information on distribution batches that are given to the warehouse floor
1. `zones` holding information about what zones items are located in within a location/warehouse
1. `locations` holding information for each warehouse holding stock

In a location/warehouse you often have the same stock item located on different shelves within the same warehouse. It may sound counterintuitive, but having multiple locations for popular stock items can greatly speed up the fulfillment process. Usually high demand stock items are paired closely together in order to increase pick efficiency and ultimately faster fulfillment times.

## The problem

If a carton contains items that have more than one zone, they will duplicate rows. We've been asked specifically to list all the zones in the same column and only have one row per carton.

## Incorrect query

``` sql
SELECT
cartons.id,
cartons.status,
zones.code,
waves.id,
sum(carton_items.quantity) as item_quantity
FROM cartons WITH (NOLOCK)
JOIN waves WITH (NOLOCK) ON waves.id = cartons.wave_id
JOIN carton_items WITH (NOLOCK) ON carton_items.carton_id = cartons.id
JOIN zones WITH (NO LOCK) ON zones.id = carton_items.zone_id
WHERE
cartons.status = 'in_distribution'
GROUP BY
cartons.id,
cartons.status,
zones.code,
waves.id
ORDER BY
waves.id ASC,
cartons.id ASC
;
```

## Incorrect query output

The below output shows that we would get 3 rows back from this query which is not the desired result.

``` text
|------------|---------------|-----------------|----------|--------------|
|cartons.id  |cartons.status |zones.code       |waves.id  |item_quantity |
|------------|---------------|-----------------|----------|--------------|
|12345       |in_distribution|OR               |987654321 |12            |
|12345       |in_distribution|GR               |987654321 |12            |
|12345       |in_distribution|BL               |987654321 |12            |
```

Instead we want to combine all the results in `zones.code` so that only one carton appears per row.

## Solution query

Because we're going to combine all `zones.code` into one string, I'm going to change the name of the column to `zones_list`. We're going to acheive this by creating a subquery that grabs all of the `zones` connected to `carton_items`. We'll be using two SQL functions `STUFF()` and `FOR XML PATH` to ensure the string is properly comma delimited.

``` sql
SELECT
cartons.id,
cartons.status,
STUFF((SELECT  ', ' + z.code
        FROM cartons AS c WITH (NOLOCK)
        JOIN carton_items AS ci WITH (NOLOCK) ON ci.carton_id = c.id
        JOIN zones AS z WITH (NOLOCK) ON ci.zone_id = z.id 
			  WHERE c.id = cartons.id
			  GROUP BY
			  z.code
        FOR XML PATH('')), 1, 1, '') AS zones_list,
waves.id,
sum(carton_items.quantity) as item_quantity
FROM cartons WITH (NOLOCK)
JOIN waves WITH (NOLOCK) ON waves.id = cartons.wave_id
JOIN carton_items WITH (NOLOCK) ON carton_items.carton_id = cartons.id
WHERE
cartons.status = 'in_distribution'
GROUP BY
cartons.id,
cartons.status,
waves.id
ORDER BY
waves.id ASC,
cartons.id ASC
;
```
## Solution query output

If there are three zones connected to a carton OR, GR, BL we now get a single row returned with a comma separated list in the form of a string as requested.

``` text
|------------|---------------|-----------------|----------|--------------|
|cartons.id  |cartons.status |zones_list       |waves.id  |item_quantity |
|------------|---------------|-----------------|----------|--------------|
|12345       |in_distribution|OR, GR, BL       |987654321 |12            |
```

## Explaining the syntax

The `STUFF() function` allows us to create a string by "stuffing" things inside of it.

There are 3 parameters which are:

1. the string we want to stuff (in our case it's the string we're creating from the subquery)
2. the location we want to start deleting and inserting characters (in our case the first character)
3. the number of characters we want to delete (the first character being output by our subquery is a `,` and we definitely want to delete that)

`FOR XML PATH('')` at the end of the query is a function used to create XML from a query. By declaring an empty string in the function `''` we can comma delimit each entry.

The importance of the `STUFF()` function is that it removes the first comma from the list.
