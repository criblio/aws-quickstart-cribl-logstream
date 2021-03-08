# AWS Foundation
This page will guide you to build out your AWS environment using some best practices so that you can take advantage of the Cribl LogStream Marketplace offering.  

## Setup your VPC 
Before you can deploy any EC2 instances, you will need to deploy a VPC or [Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) in your environment. This process will 

### VPC CloudFormation Template

- Click on this VPC Creation Cloudformation Template : https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com/cftemplates/%2Fvpc-flow-create.yaml&stackName=cribl-vpc&param_ClassB=0

### Create an EC2 Key pair 

- Log into the EC2 Service Page
- Go to "Key Pairs" from the menu on the left side
- Click "Create key pair
- Give your key pair a name
- The pem will be downloaded, move it to your ssh folder and secure it (e.g. linux/mac `chmod 400 mykeypair.pem`)

### Setup VPC Flow Logs

[Setup VPC Flow Logs and S3 collction](steps/vpcflowlogs2metrics.md)

### Setup CloudTrail Logs

[Setup CloudTrail Logs and S3 collection](steps/cloudtrail.md)

### Setup Cribl LogStream Distributed

[Follow these steps to setup Cribl LogStream via the AWS Marketplace](https://github.com/criblio/aws-quickstart-cribl-logstream#deployment-steps)