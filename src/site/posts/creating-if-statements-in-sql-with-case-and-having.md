---
layout: layouts/post.njk
title: Creating IF functionality with SQL using CASE and a subquery
subtitle: How combining CASE and a subquery can give you SQL IF statement functionality
date: 2021-08-30
tags:
    - tutorial
    - sql
    - post
    - feature
---

When you have an app that is a number of years old, you'll find that the database schema has changed over the years. As a DB admin or analyst, this can be very frustrating as you'll find that you have to write logic in your SQL to work around having data you need in two different places/tables, with the need to combine them into one column for analysis.

## Scenario

You need to export all of your article and author data from one database to another. The problem is that author data is stored in two locations after a change was made to the database schema.

Initially the app stored authors within the `article` table as a string value in the `author` column.

<div class="table">

article_id    |author        |slug          
--------------|--------------|--------------
1             |Mary Poppins  |twas-an-east-wind
2             |Jane Banks    |my-dad-doesnt-listen         

</div>

After a while, a change was made to the app where two new tables were created, `authorships` and `author`. This allowed multiple authors to be connected to an article; a much better choice of schema. Now in the `authors` table we can have additional information about the author that can be used to populate an author page and have separate links on an article page and ensure we keep each author separate in the database.

<div class="table">

author_id    |first_name    |last_name     |twitter
-------------|--------------|--------------|-----------
110          |Mary          |Poppins       |twitter.com/mpoppins
111          |Jane          |Banks         |twitter.com/jbanks

</div>

The `authorships` table can associate many articles to many authors. In the example below we have two authors `110` and `111` assigned to `article_id` `1`.

<div class="table">

author_id    |article_id
-------------|--------------
110          |1             
111          |1             

</div>

The problem is that when the new tables were created, there wasn't a migration for the old data to the new tables. So some articles will have the author information stored in the `articles` table and other articles will have a `BLANK` value in the `articles` table and instead have information stored in the `authorships`/`authors` tables.

So you are now faced with three tables (`articles`, `authorships` and `authors`) that have unique information for the authorship of each article.

### Tech debt

Why did the app developer not migrate the author data? The app requires logic to render the article page properly. It has to now check both tables for an author entry and use an IF statement to determine what table to use. Ideally the app developer would have migrated all old articles to use the new `authorships` and `author` tables, so that all article data is stored in the same way in the database, instead of creating a divide in how the author data is stored.

This is called "technical debt" and you need to pay up.

## IF statement to the rescue

If you were to solve this with an IF statement with a scripting language like Python, you could do the following:

```python
# check to see if the article table has an author
  if ( article_author != NULL ):
    # get the author from the article table
    author_byline = article_author
  else:
    # get the author from the authorships table
    author_byline = authorship_author
```

This is straighforward and may be the first thing you think of doing. IF/ELSE is what computers were designed for. However when you're doing analysis, you'll want to avoid having to run your analysis with an additional scripting language like Python etc. We can *transform* the data on the fly using the right SQL operators. 

## Creating an IF statement with SQL using `CASE`

`CASE` is a great way to perform conditional logic in your SQL. You can throw it in your `SELECT` or `WHERE` statement. Here is an example.

```sql
SELECT
  CASE
    WHEN author.first_name = 'Mary' AND author.last_name = 'Poppins'
    THEN CONCAT(author.first_name, ' ', author.last_name, ' is practically perfect in every way')
    ELSE CONCAT(author.first_name, ' ', author.last_name)
  END
FROM
authors
;
```

Here we check the author name. `WHEN` author matches 'Mary Poppins' `THEN` we create a new value on the fly by using [CONCAT()](https://www.postgresqltutorial.com/postgresql-concat-function/) to put together a string of text that appends ' is practically perfect in every way'. The end result being "Marry Poppins is practically perfect in every way".

Silly example, but you get the point.

### Writing the initial query

I always like to write pseudo SQL in the beginning so I can annotate the information I need with any details that are helpful to remember. I also add a `LIMIT 100` (if using MySQL or Postgres) or `TOP 100` (if using MSSQL) to ensure I'm not burdening the database server. I do this as a habit to ensure I'm not needlessly grabbing 100,000's of rows for no reason. I just need to QA a small data set for now.

```sql
SELECT
articles.article_id
, articles.article_url
-- check for the author name from the articles table
, articles.author
-- check for the author name from the authorships/author table
, CONCAT(authors.first_name, ' ', authors.last_name)
, articles.article_status
FROM
articles
LIMIT 100
```

This query is unusable. But it's a starting point to help me understand the data objectives I have.

You may notice that in order to get `authors.first_name` and `authors.last_name` you would need to `JOIN` two tables `authors` and `authorships`. That _could_ be done with the following `JOIN` statements:

```sql
LEFT JOIN authorships ON articles.article_id = authorships.article_id
LEFT JOIN authors ON authors.author_id = authorships.author.id
```

I'd do a `LEFT JOIN` here, as I know that only a subset of articles have author information assigned to them. A `LEFT JOIN` ensures that I show all the article rows even if the `authorships` table doesn't have any data connected to an article. If I left it as a simple `JOIN` or `INNER JOIN` then the only results I'd get back would be articles that _do_ have a connected author via the `authorships` table.

This all said though, we actually want to add this `JOIN` inside of a subquery. 

### Writing an SQL Subquery

A subquery is a regular query that is put inside of parenthesis. You can add a subquery in a `SELECT` statement, or in a `WHERE` statement. I personally like to write my subqueries separately and test them before adding them as a subquery. Helps with QA.

### Complete SQL query



```sql
SELECT
article.article_id
, article.url
, CASE
	WHEN article.author IS NULL
	THEN (SELECT CONCAT(authors.first, ' ', authors.last)
			FROM authors
			JOIN authorships ON authorships.author_id = authors.author_id
			WHERE authorships.article_id = a.article_id 
			LIMIT 1)
  ELSE article.author
  END as author
, articles.article_status
FROM
articles
;
```