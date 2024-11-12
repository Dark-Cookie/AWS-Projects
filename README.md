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

- **Project 5**:  Create a Serverless API

  - **Description**: This project involves setting up a Lambda function designed to respond with the simple message, "Hello Serverless World!" An API Gateway is configured to expose this Lambda function as a RESTful API endpoint, which is tested using Postman. The API is secured with an API key to control access and ensure that only authorized clients can interact with the service.
  - **Service Used**: AWS Lambda, Amazon API Gateway, AWS API Key
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20200/5.%20Create%20a%20Serverless%20API)

- **Project 6**:  Create a CloudWatch Alarm

  - **Description**: Set up a CloudWatch Alarm to monitor network traffic (NetworkIn) for an EC2 instance.
  - **Service Used**: AWS CloudWatch, AWS EC2
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20200/6.%20Create%20a%20CloudWatch%20Alarm)

- **Project 7**:  Create a New CMK in KMS and Encrypt an Object

  - **Description**: Created a new Customer Master Key (CMK) using AWS Key Management Service (KMS) and encrypted a file that was uploaded in an S3 bucket.
  - **Service Used**: AWS Key Management Service (KMS), Amazon S3
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20200/7.%20Create%20a%20New%20CMK%20in%20KMS%20and%20Encrypt%20an%20Object)

- **Project 8**:  Create an EFS Shared File System

  - **Description**: Set up an Amazon Elastic File System (EFS) to provide a shared file system across multiple Amazon EC2 instances in separate Availability Zones within the same region.
  - **Service Used**: Amazon Elastic File System (EFS), Amazon Elastic Compute Cloud (EC2)
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20200/8.%20Create%20an%20EFS%20Shared%20File%20System)

## Level 300 (Advanced)

**Projects at this level are for those with significant AWS experience. They include complex architectures and integrations.**

- **Project 1**:  SQLServer Native Backup and Restore on RDS

  - **Description**: Implemented SQL Server backup and restore on AWS RDS involving Amazon S3 bucket.
  - **Service Used**: Amazon RDS, Amazon S3
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20300/1.%20SQLServer%20Native%20Backup%20and%20Restore%20on%20RDS)

- **Project 2**:  Deploy a VPC with Terraform

  - **Description**: Created Virtual Private Cloud (VPC), route tables (both public and private), route table associations, an internet gateway, an Elastic IP, and a NAT gateway and EC2 instance in both a public and a private subnet using Terraform (IaC)
  - **Service Used**: Amazon VPC, Amazon EC2, Terraform
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20300/2.%20Deploy%20a%20VPC%20with%20Terraform)

## Level 400 (Expert)

**These are the most challenging projects, showcasing advanced AWS solutions and best practices.**

- **Project 1**:  Create a Cluster of Virtual Machines Using Docker Swarm

  - **Description**: Set up a Docker Swarm cluster across five EC2 instances, with one manager node and four worker nodes, and tested it by deploying an Nginx service.
  - **Service Used**: Amazon EC2, Docker (Swarm), Nginx
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/1.%20Create%20a%20Cluster%20of%20Virtual%20Machines%20Using%20Docker%20Swarm)

- **Project 2**:  Build a Basic Web Application

  - **Description**: This project involves building a full-stack web application using AWS Amplify. It features a simple React frontend with user authentication, a serverless function to handle user sign-ups, and a DynamoDB database for storing user emails. The application leverages AWS’s robust and scalable cloud services to deliver a seamless user experience, allowing users to sign up, log in, and store information securely.
  - **Service Used**: AWS Amplify, AWS AppSync, AWS Lambda, Amazon DynamoDB,
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/2.%20Build%20a%20Basic%20Web%20Application)

- **Project 3**:  Build a Serverless Recipe Generator with AWS Amplify and Amazon Bedrock

  - **Description**: In this project, I built a serverless web application using AWS Amplify, integrated with Amazon Bedrock and the Claude 3 Sonnet foundation model for Generative AI. The application allows users to enter a list of ingredients, and in return, it generates creative and delicious recipes powered by AI. The front end is hosted on AWS Amplify, offering continuous deployment, while the backend handles requests to generate recipes from a list of ingredients. AWS services like Cognito for authentication, AppSync for API management, and Lambda for serverless functions are used to power the app.
  - **Service Used**:AWS Amplify, AWS Cognito, AWS AppSync, AWS Lambda, Amazon Bedrock
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/3.%20Build%20a%20Serverless%20Web%20Application%20using%20Generative%20AI)

- **Project 4**:  Building with Generative AI on AWS using PartyRock, Amazon Bedrock, and Amazon Q

  - **Description**: In this project, I worked on three independent projects using Amazon Bedrock and PartyRock:

1. Using PartyRock, I quickly built a book recommendation app that generates personalized suggestions based on the user’s mood and allows for an interactive chatbot experience. This no-code tool made it easy to create and deploy a simple app without writing a single line of code.

2. In Amazon Bedrock, I experimented with powerful foundation models like Claude 3 Sonnet for chat, Amazon Titan for text generation, and Titan Image Generator for creating images from text prompts. This step showed me how to integrate AI models for more creative and dynamic use cases in real-world applications.

3. Lastly, I implemented a document-based AI model that retrieves and uses context to answer questions. I set up embeddings using Amazon Titan, performed similarity searches with FAISS, and used the Claude 3 Sonnet model to generate accurate, context-based responses to user queries. This showcased how to build applications that not only generate content but also pull in relevant information from external sources.

  - **Service Used**:  PartyRock, Amazon Bedrock
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/4.%20Building%20with%20Generative%20AI%20on%20AWS%20using%20PartyRock%2C%20Amazon%20Bedrock%2C%20and%20Amazon%20Q)

