# CloudTrail Data Collection


## Overview of Use Case
This use case is to get CloudTrail logs into your system of record and then convert the log events to metric data. 

### AWS Setup Overview
We are going to create the following components in your AWS Environment:

#### Pull Deployment
- CloudTrail enabled and sending to S3 Bucket
    - IAM Roles that can write from CloudTrail to S3 Bucket
- SQS for S3 triggers
    - Create and enable SQS event when objects are created in S3 bucket

## Step 1 - Enable CloudTrail Audit Logs 
Click on the following CloudFormation template to create a VPC within your AWS Deployment: 

## Step 2 - Setup Cribl LogStream to Collect from S3 (Pull)

![Pull from S3](https://quickstart-cribl-logstream.s3.amazonaws.com/architecture/Cribl_LS_S3_SQS_Collection.png) 

- [Click Here to setup SQS and S3](sqs_s3_pull/sqs_s3_pull_cloudtrail.md) 