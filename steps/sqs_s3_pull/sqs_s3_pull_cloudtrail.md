# Collecting CloudTrail Data from S3 buckets using Cribl LogStream
In this section we are going to setup CloudTrail to send data into an S3 bucket and configure the S3 bucket to send an event to SQS whenever CloudTrail puts data into the S3 bucket. This will trigger Cribl LogStream workers to pull the events and send them into your logging solution of choice. Let's get started.

## Setup CloudTrail to Send to an S3 Bucket



### Cribl LogStream Setup

