---
layout: layouts/post.njk
title: Create an AWS Lambda to Query Data with Athena and Output to S3 Bucket using Python
subtitle: A serverless example using AWS Lambda, Athena and Python to ETL data
date: 2021-08-16
tags:
    - tutorial
    - python
    - aws
    - feature
    - post
---

Traditionally I've used servers to run ETL jobs. PHP specifically has been my main tool over the years. I've used PHP since my Wordpress days back in 2007 and I've enjoyed using it. Simple, readable and readily available across the web ecosystem.

However, as great and easy as it is to get rolling, when you're running production services, worrying about the infrastructure and dev ops side of things can catch you off-guard and ruin your day if there is a problem. Serverless services such as AWS Lambda promise to take away that pain and provide you a way to focus on the real thing you care about, the execution of the code you write.

I'm a sucker for simplicity. I'll work really hard to be really lazy.

## Why Amazon Athena?

Databases cost a lot to run in both time and cash. If you don't need high availablity and speed for your database, you should consider storing your data in a storage solution like Amazon Athena. It will store your data in S3 at S3 prices and only charge you for the queries you execute (time + size of data queried). For example, if you need to process some data every day, you likely don't need a production server running all day. This is especially true if you are running large queries on large datasets. Instead you could have your data dropped off in an S3 bucket in CSV format and you could use Athena to query those files the same way you'd query a regular SQL database. 

> Athena helps you analyze unstructured, semi-structured, and structured data stored in Amazon S3. Examples include CSV, JSON, or columnar data formats such as Apache Parquet and Apache ORC. You can use Athena to run ad-hoc queries using ANSI SQL, without the need to aggregate or load the data into Athena.
<p class="caption"><a href="https://docs.aws.amazon.com/athena/latest/ug/when-should-i-use-ate.html" title="Amazon Athena Overview">AWS Athena Docs</a></p>
  
I like using Athena as I can use SQL which I'm comfortable using while benefiting from having unlimited storage in S3. I don't need to worry about maintaining and paying for a database. The permissions are set within AWS and it is locked by default down to the AWS console if all the defaults are kept.

If storage (AWS S3) and data querying (AWS Athena) are both serverless, then why not use AWS lambda to run the job?

## Lambda 1: Query Athena and load the results into S3 (Python)

In the example below, the code instructs the Lambda to import `boto3` (the [AWS SDK for Python](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)) and use it to run a query against a database/table, then output the results of that query in CSV format and upload to a selected S3 bucket.

