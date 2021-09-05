---
layout: layouts/post.njk
title: How to Use Rollbar for Logging in a Custom App
subtitle: How to integrate with Rollbar for additional logging, visualization, automation and query capability
date: 2020-12-27
tags:
    - tutorial
    - php
    - feature
    - post
---

I run an automation service and each night it runs through several cron jobs that ETL data and deliver it to various services. For a long time I used [Pushover](https://pushover.net) to tell me of the status of how many records each job parsed and sent over and alert me if there are any issues.

## Pushover FTW!

What I like about Pushover is that you can create groups and have alerts and notifications post to a group. It's a very low cost way to have small groups be aware of any problems. In my case, I have it alert product managers, customer service and email marketing campaign managers, so that if a service goes down that impacts customers, they know to delay a campaign, or help navigate an issue with a customer.

## Why Rollbar?

After the automation service started to increase in its number of jobs, the Pushover notifications coming to my phone started to get noisy. I had enough information to know it was running well, and I still get alerts when issues occur. However, now I wanted to log each job, how many records it parsed, when and what files were created and where it dropped them off.

I started to create a database schema to track these "Sync Events." However, as I was thinking ahead, I thought of the need to create visualizations, error reporting, querying the logs and undoubtably more.

I had played around with Rollbar enough. The pricing is very fair, especially if you're not logging a large amount of data. Rollbar is also very friendly to integrate with and use. It's helped debug certain issues quickly as you can verify what occurred and at what time. It's been a good upgrade.

## Integrating Rollbar with PHP

I followed the basic PHP installation and setup in the [Rollbar docs](https://docs.rollbar.com/docs/basic-php-installation-setup).

I installed Rollbar using [Composer](https://getcomposer.org), which automatically handles loading the classes into the project. 

``` json
{
    "require": {
        "rollbar/rollbar": "^1"
    }
}
```

The I added the Rollbar config to my own app configuration file that is loaded on each page and job that runs on the app.

``` php
use \Rollbar\Rollbar;

$config = array(
    // required
    'access_token' => 'YOUR_ACCESS_TOKEN',
    // optional - environment name. any string will do.
    'environment' => 'YOUR_ENVIRONMENT_NAME',
    // optional - path to directory your code is in. used for linking stack traces.
    'root' => '/var/www/myapp'
);
Rollbar::init($config);
```

This took care of the basics of application errors, warnings and critical events. But what I wanted to do was send specific informational events with additional data. Rollbar is fantastic at handling this, as you can send an array over in your message/log and it will automatically save the data and allow you to view and query that data in Rollbar. This can also allow you to setup specific notifications based on certain events. This is what got me most excited about using Rollbar.

## Sending a custom message to Rollbar

Below you can see that I post a message at a level of `INFO` and then send over an array of data which can then be stored as an object along with the event.

``` php
// LOG TO ROLLBAR
Rollbar::log(
  Level::INFO,
  "Cron: $service $sync_event_name ($status)",
  // key-value additional data
  array(
    "sync_event" => "$sync_event_name",
    "service" => $service,
    "service_location" => $_SERVER['PHP_SELF'],
    "records_sent" => $records_sent,
    "success" => $status,
    "duration_seconds" => $duration_seconds,
    "file_name" => $csv_filename
  )
);
```

## How Rollbar categorizes things: Items vs Occurances

Each message/log that is sent over from your app to Rollbar will be classified and grouped as an "Item." This means if you send over two logs that have the same message, it will categorize them as one item. This helps Rollbar determine how frequently a particular item is occuring.

Occurances are unique instances of log events that can roll up into an item. This helps you look into each specific occurance.

For example, if you go into an item in Rollbar, you will see a tab called "Occurances." Here you will see a list of each occurance of an item. It has some basic information already for each item, such as a Timestamp of the occurance, the IP of the host etc.

Along with each occurance you will also see additional fields under `extra`. Each array key and value can be access under the `extra` object in Rollbar. For example, above we are sending over `records_sent`. We can see this data in the `extra.records_sent` column and it is now also queryable in RQL (Rollbar Query Language)

## Rollbar RQL (Rollbar Query Language)

Rollbar has its own SQL interface for querying log data called RQL. It's effectively vanilla SQL that has syntax very similar to MySQL or PostGres.

``` sql
SELECT *
FROM item_occurrence
WHERE item.counter = 88
ORDER BY timestamp DESC
LIMIT 20
```

In the above SQL statement we can pull all occurences of item number 88. (Rollbar automatically numbers items/messages it receives and does its best to group them together.) If you have a good naming convention for your messages, it greatly helps Rollbar in how it groups them.

If we want to see the number of occurances for a specific item in the last 24 hours we could simply change the `WHERE` statement to be:

``` sql
SELECT *
FROM item_occurrence
WHERE item.counter = 88
    AND timestamp > unix_timestamp() - 60 * 60 * 24
ORDER BY timestamp DESC
LIMIT 20
```

## Conclusion

Rollbar is a very cost effective solution for logging management. Like any logging service, taking care to set it up well can help many teams understand what is happening and reducing needless messages/items will help ensure that it won't get to a noise level where people ignore all logs, even the important ones!

Setting up custom RQL statements and notifications can help automate your application alerts so that you can get notified of specific issues, like credit card fraud, or third-party service endpoints failing.
