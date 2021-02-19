# CloudTrail Data Collection


## Overview of Use Case
This use case is to get CloudTrail logs into your system of record. We will go over some tips to help reduce the volume of these events by removing redundant or `null` values. 

### AWS Setup Overview
We are going to create the following components in your AWS Environment:

<img src="https://quickstart-cribl-logstream.s3.amazonaws.com/architecture/design/cloudtrail_cf_template_design.png" width="400" height="400">

#### Pull Deployment
- CloudTrail enabled and sending to S3 Bucket
    - IAM Roles that can write from CloudTrail to S3 Bucket
- SQS for S3 triggers
    - Create and enable SQS event when objects are created in S3 bucket

## Step 1 - Enable CloudTrail Audit Logs 

This quick link will enable a new CloudTrail (cribl-cloudtrail-sqs-s3-Trail-<uniqueID>), create an S3 bucket where the CloudTrail events will land. An SQS will also be created with the appropriate policy to allow events from S3 to be sent into this SQS: https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com%2Fcftemplates%2Fcloudtrail-s3%252Bsqs.yaml&stackName=cribl-cloudtrail-sqs-s3&param_CloudWatchLogsRetentionInDays=7&param_ExternalTrailBucket=&param_LogFilePrefix=&param_ParentAlertStack=&param_PermissionsBoundary=&param_S3DataEvents=true&param_SQS=cribl-sqs-cloudtrail 

## Step 2 - Setup Cribl LogStream to Collect from S3 (Pull)

![Pull from S3](https://quickstart-cribl-logstream.s3.amazonaws.com/architecture/Cribl_LS_S3_SQS_Collection.png) 

- [Click Here to setup SQS and S3](sqs_s3_pull/sqs_s3_pull_cloudtrail.md) 