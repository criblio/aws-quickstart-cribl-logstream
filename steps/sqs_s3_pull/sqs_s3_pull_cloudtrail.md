# Collecting CloudTrail Data from S3 buckets using Cribl LogStream
In this section we are going to setup CloudTrail to send data into an S3 bucket and configure the S3 bucket to send an event to SQS whenever CloudTrail puts data into the S3 bucket. This will trigger Cribl LogStream workers to pull the events and send them into your logging solution of choice. Let's get started.

## Setup CloudTrail to Send to an S3 Bucket
This quick link will enable a new CloudTrail (cribl-cloudtrail-sqs-s3-Trail-<uniqueID>), create an S3 bucket where the CloudTrail events will land. An SQS will also be created with the appropriate policy to allow events from S3 to be sent into this SQS: https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquickstart-cribl-logstream.s3.amazonaws.com%2Fcftemplates%2Fcloudtrail-s3%252Bsqs.yaml&stackName=cribl-cloudtrail-sqs-s3&param_CloudWatchLogsRetentionInDays=7&param_ExternalTrailBucket=&param_LogFilePrefix=&param_ParentAlertStack=&param_PermissionsBoundary=&param_S3DataEvents=true&param_SQS=cribl-sqs-cloudtrail 

### Cribl LogStream Setup

These steps will setup Cribl LogStream to collect the CloudTrail logs from the S3 bucket created by the CloudFormation template triggered above. 

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-01.png)

- Setup Cribl S3 Source
    - Click on `Default Group`
        - Click on `Data` -> `Sources`
        - Click on `Amazon S3`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-02.png)

- Click `+Add New` S3 Sources
- Input ID : `cloudtrail`
- Queue: `cribl-sqs-cloudtrail`
- API key: Leave Blank since we are using EC2 Roles
- Secret key: Leave Blank since we are using EC2 Roles
- Region: select your region, e.g. US East (N.Virginia)
- Click `Save`
- Click on `Commit default`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-03.png)

- Enter update comments (e.g. `Added S3 CloudTrail Logs Collection`)
- Click `Commit`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-04.png)

- Click `Deploy default`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-05.png)

- Click `Yes`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-06.png)

- Click `Live`
- Navigate to `Live Data`
- Click `Capture`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-07.png)

Data can be collected for a sample file:
- Description: `CloudTrail`
- Tags: cloudtrail, aws
- Scroll down and click `Save Sample File`
- Click on `Pipelines`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-08.png)

- Click on `Add Pipeline`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-09.png)

- Name the pipeline `cloudtrail`
- Description: `Collect CloudTrail Data from AWS`
- Click `Save`


![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-10.png)

- Click on `Routes` to start sending your data into your log aggregation tool.