# CloudTrail Data Collection


## Overview of Use Case
This use case is to get CloudTrail logs into your system of record and then convert the log events to metric data. 

### AWS Setup Overview
We are going to create the following components in your AWS Environment:

#### Push Deployment 
- CloudTrail enabled and sending to CloudWatch Logs Group
    - IAM Roles that can write from CloudTrail to CloudWatch Logs
- Lambda function to send data to Cribl HTTP endpoint
    - Create and enable trigger from CloudWatch logs trigger

`OR`

#### Pull Deployment
- CloudTrail enabled and sending to S3 Bucket
    - IAM Roles that can write from CloudTrail to S3 Bucket
- SQS for S3 triggers
    - Create and enable SQS event when objects are created in S3 bucket

## Step 1 - Enable CloudTrail Audit Logs 
Click on the following CloudFormation template to create a VPC within your AWS Deployment: 

> ## Which path is right for me?
> If you are new to AWS and just want to collect the VPC Flow Logs without much trouble, use the [Pull Method](/sqs_s3_pull/sqs_s3_pull_cloudtrail.md). It is easier to setup and does not require any additional resources (e.g. ELB, lambda functions, etc.). 
>

>
> ### Pull vs. Push 
> Here are some quick pros / cons for either Pulling or Pushing your data: 
> | Pull | Push |
> | ---- | ---- |
> | Pros: | Pros: |
> | - Low Cost | - Low Latency |
> | - Higher fault tollerance | - Push to HTTP Endpoint |
> | - Easier Setup | - Faster scale out for large volumes |
> | Cons: | Cons: |
> | - High Latency | - High Cost |
> | - Requires configuring SQS and S3 buckets | - ELB Requirement | 
> | - Autoscale group required for scaling collection tier | - Lambda function requirement |
>

## Step 2 - Setup Cribl LogStream to Collect from HTTP (Push) or S3 (Pull)

| Pull Logs from S3 | Push Logs to HTTP Endpoint  |
| -------------------------- | ----------------- |
| ![Pull from S3](https://quickstart-cribl-logstream.s3.amazonaws.com/architecture/Cribl_LS_S3_SQS_Collection.png) |  ![Push to HTTP](https://quickstart-cribl-logstream.s3.amazonaws.com/architecture/HTTP_to_Cirbl_Log.png) |
| |
| [Click Here for Pull Using SQS and S3](/sqs_s3_pull/sqs_s3_pull_cloudtrail.md) |  [Click Here for Push Using Lambda](/lambda_push/lambda_push_cloudtrail.md)| 

### Optional :  
` If you decide later to change how you collect your VPC Flow Logs, you can come back here and select the other path.`