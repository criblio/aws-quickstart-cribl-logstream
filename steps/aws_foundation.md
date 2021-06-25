# AWS Foundation
This page will guide you to build out your AWS environment using some best practices so that you can take advantage of the Cribl LogStream Marketplace offering.  

## Setup your VPC 
Before you can deploy any EC2 instances, you will need to deploy a VPC or [Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) in your environment.

## VPC CloudFormation Template

![Architecture](/architecture/vpc_create_sqs_s3_cribl_design.png)

### Step 1 - Create VPC 
Click on the following CloudFormation template to create a VPC within your AWS Regional Deployment: 
- [US East 1](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com/cftemplates/%2Fvpc-flow-create.yaml&stackName=cribl-vpc&param_ClassB=0)
- [US East 2](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com/cftemplates/%2Fvpc-flow-create.yaml&stackName=cribl-vpc&param_ClassB=0)
- [US West 1](https://console.aws.amazon.com/cloudformation/home?region=us-west-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com/cftemplates/%2Fvpc-flow-create.yaml&stackName=cribl-vpc&param_ClassB=0)
- [US West 2](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com/cftemplates/%2Fvpc-flow-create.yaml&stackName=cribl-vpc&param_ClassB=0)


### Step 2 - Setup VPC Flow Logs to Send events into an S3 bucket
Once your VPC has been created, can click here and it will send the VPC Flow logs into an S3 bucket that you specify : 
- [US East 1](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com%2Fcftemplates%2Fvpc-flow-s3%252Bsqs.yaml&stackName=cribl-vpc-sqs-s3&param_ExternalLogBucket=&param_LogFilePrefix=&param_ParentVPCStack=cribl-vpc&param_SQS=cribl-sqs&param_TrafficType=ALL)
- [US East 2](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com%2Fcftemplates%2Fvpc-flow-s3%252Bsqs.yaml&stackName=cribl-vpc-sqs-s3&param_ExternalLogBucket=&param_LogFilePrefix=&param_ParentVPCStack=cribl-vpc&param_SQS=cribl-sqs&param_TrafficType=ALL) 
- [US West 1](https://console.aws.amazon.com/cloudformation/home?region=us-west-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com%2Fcftemplates%2Fvpc-flow-s3%252Bsqs.yaml&stackName=cribl-vpc-sqs-s3&param_ExternalLogBucket=&param_LogFilePrefix=&param_ParentVPCStack=cribl-vpc&param_SQS=cribl-sqs&param_TrafficType=ALL) 
- [US West 2](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com%2Fcftemplates%2Fvpc-flow-s3%252Bsqs.yaml&stackName=cribl-vpc-sqs-s3&param_ExternalLogBucket=&param_LogFilePrefix=&param_ParentVPCStack=cribl-vpc&param_SQS=cribl-sqs&param_TrafficType=ALL)

> This CloudFormation Template will continue where the first CloudFormation template used to create the initial VPC left off and will enable VPC Flow Logs and have them write to an S3 bucket. The S3 bucket will have events enabled to trigger an SQS which will be used by Cribl LogStream to pull VPC Flow data.

Here is what the completed CloudFormation screen should look like:
![Complete VPC Flow](/screenshots/s3bucket/vpcflow/vpc-data-collection.png)

-------------

## Collect CloudTrail Data from S3
![Architecture](/architecture/cloudtrail_cf_template_design.png)

This quick link will enable a new CloudTrail (cribl-cloudtrail-sqs-s3-Trail-`<uniqueID>`), create an S3 bucket where the CloudTrail events will land. An SQS will also be created with the appropriate policy to allow events from S3 to be sent into this SQS: 
- [US East 1](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com%2Fcftemplates%2Fcloudtrail-s3%252Bsqs.yaml&stackName=cribl-cloudtrail-sqs-s3&param_CloudWatchLogsRetentionInDays=7&param_ExternalTrailBucket=&param_LogFilePrefix=&param_ParentAlertStack=&param_PermissionsBoundary=&param_S3DataEvents=true&param_SQS=cribl-sqs-cloudtrail)
- [US East 2](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com%2Fcftemplates%2Fcloudtrail-s3%252Bsqs.yaml&stackName=cribl-cloudtrail-sqs-s3&param_CloudWatchLogsRetentionInDays=7&param_ExternalTrailBucket=&param_LogFilePrefix=&param_ParentAlertStack=&param_PermissionsBoundary=&param_S3DataEvents=true&param_SQS=cribl-sqs-cloudtrail) 
- [US West 1](https://console.aws.amazon.com/cloudformation/home?region=us-west-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com%2Fcftemplates%2Fcloudtrail-s3%252Bsqs.yaml&stackName=cribl-cloudtrail-sqs-s3&param_CloudWatchLogsRetentionInDays=7&param_ExternalTrailBucket=&param_LogFilePrefix=&param_ParentAlertStack=&param_PermissionsBoundary=&param_S3DataEvents=true&param_SQS=cribl-sqs-cloudtrail)
- [US West 2](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com%2Fcftemplates%2Fcloudtrail-s3%252Bsqs.yaml&stackName=cribl-cloudtrail-sqs-s3&param_CloudWatchLogsRetentionInDays=7&param_ExternalTrailBucket=&param_LogFilePrefix=&param_ParentAlertStack=&param_PermissionsBoundary=&param_S3DataEvents=true&param_SQS=cribl-sqs-cloudtrail)

Here is what the completed CloudFormation screen should look like:
![Complete VPC Flow](/screenshots/s3bucket/vpcflow/cloudtrail-data-collection.png)

------------

## EC2 Setup
### Create an EC2 Key pair 

- Log into the EC2 Service Page
- Go to "Key Pairs" from the menu on the left side
- Click "Create key pair
- Give your key pair a name
- The pem will be downloaded, move it to your ssh folder and secure it (e.g. linux/mac `chmod 400 mykeypair.pem`)
-------------
## Continue to Cribl LogStream Use Cases :

### Setup VPC Flow Logs

[Setup VPC Flow Logs and S3 collction](vpcflowlogs2metrics.md)

### Setup CloudTrail Logs

[Setup CloudTrail Logs and S3 collection](cloudtrail.md)
