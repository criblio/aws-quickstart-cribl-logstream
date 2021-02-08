# VPC Flow Logs-to-metrics


## Overview of Use Case
This use case is to get VPC Flow logs into your system of record and then convert the log events to metric data. This is based on the [excellent blog post](https://cribl.io/blog/practical-logs-to-metrics-conversion-with-cribl/) by one of the Cribl Co-Founder Dritran Bitincka. There are two ways to collect VPCFlow Logs, depending on cost and latency concerns, you might want to go one way or the other. First, if cost is an issue and you don't mind receiving the data after a few minutes, collecting the logs via S3 is the best approach. However, if you need the logs more quickly, then pushing the data from CloudWatch Logs to an HTTP endpoint would be the recommended approach. There is a higher cost for going down this route which is explained [here](https://stackoverflow.com/questions/55472308/cloudwatch-log-store-costing-vs-s3-costing). 

This guide will walk you through setting up a VPC and then setup logging VPC Flow logs to a CloudWatch Log Group or an S3 bucket. Once the data is in, then we will configure Cribl LogStream to collect the data either via Push or Pull methods. 

### AWS Setup Overview
We are going to create the following components in your AWS Environment:

#### Push Deployment 
- VPC with VPC Flow Logs enabled and sending to CloudWatch Logs Group
    - IAM Roles that can write from VPC to CloudWatch Logs
- Lambda function to send data to Cribl HTTP endpoint
    - Create and enable trigger from CloudWatch logs trigger

`OR`

#### Pull Deployment
- VPC with VPC Flow Logs enabled and sending to S3 Bucket
    - IAM Roles that can write from VPC to S3 Bucket
- SQS for S3 triggers
    - Create and enable SQS event when objects are created in S3 bucket

## Step 1 - Create VPC 
Click on the following CloudFormation template to create a VPC within your AWS Deployment: https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com/cftemplates/%2Fvpc-flow-create.yaml&stackName=cribl-vpc&param_ClassB=0.

> ## Which path is right for me?
> If you are new to AWS and just want to collect the VPC Flow Logs without much trouble, use the [Pull Method](/sqs_s3_pull/sqs_s3_pull_vpc.md). It is easier to setup and does not require any additional resources (e.g. ELB, lambda functions, etc.). 
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
| [Click Here for Pull Using SQS and S3](sqs_s3_pull/sqs_s3_pull_vpc.md) |  [Click Here for Push Using Lambda](lambda_push/lambda_push_vpc.md)| 

### Optional :  
` If you decide later to change how you collect your VPC Flow Logs, you can come back here and select the other path.`