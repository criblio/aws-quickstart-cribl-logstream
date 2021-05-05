# Collecting CloudWatch Streaming Metrics from S3 buckets using Cribl LogStream
In this section we are going to setup Cribl LogStream to collect the CloudTrail data from the S3 bucket we just created by listening to the SQS notifications.

### Cribl LogStream Setup

These steps will setup Cribl LogStream to collect the CloudWatch Streaming Metrics from the S3 bucket created by the CloudFormation template triggered above. 

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-01.png)

- Setup Cribl S3 Source
    - Click on `Default Group`
        - Click on `Data` -> `Sources`
        - Click on `Amazon S3`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-01.png)

- Click `+Add New` S3 Sources
- Input ID : `cloudwatch-metrics`
- Queue: `cloudwatch-metrics`
- API key: Leave Blank since we are using EC2 Roles
- Secret key: Leave Blank since we are using EC2 Roles
- Region: select your region, e.g. US East (N.Virginia)
- Click `Save`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-02.png)

- Click on `Commit default`
- Enter update comments (e.g. `Added S3 CloudWatch Streaming Metrics`)

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-03.png)

- Click `Commit`
- Click `Deploy default`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-04.png)

- Click `Yes`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-05.png)

- Click `Live` under Status for the `cloudwatch-metrics` source

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-06.png)

- Note the status of your worker nodes
- Click on `Live Data`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cw-metrics/sqs-s3-cwm-07.png)

- Click `Capture`

- Scroll Down

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-07.png)

Data can be collected for a sample file:
- Description: `CloudWatchMetrics`
- Tags: cloudwatch, aws
- Scroll down and click `Save Sample File`
- Click on `Pipelines`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-08.png)

- Click on `Add Pipeline`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-09.png)

- Name the pipeline `cloudtrail`
- Description: `Collect CloudTrail Data from AWS`
- Click `Save`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-10.png)

- Add function : `parser`
- Description: `Break out CloudTrail Data`
- Filter: `True`
- Operation Mode* : `Extract`
- Type*: `JSON Object`
- Source Field: `_raw`
- Destination Field: `_raw`
- Fields to Remove: `previousDigestSignature` `previousDigestHashValue` `*.responseElements.credentials.sessionToken` 

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-14.png)

- Add function: `Drop`
- Filter: `/(^Describe|^List|^Get)/.test(eventName)`

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-15.png)


- Click `Save` 
- Click on `Basic Statistics` to see the estimated reduction in volume:

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-16.png)

- Click on `Commit default` and `Deploy default` to deploy your changes. 

- Click on `Routes` to start sending your data into your log aggregation tool.

![Cribl Sources](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/cloudtrail/sqs-s3-cls-ct-17.png)