This example is taken from [this AWS knowledge center doc](https://aws.amazon.com/premiumsupport/knowledge-center/schedule-query-athena/)

```python
import time
import boto3

query = 'SELECT * FROM default.tb'
DATABASE = 'DATABASE-NAME-HERE'
output='s3://AWSDOC-EXAMPLE-BUCKET/'

def lambda_handler(event, context):
    client = boto3.client('athena')

    # Execution
    response = client.start_query_execution(
        QueryString=query,
        QueryExecutionContext={
            'Database': DATABASE
        },
        ResultConfiguration={
            'OutputLocation': output,
        }
    )
    return response
    return
```

In my case, I wanted to change the location of the S3 bucket to point to a directory within the bucket. So I added a `path` variable and appended the path to the end of the output location in the `lambda_handler`

```python
import time
import boto3

query = 'SELECT * FROM default.tb'
DATABASE = 'DATABASE-NAME-HERE'
output='s3://AWSDOC-EXAMPLE-BUCKET/'
path='parent-directory-name/child-directory-name/etc'

def lambda_handler(event, context):
    client = boto3.client('athena')

    # Execution
    response = client.start_query_execution(
        QueryString=query,
        QueryExecutionContext={
            'Database': DATABASE
        },
        ResultConfiguration={
            # first {} contains the output variable, then adds a '/' character for the directory and then the path variable
            'OutputLocation': "{}/{}".format(output, path),
        }
    )
    return response
    return
```

Now write the SQL you want to use. In this example, I'll query all of the email complaints from a Pinpoint application/project over the last 3 days.

```sql
SELECT
  complained_email_address
FROM complain_count
WHERE
  ingest_timestamp >= NOW() - interval '3' day
  AND application_id = '123456789'
GROUP BY complained_email_address
;
```

Now I can add that to the lambda code I wrote above and change the name of the database to my database name in Athena `pinpoint_events`. I'll also add the bucket name `pinpoint-event-data` as the AWS S3 bucket name with the directory path `application_1/complaints`.

```python
import time
import boto3

query = 'SELECT complained_email_address FROM complain_count WHERE ingest_timestamp >= NOW() - interval '3' day AND application_id = '123456789' GROUP BY complained_email_address;'
DATABASE = 'pinpoint_events'
output='s3://pinpoint-events-data/'
path='application_1/complaints'

def lambda_handler(event, context):
    client = boto3.client('athena')

    # Execution
    response = client.start_query_execution(
        QueryString=query,
        QueryExecutionContext={
            'Database': DATABASE
        },
        ResultConfiguration={
            # first {} contains the output variable, then adds a '/' character for the directory and then the path variable
            'OutputLocation': "{}/{}".format(output, path),
        }
    )
    return response
    return
```

You can setup a test in AWS Lambda function and just pass in a junk json payload to initiate the function.

```json
{
  "key1": "value1",
  "key2": "value2",
  "key3": "value3"
}
```

It doesn't matter what is here, none of it will be used in the `lambda_handler` function. We're just wanting to invoke the function and we need to pass some json in the request to do that.

After clicking "Test" you should a response like the following:

```json
Response
{
  "QueryExecutionId": "123456789",
  "ResponseMetadata": {
    "RequestId": "987654321",
    "HTTPStatusCode": 200,
    "HTTPHeaders": {
      "content-type": "application/x-amz-json-1.1",
      "date": "Wed, 18 Aug 2021 06:17:16 GMT",
      "x-amzn-requestid": "794613",
      "content-length": "59",
      "connection": "keep-alive"
    },
    "RetryAttempts": 0
  }
}
```

The `HTTPStatusCode` shows you that the Lambda ran successfully. You could skip ahead and check to see if your S3 bucket now contains a new .csv file with the data you wanted. However, you may have an error in your query. **This will not reflect in this response**. All this response is saying is, "yeah, looks good to me and I've passed it over to Athena to execute." In order to see the details of the query execution, you'll need to grab the `QueryExecutionId` and run some python in a python shell to view the results.

## Debugging your Athena Query within your Lambda Function

This is a little more painful than I wish it was. So definitely do all your query QA directly in the AWS Athena console first.

Initiate the Cloud Shell in AWS (this assumes your role has permissions setup for this). You can refer to my previous post on [how to install an iPython shell](/python-basics-for-aws-lambda-development/) on your local machine (and AWS Cloud Shell).

Import `boto3`

```bash
import boto3
```

Grab the `QueryExecutionId` which in this example is `123456789` and assign it to a varaible, then setup a client and grab the `QueryExecution` response.

```bash
execution_id = '123456789'
client = boto3.client('athena')
response = client.get_query_execution(QueryExecutionId=execution_id)
```

Here is an example of a response that had an **error** due to insufficient permissions.

```json
{'QueryExecution': {'QueryExecutionId': '123456789',
  'Query': "SELECT complained_email_address FROM complain_count WHERE ingest_timestamp >= NOW() - interval '3' day AND application_id = '123456789' GROUP BY complained_email_address;",
  'StatementType': 'DML',
  'ResultConfiguration': {'OutputLocation': 's3://pinpoint-events-data/application_1/complaints/baf0294e-d839-47b6-be58-ca14615e4794.csv'},
  'QueryExecutionContext': {'Database': 'pinpoint_events-data'},
  'Status': {'State': 'FAILED',
   'StateChangeReason': 'Insufficient permissions to execute the query.  User: arn:aws:sts::123:assumed-role/pinpoint_role/pinpoint_process_complaints_lambda_name is not authorized to perform: glue:GetPartitions on resource: arn:aws:glue:us-west-2:123:catalog ',
   'SubmissionDateTime': datetime.datetime(2021, 8, 16, 23, 29, 6, 869000, tzinfo=tzlocal()),
   'CompletionDateTime': datetime.datetime(2021, 8, 16, 23, 29, 17, 584000, tzinfo=tzlocal())},
  'Statistics': {'EngineExecutionTimeInMillis': 10508,
   'DataScannedInBytes': 0,
   'TotalExecutionTimeInMillis': 10715,
   'QueryQueueTimeInMillis': 171,
   'QueryPlanningTimeInMillis': 10151,
   'ServiceProcessingTimeInMillis': 36},
  'WorkGroup': 'primary',
  'EngineVersion': {'SelectedEngineVersion': 'AUTO',
   'EffectiveEngineVersion': 'Athena engine version 2'}},
 'ResponseMetadata': {'RequestId': 'foo',
  'HTTPStatusCode': 200,
  'HTTPHeaders': {'content-type': 'application/x-amz-json-1.1',
   'date': 'Mon, 16 Aug 2021 23:42:11 GMT',
   'x-amzn-requestid': 'foo',
   'content-length': '2482',
   'connection': 'keep-alive'},
  'RetryAttempts': 0}}
```

Here is an example of a reponse that was **successfull**

```json
{'QueryExecution': {'QueryExecutionId': 'baf0294e-d839-47b6-be58-ca14615e4794',
  'Query': "SELECT complained_email_address FROM complain_count WHERE ingest_timestamp >= NOW() - interval '3' day AND application_id = '123456789' GROUP BY complained_email_address;",
  'StatementType': 'DML',
  'ResultConfiguration': {'OutputLocation': 's3://pinpoint-events-data/application_1/complaints/baf0294e-d839-47b6-be58-ca14615e4794.csv'},
  'QueryExecutionContext': {'Database': 'pinpoint_events-data'},
  'Status': {'State': 'SUCCEEDED',
   'SubmissionDateTime': datetime.datetime(2021, 8, 16, 23, 42, 47, 594000, tzinfo=tzlocal()),
   'CompletionDateTime': datetime.datetime(2021, 8, 16, 23, 42, 50, 720000, tzinfo=tzlocal())},
  'Statistics': {'EngineExecutionTimeInMillis': 2937,
   'DataScannedInBytes': 6202,
   'TotalExecutionTimeInMillis': 3126,
   'QueryQueueTimeInMillis': 134,
   'QueryPlanningTimeInMillis': 1929,
   'ServiceProcessingTimeInMillis': 55},
  'WorkGroup': 'primary',
  'EngineVersion': {'SelectedEngineVersion': 'AUTO',
   'EffectiveEngineVersion': 'Athena engine version 2'}},
 'ResponseMetadata': {'RequestId': 'foo',
  'HTTPStatusCode': 200,
  'HTTPHeaders': {'content-type': 'application/x-amz-json-1.1',
   'date': 'Mon, 16 Aug 2021 23:43:03 GMT',
   'x-amzn-requestid': 'foo',
   'content-length': '1895',
   'connection': 'keep-alive'},
  'RetryAttempts': 0}}
```

You can see above the `ResultConfiguration` contains the location of the new file with the filename that was automatically generated `s3://pinpoint-events-data/application_1/complaints/baf0294e-d839-47b6-be58-ca14615e4794.csv`

You can now load up your S3 bucket and see the data there in CSV format ready for you. But now that we have data there, we'll need to process it. The way that AWS Lambda's work is by doing a simple process really well, so next up, we need to trigger a lambda to run when a file is added to this S3 location, parse the data and submit it to a new destination.

## Lambda 2: Load the new data from S3, parse it and send to an API endpoint for processing (Python)

Now we're going to grab the file contents for a specific file in S3 and process it.

```bash
import boto3

s3_client = boto3.resource('s3')
bucket = 'pinpoint-events-data'
key = 'application_1/complaints/baf0294e-d839-47b6-be58-ca14615e4794.csv'
s3_obj = s3_client.Object(bucket, key.replace('+', ' '))
data = s3_obj.get()['Body'].read()
```

So far we've connected to S3 and have loaded the contents of the file into the `data` variable. Now we need to loop through each row of data.

```bash
# helpful package with helpers for handling csv files
import csv

data_csv = csv.DictReader(data.decode('utf-8').split('\n'), delimiter=',')
rows = [l for l in data_csv]
```

Now calling the `rows` variable, you should see your csv data output something like this.

```bash
[OrderedDict([('complained_email_address', 'email1@email.com')]),
OrderedDict([('complained_email_address', 'email2@email.com')]),
OrderedDict([('complained_email_address', 'email3@email.com')]),
OrderedDict([('complained_email_address', 'email4@email.com')]),
OrderedDict([('complained_email_address', 'email5@email.com')])]
```

Essentially now you have an array you can use which in python is called a [dictionary](https://www.w3schools.com/python/python_dictionaries.asp). So you can call specific results, such as row 2, with the following.

```bash
rows[1]
```

The first dictionary item starts at `0` just like most other languages. If you had additional columns in your data, for example, `user_id`...

```bash
[OrderedDict([('complained_email_address', 'email1@email.com'),('user_id', 187)])
OrderedDict([('complained_email_address', 'email2@email.com'),('user_id', 223)]),
OrderedDict([('complained_email_address', 'email3@email.com'),('user_id', 346)]),
OrderedDict([('complained_email_address', 'email4@email.com'),('user_id', 448)]),
OrderedDict([('complained_email_address', 'email5@email.com'),('user_id', 599)])]
```

You would simply call

```bash
rows[1]['user_id']
```

This would output `223`

(to be continued!)
