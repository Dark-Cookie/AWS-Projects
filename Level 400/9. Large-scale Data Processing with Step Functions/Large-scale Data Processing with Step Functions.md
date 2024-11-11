
![](https://cdn-images-1.medium.com/max/3584/1*YS-gLYRg7AJuu39U1TW3Pg.png)

## AWS Project: Large-scale Data Processing with Step Functions

### Introduction

Processing large volumes of data in an efficient, scalable, and reliable manner is critical in today’s data-driven world. AWS Step Functions, part of AWS’s serverless suite, offers a robust way to build, monitor, and orchestrate complex workflows by coordinating multiple AWS services. In this project, we implemented a data processing workflow designed to handle large-scale datasets. AWS Step Functions serves as the backbone for orchestrating tasks across a serverless architecture, leveraging the power of **Amazon S3** for storage, **IAM** for secure access management, **CloudWatch** for logging and monitoring, and **AWS X-Ray** for request tracing.

This blog will walk through each phase of setting up a large-scale data processing workflow using AWS Step Functions. By the end, readers will understand how to build and manage data processing workflows that are scalable, reliable, and transparent.

### Tech Stack

Key AWS services, tools, and technologies used in this project include:

* **AWS Step Functions**: Orchestrates the data processing workflow

* **Amazon S3**: Provides centralized storage for input and output data

* **IAM (Identity and Access Management)**: Ensures secure permissions and data access policies

* **CloudWatch**: Monitors each step of the workflow, logging actions and errors

* **AWS X-Ray**: Provides detailed tracing and performance insights into the workflow’s execution

### Prerequisites

To follow along with this project, you’ll need the following prerequisites:

 1. **AWS Knowledge**: Familiarity with AWS services like S3, IAM, and CloudWatch.

 2. **Serverless and Workflow Concepts**: Basic understanding of serverless architecture and workflow orchestration.

 3. **AWS CLI and SDKs**: Installed and configured for managing AWS resources.

 4. **IAM Permissions**: Ensure the necessary IAM roles and policies for accessing and executing Step Functions.

### Problem Statement or Use Case

Organizations often deal with complex workflows that require efficient processing of large data sets. This project addresses the need for a scalable and automated solution to manage such workflows reliably. Key challenges include:

* **Coordination of Multiple Services**: Processing large datasets often involves multiple tasks and services that need to be coordinated systematically.

* **Scalability**: As data size grows, the system must scale to handle the increased load.

* **Visibility and Monitoring**: Detailed logging, tracing, and monitoring are essential for troubleshooting and optimizing the workflow.

AWS Step Functions, combined with other AWS services, provides an ideal solution for orchestrating complex workflows in a serverless environment. This architecture not only improves processing efficiency but also enhances visibility and simplifies troubleshooting.

## Architecture Diagram

Below is the architecture diagram for the data processing workflow. The visual aid below illustrates how AWS Step Functions orchestrates interactions among S3, IAM, CloudWatch, and AWS X-Ray to create a reliable and traceable workflow.

![](https://cdn-images-1.medium.com/max/3064/1*9a6RlsaQGb7RPZBYbkcwWw.png)

### Component Breakdown

Each component within this solution has a critical role:

 1. **AWS Step Functions**: Manages the flow of data processing tasks, coordinating AWS services and monitoring the progress of each step.

 2. **Amazon S3**: Acts as a centralized storage solution for data inputs and outputs, making it easy to retrieve and store processed data.

 3. **IAM**: Controls access to resources, ensuring only authorized services and users can interact with sensitive data.

 4. **CloudWatch**: Provides insights into each task’s status, allowing us to set up alarms for failure states or performance bottlenecks.

 5. **AWS X-Ray**: Traces and analyzes the performance of the workflow, helping to optimize and troubleshoot issues effectively.

## Step-by-Step Implementation

## Introduction to Distributed Map

## [Setup](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/workshop-setup/self-service/hello-dmap#setup)

Important

Follow the instructions on this page only if you are executing this workshop in your own account. To skip these instructions [click here](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/hello_dmap).

* Click on the **Launch** link against any of the regions in the table below to start the deployment.

RegionDeployment

* **US East (Northern Virginia)** us-east-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/template?stackName=sfw-hello-distributed-map&templateURL=https://ws-assets-prod-iad-r-iad-ed304a55c2ca1aee.s3.us-east-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_hellodmap.yml)

* **Europe (Ireland)** eu-west-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/template?stackName=sfw-hello-distributed-map&templateURL=https://ws-assets-prod-iad-r-dub-85e3be25bd827406.s3.eu-west-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_hellodmap.yml)

* **Asia Pacific (Singapore)** ap-southeast-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/create/template?stackName=sfw-module-distributed-map&templateURL=https://ws-assets-prod-iad-r-sin-694a125e41645312.s3.ap-southeast-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_hellodmap.yml)

* **Asia Pacific (Sydney)** ap-southeast-2 [Launch](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/create/template?stackName=sfw-hello-distributed-map&templateURL=https://ws-assets-prod-iad-r-syd-b04c62a5f16f7b2e.s3.ap-southeast-2.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_hellodmap.yml)

The location of the CloudFormation template will be auto-populated in the **Amazon S3 URL** field, as shown in the image below. Click **Next**.

![](https://cdn-images-1.medium.com/max/4000/0*zrDCGMsQXe6YR8fS.png)

* On the *Specify stack details* page, **Stack name** will be auto-populated to sfw-hello-distributed-map. (You can enter a different name if you want.) Click **Next** two times.

![](https://cdn-images-1.medium.com/max/4000/0*tfOqR7y_tiOz1Fqj.png)

* On the *Review* page, scroll to the bottom, check the **Capabilities** box if shown, then click **Create stack**.

![](https://cdn-images-1.medium.com/max/4000/0*nfxv0mXx7V80GC96.png)

* Wait until the stack shows *CREATE_COMPLETE* status.

![](https://cdn-images-1.medium.com/max/4000/0*BQfxt3ONw-NjUlxt.png)

## Building a Distributed Map Workflow

## [Setup](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/workshop-setup/self-service/process-multiple-files#setup)

Important

Follow the instructions on this page only if you are executing this workshop in your own account. To skip these instructions [click here](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/process_multiple_files).

* Click on the **Launch** link against any of the regions in the table below to start the deployment.

Region Deployment

* **US East (Northern Virginia)** us-east-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/template?stackName=sfw-processmulti-distributed-map&templateURL=https://ws-assets-prod-iad-r-iad-ed304a55c2ca1aee.s3.us-east-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_processmultifile.yml)

* **Europe (Ireland)** eu-west-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/template?stackName=sfw-processmulti-distributed-map&templateURL=https://ws-assets-prod-iad-r-dub-85e3be25bd827406.s3.eu-west-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_processmultifile.yml)

* **Asia Pacific (Singapore)** ap-southeast-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/create/template?stackName=sfw-processmulti-distributed-map&templateURL=https://ws-assets-prod-iad-r-sin-694a125e41645312.s3.ap-southeast-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_processmultifile.yml)

* **Asia Pacific (Sydney)** ap-southeast-2 [Launch](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/create/template?stackName=sfw-processmulti-distributed-map&templateURL=https://ws-assets-prod-iad-r-syd-b04c62a5f16f7b2e.s3.ap-southeast-2.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_processmultifile.yml)

The location of the CloudFormation template will be auto-populated in the **Amazon S3 URL** field, as shown in the image below. Click **Next**.

![](https://cdn-images-1.medium.com/max/4000/0*Z29GefbO-Ji3Evpa.png)

* On the *Specify stack details* page, **Stack name** will be auto-populated to sfw-processmulti-distributed-map. (You can enter a different name if you want.) Click **Next** two times.

![](https://cdn-images-1.medium.com/max/4000/0*xLbCKFl7M-rT6cJp.png)

-On the *Review* page, scroll to the bottom, check the **Capabilities** box if shown, then click **Create stack**.

![](https://cdn-images-1.medium.com/max/4000/0*S9v6pHOzfoMfRY8x.png)

* Wait until the stack shows *CREATE_COMPLETE* status.

![](https://cdn-images-1.medium.com/max/4000/0*OL3ycBZKu2bZ87Ts.png)

## Advanced — Optimization

## [Setup](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/workshop-setup/self-service/optimization#setup)

Important

Follow the instructions on this page only if you are executing this workshop in your own account. To skip these instructions [click here](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization).

* Click on the Launch link against any of the regions in the table below to start the deployment.

Region Deployment

* **US East (N. Virginia)** us-east-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/template?stackName=sfw-optimization-distributed-map&templateURL=https://ws-assets-prod-iad-r-iad-ed304a55c2ca1aee.s3.us-east-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_optimization.yml)

* **Europe (Ireland)** eu-west-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/template?stackName=sfw-optimization-distributed-map&templateURL=https://ws-assets-prod-iad-r-dub-85e3be25bd827406.s3.eu-west-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_optimization.yml)

* **Asia Pacific (Singapore)** ap-southeast-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/create/template?stackName=sfw-optimization-distributed-map&templateURL=https://ws-assets-prod-iad-r-sin-694a125e41645312.s3.ap-southeast-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_optimization.yml)

* **Asia Pacific** (Sydney) ap-southeast-2 [Launch](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/create/template?stackName=sfw-optimization-distributed-map&templateURL=https://ws-assets-prod-iad-r-syd-b04c62a5f16f7b2e.s3.ap-southeast-2.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_optimization.yml)

Location of the CloudFormation template will be auto populated in the Amazon S3 URL field as shown in the diagram below. Click Next

![](https://cdn-images-1.medium.com/max/4000/0*U05i0W_mLop6JisC.png)

* On the *Specify stack details* page, *Stack name* would be auto populated to sfw-optimization-distributed-map. You can specify a different name if you want.

![](https://cdn-images-1.medium.com/max/4000/0*ntqCkzefCo5PGQ5K.png)

* Click *Next* two times and on the last Review page, scroll to the bottom. Click the checkbox if shown and then click Create stack.

![](https://cdn-images-1.medium.com/max/4000/0*vRNIB7k67YOQ293w.png)

* Wait till the stack shows CREATE_COMPLETE status.

![](https://cdn-images-1.medium.com/max/4000/0*hMo7b5y_dL0AA67b.png)

## Use Case — Healthcare Claims Processing

## [Setup](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/workshop-setup/self-service/healthcare-claims-processing#setup)

Important

Follow the instructions on this page only if you are executing this workshop in your own account. To skip these instructions [click here](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/healthcare-claims-processing).

* Select Launch link against any of the regions in the table below to start the deployment.

Region Deployment

* **US East (N. Virginia)** us-east-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/template?stackName=sfw-healthcare-processing&templateURL=https://ws-assets-prod-iad-r-iad-ed304a55c2ca1aee.s3.us-east-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_healthcare_processing.yml)

* **Europe (Ireland)** eu-west-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/template?stackName=sfw-healthcare-processing&templateURL=https://ws-assets-prod-iad-r-dub-85e3be25bd827406.s3.eu-west-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_healthcare_processing.yml)

* **Asia Pacific (Singapore)** ap-southeast-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/create/template?stackName=sfw-healthcare-processing&templateURL=https://ws-assets-prod-iad-r-sin-694a125e41645312.s3.ap-southeast-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_healthcare_processing.yml)

* **Asia Pacific** (Sydney) ap-southeast-2 [Launch](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/create/template?stackName=sfw-healthcare-processing&templateURL=https://ws-assets-prod-iad-r-syd-b04c62a5f16f7b2e.s3.ap-southeast-2.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_healthcare_processing.yml)

Location of the CloudFormation template will be auto populated in the Amazon S3 URL field as shown in the diagram below. Select Next

![](https://cdn-images-1.medium.com/max/4000/0*temi3DHm5cpXRfvo.png)

* On the *Specify stack details* page, *Stack name* would be auto populated to sfw-healthcare-processing. You can specify a different name if you want.

![](https://cdn-images-1.medium.com/max/4000/0*73Qyhv6UpOrsEkZy.png)

* Select *Next* two times and on the last Review page, scroll to the bottom. Select the checkbox if shown and then Select Create stack.

![](https://cdn-images-1.medium.com/max/4000/0*J1giJP7e9H_SifKd.png)

* Wait till the stack shows CREATE_COMPLETE status.

![](https://cdn-images-1.medium.com/max/4000/0*DHo2dtvkI0ElCfhj.png)

## Use Case — Security Vulnerability Scanning

## [Setup](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/workshop-setup/self-service/security-vulnerability-scanning#setup)

Important

Follow the instructions on this page only if you are executing this workshop in your own account. To skip these instructions [click here](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/security-vulnerability-scanning).

* Click on the **Launch** link against any of the regions in the table below to start the deployment.

Region Deployment

* **US East (Northern Virginia)** us-east-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/template?stackName=vulnerability-scanning-module&templateURL=https://ws-assets-prod-iad-r-iad-ed304a55c2ca1aee.s3.us-east-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_securityscanning.yml)

* **Europe (Ireland)** eu-west-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/template?stackName=vulnerability-scanning-module&templateURL=https://ws-assets-prod-iad-r-dub-85e3be25bd827406.s3.eu-west-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_securityscanning.yml)

* **Asia Pacific (Singapore)** ap-southeast-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/create/template?stackName=vulnerability-scanning-module&templateURL=https://ws-assets-prod-iad-r-sin-694a125e41645312.s3.ap-southeast-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_securityscanning.yml)

* **Asia Pacific (Sydney)** ap-southeast-2 [Launch](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/create/template?stackName=vulnerability-scanning-module&templateURL=https://ws-assets-prod-iad-r-syd-b04c62a5f16f7b2e.s3.ap-southeast-2.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_securityscanning.yml)

The location of the CloudFormation template will be auto-populated in the **Amazon S3 URL** field. Click **Next**.

* On the *Specify stack details* page, **Stack name** will be auto-populated to vulnerability-scanning-module. (You can enter a different name if you want.) Click **Next** two times.

* On the *Review* page, scroll to the bottom then select the **Capabilities and transforms** boxes if shown.

* Click **Submit** and wait until the stack shows *CREATE_COMPLETE* status.

## Use Case — Monte Carlo Simulation

## [Setup](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/workshop-setup/self-service/monte-carlo-simulation#setup)

Important

Follow the instructions on this page only if you are executing this workshop in your own account. To skip these instructions [click here](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/monte-carlo-simulation).

## [About The Stacks](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/workshop-setup/self-service/monte-carlo-simulation#about-the-stacks)

The solution is deployed in two sets. The first stack will deploy a stack of resources that will generate our simulated dataset. It is orchestrated by Step Functions and consists of three Lambda Functions that will generate the data as well as generate a simulated S3 Inventory Report. The second stack will execute the first state machine as well as deploy the components for the module.

## [Deploy The Data Generation Stack](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/workshop-setup/self-service/monte-carlo-simulation#deploy-the-data-generation-stack)

* Click on the Launch link against any of the regions in the table below to start the deployment.

* RegionDeployment**US East (N. Virginia)** us-east-1[Launch ](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/template?stackName=sfn-datagen&templateURL=https://ws-assets-prod-iad-r-iad-ed304a55c2ca1aee.s3.us-east-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_montecarlosimulationdatagen.yml)**Europe (Ireland)** eu-west-1[Launch ](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/template?stackName=sfn-datagen&templateURL=https://ws-assets-prod-iad-r-dub-85e3be25bd827406.s3.eu-west-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_montecarlosimulationdatagen.yml)**Asia Pacific (Singapore)** ap-southeast-1[Launch ](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/create/template?stackName=sfn-datagen&templateURL=https://ws-assets-prod-iad-r-sin-694a125e41645312.s3.ap-southeast-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_montecarlosimulationdatagen.yml)**Asia Pacific** (Sydney) ap-southeast-2[Launch](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/create/template?stackName=sfn-datagen&templateURL=https://ws-assets-prod-iad-r-syd-b04c62a5f16f7b2e.s3.ap-southeast-2.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_montecarlosimulationdatagen.yml)

* Take Defaults — Click Next 3 times and Submit

* Deployment Screenshots

## [Deploy The Data Processing Stack](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/workshop-setup/self-service/monte-carlo-simulation#deploy-the-data-processing-stack)

* Click on the Launch link against any of the regions in the table below to start the deployment.

Region Deployment

* **US East (N. Virginia)** us-east-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/template?stackName=sfn-dataproc&templateURL=https://ws-assets-prod-iad-r-iad-ed304a55c2ca1aee.s3.us-east-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_montecarlosimulationdataproc.yml)

* **Europe (Ireland)** eu-west-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/template?stackName=sfn-dataproc&templateURL=https://ws-assets-prod-iad-r-dub-85e3be25bd827406.s3.eu-west-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_montecarlosimulationdataproc.yml)

* **Asia Pacific (Singapore)** ap-southeast-1 [Launch](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/create/template?stackName=sfn-dataproc&templateURL=https://ws-assets-prod-iad-r-sin-694a125e41645312.s3.ap-southeast-1.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_montecarlosimulationdataproc.yml)

* **Asia Pacific** (Sydney) ap-southeast-2 [Launch](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/create/template?stackName=sfn-dataproc&templateURL=https://ws-assets-prod-iad-r-syd-b04c62a5f16f7b2e.s3.ap-southeast-2.amazonaws.com/2a22e604-2f2e-4d7b-85a8-33b38c999234/templates/module_montecarlosimulationdataproc.yml)

* Take Defaults — Click Next 3 times and Submit

**Deployment Screenshots**

### [Step 1](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/workshop-setup/self-service/monte-carlo-simulation#step-1)

![](https://cdn-images-1.medium.com/max/4000/0*5DAFNbQgi2wpsSxm.png)

### [Step 2](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/workshop-setup/self-service/monte-carlo-simulation#step-2)

![](https://cdn-images-1.medium.com/max/4000/0*0cfo_z0lJqsiHdhV.png)

### [Step 3](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/workshop-setup/self-service/monte-carlo-simulation#step-3)

![](https://cdn-images-1.medium.com/max/4000/0*Ye_3R7fyse9xdqUe.png)

### [Step 4](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/workshop-setup/self-service/monte-carlo-simulation#step-4)

![](https://cdn-images-1.medium.com/max/4000/0*xHxrDnpTpgL04Wd9.png)

## Module 1 — Basics

## Introduction to Distributed Map

## [What is Distributed Map?](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/hello-dmap#what-is-distributed-map)

Distributed map is a map state that executes the same processing steps for multiple entries in a dataset at 10,000 concurrency. This means you can run a large-scale parallel data processing workload without worrying about how to parallelize the executions, workers, and data. Distributed map can iterate over millions of objects such as logs, images or records inside .csv or json files stored in Amazon S3. It can launch up to 10,000 parallel child workflows to process the data. You can include any combination of AWS services like AWS Lambda functions, Amazon ECS tasks, or AWS SDK calls in the child workflow.

Distributed map is the new addition to the two types of maps available in Step Functions. Primary difference between the inline map and distributed map are -

 1. Distributed map has higher concurrency compared to inline map’s 40 concurrency

 2. Distributed map can directly iterate on the data from Amazon S3.

 3. Each sub workflow of Distributed map runs as a separate child workflow that avoids 25K execution history limit of Step Functions.

![](https://cdn-images-1.medium.com/max/2844/0*PcSwHK_pHUp2u5KF.png)

## [What you do in the module](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/hello-dmap#what-you-do-in-the-module)

In this module, you will explore a pre-created workflow using distributed map. The sample workflow processes [amazon reviews data ](https://cseweb.ucsd.edu/~jmcauley/datasets.html#amazon_reviews)from [repo ](https://github.com/MengtingWan/marketBias/tree/master/data). It is 83MB CSV file with 1,292,954 records.

![](https://cdn-images-1.medium.com/max/2956/0*KV2ucds-UVPEL6UE.png)

The workflow iterates on the electronics review data and filters highly rated reviews. You will download the data to S3, run the workflow, and verify the results. In the process, you will learn how to build and run a simple distributed map workflow yourself.

## [Services in this module](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/hello-dmap#services-in-this-module)

* [AWS Step Functions ](https://aws.amazon.com/step-functions/)— Serverless visual workflow service

## Reviewing the Workflow

Open the [Step Functions console ](https://console.aws.amazon.com/states/home)then select the state machine containing “HelloDmap” in its name.

Choose **Edit** to edit the workflow in Workflow Studio.

Review the definition by selecting the **Definition** toggle at the right side of the page.

Select each state in the design to review its definition.

![](https://cdn-images-1.medium.com/max/2434/0*68xEgneAiHLCB16-.png)

Take a closer look at the map definition. You define it as *DISTRIBUTED* to tell Step Functions to run the map state in distributed mode.

![](https://cdn-images-1.medium.com/max/2000/0*PudKk_GNJN5KG2r7.png)

Did you notice *ItemReader* in the definition? This is how you tell distributed map to process a csv or JSON file. Notice that the reader uses the *s3.getObject* API to read the object. Yes! It reads the content of the csv file and distributes the data to child workflows in batches, running at a concurrency of 10,000!

![](https://cdn-images-1.medium.com/max/2000/0*Bkk0IQ-5h8QBvHJb.png)

Read ahead and you will notice that there are a few more settings.

Firstly, you can do batching. Do you see *MaxItemsPerBatch* set as 1000?

You can not only run 10,000 (10K) workflows, you can also batch the data to each workflow which means, in a single iteration, you can process **10K * 1K = 10M** records from the csv file!

Secondly, you can write the output of the distributed map or the child workflow execution results to an S3 location in an aggregated fashion.

Thirdly, you can set failure toleration. What does that mean? You don’t want to run 100M records when half of them are bad data. It is a waste of time and money to process those records. By default, the failure toleration is set to 0. Any single child workflow failure will result in the failure of the workflow.

Data quality is a big challenge with large data processing. So, you can set a percentage or number of items that can be tolerated as failures. When failures exceed that tolerance, the Step Functions workflow fails, saving you time and money.

![](https://cdn-images-1.medium.com/max/2000/0*hgCUiDoBZ-8AxB6b.png)

Next, you can see what is inside of the distributed map. In this introduction module, we did not use any compute services like Lambda to process the records. You can see a *pass* state filtering highly rated reviews. Pass state is really useful to transform and filter input. It is simple and easy to demonstrate the 10K concurrency, without worrying about the scale of a downstream service. However, in a real-world scenario, you include any combination of AWS services like AWS Lambda functions, Amazon ECS tasks, or AWS SDK calls in the child workflow.

![](https://cdn-images-1.medium.com/max/2000/0*ebGJAlDa6Yz6MmZL.png)

The states inside the distributed map are run as separate child workflows. The number of child workflows is dependent on the concurrency setting and the volume of the records to process. For example, you might set the concurrency to 1000 and batch size to 100, but if the total number of records in the file is just 20K, Step Functions only needs 200 child workflows (20,000 / 100 = 200). On the other hand, if the file has 200K records, Step Functions will spin up 1000 child workflows to reach the max concurrency and as child workflows complete, Step Functions will spin up child workflows until all 2000 (200,000 /100 = 2000) child workflows are completed.

## Running the Workflow

## [Prepare data set](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/hello-dmap/start-execution#prepare-data-set)

* You are going to download the data file to the local computer and upload to S3.

* Run the following command in your local machine.

* Linux or macOS (bash)

    curl https://raw.githubusercontent.com/MengtingWan/marketBias/master/data/df_electronics.csv --output df_electronics.csv

* Windows (PowerShell)

    Invoke-WebRequest https://raw.githubusercontent.com/MengtingWan/marketBias/master/data/df_electronics.csv -OutFile df_electronics.csv

* Open the [S3 console ](https://console.aws.amazon.com/s3)then select the bucket containing “hellodmapdatabucket” in its name.

* Copy the downloaded file “df_electronics.csv” to the S3 bucket.

## [Run the workflow](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/hello-dmap/start-execution#run-the-workflow)

* Open the [Step Functions Console ](https://console.aws.amazon.com/states/home), select and open the state machine containing **HelloDmapStateMachine** in its name.

* Choose **Start execution** in the top right corner.

* In the popup, enter the following input:

    {
        "key":"df_electronics.csv",
        "output":"results"
    }

* You are providing the name of the file and S3 prefix where you want the results of the distributed map to be stored.

* Click **Start execution**.

* In a few seconds, you will see the execution start to run. It takes a couple of minutes to complete the processing.

![](https://cdn-images-1.medium.com/max/2000/0*vKZvMp4Nj-2DATkW.png)

Congratulations! You have successfully run the hello distributed map workflow.

## Viewing the Workflow Results

## [View map run](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/hello-dmap/view-results#view-map-run)

Navigate to the bottom of the workflow execution page and click **Map Run**.

![](https://cdn-images-1.medium.com/max/2000/0*eqXpFcTXMLyIDBc0.png)

You can see the status of the distributed processes in the **Item processing status** card. It shows the number of records processed and the duration. 1,292,954 records processed in less than 90 seconds! At the bottom of the page, you can see the links to all the child workflow executions.

![](https://cdn-images-1.medium.com/max/2334/0*NkzFCuGJPyUqs6VX.png)

Open one of the child workflows and view the execution input/output. You see in the output window that the *pass* state filtered the records with ratings of 4 and above.

![](https://cdn-images-1.medium.com/max/2294/0*DDSxQgAnOfHv4iuT.png)

## [Verifying S3 results bucket](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/hello-dmap/view-results#verifying-s3-results-bucket)

Recall you also stored the results of the workflow to an S3 bucket! Open the [S3 console ](https://console.aws.amazon.com/s3)then select the bucket containing **hellodmapresultsbucket** in its name.

![](https://cdn-images-1.medium.com/max/2014/0*5MnNL1k5zb5DLURg.png)

Navigate to contents inside results prefix, select "SUCCEEDED_0.json", and download the file to view the results. You notice that the content of the file is the aggregated result of all the child workflows. If you are building *map reduce* use cases, this content can be used for the downstream. To learn more about how result writer works, follow the link [here ](https://docs.aws.amazon.com/step-functions/latest/dg/input-output-resultwriter.html).

Fantastic! You processed **1.3M** records in less than **90 seconds** without using servers and running complex code. Distributed map is **exciting**, right!?!

## Summary

You have now reviewed a pre-created workflow with distributed map, learned some important attributes of the distributed map definition, and ran the workflow yourself using the AWS console.

While the AWS console provides a convenient way to execute workflows at the click of a button, in real-world production environments, large scale data processing workflows are commonly invoked per schedule or the occurrence of an event (ex. a file upload).

Two common patterns to invoke AWS Step Functions state machines are [Amazon EventBridge schedules ](https://docs.aws.amazon.com/scheduler/latest/UserGuide/schedule-types.html)and [S3 event notifications ](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventNotifications.html). You can use the following links for instructions to set up event-driven or scheduled executions for the workflows.

* [Periodically start a workflow execution using an Amazon EventBridge Scheduler](https://docs.aws.amazon.com/step-functions/latest/dg/using-eventbridge-scheduler.html)

* [Start a state machine execution in response to S3 events](https://docs.aws.amazon.com/step-functions/latest/dg/tutorial-cloudwatch-events-s3.html)

In the next module, you will build a distributed map workflow yourself. Get ready for some challenging questions as well!

## Building a Distributed Map Workflow

## [Introduction](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/process-multiple-files#introduction)

In the previous module, you saw an example of distributed processing with a single S3 object. Distributed map not only iterates on a single large object located in S3, it can also iterate on a collection of objects in S3. You can iterate through and process each object in parallel and aggregate the results. This supports various use cases, such as processing thousands of log files, a Monte Carlo simulation which runs the same processing for multiple inputs, running a backfill process that scans millions of files for security vulnerability for past dates.

Below diagram shows how distributed map works for multiple S3 objects. Notice that it uses S3.listObjectV2 instead of S3.GetObject which is used in the previous sub module. When processing multiple objects, distributed map lists the metadata of the objects, distributes batches of the metadata to the child workflows. This means you can process any file format; structured, unstructured, and semi-structured.

![](https://cdn-images-1.medium.com/max/2808/0*8o69LKov5HfpmHJC.png)

## [What you do in the module](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/process-multiple-files#what-you-do-in-the-module)

In this module, you are going to build a distributed map workflow that processes thousands of weather data files from [NOAA climatology data ](https://docs.opendata.aws/noaa-ghcn-pds/readme.html). The workflow is going to find the highest precipitation for each weather station and store the results in DynamoDB table. Each weather station is an individual S3 object containing various weather data and there are about 1,000 stations.

## [Services used](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/process-multiple-files#services-used)

* [AWS Step Functions ](https://aws.amazon.com/step-functions/)— Serverless visual workflow service

* [AWS Lambda ](https://aws.amazon.com/lambda/)— Compute service; functions in serverless runtimes

* [Amazon DynamoDB ](https://aws.amazon.com/dynamodb/)is a fully managed, serverless, key-value NoSQL database designed to run high-performance applications. DynamoDB offers built-in security, continuous backups, automated multi-Region replication, in-memory caching, and data export tools.

## [Pre-created resources](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/process-multiple-files#pre-created-resources)

To quickly build the workflow, we have created a few resources ahead of time.

* Lambda function to find the highest precipitation for the station

* One S3 bucket for the dataset and another S3 bucket for storing distributed map results

* Sample dataset of 1,000 S3 objects from NOAA climatology data

* Amazon DynamoDB table to store the precipitation data

## Building the Workflow

## [Workflow Studio](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/process-multiple-files/build#workflow-studio)

Open the [Step Functions console ](https://console.aws.amazon.com/states/home)then choose **Create state machine** button.

Choose the **Blank** card and choose **Select**.

You are in Workflow Studio. Take a moment to explore it. You will see the actions, flows, and patterns on the left side. You can drag the states on the left to the center of the page where you see the workflow design. You can configure the input, output, errors, etc. on the right side of the UI. If you click the **Definition** toggle, you can view the *ASL* definition of the workflow.

Take some time to explore the menus.

![](https://cdn-images-1.medium.com/max/2504/0*rYIuQIR2CojoBjIo.png)

Alright! Let’s start building the workflow.

Select the **Flow** tab under the search text box on the left side.

Drag the **Map** state between the Start and End states.

Change the **Processing mode** to **Distributed — new**.

![](https://cdn-images-1.medium.com/max/2000/0*8iecgGyIJ7cgldPO.png)

Alright. You are now going to configure additional attributes of the distributed map. First, you will configure where to read the dataset from. You will pass the location of the precreated dataset in S3 as input.

Select **Amazon S3** as the *Item source*.

Select **S3 object list** in **S3 item source** dropdown.

Select **Get bucket and prefix at runtime from state input** in **S3 bucket** dropdown.

For **Bucket Name**, enter $.bucket

For **Prefix**, enter $.prefix

![](https://cdn-images-1.medium.com/max/2000/0*l6X70C0QL6TvW56s.png)

**Enable batching** and set the **Max items per batch** to 100.

![](https://cdn-images-1.medium.com/max/2000/0*J_zmUU_dv2GW4Pze.png)

Leave everything else as default and move on to adding the child workflow components.

Enter Lambda

in the search textbox at the top left.

Drag and drop the **Lambda — Invoke** action to the center.

Click on the **Function name** dropdown and select function with **HighPrecipitation** in its name.

![](https://cdn-images-1.medium.com/max/2500/0*m6vgYYU6L7m9EWRz.png)

Review the definition in the workflow studio by clicking the **Definition** toggle at the right.

Notice that the *ItemReader* object uses listobjectV2. In sub-module 1, you saw *GetObject* in ItemReader. The reason is that you processed a single S3 object in sub-module 1 and you are processing multiple S3 objects here in this sub-module. You can nest both patterns to read multiple csv/json files in a highly parallel fashion.

          "ItemReader": {
            "Resource": "arn:aws:states:::s3:listObjectsV2",
            "Parameters": {
              "Bucket.$": "$.bucket",
              "Prefix.$": "$.prefix"
            }
          },

Select the **Config** tab next to the state machine name at the top of the page and change the State machine name to FindHighPrecipitationWorkflow.

Choose an existing IAM role with the name StatesHighPrecipitation

Choose **Create** button at the top.

Run the workflow by choosing **Start execution** button.

Wait! You need the bucket and prefix as input to the workflow.

Open the [S3 console ](https://console.aws.amazon.com/s3)then copy the full name of the bucket containing **MultiFileDataBucket** in its name.

Return to the *Start execution* popup and enter the following json as input, replacing the bucket name with your bucket name from S3:

    {
      "bucket": "bucketname",
      "prefix":"csv/by_station"
    }

To verify the ASL definition

    {
      "Comment": "A description of my state machine",
      "StartAt": "Map",
      "States": {
        "Map": {
          "Type": "Map",
          "ItemProcessor": {
            "ProcessorConfig": {
              "Mode": "DISTRIBUTED",
              "ExecutionType": "STANDARD"
            },
            "StartAt": "Lambda Invoke",
            "States": {
              "Lambda Invoke": {
                "Type": "Task",
                "Resource": "arn:aws:states:::lambda:invoke",
                "OutputPath": "$.Payload",
                "Parameters": {
                  "Payload.$": "$",
                  "FunctionName": "arn:aws:lambda:{region}:{account}:function:sfw-processmulti-distribu-HighPrecipitationFunctio-vH7XVagF8llI:$LATEST"
                },
                "Retry": [
                  {
                    "ErrorEquals": [
                      "Lambda.ServiceException",
                      "Lambda.AWSLambdaException",
                      "Lambda.SdkClientException",
                      "Lambda.TooManyRequestsException"
                    ],
                    "IntervalSeconds": 1,
                    "MaxAttempts": 3,
                    "BackoffRate": 2
                  }
                ],
                "End": true
              }
            }
          },
          "End": true,
          "Label": "Map",
          "MaxConcurrency": 1000,
          "ItemReader": {
            "Resource": "arn:aws:states:::s3:listObjectsV2",
            "Parameters": {
              "Bucket.$": "$.bucket",
              "Prefix.$": "$.prefix"
            }
          },
          "ItemBatcher": {
            "MaxItemsPerBatch": 100,
            "BatchInput": {
              "Bucket.$": "$.bucket"
            }
          }
        }
      }
    }

## [Fix the workflow](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/process-multiple-files/build#fix-the-workflow)

Expand the error ribbon to see the reason for failure.

![](https://cdn-images-1.medium.com/max/2212/0*X1xMAwybC9SMX4T0.png)

By default, distributed map fails even when a single child fails. Let’s explore what actually caused the child workflow to fail.

Click **Map Run** to check the child workflow executions.

![](https://cdn-images-1.medium.com/max/2240/0*Ys4DDrOpACidQmZO.png)

Select a child workflow execution to see the error.

![](https://cdn-images-1.medium.com/max/4000/0*-m5x7j5aTCRDcKIV.png)

It looks like Lambda is expecting event[BatchInput][Bucket] and it is not found. Explore the input to the Lambda function by selecting **Execution input and output** at the top.

![](https://cdn-images-1.medium.com/max/2216/0*1ChlRrtfi-0fXYpV.png)

The input only contains the S3 key. It is missing the bucket name.

Alright! You are going back to Workflow Studio to pass the bucket name as input to the Lambda function.

Close this tab and return to the previous tab, or you can browse to the workflow by following the breadcrumb at the top.

Choose **Edit** button.

Select the map state in the workflow design.

Modify the Batch input under **Item batching** to include the bucket name.

    { 
      "Bucket.$": "$.bucket"
    }

Batch Input enables you to send additional input to the child workflow steps as global data. For ex. Bucket name is a global data and does not have to be in individual line item.

![](https://cdn-images-1.medium.com/max/2000/0*hBc7aERK2M40NHUg.png)

**Save** the workflow and choose **Execute** button.

Do not forget to execute the workflow with the proper bucket and prefix input. If you would like, you can get the input by navigating to the previous execution of the workflow.

Voila!! It is a success now!

## Viewing the Workflow Results

## [Verify Results](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/process-multiple-files/view-results#verify-results)

Open the [Lambda console ](https://console.aws.amazon.com/lambda)and select the function containing **HighPrecipitation** in its name.

Explore the **Code**.

The Lambda function writes the calculated value to DynamoDB table.

    def _write_results_to_ddb(high_by_station: Dict[str, Dict]):
        dynamodb = boto3.resource("dynamodb")
        table = dynamodb.Table(os.environ["RESULTS_DYNAMODB_TABLE_NAME"])

The table name comes from the env variable RESULTS_DYNAMODB_TABLE_NAME.

Click **Configuration** and select **Environment Variable** to view the table name.

![](https://cdn-images-1.medium.com/max/3652/0*LRHKrhyEHln3xDM4.png)

Open the [DynamoDB console ](https://console.aws.amazon.com/dynamodbv2)then select **Tables** from left side menu.

Select the table name that you saw in the Lambda configuration and choose **Explore table items**. You can now view the calculated highest precipitation across stations.

![](https://cdn-images-1.medium.com/max/2000/0*EhqJOWcs6PXMF9Es.png)

## Summary

In this module, you built a data processing workflow with distributed map, configuring Step Functions to distribute the S3 objects across multiple child workflows to process them in parallel.

Important

When processing large numbers of objects in S3 with Distributed Map, you have a couple of different options for listing those objects: S3 *listObjectsV2* and [S3 Inventory List ](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-inventory.html). With S3 *listObjectsV2*, Step Functions is making S3 *listObjectsV2 *API calls on your behalf to retrieve all of the items needed to run the Distributed Map. Each call to *listObjectsV2* can only return a maximum of 1000 S3 objects. This means that if you have 2,000,000 objects to process, Step Functions has to make at least 2000 API calls. This API is fast and it won’t take too long, but if you have an S3 Inventory file that has all the objects listed in it that you need to process, you can use that as the input.

Using an S3 Inventory file as the input for a Distributed Map when processing large numbers of files is faster than S3 *listObjectsV2*. This is because, for S3 Inventory ItemReaders, there is a single S3 *getObject* call to get the manifest file and then one call for each Inventory file. If you know that your Distributed Map is going to run on a set schedule you can schedule the S3 Inventory to be created ahead of time.

## Module 2 — Advanced

Welcome to the Advanced module of the data processing workshop!

In the [basics](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics) module, you learnt about how to use distributed map to build large scale data processing solution. For a production ready application, you need to make sure the solution is optimized for performance, cost etc. We will focus on optimizing cost and performance of the distributed map in this module.

## Optimization

## [Introduction](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization#introduction)

In this module, we use the same example workflow from earlier sub module [Building a distributed map workflow](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/basics/process-multiple-files). But we pre-created the workflow to make it easier for you.

## [Services used](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization#services-used)

* [AWS Step Functions ](https://aws.amazon.com/step-functions/)— Serverless visual workflow service

* [AWS Lambda ](https://aws.amazon.com/lambda/)— compute service; functions in serverless runtimes

* [Amazon DynamoDB ](https://aws.amazon.com/dynamodb/)is a fully managed, serverless, key-value NoSQL database designed to run high-performance applications. DynamoDB offers built-in security, continuous backups, automated multi-Region replication, in-memory caching, and data export tools.

## [Pre-created resources](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization#pre-created-resources)

* A Lambda function to find the highest precipitation for the station.

* One S3 bucket for data set and another S3 bucket for storing distributed map results.

* Sample data set of 1000 S3 objects from [NOAA climatology data ](https://docs.opendata.aws/noaa-ghcn-pds/readme.html).

* Amazon DynamoDB table to store the precipitation data.

* A Step Functions workflow that processes the sample data.

## [What you do in the module](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization#what-you-do-in-the-module)

Using the precreated Step Functions workflow, you will tune some attributes/fields of distributed map and understand the performance and cost impact of the change.

## Choosing the Workflow Type

## [Workflow types](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization/workflow-type#workflow-types)

Step Functions offers two types of workflows — Standard and Express. Standard workflows are ideal for long running workflow; it can run for 365 day whereas express workflows can run only for 5 minutes. Another important distinction is how pricing works. Standard workflows are priced by state transition while express workflows are priced by number of request and the duration. To learn more about the difference, [click here ](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-standard-vs-express.html).

When you use distributed map, Step Functions spins up child workflows to run the states inside the distributed map. The number of child workflows to spin up is dependent on the number of objects or records to process, batch size and concurrency. You can define the child workflow to run as either standard or express based on your use case.

In the following sections, you will learn how to change workflow type of distributed map child workflows, try out a technique to find if express workflow suits your use case, and the cost impact of running standard vs express.

## [Workflow studio](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization/workflow-type#workflow-studio)

Navigate to [Step Functions Console ](https://console.aws.amazon.com/states/home), select State machines from the right menu.

Select the workflow that starts with **OptimizationStateMachine**.

Choose **edit** button to edit the workflow in workflow studio.

Review the definition in the workflow studio by enabling **definition** at the right.

Highlight the **Distributed map high precipitation** step in the workflow graphic.

![](https://cdn-images-1.medium.com/max/2000/0*MZLWQd02fjWQHovM.png)

Check the child workflow **ExecutionType**. It is set as **STANDARD**.

          "ItemProcessor": {
            "ProcessorConfig": {
              "Mode": "DISTRIBUTED",
              "ExecutionType": "STANDARD"
            },

Also, explore the batch setting. Each child workflow receives a batch of 100 objects.

          "ItemBatcher": {
            "MaxItemsPerBatch": 100
          },

## [Identify if workflow can be Express](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization/workflow-type#identify-if-workflow-can-be-express)

Child workflow can be run either STANDARD or EXPRESS. Express workflows are generally less expensive and run faster than Standard workflows.

Sometimes, you may not be sure if your workflow runs within 5 minutes. In this section, you are going to use a feature of distributed map that allows you to test your data with small number of items. This technique is helpful in couple of ways

* To determine the duration of the child workflow

* To gain confidence that the child workflow logic will run fine when running with full data set.

Start making the changes

* Toggle **Definition** button to edit the configuration.

* Expand **Additional configuration** and select **Limit number of items**.

* Type 1 in **Max Items** textbox.

![](https://cdn-images-1.medium.com/max/2000/0*xeFJeHHdQE2T3jZC.png)

* Save and execute with default input.

* Select **Map Run** from the execution page.

![](https://cdn-images-1.medium.com/max/2240/0*7wQeLjdQtDsS1YR8.png)

* Observe the duration for single item. It is around 3 seconds.

![](https://cdn-images-1.medium.com/max/2428/0*rTwycWYRKe9Pdh6q.png)

* Repeat the above steps with 100 items.

* Explore the **Map Run** page to find the duration for 100 items. It is less than 30 seconds.

* Now you know it takes 3 seconds to run 1 item and 26 seconds for 100 items, you can even run all the 1000 items in a single child workflow with 1 concurrency. But, you don’t utilize any parallelism to speed up the process.

This simple test is really handy in finding the right batch size and choosing the workflow type without running the entire dataset!!!

[Changing the workflow type to Express](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization/workflow-type#changing-the-workflow-type-to-express)

* Return to workflow studio.

* Change the workflow type to **EXPRESS**.

![](https://cdn-images-1.medium.com/max/2000/0*3BPjKGVfj-dvcAP7.png)

* Unselect **Limit number of items** under **Additional configuration** to revert back to the setting’s pre-test state.

* Save the workflow and run.

* Explore the **Map Run** page and note down the duration.

If you now run this workflow as a Standard workflow type and compare the durations, you will notice the Express workflow execution was faster.

## [Review Cost impact](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization/workflow-type#review-cost-impact)

Consider you are processing 500K objects and set the batch size to 500.

500k objects / 500 objects per workflow = **1000** child workflows

Distributed map runs a total of 1000 child workflows to process 500K objects.

## [Standard child workflow execution cost](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization/workflow-type#standard-child-workflow-execution-cost)

In the example we used in this module, we have one Lambda function inside the child workflow. so, the number of state transition per child workflow is 2:- one transition for starting the child workflow and one for Lambda function.

Total cost = (number of transitions per execution x number of executions) x $0.000025

**Total cost** = (2 * 1000) x $0.000025 = **$0.05**

Let’s assume you have 5 steps inside your child workflow, the number of state transitions per child workflow is 6.

Total cost = (number of transitions per execution x number of executions) x $0.000025

**Total cost** = (6 * 1000) x $0.000025 = **$0.15**

Let’s take another scenario. You have 2 steps inside your child workflow, the number of state transitions per child workflow is 3. Let’s assume you can not utilize batching, the number of child workflows to complete the work = 500K

Total cost = (number of transitions per execution x number of executions) x $0.000025

**Total cost** = (3 * 500K) x $0.000025 = **$37.4**

## [Express child workflow execution cost](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization/workflow-type#express-child-workflow-execution-cost)

With Express workflows, you pay for the number of requests for your workflow and the duration. With scenario outlined earlier under Review cost impact, we need additional dimension of how long workflow runs to calculate the express workflow cost. Let's assume express child workflow runs for an average of 100sec to process 500 objects using 64-MB memory.

**Duration cost** = (Avg billed duration ms / 100) * 0.0000001042

**Duration cost** = (100,000 MS /100) * $ 0.0000001042 = $0.0001042

**Express request cost** = $0.000001 per request ($1.00 per 1M requests)

**workflow cost** = (Express request cost + Duration cost) x Number of Requests

**workflow cost** = ($0.000001 + $0.0001042) x 1000 = **$0.10**

Express child workflow introduces 1 state transition per child workflow regardless of how many states you have inside the workflow. This is to start each child execution.

**Transition cost** = (1 * 1000) x $0.000025 = **$0.025**

**Total cost** = $0.10 + $0.025 = **$0.125**

If we repeat the calculation for an express workflow that runs for 30 seconds, the total cost = **$0.057**

If we repeat the calculation for an express workflow that runs for 1 second to process 1 object because you cannot utilize batching, the total cost = **$13.42**

**What did you observe?**

Express workflows are cheaper when the duration is lesser. They are also cost effective if there are more steps in the child workflow or your distributed map cannot make use of batching. Remember, Standard workflows are priced by state transitions meaning when number of steps and number of child workflow executions increase, cost increases.

You can look at from the below chart how express workflow duration affect the cost.

![](https://cdn-images-1.medium.com/max/4000/0*cmBFqFhnEUna8N37.png)

## Balancing Cost and Performance Using Concurrency and Batching

Higher parallelism generally means you can run the workflows faster. Higher parallelism with no or sub optimal batching results in higher cost for couple of reasons;

 1. There is more state transitions for standard workflows and request cost for express workflows

 2. Services you use inside the child workflow might have cost for requests.

Higher parallelism also causes scaling bottlenecks for downstream services inside the child workflow. You can use distributed map concurrency control to control the number of parallel workflow. If you have multiple workflows and need to manage downstream scaling then you can use techniques such as [queueing ](https://docs.aws.amazon.com/step-functions/latest/dg/connect-sqs.html)and [activities ](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-activities.html).

In the following sections, you will run a few experiments with batch size and understand the performance and cost impact of batch size between standard and express workflows.

## [Review the workflow](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization/batching#review-the-workflow)

Navigate to [Step Functions Console ](https://console.aws.amazon.com/states/home), select State machines from the right menu.

Select the workflow that starts with **OptimizationStateMachine**.

Choose **edit** to edit the workflow in workflow studio.

Review the definition in the workflow studio by enabling **definition** at the right.

Highlight the **Distributed map high precipitation** step in the workflow graphic.

![](https://cdn-images-1.medium.com/max/2000/0*6T6U-mI0ODHO9o1O.png)

Each child workflow receives a batch of 100 objects. The concurrency or the parallelism of workflow is set as 1000.

          "ItemBatcher": {
            "MaxItemsPerBatch": 100
          },
          "MaxConcurrency": 1000,

## [Run the workflow](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization/batching#run-the-workflow)

* Execute the workflow with default input.

* Select **Map Run** from the Execution page

* Examine the map run results.

* Note down the duration. You can see there is 10 child workflow executions. As there are only 1000 objects, with the batch setting of 100, only 10 parallel workflows are triggered.

* Close all the tabs

## [Change batch setting](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization/batching#change-batch-setting)

* Navigate to [Step Functions Console ](https://console.aws.amazon.com/states/home), select State machines from the right menu.

* Select the workflow that starts with **OptimizationStateMachine**.

* Choose **edit** to edit the workflow in workflow studio.

* Highlight the Distributed map high precipitation step and view the configurations

* Modify the **MaxItemsPerBatch** to 1

![](https://cdn-images-1.medium.com/max/2000/0*SwKfS7YTf5vN2xL5.png)

* Save and Execute the workflow with default input

* Explore the map run results. You can see 1000 child workflow executions.

* All the child workflows are completed in little under 25 seconds

Repeat the exercise with different batch settings. What did you observe?

Yes, The total duration increases when you increase the batch size since a single Lambda is looping through an array passed to it, thus increasing the duration of Lambda execution.

## [Review the cost impact](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization/batching#review-the-cost-impact)

You are now going to review the impact of cost with different batch sizes. Assume your workflow is processing 50M S3 objects. Now, let’s look at the cost of distributed map if you define the execution type as standard against express.

## [Standard child workflow](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization/batching#standard-child-workflow)

Assume you need to process 50M objects. You have 2 steps inside your child workflow. Each child workflow processes 100 objects in a batch. The number of state transition per child workflow is 3. Total number of child workflows to process 50M objects is 500,000.

**Total cost** = (number of transitions per execution x number of executions) x $0.000025

Total number of child workflows to process 50M objects = **500,000**

**Total cost** = (3 * 500000) x $0.000025 = **$37.5**

## [Express vs Standard vs Batch size](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization/batching#express-vs-standard-vs-batch-size)

![](https://cdn-images-1.medium.com/max/2000/1*Am5p6iysJMoleUjGx9HEZw.png)

Here is a visual representation of the same information

![](https://cdn-images-1.medium.com/max/2760/0*XRuYGqz9V1vzZRm8.png)

What did you observe?

Important

The degree of parallelism is determined by the Concurrency setting in Distributed Map, which determines the maximum number of parallel child workflows you want to execute at once. A key consideration here is the service quotas of the AWS services called in your child workflow. For example AWS Lambda in most large regions has a default concurrency quota of 1500 and a default burst limit of 3000, other services such as AWS Rekognition or AWS Textract have much lower default quotas.

The other thing to keep in mind is any performance limitations of other systems that your child workflow interacts with. An example here would be an on-prem relational database that Lambda within a child workflow connects to. This database might have a limit to the number of connections it can support, so you would need to limit your concurrency accordingly. Once you identify all of the AWS Service quotas and any additional concurrency limitation you’ll want to test various combinations of batch size and concurrency to find the best performance within your concurrency constraints

Review the documentation:

* [Lambda performance optimization](https://aws.amazon.com/blogs/compute/operating-lambda-performance-optimization-part-1/)

## Module 3 — Use Cases

* [HealthCare Claims Processing](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/healthcare-claims-processing)
>  In the US healthcare system, claims are typically categorized as professional, institutional, or dental claims when they are submitted to health insurance payers. Health plans are responsible for validating these claims, responding to the provider, assessing the claims, making payments to the provider, and providing an explanation of benefits to the member. In this module, we focus on the validation phase of the claims process, which occurs after the claims data has already been converted to comply with the FHIR specification. During the validation phase, various business rules are applied to validate and enrich the claims. This represents the final step in the incoming flow of claims before they are transformed into custom data formats required by backend claims adjudication systems.

* [Vulnerability Scanning](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/security-vulnerability-scanning)
>  Discovering and reporting vulnerabilities and security issues by scanning documents is a common process. If you are a security partner, when a new customer is onboarded, there can be hundreds of thousands of files to scan. Similarly, if a security procedure changes, previously scanned files may need to be rescanned. Scanning a large number of files is a both time consuming and expensive process. In this module, you will learn to scale a vulnerability scanning application to quickly and efficiently handle hundreds of thousands of files.

* [Monte Carlo Simulation](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/monte-carlo-simulation)
>  A Monte Carlo simulation is a mathematical technique that allows us to predict different outcomes for various changes to a given system. In financial portfolio analysis the technique can be used to predict likely outcomes for aggregate portfolio across a range of potential conditions such as aggregate rate of return or default rate in various market conditions. The technique is also valuable in scenarios where your business case requires predicting the likely outcome of individual portfolio assets such detailed portfolio analysis or stress tests.

## Healthcare Claims Processing

**Estimated Duration: 30 minutes**

## [Introduction](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/healthcare-claims-processing#introduction)

In the US healthcare system, claims are typically categorized as professional, institutional, or dental claims when they are submitted to health insurance payers. Health plans are responsible for validating these claims, responding to the provider, assessing the claims, making payments to the provider, and providing an explanation of benefits to the member. In this module, we focus on the validation phase of the claims process, which occurs after the claims data has already been converted to comply with the FHIR specification. During the validation phase, various business rules are applied to validate and enrich the claims. This represents the final step in the incoming flow of claims before they are transformed into custom data formats required by backend claims adjudication systems.

You will build a Step Functions workflow that processes healthcare claims data in a highly parallel fashion. The workflow uses the Distributed Map state that runs multiple child workflows, each processing a batch of the overall claims data. Each child workflow picks a set of individual claims files and processes them using [AWS Lambda ](https://aws.amazon.com/lambda/)functions that load the data to an [Amazon DynamoDB ](https://aws.amazon.com/dynamodb/)table and then apply rules to determine validity of the claims. Upon processing the claims, the functions returns the output back to the workflow.

![](https://cdn-images-1.medium.com/max/4000/0*E1u5bqydAW3m2s9y.png)

## [What you will accomplish](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/healthcare-claims-processing#what-you-will-accomplish)

* Learn how to use and configure a Distributed Map state for Healthcare claims data processing

* Analyze the results of Distributed Map run

* Challenge yourself to optimize the solution

## [Services in this module](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/healthcare-claims-processing#services-in-this-module)

* [Amazon S3 ](https://aws.amazon.com/s3/)— Object storage built to retrieve any amount of data from anywhere

* [AWS Step Functions ](https://aws.amazon.com/step-functions/)— Visual workflows for distributed applications

* [AWS Lambda ](https://aws.amazon.com/lambda/)— Serverless compute service; Run code without thinking about servers or clusters

* [Amazon DynamoDB ](https://aws.amazon.com/dynamodb/)— Fully managed, serverless, key-value NoSQL database designed to run high-performance applications

## [What’s included in this module](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/healthcare-claims-processing#what's-included-in-this-module)

Data processing code in the following [AWS Lambda ](https://aws.amazon.com/lambda/)functions:

* **DMapHealthCareClaimProcessingFunction**: This function reads a claims file and stores data in an [Amazon DynamoDB ](https://aws.amazon.com/dynamodb/)table (*DMapHealthCareClaimTable*).

* **DMapHealthCareRuleEngineLambdaFunction**: This function reads data from the [Amazon DynamoDB ](https://aws.amazon.com/dynamodb//)table and applies rules to determine whether the claims need to be accepted or rejected and returns the output. If it is rejected, it will return the reason as well.

## Exploring the Dataset

Navigate to the S3 Bucket. Search for **dmapworkshophealthcare** bucket. This bucket contains 1026 JSON files with around 60,000 records, with a total size of 270MB. These JSON files are generated using [Synthea Health Library ](https://github.com/synthetichealth/synthea)to simulate patient claims data in [FHIR ](https://www.hl7.org/fhir/overview.html)format.

The excerpt below shows one such daily claim record.

Each claim has a claim coding, the service dates, the provider details, the procedure details, an item-wise bill, the total amount and any additional information required for processing.

    {
            "resourceType": "Claim",
            "id": "d6c1872c-9b97-0fc1-161a-5c2c9b3f54bf",
            "status": "active",
            "type": {
                "coding": [
                    {
                        "system": "http://terminology.hl7.org/CodeSystem/claim-type",
                        "code": "institutional"
                    }
                ]
            },
            "use": "claim",
            "patient": {
                "reference": "urn:uuid:86ad439b-3df7-8882-2458-af0d0743d12b",
                "display": "Alvin56 Zulauf375"
            },
            "billablePeriod": {
                "start": "2013-09-09T11:01:44+00:00",
                "end": "2013-09-09T12:01:44+00:00"
            },
            "created": "2013-09-09T12:01:44+00:00",
            "provider": {
                "reference": "Organization?identifier=https://github.com/synthetichealth/synthea|5c896155-eb9a-383e-9162-a43ebb7f1cc5",
                "display": "LINDEN PONDS"
            },
            "priority": {
                "coding": [
                    {
                        "system": "http://terminology.hl7.org/CodeSystem/processpriority",
                        "code": "normal"
                    }
                ]
            },
            "facility": {
                "reference": "Location?identifier=https://github.com/synthetichealth/synthea|de4402eb-c9e7-3723-9584-345f665c5f5c",
                "display": "LINDEN PONDS"
            },
            "procedure": [
                {
                    "sequence": 1,
                    "procedureReference": {
                        "reference": "urn:uuid:ece5738e-95ce-4dc6-1df0-95f4dcccce9d"
                    }
                }
            ],
            "insurance": [
                {
                    "sequence": 1,
                    "focal": true,
                    "coverage": {
                        "display": "Medicaid"
                    }
                }
            ],
            "item": [
                {
                    "sequence": 1,
                    "productOrService": {
                        "coding": [
                            {
                                "system": "http://snomed.info/sct",
                                "code": "182813001",
                                "display": "Emergency treatment (procedure)"
                            }
                        ],
                        "text": "Emergency treatment (procedure)"
                    },
                    "encounter": [
                        {
                            "reference": "urn:uuid:c6f74fed-bdb9-6f50-29c5-3519fd948936"
                        }
                    ]
                },
                {
                    "sequence": 2,
                    "procedureSequence": [
                        1
                    ],
                    "productOrService": {
                        "coding": [
                            {
                                "system": "http://snomed.info/sct",
                                "code": "65546002",
                                "display": "Extraction of wisdom tooth"
                            }
                        ],
                        "text": "Extraction of wisdom tooth"
                    },
                    "net": {
                        "value": 14150.92,
                        "currency": "USD"
                    }
                }
            ],
            "total": {
                "value": 14297.1,
                "currency": "USD"
            }
        }

In the next step, you will build a workflow with a Distributed Map state to analyze this dataset.

## Creating the Data Analysis Workflow

In this step, you’ll create a workflow in Workflow Studio to analyze the Healthcare claims data using a Distributed Map state.

## [Creating the Workflow](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/healthcare-claims-processing/step-3#creating-the-workflow)

 1. Navigate to [AWS Step Functions ](https://console.aws.amazon.com/states/home)in your AWS console. Make sure you are in the correct region.

 2. If you are not on the State machines page, choose State machines on the left side hamburger menu icon and then select **Create state machine**

 3. On the Choose a template overlay, choose the **Blank** template, and select **Select**.

![](https://cdn-images-1.medium.com/max/3608/0*zn87drkOoeWjpyeX.png)

 1. Select patterns tab and drag **Process S3 objects** onto the Workflow Studio canvas.

![](https://cdn-images-1.medium.com/max/4000/0*9LW4dQzKidw6UyG5.png)

 1. Configure the Distributed Map state with the following values:

![](https://cdn-images-1.medium.com/max/2536/1*eZg4u0uwKu2_FzzQdKRE8w.png)

![](https://cdn-images-1.medium.com/max/4000/0*xCcd_T9_J7FAhlNz.png)

 1. Select the **Lambda Invoke** state within the Distributed Map state. Configure the state with the following values.

![](https://cdn-images-1.medium.com/max/2000/1*KvIiGh8_rtWV49CxCtwxqQ.png)

![](https://cdn-images-1.medium.com/max/4000/0*ePBxJW50GP-olKWw.png)

 1. Search for **AWS Lambda** and drag the *Invoke* state onto the canvas within the Distributed Map under the existing Lambda state. Configure the state with the following values:

![](https://cdn-images-1.medium.com/max/2000/1*6kdQKiYT17RCN-WdFHaljw.png)

![](https://cdn-images-1.medium.com/max/4000/0*eug2wSFefS7GR3Qf.png)

 1. Select the **Config** tab next to the state machine name at the top of the page and Edit the State machine name: HealthCareClaimProcessingStateMachine

If you don’t use this exact name, you may receive an IAM error in the next step.

![](https://cdn-images-1.medium.com/max/2000/0*Fs2pqU23W1tkLYx2.png)

![](https://cdn-images-1.medium.com/max/4000/0*apH9V5ci8_OdoBGo.png)

 1. For the Execution role, choose an existing role containing HealthCareClaimProcessingStateMachineRole in its name

 2. Leave the rest of the defaults and select **Create**.

In the next step, you’ll execute the workflow and view the results of the data processing job.

## Executing the Workflow and Viewing the Results

## [View map run](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/healthcare-claims-processing/step-4#view-map-run)

 1. Select **Start execution** and use the default input payload.

 2. The execution will take up to 5 minutes to complete successfully.

 3. In the execution details page, select Distributed Map state in the Graph View, then select Details tab.

![](https://cdn-images-1.medium.com/max/4000/0*R2BHMoWlun1pzK0A.png)

 1. Select Map Run link to view details of the Distributed Map execution.

 2. This page provides a summary of the Distributed Map job.

![](https://cdn-images-1.medium.com/max/4000/0*SDX7tSazW060moAL.png)

* We can see that 21 child workflow executions completed successfully with 0 failures. Each child workflow processed 50 files.

* We can view the duration of each child workflow execution. You can see overlapping timestamps for the start and end times, indicating that the data was processed in parallel.

* If you select the execution name, you can use the Execution Input and output tab to view the input files for a child workflow execution and the execution output with details.

![](https://cdn-images-1.medium.com/max/4000/0*0juLgC9ByJhGmZ4R.png)

## [Verifying DynamoDB results](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/healthcare-claims-processing/step-4#verifying-dynamodb-results)

The Validate Claim function will apply the rules on the claims and store the claim status (**Approved** / **Rejected**) along with the rejected reason in the DynamoDB table **DMapHealthCareClaimTable**. You can use the gear icon on the right side of the screen to select which columns you want to view.

![](https://cdn-images-1.medium.com/max/4000/0*eNBLgaNSmMAnnxYp.png)

## [Summary](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/healthcare-claims-processing/step-4#summary)

You have come to the end of the Healthcare Claim Processing Module. In this module, you created a Workflow with distributed map, learnt some important attributes of distributed map definition and run the workflow yourself.

**Congratulations!** You used Distributed Map state to quickly process a large dataset using parallel processing.

## Extra Credits

Great! You have now executed and analyzed the results of the workflow. **Well done!**

But you cannot stop there! You will need to optimize for performance and cost!

Here is a list of things to try in order to understand the various handles you have at your disposal to optimize a workflow:

* Increase the concurrency limit to 1000 and execute it again. Does it change the duration of the execution?

* What happens if you decrease the Item Batching size to 25 and execute the workflow? What is the impact on duration as well as cost?

* What combination of concurrency limit and batching size would be optimal?

* What happens if you change the type of the workflow to ‘Express’ and execute it? What is the impact on cost? Would this workflow type work for any batching size of the provided data set?

Review what you learnt previously in this workshop on [concurrency and batching](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/advanced/optimization/batching)

## Security Vulnerability Scanning

**Estimated Duration: 30 minutes**

## [Introduction](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/security-vulnerability-scanning#introduction)

You are developing a security vulnerability scanning application that alerts you of sensitive information in plain text files. The application you have built executes a workflow that scans a single file for exposed social security numbers (SSNs) and, if one is detected, sends a message to a queue for further downstream processing.

![](https://cdn-images-1.medium.com/max/2000/0*FtjM5UeTa_RtrF5c.png)

You intended to scan files singly as they got uploaded to your S3 bucket, but, due to delays in development, you have accumulated a large backlog of unscanned, potentially vulnerable files in S3.

## [What you do in the module](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/security-vulnerability-scanning#what-you-do-in-the-module)

In this module, you will scale your security vulnerability scanning application using Step Functions Distributed Map to quickly address this backlog by processing multiple files concurrently.

![](https://cdn-images-1.medium.com/max/2000/0*93SakszSd7I-B4U2.png)

## [Services in this module](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/security-vulnerability-scanning#services-in-this-module)

* [Amazon S3 ](https://aws.amazon.com/s3/)— Object storage built to retrieve any amount of data from anywhere

* [AWS Step Functions ](https://aws.amazon.com/step-functions/)— Visual workflows for distributed applications

* [AWS Lambda ](https://aws.amazon.com/lambda/)— Run code without thinking about servers or clusters

* [Amazon SQS ](https://aws.amazon.com/sqs/)— Fully managed message queuing for microservices, distributed systems, and serverless applications

## Reviewing the Workflow

 1. Open the [Step Functions console ](https://us-east-1.console.aws.amazon.com/states/home).

 2. Select the state machine containing “**VulnerabilityScanningStateMachine**” in its name.

 3. Click **Edit** to review the design in Workflow Studio.

![](https://cdn-images-1.medium.com/max/2000/0*6JyBxdJSzmFL62uT.png)

 1. Select the Lambda state titled “**Scan**”.

 2. Under **API Parameters**, click **View function** to review the **Code**.

Reference

![](https://cdn-images-1.medium.com/max/2000/0*eMYus7G5gRzMWKHt.png)

 1. Return to Workflow Studio.

 2. Select the **Choice** state.

 3. Under **Choice Rules**, click the edit icons to expand the rule logic.

The state machine orchestrates a Lambda function which scans a single file and, if an exposed SSN is detected, sends an SQS message with the location of the SSN and its serial number (the last four numbers).

## Executing the Workflow

 1. Open the [S3 console ](https://s3.console.aws.amazon.com/s3/home).

 2. Click the name of the bucket containing “**vulnerabilitydatabucket**”.

 3. Copy both the full name of the bucket and the name of a file in that bucket.

 4. Return to Workflow Studio.

 5. Click **Execute**.

 6. Enter the following input, replacing [bucket name] and [file name] with the names you copied:

    {
        "detail": {
            "bucket": {
                "name": "[bucket name]"
            },
            "object": {
                "key": "[file name]"
            }
        }
    }

![](https://cdn-images-1.medium.com/max/2112/0*1drdWXgqhC0W3iMP.png)

 1. Click **Start execution** then wait a moment for the execution to succeed.

![](https://cdn-images-1.medium.com/max/2000/0*cufEutXJPE_IRlMd.png)

Your successful graph will vary depending on the file name you copied. Only executions that process a file with an exposed SSN will send a message to the queue.

## Scaling the Workflow with Distributed Map

In this section, you will add a Distributed Map state around the existing workflow to process multiple files concurrently.

 1. Click **Edit state machine**.

 2. Select the **Code** tab.

 3. Overwrite the JSON definition with the following:

    {
      "StartAt": "Map",
      "States": {
        "Map": {
          "Type": "Map",
          "ItemProcessor": {
            "ProcessorConfig": {
              "Mode": "DISTRIBUTED",
              "ExecutionType": "STANDARD"
            },
            "StartAt": "Scan",
            "States": {
              "Scan": {
                "Type": "Task",
                "Resource": "arn:aws:states:::lambda:invoke",
                "OutputPath": "$.Payload",
                "Parameters": {
                  "Payload.$": "$",
                  "FunctionName": ""
                },
                "Retry": [
                  {
                    "ErrorEquals": [
                      "Lambda.ServiceException",
                      "Lambda.AWSLambdaException",
                      "Lambda.SdkClientException",
                      "Lambda.TooManyRequestsException"
                    ],
                    "IntervalSeconds": 2,
                    "MaxAttempts": 6,
                    "BackoffRate": 2
                  }
                ],
                "Next": "Choice"
              },
              "Choice": {
                "Type": "Choice",
                "Choices": [
                  {
                    "Variable": "$.ssns",
                    "IsPresent": true,
                    "Next": "Queue"
                  }
                ],
                "Default": "Pass"
              },
              "Queue": {
                "Type": "Task",
                "Resource": "arn:aws:states:::sqs:sendMessage",
                "Parameters": {
                  "MessageBody.$": "$",
                  "QueueUrl": ""
                },
                "End": true
              },
              "Pass": {
                "Type": "Pass",
                "End": true
              }
            }
          },
          "End": true,
          "Label": "Map",
          "MaxConcurrency": 1000,
          "ItemReader": {
            "Resource": "arn:aws:states:::s3:listObjectsV2",
            "Parameters": {
              "Bucket.$": "$.bucket",
              "Prefix.$": "$.prefix"
            }
          }
        }
      }
    }

 1. Select the **Design** tab.

![](https://cdn-images-1.medium.com/max/2000/0*ru-9qp7ks4dZEJba.png)

 1. Select the Lambda state titled “**Scan**”.

 2. Under **API Parameters**, click on the **Enter function name** dropdown then select the name containing “**VulnerabilityScanning**”.

 3. Select the SQS state titled “**Queue**”.

 4. Under **API Parameters**, click on the **Enter queue URL** dropdown then select the URL containing “**VulnerabilitiesQueue**”.

 5. Select the **Map** state.

 6. Under **Item source**, expand **Additional configuration**.

Limiting the number of items that are sent to the Map state is useful for performing trial executions.

 1. Select **Limit number of items** then enter 1000 under **Max items**.

Modify items before they are passed on to child executions, selecting only the relevant items and adding input where needed with *ItemSelector*.

 1. Select **Modify Items with ItemSelector** then enter the following JSON:

    {
        "detail": {
            "bucket": {
                "name.$": "$.bucket"
            },
            "object": {
                "key.$": "$$.Map.Item.Value.Key"
            }
        }
    }

Express Workflows are ideal for quick, high-volume workloads like this. They can run for up to five minutes.

 1. Under **Child execution type**, choose **Express**.

 2. Select the **Code** tab again to review the JSON definition.

Your *FunctionName* and *QueueUrl* will be different.

    {
      "StartAt": "Map",
      "States": {
        "Map": {
          "Type": "Map",
          "ItemProcessor": {
            "ProcessorConfig": {
              "Mode": "DISTRIBUTED",
              "ExecutionType": "EXPRESS"
            },
            "StartAt": "Scan",
            "States": {
              "Scan": {
                "Type": "Task",
                "Resource": "arn:aws:states:::lambda:invoke",
                "OutputPath": "$.Payload",
                "Parameters": {
                  "Payload.$": "$",
                  "FunctionName": "arn:aws:lambda:us-east-1:172187416625:function:vulnerability-scanning-mo-VulnerabilityScanningFun-G786d6ZNTaPI:$LATEST"
                },
                "Retry": [
                  {
                    "ErrorEquals": [
                      "Lambda.ServiceException",
                      "Lambda.AWSLambdaException",
                      "Lambda.SdkClientException",
                      "Lambda.TooManyRequestsException"
                    ],
                    "IntervalSeconds": 2,
                    "MaxAttempts": 6,
                    "BackoffRate": 2
                  }
                ],
                "Next": "Choice"
              },
              "Choice": {
                "Type": "Choice",
                "Choices": [
                  {
                    "Variable": "$.ssns",
                    "IsPresent": true,
                    "Next": "Queue"
                  }
                ],
                "Default": "Pass"
              },
              "Queue": {
                "Type": "Task",
                "Resource": "arn:aws:states:::sqs:sendMessage",
                "Parameters": {
                  "MessageBody.$": "$",
                  "QueueUrl": "https://sqs.us-east-1.amazonaws.com/172187416625/vulnerability-scanning-module-VulnerabilitiesQueue-UtFIvCLDH5Lh"
                },
                "End": true
              },
              "Pass": {
                "Type": "Pass",
                "End": true
              }
            }
          },
          "End": true,
          "Label": "Map",
          "MaxConcurrency": 1000,
          "ItemReader": {
            "Resource": "arn:aws:states:::s3:listObjectsV2",
            "Parameters": {
              "Bucket.$": "$.bucket",
              "Prefix.$": "$.prefix"
            },
            "ReaderConfig": {
              "MaxItems": 1000
            }
          },
          "ItemSelector": {
            "detail": {
              "bucket": {
                "name.$": "$.bucket"
              },
              "object": {
                "key.$": "$$.Map.Item.Value.Key"
              }
            }
          }
        }
      }
    }

    }
    }

 1. If the changes to the JSON definition look accurate, click **Save**.

## Executing the Map Workflow

In this section, you will test last section’s saved workflow. You will execute the workflow with the necessary input, explore child workflow executions, and verify the outputs by polling messages in the SQS queue.

 1. Click **Execute**.

 2. Enter the following input, replacing [bucket name] with the bucket name you copied:

    {
        "bucket": "[bucket name]",
        "prefix": ""
    }

![](https://cdn-images-1.medium.com/max/2112/0*K2DRU5jrcb7ZrKtK.png)

 1. Click **Start execution**.

![](https://cdn-images-1.medium.com/max/2000/0*VhLkEDWZv_UfXPOk.png)

 1. Under **Events**, click **Map Run**.

Because you added a limitation on the number of items processed, under **Executions**, there are only 1000 workflows running despite there being more files in S3.

 1. Click a given child execution.

 2. After a successful execution, select the **Execution input and output** tab.

![](https://cdn-images-1.medium.com/max/3100/0*J9Obr6vBsTHbt2PC.png)

Again, your output will vary depending on the file that was executed on. Executions that process a clean file with no exposed SSN will return an empty object.

 1. Return to the parent execution view.

![](https://cdn-images-1.medium.com/max/2000/0*WXXYZmanQ5eJbFMM.png)

The final graph view should look like this upon completion.

 1. Open the [SQS console ](https://us-east-1.console.aws.amazon.com/sqs/v2/home).

 2. Select the queue containing “**VulnerabilitiesQueue**” in its name.

 3. Click **Send and receive messages**.

 4. Click **Poll for messages**.

 5. Click a given message to view the **Body**.

The body of the message contains the location and serial number of the SSN(s).

 1. Click **Done**.

Notice the number of *Messages available* in the queue. You will now purge the queue in anticipation of the next full state machine execution.

 1. Click the queue name.

![](https://cdn-images-1.medium.com/max/2000/0*HzVEfI-yjfGXIUmu.png)

 1. Under the queue name, click **Purge**.

 2. Enter purge then click **Purge**.

## Optimizing the Workflow with Batching

In this section, you will modify your Step Functions workflow and Lambda function to enable batch processing, optimizing the performance and cost of your workflow.

## [Modify the Workflow](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/security-vulnerability-scanning/edit-workflow-2#modify-the-workflow)

 1. Return to the Step Functions execution page.

 2. Click **Edit state machine**.

 3. Select the **Map** state.

You can optimize the performance and cost of your workflow by selecting a batch size that balances the number of items against the items processing time. If you use batching, Step Functions adds the items to an Items array. It then passes the array as input to each child workflow execution.

 1. Under **Item batching**, select **Enable batching**.

 2. Set the **Max MBs per batch** to 50 KBs.

With batch input, you can pass a global JSON input to each child execution, merged with the inputs for items. In the last section, the bucket name was included in every single item, increasing the total input size. Instead of including the bucket name repeatedly with ItemSelector, you will include it once in the batch input.

 1. Enter the following **Batch input**:

    {
      "bucket.$": "$.bucket"
    }    

 1. Under **Item source**, expand **Additional configuration**.

 2. Under **Modify items with ItemSelector**, overwrite the JSON with the following, removing details of the bucket:

    {
      "key.$": "$$.Map.Item.Value.Key"
    }    

 1. Unselect **Limit number of items** to process all your files.

*Express* Workflows only run for up to five minutes and batch processing increases the number of files processed per child execution. If you configure a sufficiently large batch size, you may need to use a *Standard* Workflow.

 1. Under **Child execution type**, select **Standard**.

 2. Select the **Code** tab to review the JSON definition.

Your *FunctionName* and *QueueUrl* will be different.

    {
      "StartAt": "Map",
      "States": {
        "Map": {
          "Type": "Map",
          "ItemProcessor": {
            "ProcessorConfig": {
              "Mode": "DISTRIBUTED",
              "ExecutionType": "STANDARD"
            },
            "StartAt": "Scan",
            "States": {
              "Scan": {
                "Type": "Task",
                "Resource": "arn:aws:states:::lambda:invoke",
                "OutputPath": "$.Payload",
                "Parameters": {
                  "Payload.$": "$",
                  "FunctionName": "arn:aws:lambda:us-east-1:172187416625:function:vulnerability-scanning-mo-VulnerabilityScanningFun-G786d6ZNTaPI:$LATEST"
                },
                "Retry": [
                  {
                    "ErrorEquals": [
                      "Lambda.ServiceException",
                      "Lambda.AWSLambdaException",
                      "Lambda.SdkClientException",
                      "Lambda.TooManyRequestsException"
                    ],
                    "IntervalSeconds": 2,
                    "MaxAttempts": 6,
                    "BackoffRate": 2
                  }
                ],
                "Next": "Choice"
              },
              "Choice": {
                "Type": "Choice",
                "Choices": [
                  {
                    "Variable": "$.ssns",
                    "IsPresent": true,
                    "Next": "Queue"
                  }
                ],
                "Default": "Pass"
              },
              "Queue": {
                "Type": "Task",
                "Resource": "arn:aws:states:::sqs:sendMessage",
                "Parameters": {
                  "MessageBody.$": "$",
                  "QueueUrl": "https://sqs.us-east-1.amazonaws.com/172187416625/vulnerability-scanning-module-VulnerabilitiesQueue-UtFIvCLDH5Lh"
                },
                "End": true
              },
              "Pass": {
                "Type": "Pass",
                "End": true
              }
            }
          },
          "End": true,
          "Label": "Map",
          "MaxConcurrency": 1000,
          "ItemReader": {
            "Resource": "arn:aws:states:::s3:listObjectsV2",
            "Parameters": {
              "Bucket.$": "$.bucket",
              "Prefix.$": "$.prefix"
            },
            "ReaderConfig": {}
          },
          "ItemSelector": {
            "key.$": "$$.Map.Item.Value.Key"
          },
          "ItemBatcher": {
            "MaxInputBytesPerBatch": 51200,
            "BatchInput": {
              "bucket.$": "$.bucket"
            }
          }
        }
      }
    }

 1. If the changes to the JSON definition look accurate, click **Save**.

## [Modify the Lambda Function](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/security-vulnerability-scanning/edit-workflow-2#modify-the-lambda-function)

Since you have enabled batching and changed the input to each child execution, you will need to refactor your Lambda function code to read in the bucket name from BatchInput and process multiple files.

 1. Select the **Design** tab.

 2. Select the Lambda state titled “**Scan**”.

 3. Under **API Parameters**, click **View function** to edit the **Code**.

![](https://cdn-images-1.medium.com/max/2000/0*KNhlR1AASypKPHkU.png)

 1. Overwrite the code with the following:

    import json
    import boto3
    import re
    
    def handler(event, context):
      
      bucket = event["BatchInput"]["bucket"]
    
      ssns = []
      for item in event["Items"]:
          
        key = item["key"]
        
        obj = boto3.client('s3').get_object(
          Bucket=bucket, 
          Key=key
        )
        body = obj['Body'].read().decode()
        
        searches = re.findall("ssn=[^\s]+", body)
        if searches:
          ssns.extend([{"key": key, "serial": number[-4:]} 
            for ssn, number in (search.split("=") for search in searches)
        ])
    
      if ssns:
        return {"ssns": ssns}
      else:
        return {}

 1. Click **Deploy**.

## Executing the Batch Workflow

 1. Return to Workflow Studio.

 2. Click **Execute**.

 3. Enter the following input, replacing [bucket name] with the bucket name you copied::

    {
        "bucket": "[bucket name]",
        "prefix": ""
    }

![](https://cdn-images-1.medium.com/max/2112/0*PliJjnx-g9wIHvb3.png)

 1. Click **Start execution**.

![](https://cdn-images-1.medium.com/max/2000/0*SOPPk_I_QAC7V3Kl.png)

 1. Under **Events**, click **Map Run**.

 2. Wait a moment, if necessary, then click a given child execution.

![](https://cdn-images-1.medium.com/max/2000/0*DG7W-d4rvj5SFPKy.png)

 1. Under **Events**, expand the **ID** 3 dropdown to view the new input to the Lambda function titled “Payload”.

![](https://cdn-images-1.medium.com/max/2000/0*fCtyPgvPb89puJlT.png)

 1. After a successful invocation, expand the **ID** 6 dropdown to see the output of the Lambda function.

 2. Return to the parent execution view.

![](https://cdn-images-1.medium.com/max/2000/0*9NXUdlaWyhi8R_Su.png)

The final graph view should look like this upon completion.

 1. Open the [SQS console ](https://us-east-1.console.aws.amazon.com/sqs/v2/home).

 2. Click the refresh icon, if necessary.

Notice the number of *Messages available* in the queue. Since you removed the limitation on the number of items processed, it is much higher than before.

## Summary

In this module, you added a Distributed Map state to your workflow to scale your security vulnerability scanning application across multiple files concurrently. By refactoring your application and enabling batch processing, you further optimized the performance and cost of your workflow.

![](https://cdn-images-1.medium.com/max/2000/0*O3yAXqX0g668rMaz.png)

Beyond security vulnerability scanning, AWS Step Functions can be used for other data processing use cases, from typical cleaning and normalization to healthcare claims processing.

## Monte Carlo Simulation

**Estimated Duration: 30 minutes**

## [Introduction](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/monte-carlo-simulation#introduction)

A Monte Carlo simulation is a mathematical technique that allows us to predict different outcomes for various changes to a given system. In financial portfolio analysis the technique can be used to predict likely outcomes for aggregate portfolio across a range of potential conditions such as aggregate rate of return or default rate in various market conditions. The technique is also valuable in scenarios where your business case requires predicting the likely outcome of individual portfolio assets such detailed portfolio analysis or stress tests.

For this fictitious use case we will be working with a portfolio of personal and commercial loans owned by our company. Each loan is represented by a subset of data housed in individual S3 objects. Our company has tasked us with trying to predict which loans will default in the event of a Federal Reserve rate increase.

Loan defaults occur when the borrower fails to repay the loan. Predicting which loans in a portfolio would default in various scenarios helps companies understand their risk and plan for future events.

## [What is a Worker?](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/monte-carlo-simulation#what-is-a-worker)

In this solution we are distributing the data using a Step Functions [Activity ](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-activities.html). Activities are an AWS Step Functions feature that enables you to have a task in your state machine where the work is performed by a worker that can be hosted on Amazon Elastic Compute Cloud (Amazon EC2), Amazon Elastic Container Service (Amazon ECS), mobile devices — basically anywhere. Think of activity as a Step Functions managed internal queue. You use an activity state to send data to the queue, one or more workers will consume the data from the queue. For this solution we utilize Amazon ECS on Amazon Fargate to run our Activity Workers (Worker).

## [Solution Overview](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/monte-carlo-simulation#solution-overview)

The solution uses [AWS Step Functions ](https://aws.amazon.com/step-functions/)to provides end to end orchestration for processing billions of records with your simulation or transformation logic using AWS Step Functions [Distributed Map ](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-asl-use-map-state-distributed.html)and [Activity ](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-activities.html)features. At the start of the workflow, Step Functions will scale the number of workers to a (configurable) predefined number. It then reads in the dataset and distributes metadata about the dataset in [batches ](https://docs.aws.amazon.com/step-functions/latest/dg/input-output-itembatcher.html)to the Activity. The workers are polling the Activity looking for data to process. Upon receiving a batch, the worker will process the data and report back to Step Functions that the batch has been completed. This cycle continues until all records from the dataset have been processed. Upon completion, Step Functions will scale the workers back to zero.

The workers in this example are containers, running in [Amazon Elastic Container Service (ECS) ](https://aws.amazon.com/ecs/)with an [Amazon Fargate ](https://aws.amazon.com/fargate/)[Capacity Provider ](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cluster-capacity-providers.html). Though the workers could potentially run almost anywhere so long as they had access to poll the Step Functions Activity and report SUCCESS/FAILURE back to Step Functions.

![](https://cdn-images-1.medium.com/max/3064/0*LnOE-CWxe1Ht5UKM.png)

## [Module Goals](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/monte-carlo-simulation#module-goals)

* Learn how Step Functions Distributed Map can use a Step Functions Activity to distribute work to workers almost anywhere

* Learn how Step Functions can manage workers through its built-in Amazon ECS integrations

## Updating the ECS Service — Part 1

The solution uses Amazon ECS to run the workers that handle the actual data processing. In this example we have created an ECS Service that will run a variable number of ECS Tasks, controlled by our Step Functions workflow. The workers run asynchronously from the Distributed Map, which uses an Activity to distribute the dataset. In this step you will configure that ECS Service to use a Task Definition that was predefined in CloudFormation. Let’s get started.

Important

A task definition is a blueprint for your application. It is a text file in JSON format that describes the parameters and one or more containers that form your application. You can learn more [here](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html)

 1. Navigate to the [ECS Console Page](https://console.aws.amazon.com/ecs/home)

 2. Choose the ECS Cluster named “sfn-fargate-dataproc-xxxxxxxx” (note the similar Cluster named sfn-fargate-datagen-xxxxxxxx, please choose the one ending in dataproc)

 3. Choose ECS Cluster

![](https://cdn-images-1.medium.com/max/4000/0*a6jYTSzENMvEZ7bf.png)

 1. Choose the ECS Service named “sfn-fargate-dataproc-xxxxxxxx”

 2. Choose ECS Service

![](https://cdn-images-1.medium.com/max/4000/0*MpqgAWpZAq4PLeSb.png)

 1. Click the Update Service button in the top right of the ECS Service page

 2. Update Service

![](https://cdn-images-1.medium.com/max/4000/0*OnXHXXudzxbCw4PM.png)

 1. In the field “Family”, click the Dropdown menu and change the current Task Definition, sfn-fargate-dataproc-placeholder-xxxxxxxx, and choose the Task Definition named sfn-fargate-dataproc-*small*-xxxxxxxx

 2. Choose Task Definition

![](https://cdn-images-1.medium.com/max/2000/0*77vBG4hWb0sqcdF8.png)

 1. Leave all other fields default. Scroll to the bottom of the page and click Update.

That’s it! You have successfully updated your ECS Service to use a new Task Definition. Now lets run our Step Functions State Machine.

## Executing the Workflow

**Let’s go ahead and run the Step Function and then we will walk through each step as well as some optimizations for you to consider….**

 1. In your AWS Console navigate to the AWS Step Functions Console by using the search bar in the upper left corner of your screen, typing “step functions” and clicking the Step Functions icon.

Console Navigation

![](https://cdn-images-1.medium.com/max/2132/0*VnWTIrZNkZ01jYJo.png)

 1. Open the “sfn-fargate-dataproc-xxxxxxxxxxxx” Step Function by clicking on the Link and then click the “Start Execution” button in the upper right-hand corner of the details screen.

Step Function Selection

![](https://cdn-images-1.medium.com/max/2140/0*5P0_BuT2I8s6nEeY.png)

 1. Click the “**Start execution**” button to start the workflow.

Step Function Execution

![](https://cdn-images-1.medium.com/max/4000/0*aAtpjW6vkocN133D.png)

3a. You will be prompted for JSON input, leave default and click “**Start Execution**”

Step Function Execution — Take Defaults

![](https://cdn-images-1.medium.com/max/4000/0*y21gW4gCsxfXNM6_.png)

 1. We can then monitor the process of the Step Function State Machine process from the execution status screen. Processing of the records in the simulated dataset takes just a few minutes.

![](https://cdn-images-1.medium.com/max/2000/0*aCJwNdAdL8V0Ow3-.png)

Now, that will take about 5 minutes to complete. While it’s running, lets dive a little deeper into each step in the workflow.

## Reviewing the Workflow

## [Distributed Map](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/monte-carlo-simulation/reviewing-the-workflow#distributed-map)

The Processing DMap step is an AWS Step Functions Distributed Map step that reads the S3 Inventory manifest provided by the Parent Map and processes the referenced [S3 Inventory ](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-inventory.html)files. For this use case each line of the S3 Inventory file contains metadata referencing an object is S3 containing a single customer loan. The Distributed Map feature creates batches of 400 loan files per our configuration for concurrent distribution to the Step Functions Activity step. Each Distributed Map step supports up to ten thousand concurrent workers. For this example, the Runtime Concurrency is set to 1000. As Step Functions adds messages to the Activity, the workers are polling to pull batches for processing. Once complete, the worker reports back to Step Functions to acknowledge the batch has been completed. Step Functions removes that message from the Activity and adds a new batch, it will repeat this process until all batches have been completed.

Distributed Map Configuration Example

![](https://cdn-images-1.medium.com/max/2000/0*1gmEP1_hUaOlMBBp.png)

![](https://cdn-images-1.medium.com/max/2000/0*hQkzkL9JKTtRbpUr.png)

As Step Functions Distributed Map can scale to massive concurrency very quickly it is advisable to configure back off and retries within our process to allow downstream systems to scale to meet the processing needs. We utilize Step Functions Distributed Map retry feature to implement graceful back offs without any code. In this example we have configured retry logic for S3 bucket. Each new S3 bucket allocates a single throughput partition of 5,500 reads and 3,500 writes per second which auto-scales based on usage patterns. To allow S3 to auto-scale to meet our workloads write demands the we configure the base retry delay, maximum retires, and back-off within the step function.

Step Functions Retry Configuration

![](https://cdn-images-1.medium.com/max/3074/0*KdF5r6oiuxUq8D0k.png)

## [Amazon ECS Workers](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/monte-carlo-simulation/reviewing-the-workflow#amazon-ecs-workers)

The Amazon Elastic Container Service (ECS) Cluster is using a Fargate Spot [Capacity Provider ](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cluster-capacity-providers.html)to reduce costs and eliminate maintaining EC2 instances. Fargate provides us with AWS managed compute for scheduling our containers.

ECS Cluster / Capacity Provider

![](https://cdn-images-1.medium.com/max/4000/0*wv5LMNX1s6dzAj6Y.png)

The Task Definition in the solution is where we define resources required for our containers to complete. Resources such as network requirements, number of vCPU’s, amount of RAM, etc. In this solution, to avoid building a container, we’re simply using an unmodified Amazon Linux 2023 container but specifying a bootstrap sequence. Bootstrapping lets us give the container a series of commands to execute on start. When the container comes up, it will download a pre-generated python script from our S3 bucket and execute it. This python script has the sample logic we want to run against our dataset.

Task Definition

    {
        "taskDefinitionArn": "arn:aws:ecs:us-east-2:123456789101:task-definition/sfn-fargate-dataproc-06f7c24a2819:1",
        "containerDefinitions": [
            {
                "name": "sfn-fargate-dataproc-06f7c24a2819",
                "image": "public.ecr.aws/amazonlinux/amazonlinux:2023",
                "cpu": 256,
                "memory": 512,
                "links": [],
                "portMappings": [],
                "essential": true,
                "entryPoint": [],
                "command": [
                    "/bin/sh",
                    "-c",
                    "yum -y update && yum -y install awscli python3-pip && python3 -m pip install boto3 && aws s3 cp s3://${SOURCEBUCKET}/script/fargate.py . && python3 fargate.py"
                ],
                "environment": [
                    {
                        "name": "ACTIVITY_ARN",
                        "value": "arn:aws:states:us-east-2:123456789101:activity:sfn-fargate-activity-06f7c24a2819"
                    },
                    {
                        "name": "DESTINATIONBUCKET",
                        "value": "sfn-datagen-destination-060aa4adf045"
                    },
                    {
                        "name": "RECORDCOUNT",
                        "value": "105000"
                    },
                    {
                        "name": "SOURCEBUCKET",
                        "value": "sfn-datagen-source-060aa4adf045"
                    },
                    {
                        "name": "REGION",
                        "value": "us-east-2"
                    }
                ],
                "environmentFiles": [],
                "mountPoints": [],
                "volumesFrom": [],
                "secrets": [],
                "dnsServers": [],
                "dnsSearchDomains": [],
                "extraHosts": [],
                "dockerSecurityOptions": [],
                "dockerLabels": {},
                "ulimits": [],
                "logConfiguration": {
                    "logDriver": "awslogs",
                    "options": {
                        "awslogs-group": "sfn-fargate-dataproc-06f7c24a2819",
                        "awslogs-region": "us-east-2",
                        "awslogs-stream-prefix": "sfn-fargate"
                    },
                    "secretOptions": []
                },
                "systemControls": []
            }
        ],
        "family": "sfn-fargate-dataproc-06f7c24a2819",
        "taskRoleArn": "arn:aws:iam::123456789101:role/sfn-fargate-ecs-task-role-06f7c24a2819",
        "executionRoleArn": "arn:aws:iam::123456789101:role/sfn-fargate-ecs-exec-role-06f7c24a2819",
        "networkMode": "awsvpc",
        "revision": 1,
        "volumes": [],
        "status": "ACTIVE",
        "requiresAttributes": [
            {
                "name": "com.amazonaws.ecs.capability.logging-driver.awslogs"
            },
            {
                "name": "ecs.capability.execution-role-awslogs"
            },
            {
                "name": "com.amazonaws.ecs.capability.docker-remote-api.1.19"
            },
            {
                "name": "com.amazonaws.ecs.capability.docker-remote-api.1.17"
            },
            {
                "name": "com.amazonaws.ecs.capability.task-iam-role"
            },
            {
                "name": "com.amazonaws.ecs.capability.docker-remote-api.1.18"
            },
            {
                "name": "ecs.capability.task-eni"
            }
        ],
        "placementConstraints": [],
        "compatibilities": [
            "EC2",
            "FARGATE"
        ],
        "requiresCompatibilities": [
            "FARGATE"
        ],
        "cpu": "256",
        "memory": "512",
        "registeredAt": "2023-09-02T15:36:54.377Z",
        "registeredBy": "arn:aws:sts::123456789101:assumed-role/haughtmx-vscode-remote-ssh-role/i-00c5182010adb45a3",
        "tags": [
            {
                "key": "Name",
                "value": "sfn-fargate-dataproc-06f7c24a2819"
            }
        ]
    }

The tasks are managed with an ECS Service. A Service can allow for integration with other services such as Elastic Load Balancers, but in our case, we’re using it to manage how many tasks are running at any given time. In the “Scale Out Workers” step of the workflow we are using Step Functions built-in integration with ECS to scale out the Service to 50. This way, once Distributed Map begins filling the Activity with batches of work, the containers are already running and immediately begin picking up batches for processing.

ECS Service

![](https://cdn-images-1.medium.com/max/4000/0*7Ly1yPuIvuawF318.png)

## [Data Processing](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/monte-carlo-simulation/reviewing-the-workflow#data-processing)

The Run Activity step consists of a single Step Functions Activity which accepts a JSON payload from Distributed Map containing a batch of Loan objects stored in S3. The Run Activity step will continuously add/remove batches of loans by reading the contents of the S3 object. These batches are picked up by our workers and processed. After the worker reports that a batch was successfully processed, Step Functions will remove the batch from the Activity and adds a new one in its place, maintaining our concurrency limit of batches until all batches are processed. In this example the output files contain batched loans to facilitate more efficient reads for analytics and ML workloads.

Step Functions error handling features removes the need to implement catch and retry logic within the python code. If a batch fails processing or a worker fails to report the batch as complete, Step Functions will wait the allotted time and re-add the batch for another worker to pick up and process.

Example Processing Python Code

    import boto3
    import json
    import csv
    from io import StringIO
    import os
    import time
    from random import randint
    from botocore.client import Config
    
    # set a few variables we'll use to get our data
    activity_arn = os.getenv('ACTIVITY_ARN')
    worker_name = os.getenv('HOSTNAME')
    region = os.getenv('REGION')
    
    print('starting job...')
    
    # setup our client
    config = Config(
      connect_timeout=65,
      read_timeout=65,
      retries={'max_attempts': 0}
    )
    client = boto3.client('stepfunctions', region_name=region, config=config)
    s3_client = boto3.client('s3', region_name=region)
    s3 = boto3.resource('s3')
    
    # now we start polling until we have nothing left to do. i realize this should
    # be more functions and it's pretty gross but it works for a demo :) 
    while True:
      response = client.get_activity_task(
        activityArn = activity_arn,
        workerName = worker_name
      )
    
      if 'input' not in response.keys() or 'taskToken' not in response.keys():
        print('no tasks to process...waiting 30 seconds to try again')
        time.sleep(30)
        continue
        # break
    
      token = response['taskToken']
      data = json.loads(response['input'])
      items = data['Items']
      other = data['BatchInput']
      rndbkt = other['dstbkt'] 
      success = True
      cause = ""
      error = ""
      results = ["NO", "NO", "NO", "NO", "YES", "NO", "NO", "NO", "NO", "NO", "NO", "NO", "NO", "NO", "NO", "NO", "NO", "NO", "NO", "NO"]
      for item in items:
        try:
          source = s3_client.get_object(Bucket=other['srcbkt'], Key=item['Key'])
          content = source.get('Body').read().decode('utf-8')
          buf = StringIO(content)
          reader = csv.DictReader(buf)
          objects = list(reader)
    
          # just randomly assign a value with a theoretical ballpark of 5% of the values being 'YES'
          objects[0]['WillDefault'] = results[randint(0,19)]
    
          stream = StringIO()
          headers = list(objects[0].keys())
          writer = csv.DictWriter(stream, fieldnames=headers)
          writer.writeheader()
          writer.writerows(objects)
          body = stream.getvalue()
    
          dst = s3.Object(rndbkt, other['dstkey'] + '/' + item['Key'].split('/')[1])
          dst.put(Body=body)
    
        except Exception as e:
          cause = "failed to process object " + item['Key'],
          error = str(e)
          success = False
          break
      
      if success:
        client.send_task_success(
          taskToken = token,
          output = "{\"message\": \"success\"}"
        )
      else:
        client.send_task_failure(
          taskToken = token,
          cause = cause,
          error = error
        )

## Updating the ECS Service — Part 2

The update you made in the “Updating the ECS Service — part 1” step used a task definition with a relatively low resource setting, 0.25vcpu and 0.5GB of memory. In this step you will choose a Task Definition that uses the same container as the previous step, but with double the previous resources. Then you will re-run the State Machine and check the differences between the executions. Let’s get started.

 1. Navigate to the [ECS Console Page](https://console.aws.amazon.com/ecs/home)

 2. Choose the ECS Cluster named “sfn-fargate-dataproc-xxxxxxxx” (note the similar Cluster named sfn-fargate-datagen-xxxxxxxx, please choose the one ending in dataproc)

 3. Choose ECS Cluster

![](https://cdn-images-1.medium.com/max/4000/0*lYDTN8cphfM8Fzrd.png)

 1. Choose the ECS Service named “sfn-fargate-dataproc-xxxxxxxx”

 2. Choose ECS Service

![](https://cdn-images-1.medium.com/max/4000/0*0KeBGxLOUG9zV64N.png)

 1. Click the Update Service button in the top right of the ECS Service page

 2. Update Service

![](https://cdn-images-1.medium.com/max/4000/0*VIjMbf9kHULiwWIy.png)

 1. In the field “Family”, click the Dropdown menu and change the current Task Definition, sfn-fargate-dataproc-small-xxxxxxxx, and choose the Task Definition named sfn-fargate-dataproc-*large*-xxxxxxxx

 2. Choose Task Definition

![](https://cdn-images-1.medium.com/max/2000/0*y0riICY9lB3ZSp9M.png)

 1. Leave all other fields default. Scroll to the bottom of the page and click Update.

 2. Now that you have updated the Service, lets run the State Machine again. If you need instructions please refer back to [Execute The State Machine](https://catalog.us-east-1.prod.workshops.aws/workshops/2a22e604-2f2e-4d7b-85a8-33b38c999234/en-US/use-cases/monte-carlo-simulation/executing-the-workflow/)

Give that approximately 4 minutes to complete

## Reviewing the Results

In the first execution you used a Task Definition that allocated .25 vCPU’s and .5GB of RAM to each container. In the second execution you used a Task Definition that allocated .5 vCPU’s and 1GB of RAM to each container. Let’s check out the results and see how they differ.

 1. Go to the [Step Functions Console](https://console.aws.amazon.com/states/home)

 2. Choose the State Machine named “sfn-fargate-dataproc-xxxxxxxx”

 3. Step Function Selection

![](https://cdn-images-1.medium.com/max/4000/0*C3AIDbAMrZd3drDl.png)

 1. On the State Machine Details page you will see 2 executions, open each in a separate tab. (right-click each link and choose “Open Link in New Tab”)

 2. Open Executions

![](https://cdn-images-1.medium.com/max/4000/0*hSNlxJZmTlaB1IvQ.png)

 1. On each tab view the details of the execution and find the Duration field to see how long each execution required.

 2. Find Duration

![](https://cdn-images-1.medium.com/max/4000/0*JbRPVqT7CcqMQS5F.png)

Do they differ? For this simulated dataset, which would be considered small for a Monte Carlo Simulation, the difference is typically less than a minute but overall the increased vCPU/RAM will lead to ~25% time savings. However, to get this ~25% savings you had to double the resources for each container, effectively doubling your Fargate costs. If time is your most critical factor, this may be worth it. If cost is your most critical factor, perhaps not. The variation for both cost and time are minimal with a dataset this small, however if you extrapolate this to a dataset consisting of millions or even billions of objects, both time and cost variations are considerable. You will want to experiment with your actual workloads on what settings work best.

## Cleanup

* Login to the AWS account where you deployed the module.

* Open the [S3 console ](https://s3.console.aws.amazon.com/)then empty both buckets containing “hellodmap” in the name.

* Open the [CloudFormation console ](https://console.aws.amazon.com/cloudformation), select the stack with a name containing “sfw-hello-distributed-map” (or with the name you entered earlier), then click **Delete**.

* Make sure the stack deletion completes.

* Important

* Follow the instructions on this page only if you are executing this workshop in your own account.

* Navigate to [S3 ](https://s3.console.aws.amazon.com/). Search for **sfw-optimization**. Empty both buckets.

* Navigate to [CloudFormation ](https://console.aws.amazon.com/cloudformation)console. Search for **sfw-optimization-distributed-map**. Delete the stack.

* Navigate to [S3 ](https://s3.console.aws.amazon.com/). Search for **dmapworkshophealthcare**. Empty the bucket.

* Navigate to [CloudFormation ](https://console.aws.amazon.com/cloudformation)console. Search for **sfw-healthcare-processing**. Delete the stack.

* Open the [CloudFormation console ](https://console.aws.amazon.com/cloudformation/home)then select the stack with a name containing “**vulnerability-scanning-module**” (or with the name you entered earlier).

* Click **Delete**.

* Make sure the stack deletion completes.

* Navigate to [S3 ](https://s3.console.aws.amazon.com/). Search for **sfn-datagen**. Empty both buckets.

* Navigate to [CloudFormation ](https://console.aws.amazon.com/cloudformation)console.

* Search for **sfn-fargate**. Delete the stack.

* Search for **sfn-datagen**. Delete the stack.

## Challenges Faced and Solutions

**Challenge 1: Complex Workflow Management**

* **Solution**: Leveraged AWS Step Functions’ visual editor to design and troubleshoot each state transition, ensuring a seamless workflow.

**Challenge 2: Detailed Monitoring Across Multiple Services**

* **Solution**: Integrated AWS CloudWatch and X-Ray to gain a comprehensive view of workflow execution, enabling more effective troubleshooting.

**Challenge 3: Ensuring Data Security**

* **Solution**: Implemented strict IAM policies to ensure secure access control, preventing unauthorized access to sensitive data.

## Conclusion

This project demonstrates the effectiveness of AWS Step Functions in orchestrating a large-scale, serverless data processing workflow. By using Step Functions in tandem with Amazon S3, CloudWatch, X-Ray, and IAM, we created a solution that scales efficiently, provides high visibility into operations, and ensures data security. The architecture exemplifies how serverless services can simplify complex workflows, offering a robust approach to data processing challenges.

This project can serve as a template for businesses seeking to manage large datasets efficiently, whether for data analytics, machine learning pipelines, or ETL workflows.

## Code Repository

To explore and experiment with the project’s code and documentation, visit the [**GitHub Repository](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/9.%20Large-scale%20Data%20Processing%20with%20Step%20Functions)**. Here you can access the complete code, follow along with detailed instructions, and customize the applications to fit your own use cases.
>  ***Asif Khan — Aspiring Cloud Architect | Weekly Cloud Learning Chronicler***

[**LinkedIn](https://www.linkedin.com/in/asif0108/)**/[**Twitter](https://x.com/asif26073)**/[**GitHub](https://github.com/Dark-Cookie)**