- **Project 5**:  Multi-Tier, Highly Available, Fault-Tolerant Web Application

  - **Description**: In this project, I designed and implemented a multi-tier, highly available, and fault-tolerant web application using various AWS services including Amazon VPC, Amazon EC2, Amazon Aurora, and Amazon S3. This architecture ensures scalability, resilience, and efficient resource management. This experience is part of my journey to becoming a Cloud Engineer, focusing on building robust cloud-based applications.
  - **Service Used**:  Amazon VPC,Amazon EC2,Amazon Aurora, Amazon S3
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/5.%20Multi-Tier%2C%20Highly%20Available%2C%20Fault-Tolerant%20Web%20Application%20on%C2%A0AWS/Multi-Tier%2C%20Highly%20Available%2C%20Fault-Tolerant%20Web%20Application%20on%C2%A0AWS)

- **Project 6**:  Building a Highly Available WordPress Web Application

  - **Description**: In this project, I designed and implemented a highly available WordPress web application on AWS using various services, including Amazon VPC, Amazon RDS, Amazon EFS, and Amazon EC2 with Auto Scaling and Application Load Balancer (ALB). This architecture ensures scalability, resilience, and efficient resource management.
  - **Service Used**:  Amazon VPC, Amazon RDS, Amazon EFS, Amazon EC2
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/6.%20Create%20a%20Highly%20Available%20WordPress%20Web%20Application)

- **Project 7**:  Create a Continuous Delivery Pipeline

  - **Description**: In this project, I created a continuous delivery pipeline using AWS services, including AWS Elastic Beanstalk, AWS CodeBuild, and AWS CodePipeline. The pipeline automates the deployment of a web application, ensuring that code changes are automatically built, tested, and deployed to a highly available environment.
  - **Service Used**:  AWS CodePipeline, AWS CodeBuild, AWS Elastic Beanstalk, Amazon EC2 with Auto Scaling
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/7.%20Create%20a%20Continuous%20Delivery%C2%A0Pipeline)


- **Project 8**:  Building Web Applications based on Amazon EKS

 - **Description**: In this project, I built a web application based on Amazon Elastic Kubernetes Service (EKS). The architecture included creating a development environment using AWS Cloud9, building container images with Docker, uploading those images to Amazon Elastic Container Registry (ECR), deploying EKS clusters and services, exploring Container Insights, and implementing auto-scaling for pods and clusters. 
  - **Service Used**:  AWS Cloud9, Amazon Elastic Container Registry (ECR), Amazon Elastic Kubernetes Service (EKS), AWS Fargate
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/8.%20Building%20Web%20Applications%20Using%20Amazon%C2%A0EKS)


- **Project 9**:  Large-scale Data Processing with Step Functions

 - **Description**: In this project, I implemented a large-scale data processing workflow using AWS Step Functions to orchestrate various tasks in a serverless architecture. The workflow utilized Amazon S3 for data storage, IAM for managing permissions, CloudWatch for monitoring and logging, and AWS X-Ray for tracing requests. 
  - **Service Used**:  AWS Step Functions, Amazon S3, IAM (Identity and Access Management), CloudWatch, AWS X-Ray
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/9.%20Large-scale%20Data%20Processing%20with%20Step%20Functions)


- **Project 10**:  Deploying a Complete Machine Learning Fraud Detection Solution Using Amazon SageMaker

  - **Description**: In this project, I deployed a complete machine learning fraud detection solution using Amazon SageMaker. The architecture leverages various AWS services to build, train, and deploy a robust model capable of detecting fraudulent transactions.
  - **Service Used**:  Amazon SageMaker,AWS Lambda, Amazon S3, AWS IAM, Amazon EC2 and VPC, Amazon CloudWatch, Amazon SQS, AWS Secrets Manager, AWS CloudTrail, Amazon Route 53, AWS Systems Manager (SSM), Amazon API Gateway, Amazon SNS, Amazon CloudFormation
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/10.%20Deploying%20a%20Complete%20Machine%20Learning%20Fraud%20Detection%20Solution%20Using%20Amazon%20SageMaker)


- **Project 11**:  Serverless Data Processing on AWS

 - **Description**: In this project, I implemented a serverless data processing solution using AWS services, including Amazon Kinesis, AWS Lambda, Amazon S3, Amazon DynamoDB, Amazon Cognito, and Amazon Athena. The architecture is designed to handle real-time data streams, process and store data efficiently, and enable ad-hoc querying for insights.
  - **Service Used**:  AWS Lambda, Amazon Kinesis Data Analytics, Amazon DynamoDB, Amazon S3, Amazon Kinesis Data Firehose, Amazon Athena, Amazon Cognito
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/11.%20Serverless%20Data%20Processing%20on%20AWS)


- **Project 12**:  AWS Cloud Resume Challenge

  - **Description**: AWS Cloud Resume Challenge was is incredible learning experience and a taste of what real-world cloud architecture looks like. This project, inspired by @ForrestBrazeal  challenge, covers end-to-end deployment of a personal resume website using AWS services, combining serverless computing, infrastructure as code, CI/CD, and front-end development in a practical application.

  - **Service Used**:  Amazon S3, AWS Lambda, DynamoDB, AWS CloudFormation, Route 53, AWS Certificate Manager (ACM), API Gateway, CloudFront
  - **Link**: [Project Directory](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/12.%20Cloud%20Resume%20Challenge)
