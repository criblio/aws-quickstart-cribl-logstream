# Cribl LogStream on Amazon
> This Quick Start guide was created by [Cribl](https://cribl.io) to help automate the deployment of Cribl LogStream in your AWS environment. These  automated reference deployments use AWS CloudFormation templates to deploy EC2 instances, IAM policies and S3 buckets, following AWS best practices. 

## Overview 
Cribl has put together a simple method to launch a CloudFormation template that will automatically create the IAM EC2 role and the proper IAM policy to use with your deployment. We will be breaking down the permissions used, AWS resources deployed and their associated costs. This process will take around 5 minutes and you will have a running Cribl LogStream deployment. 

## Architecture 
Deploying this Quick Start with default parameters will build a single EC2 instance Master Instance with however many worker nodes you specify in the CloudFormation Template. The instances will be launched within the VPC and (AZ's) you specify. This deployment will also create an S3 bucket that can be used as a source or destination. The EC2 Role and the associated policy will also be created as a part of this CloudFormation template and assigned to the Worker Nodes. The Master Node does not have an EC2 policy associated with it. 

![Cribl Distributed Deplyoment](https://quickstart-cribl-logstream.s3.amazonaws.com/architecture/Cribl_LS_MP_Distributed.png)

_Figure 1: Cribl LogStream Distributed architecture deployment in AWS_

As shown in Figure 1, the Quick Start CloudFormation template sets up the following:

* In AWS: 
   * EC2 Instances
   * Security Groups
   * S3 Bucket
   * IAM Role and Policy
> IAM Policy Best practices 
>
> Per the [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) the policy created by the CloudFormation template creates an EC2 role with the following IAM Policy:

```yaml 
iamDefaultWorkerRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: !Sub '${AWS::StackName}-logstream-worker-role'
      Description: Cribl LogStream default IAM role
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              Service:
                - ec2.amazonaws.com
            Action:
              - sts:AssumeRole
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
        - arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy
      Policies:
        - PolicyName: S3Destinations
          PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Effect: Allow
                Action:
                  - s3:PutObject
                  - s3:GetObject
                  - s3:ListBucket
                  - s3:GetBucketLocation
                Resource:
                  - !Sub '${s3DefaultDestinationBucket.Arn}'
                  - !Sub '${s3DefaultDestinationBucket.Arn}/*'
        - PolicyName: S3Sources
          PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Effect: Allow
                Action:
                  - s3:GetObject
                  - s3:GetBucketLocation
                Resource: '*'
        - PolicyName: KinesisSources
          PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Effect: Allow
                Action:
                  - kinesis:Get*
                  - kinesis:List*
                  - kinesis:Describe*
                Resource: '*'
      Tags:
        - Key: Name
          Value: Cribl LogStream default IAM role
  iamDefaultWorkerInstanceProfile:
    Type: AWS::IAM::InstanceProfile
    Properties:
      Path: /
      Roles:
        - !Ref 'iamDefaultWorkerRole'
```
Here is the link to the entire CloudFormation Template: [CloudFormation Template](https://cribl-marketplace-public-artifacts-us-east-1.s3.amazonaws.com/logstream/2.3.4/distributed/template/distributed-template.yaml)

>This IAM Role ties the actions of listing, reading and writing to the specific S3 bucket created during this process. Then this policy is attached to the EC2 Worker instance rather than using Access / Secret keys for authentication. 

## Cost and licenses

You are responsible for the cost of the AWS services used while running this Quick Start reference deployment. There are no additional costs for using the Quick Start. The Cribl LogStream license included in your deployment will default to the LogStream Free 1TB license. For more information about licensing, please refer to the [Cribl LogStream Pricing Page](https://cribl.io/cribl-logstream-pricing/).

The AWS CloudFormation template for this Quick Start includes configuration parameters that you can customize. Some of these settings may affect the cost of your deployment. For cost estimates, see our estimates under "Pricing Information" in the AWS Marketplace listing. 

> Estimated Infrastructure Cost assuming US East (N.Virginia) and a C5.2xlarge instance is $256.88/month. Prices may different depending on instance type, region and other factors. Please check with your AWS billing reports or use the [AWS Calculator](https://calculator.aws/#/) to more accuratly predict your costs. 

## Planning the deployment

### Specialized knowledge
This Quick Start assumes familiarity with Amazon EC2, S3, IAM, Kinesis and CloudFormation. 

### AWS Account
If you don't already have an AWS Account, create one at https://aws.amazon.com and follow the instructions.

## Deployment options
This Quick Start provides two options:
- [Cribl LogStream Single Instance](https://aws.amazon.com/marketplace/pp/B08BRGFJK1?qid=1604503537874&sr=0-1&ref_=srh_res_product_title) 
   - This will deploy a single EC2 instance and configure the AWS services and resources listed above. We will be using this deployment for our quick start guide. This is great for development or testing purposes. 
- [Cribl LogStream Distributed](https://aws.amazon.com/marketplace/pp/B08CRVQWCJ?qid=1604503537874&sr=0-2&ref_=srh_res_product_title) 
   - This will be the deployment used in this Quick Start. It will deploy one master node and however many worker nodes you want to use. It's recommended you deploy at least 2 worker nodes.  

## Deployment Steps
1. Sign into your AWS Account.

2. Navigate to [Cribl LogStream Distributed](https://aws.amazon.com/marketplace/pp/B08CRVQWCJ?qid=1604503537874&sr=0-2&ref_=srh_res_product_title) and then click on "Continue to Subscribe." 

3. Subscribe to the software and accept the Terms and Conditions. Then click on the "Continue to Configuration." 

4. Configure your deployment, selecting your Deliver Method, Software Version and Region. Click "Continue to Launch."

5. Launch this software using "Launch CloudFormation" in the drop down, then click "Launch". 

6. Log into Cribl LogStream with the credential supplied in the "Outputs" tab on your CloudFormation stack.
   
### CloudFormation Steps

- Step 1 - Specify Template 
      - This will take you to your AWS Account and drop you in a CloudFormation Stack deploy screen. Click "Next" to start the stack creation.
- Step 2 - Specify stack details 
> Tips for deploying in your AWS Environment. If this is a new AWS account, you will need to create the following components: 
> - An SSH Key pair to log into your EC2 instance. Please follow [this guide for help on creating your key pair.](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)
> - A VPC with publicly accessible IP's. Here is a [CloudFormation template that will create the VPC for you.](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fquick-start-cribl.s3.amazonaws.com%2Fvpc-flow-create.yaml&stackName=cribl-vpc&param_ClassB=0) 
> - A Subnet ID. This will be created by using the CloudFormation template above.
> Security Group CIDR Tips. 
> - Web Access CIDR should be open to either a range of IP's that you want to grant access. You can also open it to the world if you do not have any restrictions for public access to the webport. An example CIDR would be 0.0.0.0/0.
> - SSH Access CIDR should be closed to your own IP address or a small range of IP's that you want to allow SSH access to your system. An example CIDR for your own IP would be <your_ip_here>/32 where [your IP Address](https://whatismyipaddress.com/) would be placed in the <your_ip_here>. 
> - Advanced Settings amild is only used if you are issued this by Cribl Support.
   
   - Step 3 Configure stack options
      - This step has some optional sections. All the defaults can be left as-is and you can click "Next." 
      - You can modify the options in this section if you know what you're doing or have a company policy around tagging AWS resources. 

   - Step 4 Review
      - Review all the steps, acknowledge the access capabilities at the bottom of this page and click "Create Stack." 
Once the CloudFormation stack has been created, click on the Outputs tab of your Stack and you will see the LogStream WebAccess Credentials, the URL and the stackname. Your WebURL 

## Log into Cribl LogStream

Navigate to http://<cribl_logstream_ip>:9000 and log in using admin / <ec2_instance_id> . Once you have logged into your instance take a look around and we can start diving into the use cases.

### Use Cases

- [Send Data to S3 bucket](steps/s3bucket.md)
- [VPC Flow Logs to Metrics](steps/vpcflowlogs2metrics.md)
- [CloudTrail Collection](/steps/cloudtrail.md)

## Feedback
Please submit feedback, feature ideas or report bugs using the [Issues](https://github.com/amiracle/quick-start-cribl/issues) section of this GitHub repository. If you would like to submit code, please review the Quick Start Contributor's Guide.

## Additional resources

### Cribl Resources
- [Cribl Community](https://cribl.io/community) 
- [Cribl Resources](https://cribl.io/resources)
- [Cribl Docs on Single Instance Deployments](https://docs.cribl.io/docs/deploy-single-instance)
- [Cribl Docs on Distributed Deployments](https://docs.cribl.io/docs/deploy-distributed)
- [Cribl Docs on sizing and scaling instances](https://docs.cribl.io/docs/scaling)

### AWS resources

* [Getting Started Resource Center](https://aws.amazon.com/getting-started/)
* [AWS General Reference](https://docs.aws.amazon.com/general/latest/gr/)
* [AWS Glossary](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html)

### AWS services

* [AWS CloudFormation](https://docs.aws.amazon.com/cloudformation/)
* [Amazon EC2](https://aws.amazon.com/ec2/)
* [IAM](https://docs.aws.amazon.com/iam/)
* [Amazon VPC](https://docs.aws.amazon.com/vpc/)
* [Amazon Kinesis](https://docs.aws.amazon.com/kinesis/)
