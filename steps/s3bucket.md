# Send Data to S3 bucket

## Overview of Use Case
This use case will show you how to setup Cribl LogStream to send data from any source to an S3 bucket. The CloudFormation template from the AWS Marketplace listing automatically creates an S3 bucket with an IAM policy that allows reading from and writing to the bucket. The AWS side of this deployment has already been configured, we only need to setup a route and pipeline in Cribl LogStream to send the data to the bucket.

![Cribl Architecture](https://quickstart-cribl-logstream.s3.amazonaws.com/architecture/Cribl_Data_to_S3.png)

## Enabling up Cribl LogStream Data

### Validate S3 Destination 

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-01.png)

- Click on 
    - Click on `Default Group`
        - Click on `Data` -> `Destinations`
        - Click on `Amazon S3`
- See pre-configured destination `S3 bucket`

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-02.png)

### Enable Cribl Internal Sources

- Click on `Data`
    - Click `Sources`

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-03.png)

- Click on `Cribl Internal`

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-04.png)

- Enable `CriblLogs`

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-05.png)

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-06.png)

- Click `Commit default`

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-07.png)

- Click `Commit`

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-08.png)

- Click `Deploy default`

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-10.png)

- Click `Yes`

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-11.png)

- Click on the `Copy to clipboard` function (save it for the next steps)

### Create Route

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-12.png)

- Click `Add Route`


![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-13.png)

- Route Name: `Cribl Logs to S3`
- Filter: paste from clipboard `__inputId=='cribl:CriblLogs`
- Pipeline: `passthru`
- Output: `s3:s3:default`
- Description: `Send Cribl Logs to S3 bucket`

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-14.png)

- Move this route above the `default` route. Just click and drag into position.

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-15.png)

- Click `Save`
- Click `Commit default`

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-16.png)

- Click `Commit`

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-17.png)

- Click `Deploy default`


![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-18.png)

- Click `Yes`

![Cribl Sources Screen](https://quickstart-cribl-logstream.s3.amazonaws.com/screenshots/s3bucket/s3dest/s3-dest-19.png)

- Click the S3 bucket and see the events flowing.