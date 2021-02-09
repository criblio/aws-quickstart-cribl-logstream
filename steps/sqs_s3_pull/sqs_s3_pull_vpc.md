# Pull Events using SQS and S3
After deploying our Cribl Single Instance, we will create an S3 source and start collecting data from the S3 bucket containing our VPCFlow Logs. 

## Finish AWS Setup
Once your VPC has been created, can click here and it will send the VPC Flow logs into an S3 bucket that you specify : https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com%2Fcftemplates%2Fvpc-flow-s3%252Bsqs.yaml&stackName=cribl-vpc-s3-sqs&param_ExternalLogBucket=&param_LogFilePrefix=&param_ParentVPCStack=cribl-vpc&param_SQS=cribl-sqs&param_TrafficType=ALL 

### Cribl LogStream Setup
Log into your Cribl LogStream instance:

![Step-1 Log into LogStream](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/lambda/cls-lambda-07.png)

- Go to _Sources_: 

