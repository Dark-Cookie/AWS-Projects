# AWS Projects Showcase

This repository contains a collection of AWS-based projects that demonstrate various AWS services and solutions I’ve learned and built over time.

## Levels
Projects are labeled based these four levels:

- [Level 100 (Introductory)](https://github.com/Dark-Cookie/AWS-Projects/blob/main/README.md#level-100-introductory)
- [Level 200 (Intermediate)](https://github.com/Dark-Cookie/AWS-Projects/blob/main/README.md#level-200-intermediate)
- [Level 300 (Advanced)](https://github.com/Dark-Cookie/AWS-Projects/blob/main/README.md#level-300-advanced)
- [Level 400 (Expert)](https://github.com/Dark-Cookie/AWS-Projects/blob/main/README.md#level-400-expert)

## Level 100 (Introductory)

**This folder contains projects that are suitable for beginners. They cover fundamental AWS concepts and basic services.**

- **Project 1**: Create Three Billing Alarms
  - **Description**: This project involves setting up three AWS billing alarms to help monitor and control my AWS spending.
  - **Service Used**: CloudWatch
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20100/1.%20Create%20Three%20Billing%20Alarms)

- **Project 2**: Create a Cost Budget
  - **Description**: This project involves setting up a cost budget in AWS to help monitor and control spending.
  - **Service Used**: AWS Budgets
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20100/2.%20Create%20a%20Cost%20Budget)

- **Project 3**: Launch a Hello World Website on the Internet
  - **Description**: This project involves creating a basic web server on an AWS EC2 instance that hosts a simple "Hello World" website. The objective is to familiarize myself with setting up an EC2 instance, configuring it, and deploying a basic web application to make it accessible over the Internet.
  - **Service Used**: Amazon EC2
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20100/3.%20Launch%20a%20Hello%20World%20Website%20on%20the%20Internet)

- **Project 4**:  Push a Docker Image to Amazon ECR Repository
  - **Description**: This project involves building a Docker image and pushing it to an Amazon Elastic Container Registry (ECR) repository. This process includes creating an ECR repository, tagging the Docker image, and using AWS CLI commands to authenticate and push the image to the repository.
  - **Service Used**: Amazon Elastic Container Registry (ECR), Docker, AWS CLI
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20100/4.%20Push%20a%20Docker%20Image%20to%20Amazon%20ECR%20Repository)

- **Project 5**:  Creating an Amazon RDS DB Instance (MS SQL Server)
  - **Description**: Set up and configure a Microsoft SQL Server database instance using Amazon RDS. This includes choosing the appropriate instance type, configuring security settings, setting up backups and maintenance windows, and ensuring high availability.
  - **Service Used**: Amazon Relational Database Service (RDS)
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20100/5.%20Creating%20an%20Amazon%20RDS%20DB%20Instance%20(MS%20SQL%20Server))

- **Project 6**:  Create a DynamoDB Table
  - **Description**: This project involved creating a DynamoDB table with provisioned capacity. Three random items were inserted into the table. A scan operation was performed to retrieve all items, and a query operation was used to fetch a single item based on specific criteria.
  - **Service Used**: Amazon DynamoDB
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20100/6.%20Create%20a%20DynamoDB%20Table)

- **Project 7**:  Install & Configure AWS CLI then Create an S3 Bucket
  - **Description**: This project involves setting up AWS CLI on your local machine, configuring it with your AWS credentials, and using it to create, list, and delete an S3 bucket. This process ensures that you can interact with AWS services programmatically and manage your S3 resources effectively.
  - **Service Used**: AWS CLI, Amazon S3
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20100/7.%20Install%20%26%20Configure%20AWS%20CLI%20then%20Create%20an%20S3%20Bucket)

- **Project 8**:  Create an S3 Bucket and store an object in it
  - **Description**: This project involves creating an Amazon S3 bucket using the AWS Management Console and uploading a file into the bucket. Amazon S3 (Simple Storage Service) is used to store and retrieve any amount of data at any time, and this project demonstrates the basic steps of setting up and using S3 for storage.
  - **Service Used**: Amazon S3
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20100/8.%20Create%20an%20S3%20Bucket%20and%20store%20an%20object%20in%20it)

- **Project 9**:  Introduction to SNS (Simple Notification Service)
  - **Description**: This project involves creating an Amazon SNS (Simple Notification Service) topic, subscribing an email address to the topic, and confirming the subscription through the email. After confirming, a test message is sent through the SNS topic to verify that the email address receives the notification, demonstrating the basic functionality and setup of SNS for sending notifications.
  - **Service Used**: Amazon SNS
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20100/9.%20Introduction%20to%20SNS)

