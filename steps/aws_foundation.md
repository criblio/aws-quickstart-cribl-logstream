# AWS Foundation
This page will guide you to build out your AWS environment using some best practices so that you can take advantage of the Cribl LogStream Marketplace offering.  

## Setup your VPC 
Before you can deploy any EC2 instances, you will need to deploy a VPC or [Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) in your environment. This process will 

### VPC CloudFormation Template

- Click on this VPC Creation Cloudformation Template : https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com/cftemplates/%2Fvpc-flow-create.yaml&stackName=cribl-vpc&param_ClassB=0

### Create an EC2 Keypair 

- Follow these steps to create an EC2 Keypair to SSH into your ec2 hosts:

### Setup Cribl LogStream Distributed

- [Follow these steps to setup Cribl LogStream via the AWS Marketplace](steps/mp/cls-mp.md)