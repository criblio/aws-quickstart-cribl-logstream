# CloudWatch Metric Data Collection


## Overview of Use Case
This use case will cover collecting metrics from AWS CloudWatch using their streaming feature via S3 buckets.


### AWS Setup Overview
We are going to create the following components in your AWS Environment: 

<img src="https://quickstart-cribl-logstream.s3.amazonaws.com/architecture/CloudWatch_Metrics_S3.png">

#### Pull Deployment
- CloudWatch Metric Streams enabled and sending to S3 Bucket
    - IAM Roles that can write from CloudTrail to S3 Bucket
- SQS for S3 triggers
    - Create and enable SQS event when objects are created in S3 bucket

## Step 1 - Enable CloudWatch Metric Streams 
Follow the steps in the AWS Post to enable your CloudWatch Streaming Metrics. https://aws.amazon.com/blogs/aws/cloudwatch-metric-streams-send-aws-metrics-to-partners-and-to-your-apps-in-real-time/


## Step 2 - Setup Cribl LogStream to Collect from S3 (Pull)

![Pull from S3](https://quickstart-cribl-logstream.s3.amazonaws.com/architecture/Cribl_LS_S3_SQS_Collection.png) 

- [Click Here to setup SQS and S3](sqs_s3_pull/sqs_s3_pull_cloudwatch_metrics.md) 