- **Project 10**:  Create a Lambda Function to Add Two Numbers
  - **Description**: Developed an AWS Lambda function using Python that takes two numbers as input, adds them together, and returns the result. The function also print the result out in the logs.
  - **Service Used**: AWS Lambda
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20100/10.%20Create%20a%20Lambda%20Function%20to%20Add%20Two%20Numbers)

- **Project 11**:  Host a Simple Static Webpage with S3 and CloudFront
  - **Description**: Set up an S3 bucket to host a static webpage and uploaded the webpage content. Configured an Amazon CloudFront distribution to use the S3 bucket as its origin, ensuring that the webpage content is accessible only through the CloudFront endpoint to enhance security and performance.
  - **Service Used**: Amazon S3, Amazon CloudFront
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20100/11.%20Host%20a%20Simple%20Static%20Webpage%20with%20S3%20and%20CloudFront)

- **Project 12**:  Create an IAM User
  - **Description**: Configured IAM by creating a new user with console access and adding it to a newly created group named "adminsGroup" with `AdministratorAccess` permissions. Enabled multi-factor authentication (MFA) for the root user and applied a password policy to enforce security best practices.
  - **Service Used**: AWS Identity and Access Management (IAM)
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20100/12.%20Create%20an%20IAM%20User)

- **Project 13**:  Use a Managed Config Rule
  - **Description**: Implemented and monitored an AWS Config rule to ensure compliance with encryption policies for EBS volumes. Enabled AWS Config in the US-EAST-1 region, selected the managed Config rule `encrypted-volumes`, and launched an EC2 instance with an unencrypted EBS volume to verify that the Config rule detects non-compliance.
  - **Service Used**: AWS Config
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20100/13.%20Use%20a%20Managed%20Config%20Rule)

- **Project 14**:  Deploy a CloudFormation Template from the AWS Console
  - **Description**: Downloaded a pre-made CloudFormation template and used it to create a CloudFormation stack. Monitored the deployment process through the events tab, confirmed the creation of a DynamoDB table and an S3 bucket, and then deleted the stack to ensure both resources were removed as part of the cleanup.
  - **Service Used**: AWS CloudFormation
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20100/14.%20Deploy%20a%20CloudFormation%20Template%20from%20the%20AWS%20Console)

## Level 200 (Intermediate)

**These projects are designed for those who have a basic understanding of AWS and want to explore more complex scenarios.**

- **Project 1**:  Create an Auto Scaling Group

  - **Description**: This project involves setting up an Auto Scaling Group (ASG) to manage the number of EC2 instances dynamically based on my configuration.
  - **Service Used**: Amazon EC2 Auto Scaling
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20200/1.%20Create%20an%20Auto%20Scaling%20Group)

- **Project 2**:  Deploy a Docker container image on AWS Fargate
  - **Description**: Deployed a Docker container image on AWS Fargate, allowing for scalable and managed execution of containerized applications without managing servers.
  - **Service Used**: AWS Fargate, Docker, Amazon ECS
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20200/2.%20Deploy%20a%20Docker%20container%20image%20on%20AWS%20Fargate)

- **Project 3**:  Create an Aurora RDS Database
  - **Description**: Set up an Aurora RDS database using the MySQL-compatible engine and connected to the database from a local MySQL client _(MySQL Workbench)_ to create and test a table.
  - **Service Used**: Amazon Aurora RDS
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20200/3.%20Create%20an%20Aurora%20RDS%20Database)

- **Project 4**:  Setup a Simple State Machine with Two Steps

  - **Description**: This project involves creating a state machine that orchestrates the execution of two AWS Lambda functions and verifying that the combined functionality produces the correct results.
  - **Service Used**: AWS Step Functions, AWS Lambda
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20200/4.%20Setup%20a%20Simple%20State%20Machine%20with%20at%20Least%20Two%20Steps)

- **Project 5**:  Create a CloudWatch Alarm

  - **Description**: Set up a CloudWatch Alarm to monitor network traffic (NetworkIn) for an EC2 instance.
  - **Service Used**: AWS CloudWatch, AWS EC2
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20200/6.%20Create%20a%20CloudWatch%20Alarm)

## Level 300 (Advanced)

**Projects at this level are for those with significant AWS experience. They include complex architectures and integrations.**

## Level 400 (Expert)

**These are the most challenging projects, showcasing advanced AWS solutions and best practices.**
