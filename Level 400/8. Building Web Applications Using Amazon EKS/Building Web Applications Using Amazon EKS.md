
![](https://cdn-images-1.medium.com/max/3584/1*3avNYDb0-lWGvHh9GV8UkA.png)

## AWS Project: Building Web Applications Using Amazon EKS

### Introduction

As businesses increasingly move to the cloud, scalable and efficient web application infrastructures have become essential. Kubernetes, paired with Amazon Web Services (AWS), provides a powerful solution for deploying and managing containerized applications. In this project, we built a web application leveraging **Amazon Elastic Kubernetes Service (EKS)** to harness Kubernetes’ orchestration capabilities on AWS’s infrastructure. The project’s goal was to create a resilient and scalable web application environment by deploying it on Amazon EKS with container insights, automated scaling, and a streamlined CI/CD pipeline.

This blog will guide you through the complete implementation, including setting up your development environment, containerizing applications, deploying clusters, monitoring performance, and enabling auto-scaling. Whether you’re a DevOps engineer, cloud enthusiast, or developer, this tutorial provides hands-on experience with EKS and the tools AWS offers to simplify Kubernetes-based application management.

### Tech Stack

Here’s a rundown of the AWS services, tools, and technologies used:

* **AWS Cloud9**: For an integrated development environment (IDE) on the cloud

* **Amazon Elastic Container Registry (ECR)**: To store Docker images for easy deployment

* **Amazon Elastic Kubernetes Service (EKS)**: The primary Kubernetes cluster manager

* **Container Insights**: For real-time monitoring and insights into the application

* **AWS Fargate**: For serverless deployment and resource optimization

* **CI/CD Pipeline**: To automate code deployment, enhancing release efficiency

### Prerequisites

Before diving in, ensure you meet these prerequisites:

 1. **AWS Basic Knowledge**: Familiarity with AWS core services and IAM (Identity and Access Management) permissions.

 2. **Docker & Kubernetes Basics**: Knowledge of containerization and Kubernetes concepts will be helpful.

 3. **AWS CLI & eksctl Installed**: Both AWS CLI and eksctl should be configured on your local environment.

 4. **IAM Setup**: Permissions to access AWS resources, especially EKS, ECR, Cloud9, and IAM roles.

### Problem Statement or Use Case

Person A, a member of the DevOps team of a famous Korean company, will be in charge of a project to develop a new web applications. The application should be satisfied with the following:

* Quickly reflect changes when new requests are occurred

* Easy scaling

* Operate and develop application with fewer people

After confirming the above requirements, Person A decided to build **Modern Application**, and after discussing with team members, A wants to build the web application through **MSA, container, CI/CD**. And they choose kubernetes as container orchestration tool based on majority opinion.

There are few team members and not enough time to work directly to build everything by using open source. Expanding infrastructure is also a pain point. And there are some people who don’t know Kubernetes precisely.

At this point, Person A found out managed kubernetes service, [Amazon Elastic Kubernetes Service ](https://aws.amazon.com/ko/kubernetes/). And to understand the advantages and characteristics of the **Amazon EKS**, A determined to do simple PoC(Proof of Concept)!

## Architecture Diagram

Below is the architecture diagram for the web application built on Amazon EKS. This high-level view showcases how different AWS services interact to deliver a cohesive deployment environment.

![](https://cdn-images-1.medium.com/max/3102/1*VDiCz0m9goiUfQ4QDER2jw.png)

### Component Breakdown

Each component plays a vital role in the solution architecture:

 1. **Amazon ECR**: Stores Docker container images for EKS to pull from and deploy.

 2. **Amazon EKS**: Manages Kubernetes clusters that house the application, making it scalable and robust.

 3. **Container Insights**: Monitors the health and performance of the application and clusters.

 4. **AWS Fargate**: Enables serverless container deployment, reducing infrastructure management needs.

 5. **CI/CD Pipeline**: Automates code deployment to EKS, enhancing deployment frequency and reliability.

## Step-by-Step Implementation

## Setting workspace

## [Build a workspace with AWS Cloud9](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/30-setting#build-a-workspace-with-aws-cloud9)

This workshop is conducted through AWS Cloud9, a cloud-based integrated development environment(IDE).

AWS Cloud9 is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. It includes a code editor, debugger, and terminal. Cloud9 comes prepackaged with essential tools for popular programming languages, including JavaScript, Python, PHP, and more, so you don’t need to install files or configure your development machine to start new projects.

Click [here ](https://aws.amazon.com/cloud9/?nc1=h_ls)to learn more about the features and characteristics of **AWS Cloud9**.

![](https://cdn-images-1.medium.com/max/2000/0*5BQc0h9XwwDu4UoU.png)

## AWS Cloud9

The order in which you build a workspace with AWS Cloud9 is as follows:

* IDE configuration with AWS Cloud9

* Create IAM Role

* Grant IAM Role to an AWS Cloud9 instance

* Update IAM settings in IDE

In case of AWS Event, **accounts are already prepared with belows configuration**. You can skip this.

## [IDE configuration with AWS Cloud9](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/30-setting/100-aws-cloud9#ide-configuration-with-aws-cloud9)

 1. Access [AWS Cloud9 console ](https://console.aws.amazon.com/cloud9)and click the **Create environment** button.

 2. Write down the IDE name and click Next step. In this lab, type eks-workspace.

 3. Click the other instance type radio button, select **t3.medium**. In case of platform, select **Amazon Linux 2 (recommended)** and click Next step button. Check the property value you set, then click **Create environment**.

 4. When the creation is completed, the screen below appears.

![](https://cdn-images-1.medium.com/max/2792/0*PloQcuVXft5-LW2q.png)

AWS Cloud9 requires third-party-cookies. If the screen above does not appear, refer to [here ](https://docs.aws.amazon.com/cloud9/latest/user-guide/troubleshooting.html#troubleshooting-env-loading).

## [Create IAM Role](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/30-setting/100-aws-cloud9#create-iam-role)

An IAM role is an IAM identity that you can create in your account that has specific permissions. For the IAM role, it is available for IAM users and services provided by AWS. If you grant an IAM Role to the AWS service, the service performs the delegated role on your behalf.

In this lab, we create an IAM Role with **Administrator access** policy and attach it to AWS Cloud9.

 1. Click [here ](https://console.aws.amazon.com/iam/home#/roles$new?step=type&commonUseCase=EC2%2BEC2&selectedUseCase=EC2&policies=arn:aws:iam::aws:policy%2FAdministratorAccess)to enter into IAM Role console.

 2. Check that **AWS service** and **EC2** are selected and click **Next: Permissions**.

 3. Check that the **AdministratorAccess** policy is selected and click **Next: Tags**.

 4. In the Add Tag (optional) step, click **Next: Review**.

 5. In **Role name**, type eksworkspace-admin, confirm that the AdministratorAccess managed policy has been added, and click **Create role**.

![](https://cdn-images-1.medium.com/max/2000/0*L7E1UVPA1-hKdLOE.png)

For this workshop, the AdministratorAccess policy is used to facilitate the workshop, but it is appropriate to grant minimum privileges when running a production environment.

### [Grant IAM Role to an AWS Cloud9 instance](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/30-setting/100-aws-cloud9#grant-iam-role-to-an-aws-cloud9-instance)

The AWS Cloud9 environment is powered by an EC2 instance. Therefore, grant the IAM Role that you just created to the AWS Cloud9 instance in EC2 console.

 1. Click [here ](https://console.aws.amazon.com/ec2/v2/home?#Instances:sort=desc:launchTime)to enter into EC2 instnace console.

 2. Select AWS Cloud9 instance, then click **Actions > Security > Modify IAM Role**.

![](https://cdn-images-1.medium.com/max/3312/0*C09yD5iBzPpu86Tb.png)

 1. Select eksworkspace-admin in IAM Role section, then click the **Save** button.

![](https://cdn-images-1.medium.com/max/2000/0*ZM2BtvjHmWHyKiOm.png)

## [Update IAM settings in IDE](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/30-setting/100-aws-cloud9#update-iam-settings-in-ide)

In AWS Cloud9, it dynamically manage IAM credits. Disable these credentials because they are not compatible with **EKS IAM authentication**. After that **attach the IAM Role**.

 1. Reconnect to the IDE you created in AWS Cloud9 console, click the gear icon in the upper right corner, and click **AWS SETTINGS** in the sidebar.

 2. Disable the **AWS managed temperature credits** setting in the **Credentials** topic.

 3. Exit Preference tab.

![](https://cdn-images-1.medium.com/max/2662/0*tHaoJ97oe0nA6fKF.png)

 1. Remove existing credential files to ensure **Temporary credentials** are not present.

    rm -vf ${HOME}/.aws/credentials

 1. Use the **GetCallerIdentity CLI** command line to check that Cloud9 IDE is using the correct IAM Role. **If the result value is shown** it is set correctly.

    aws sts get-caller-identity --query Arn | grep eksworkspace-admin

## kubectl

### [Install kubectl](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/30-setting/300-kubectl#install-kubectl)

**kubectl** is the CLI that commands a Kubernetes cluster.

Kubernetes uses **Kubernetes API** to perform actions related to creating, modifying, or deleting objects. When you use the kubectl CLI, the command invokes the Kubernetes API to perform the associated actions.

Click [here ](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)to **install the corresponding kubectl to the Amazon EKS version you want to deploy**. In this workshop, we will install the latest kubectl binary(as of August 2023).

    sudo curl -o /usr/local/bin/kubectl  \
       https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.4/2023-08-16/bin/linux/amd64/kubectl

    sudo chmod +x /usr/local/bin/kubectl

Use the command below to check that the latest kubectl is installed.

    kubectl version --client=true --short=true

    # Output Sample
    Client Version: v1.27.4-eks-8ccc7ba
    Kustomize Version: v5.0.1

## ETC

### [Install jq](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/30-setting/400-etc#install-jq)

jq is a command line utility that deals with data in JSON format. Using the command below, install jq.

    sudo yum install -y jq

### [Install bash-completion](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/30-setting/400-etc#install-bash-completion)

In the Bash shell, the kubectl completion script can be created using the **kubectl completion bash** command. Sourcing the completion script to the shell enables automatic completion of the kubectl command. However, because these completion scripts rely on bash-completion, you must install [bash-completion ](https://github.com/scop/bash-completion#installation)through the command below.

    sudo yum install -y bash-completion

Below is only necessary for exploring [CI/CD for EKS Cluster](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/110-cicd/300-cicd)

### [Install Git](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/30-setting/400-etc#install-git)

Click the [Git Downloader ](https://git-scm.com/downloads)link and install the git.

### [Install Python](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/30-setting/400-etc#install-python)

Install python because CDK for Python is used. Python is installed by default in the Cloud9 environment. [Python Installer ](https://www.python.org/downloads/)Select the appropriate package from the link to download and install it.

    python --version
    python3 --version

### [Check PIP](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/30-setting/400-etc#check-pip)

Checks whether PIP, a manager that installs and manages Python packages, is installed. It is installed by default in certain versions of Python and above.

    pip
    pip3

Since pip version 9.0.3 or higher is required to use CodeCommit, update pip by executing the command below.

    curl -O https://bootstrap.pypa.io/get-pip.py
    python3 get-pip.py --user

If not installed [pip install page ](https://pip.pypa.io/en/stable/cli/pip_install/)recommended to proceed with the installation according to the guide or install with the latest version of Python.

## Install eksctl

### [Install eksctl](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/30-setting/500-eksctl#install-eksctl)

There are various ways to deploy an Amazon EKS cluster. AWS console, CloudFormation, CDK, eksctl, and Terraform are examples.

In this lab, we will **deploy the cluster using eksctl**.

![](https://cdn-images-1.medium.com/max/2048/0*8mtsRLNuSpUs7_dS.png)

[eksctl ](https://eksctl.io/)is a CLI tool for easily creating and managing EKS clusters. It is written in Go language and deployed in CloudFormation form.

Download the latest eksctl binary using the command below.

    curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

Move the binary to the location /usr/local/bin.

    sudo mv -v /tmp/eksctl /usr/local/bin

Use the command below to check the installation.

    eksctl version

## AWS Cloud9 Additional Settings

On the previous chapter, we **deploy AWS Cloud9 IDE** and **install the required tools**, then proceed to the additional setup below.

 1. Set default value to AWS Region that is currently using.

    TOKEN=$(curl -s -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")

    export AWS_REGION=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" [http://169.254.169.254/latest/dynamic/instance-identity/document](http://169.254.169.254/latest/dynamic/instance-identity/document) | jq -r '.region')

    echo "export AWS_REGION=${AWS_REGION}" | tee -a ~/.bash_profile
        
    aws configure set default.region ${AWS_REGION}

Check AWS region.

    aws configure get default.region

 1. Register the account ID you are currently working on as an environment variable.

    export ACCOUNT_ID=$(aws sts get-caller-identity --query 'Account' --output text)

    echo "export ACCOUNT_ID=${ACCOUNT_ID}" | tee -a ~/.bash_profile

 1. During the docker image build, the AWS Cloud9 environment may experience a capacity shortage issue. To resolve this, run a shell script that extends the disk size.

    wget https://gist.githubusercontent.com/joozero/b48ee68e2174a4f1ead93aaf2b582090/raw/2dda79390a10328df66e5f6162846017c682bef5/resize.sh

    sh resize.sh

After completion, use the command below to check that the increased volume size is reflected in the file system.

    df -h

If you do not set AWS Region, an error occurs when you deploy the cluster and check the relevant information.

## Container Image

In this lab, you will learn how to create a container image using a docker platform.

## [Docker](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/40-container#docker)

[Docker ](https://aws.amazon.com/docker/?nc1=h_ls)is a software **platform **that allows you to build, test and deploy **containerized applications**. Docker packages software into standardized units called containers, which contain everything you need to run the software, including libraries, system tools, code, runtime, and so on.

To learn more about **Docker**, click [here ](https://www.docker.com/resources/what-container).

## [Container Image](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/40-container#container-image)

**Container image** is a combination of the files and settings required to run the container. These images can be uploaded and downloaded in the repository. And the state in which the image was executed is **container**. Container images can be downloaded and used by official image repositories such as [Docker Hub ](https://hub.docker.com/)or created directly.

![](https://cdn-images-1.medium.com/max/2404/0*njrfM5Vxx-kNDZH8.png)

## Build Container Image

This part is an independent lab for those who want to learn more about containers and container images. If you don’t proceed with this lab, you won’t have any problem with **configuring web application with Amazon EKS**. If you don’t want this lab, move onto the **Upload Image to Amazon ECR** chapter.

## [Create Container Image Yourself](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/40-container/100-build-image#create-container-image-yourself)

![](https://cdn-images-1.medium.com/max/2000/0*1DEYxr9eRuouqtJ-.png)

**Docker File** is a **setup file for building container images**. That is, think of it as a blueprint for the image to be built. When these images become containers, the application is actually running.

 1. Paste the values below in the root folder(/home/ec2-user/environment).

    cd ~/environment/

    cat << EOF > Dockerfile
    FROM nginx:latest
    RUN  echo '<h1> test nginx web page </h1>'  >> index.html
    RUN cp /index.html /usr/share/nginx/html
    EOF

Instruction component in the Docker File is as follows.

 1. **FROM**: Set the Base Image(Specify OS or version)

 2. **RUN**: Execute any commands in a new layer on top of the current image and commit the results

 3. **WORKDIR**: Where to perform instructions such as RUN, CMD, ENTRYPOINT, COPY, ADD in the Docker File

 4. **EXPOSE**: Specify the port number to connect to the host

 5. **CMD**: Commands for running application

 6. Create an image with the **docker build command**. In name, enter the name of the container image and in case of tag, if not named, you will have a value called **latest**. In this lab, you will write test-image by container image name.

* docker build -t test-image .

 1. Check the images created with the **docker images command**.

* docker images

 1. Run the image as a container with the **docker run command**. The command below uses a container image named test-image to run a container named test-nginx, which means that 8080 ports of the host and 80 ports of the container are mapped.

* docker run -p 8080:80 --name test-nginx test-image

In other words, information passed to 8080 ports on the host is forwarded through the docker to 80 ports on the container.

 1. You can use the **docker ps command** to check which containers are running on the current host. **Open a new terminal on AWS Cloud9** and type the command below.

* docker ps

![](https://cdn-images-1.medium.com/max/4000/0*JBw5R2LMs-mthVo7.png)

 1. You can check the status by outputting logs from the container with the **docker logs command**.

* docker logs -f test-nginx

 1. You can access into the inside shell environment of the container with **docker exec command**. After access, you can apprehend the internal structure and exit through the exit command.

* docker exec -it test-nginx /bin/bash

 1. In AWS cloud9, you can see which applications are currently running by clicking the top Tools > Preview > Preview Running Application.

![](https://cdn-images-1.medium.com/max/2524/0*o50Er_LuUae0Ahcl.png)

![](https://cdn-images-1.medium.com/max/2922/0*r7ggxftDhXEW5Aae.png)

 1. Stop running containers with **docker stop command**.

* docker stop test-nginx

If you type the docker ps command again, you can see that the container that was just running has disappeared from the list.

 1. Delete the container with the **docke rm command**. The container deletion is possible only when the container is stopped.

* docker rm test-nginx

 1. Delete the container image with **docke rmi command**.

* docker rmi test-image

When you type the **docker images** command, you can check that the container image that was created is not listed.

You can also use the Docker command to perform operations such as cpu or memory restrictions, and sharing directory with hosts.

## Upload container image to Amazon ECR

### [Create Amazon ECR Repository and Upload Image](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/40-container/200-eks#create-amazon-ecr-repository-and-upload-image)

Create repositories and upload container images in the docker container registry **Amazon Elastic Container Registry (ECR)**.

![](https://cdn-images-1.medium.com/max/2000/0*H4Bbqwh_AgJ-_Sln.png)

Amazon Elastic Container Registry([**Amazon ECR ](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)**) is an AWS managed container image registry service that is secure, scalable, and reliable. Amazon ECR supports private container image repositories with resource-based permissions using AWS IAM. This is so that specified users or Amazon EC2 instances can access your container repositories and images. You can use your preferred CLI to push, pull, and manage Docker images, Open Container Initiative (OCI) images, and OCI compatible artifacts.

 1. Download the source code to be containerized through the command below.

* git clone [https://github.com/joozero/amazon-eks-flask.git](https://github.com/joozero/amazon-eks-flask.git)

 1. Through the AWS CLI, create an image repository. In this lab, we will set the repository name to **demo-flask-backend**. Also, specify AWS Region code(for example, ap-northeast-2) to deploy the EKS cluster in **— region**’s value.

* aws ecr create-repository \ --repository-name demo-flask-backend \ --image-scanning-configuration scanOnPush=true \ --region ${AWS_REGION}

When you enter this CLI, information about the repository is derived from the resulting value. You can also find the repositories created in the [Amazon ECR Console ](https://console.aws.amazon.com/ecr/home).

![](https://cdn-images-1.medium.com/max/2868/0*xUbaQAtdhTg7WF5N.png)

For the tasks below, your personal account information will be included. Click on the repository you just created on the [Amazon ECR Console ](https://console.aws.amazon.com/ecr/home), then click **View push commands** in the upper right corner to find the guide below.

![](https://cdn-images-1.medium.com/max/2608/0*tLrW6qbLEHltQhrq.png)

![](https://cdn-images-1.medium.com/max/2000/0*Zzz8NCr0kovPznpI.png)

 1. To push the container image to the repository, bring the authentication token and pass the authentication to the **docker login** command. At this point, specify the user name as AWS and specify the Amazon ECR registry URI that you want to authenticate with.

* aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com

[!] If the above command does not work properly, check whether environment variable named ACCOUNT_ID is called in the terminal.

 1. Input the **downloaded source code location(for example, /home/ec2-user/environment/amazon-eks-flask)** and enter the command below to build the docker image.

* cd ~/environment/amazon-eks-flask docker build -t demo-flask-backend .

 1. When the image is built, use the **docker tag command** to enable it to be pushed to a specific repository.

* docker tag demo-flask-backend:latest $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/demo-flask-backend:latest

 1. Push the image into the repository via the **docker push** command.

* docker push $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/demo-flask-backend:latest

 1. In the Amazon ECR Console, click on the repository you just created to see the uploaded image as shown in the screen below.

![](https://cdn-images-1.medium.com/max/2718/0*SlCLoGUZrP0uq8NP.png)

 1. We have created **container images** to deploy on EKS clusters and pushed it into the **repository**.

## Create EKS Cluster

Amazon EKS clusters can be deployed in various ways.

* Deploy by clicking on [AWS console](https://console.aws.amazon.com/eks/home#/)

* Deploy by using IaC(Infrastructure as Code) tool such as [AWS CloudFormation ](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)or [AWS CDK](https://docs.aws.amazon.com/cdk/api/latest/)

* Deploy by using [eksctl](https://eksctl.io/)

* Deploy to Terraform, Pulumi, Rancher, etc.

![](https://cdn-images-1.medium.com/max/4000/0*5eEUIJqa2OLODF6l.png)

In this lab, we will create an EKS cluster using **eksctl**.

## Create EKS Cluster with eksctl

### [Create EKS Cluster with eksctl](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/50-eks-cluster/100-launch-cluster#create-eks-cluster-with-eksctl)

If you use eksctl to execute this command (eksctl create cluster) without giving any setting values, the cluster is deployed as a default parameter.

However, we will **create configuration files to customize some values** and deploy it. In later labs, when you create Kubernetes’ objects, you create a configuration file that is not just created with the kubectl CLI. This has the advantage of being able to easily identify and manage the desired state of the objects specified by the individual.

 1. Paste the values below in the root folder(/home/ec2-user/environment) location.

    cd ~/environment

    cat << EOF > eks-demo-cluster.yaml
    ---
    apiVersion: eksctl.io/v1alpha5
    kind: ClusterConfig

    metadata:
      name: eks-demo # EKS Cluster name
      region: ${AWS_REGION} # Region Code to place EKS Cluster
      version: "1.27"

    vpc:
      cidr: "10.0.0.0/16" # CIDR of VPC for use in EKS Cluster
      nat:
        gateway: HighlyAvailable

    managedNodeGroups:
      - name: node-group # Name of node group in EKS Cluster
        instanceType: m5.large # Instance type for node group
        desiredCapacity: 3 # The number of worker node in EKS Cluster
        volumeSize: 20  # EBS Volume for worker node (unit: GiB)
        privateNetworking: true
        iam:
          withAddonPolicies:
            imageBuilder: true # Add permission for Amazon ECR
            albIngress: true  # Add permission for ALB Ingress
            cloudWatch: true # Add permission for CloudWatch
            autoScaler: true # Add permission Auto Scaling
            ebs: true # Add permission EBS CSI driver

    cloudWatch:
      clusterLogging:
        enableTypes: ["*"]
    EOF

If you look at the cluster configuration file, you can define policies through **iam.attachPolicyARNs** and through **iam.withAddonPolicies**, you can also define add-on policies. After the EKS cluster is deployed, you can check the IAM Role of the worker node instance in EC2 console to see added policies.

Click [here ](https://eksctl.io/usage/creating-and-managing-clusters/)to see the various property values that you can give to the configuration file.

 1. Using the commands below, deploy the cluster.

    eksctl create cluster -f eks-demo-cluster.yaml

The cluster takes **approximately 15 to 20 minutes** to fully be deployed. You can see the progress of your cluster deployment in AWS Cloud9 terminal and also can see the status of events and resources in AWS CloudFormation console.

 1. When the deployment is completed, use command below to check that the node is properly deployed.

    kubectl get nodes

 1. Also, you can see the cluster credentials added in **~/.kube/config**.

### [The architecture as of now](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/50-eks-cluster/100-launch-cluster#the-architecture-as-of-now)

![](https://cdn-images-1.medium.com/max/2084/1*n73JqTVsFcfwZCfTxeC0Kw.png)

After creating a Kubernetes cluster with eksctl, the architecture of the services configured as of now is shown below.

## Add Console Credential

## [Attach Console Credential](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/50-eks-cluster/200-option-console#attach-console-credential)

The EKS cluster uses IAM entity(user or role) for cluster access control. The rule runs in a ConfigMap named **aws-auth**. By default, IAM entities used to create clusters are automatically granted **system:masters** privilege of the cluster RBAC configuration in the control plane.

If you access the Amazon EKS console in the current state, you cannot check any information as below.

![](https://cdn-images-1.medium.com/max/2192/0*wFRgZL5xx_InNykU.png)

When you created the cluster through IAM credentials on Cloud9 in [**Create EKS Cluster with eksctl](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/50-eks-cluster/100-launch-cluster.html)** chapter, so you need to determine the correct credential(such as your IAM Role not Cloud9 credentials) to add for your [AWS EKS Console ](https://console.aws.amazon.com/eks)access.

 1. Use the command below to define the role ARN(Amazon Resource Number).

    rolearn=$(aws cloud9 describe-environment-memberships --environment-id=$C9_PID | jq -r '.memberships[].userArn')

    echo ${rolearn}

[!] If **echo command**’s result contains **assumed-role**, perform the additional actions below.

    assumedrolename=$(echo ${rolearn} | awk -F/ '{print $(NF-1)}')
    rolearn=$(aws iam get-role --role-name ${assumedrolename} --query Role.Arn --output text)

 1. Create an identity mapping.

    eksctl create iamidentitymapping --cluster eks-demo --arn ${rolearn} --group system:masters --username admin

 1. You can check **aws-auth** config map information through the command below.

    kubectl describe configmap -n kube-system aws-auth

 1. When the above operations are completed, you will be able to get information from the control plane, the worker node, logging activation, and update information in Amazon EKS console.

![](https://cdn-images-1.medium.com/max/2596/0*rchu3LlA8Woigdpn.png)

On the Workloads tab, you can see the applications placed in the Kubernetes cluster.

![](https://cdn-images-1.medium.com/max/2224/0*xR13dUrgThKTUUIc.png)

On the Configuration tab, you can get cluster configuration detail.

![](https://cdn-images-1.medium.com/max/2208/0*plSPMYobeUXl2Mw-.png)

## Create Ingress Controller

## [Ingress Controller](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/60-ingress-controller#ingress-controller)

In this lab, we will use [AWS Load Balancer Controller ](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/)for Ingress Controller.

The **AWS ALB Ingress Controller** has been rebranded to **AWS Load Balancer Controller**.

**Ingress** is a rule and resource object that defines how to handle requests, primarily when accessing from outside the cluster to inside the Kubernetes cluster. In short, it serve as a gateway for external requests to access inside of the cluster. You can set up it for load balancing for external requests, processing TLS/SSL certificates, routing to HTTP routes, and so on. Ingress processes requests from the L7.

In Kubernetes, you can also externally expose to NodePort or LoadBalancer type in Service object, but if you use a Serivce object without any Ingress, you must consider detailed options such as routing rules and TLS/SSL to all services. That’s why Ingress is needed in Kubernetes environment.

![](https://cdn-images-1.medium.com/max/2000/1*sRI4T8S24XiV6C-xM4jusQ.png)

Ingress means the object that you have set up rules for handling external requests, and **Ingress Controller** is needed for these settings to work. Unlike other controllers that run as part of the kube-controller-manager, the ingress controller is not created with the cluster by nature. Therefore, you need to install it yourself.

## Create AWS Load Balancer Controller

### [Create AWS Load Balancer Controller](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/60-ingress-controller/100-launch-alb#create-aws-load-balancer-controller)

The [AWS Load Balancer Controller ](https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html)manages AWS Elastic Load Balancers for a Kubernetes cluster. The controller provisions the following resources.

* It satisfies Kubernetes Ingress resources by provisioning Application Load Balancers.

* It satisfies Kubernetes Service resources by provisioning Network Load Balancers.

The controller was formerly named the AWS ALB Ingress Controller. There are two **traffic modes** supported by each type of AWS Load Balancer controller:

* Instance(default): Register nodes in the cluster as targets for ALB. Traffic reaching the ALB is routed to NodePort and then proxied to the Pod.

* IP: Register the Pod as an ALB target. Traffic reaching the ALB is routed directly to the Pod. In order to use that traffic mode, you must explicitly specify it in the ingress.yaml file with comments.

![](https://cdn-images-1.medium.com/max/2000/1*8KKstQ_jY-2Dwdn8e6RAbg.png)

Create a folder named **manifests** in the root folder (for example, /home/ec2-user/environment/) to manage manifests. Then, inside the manifests folder, create a folder **alb-controller** to manage the manifest associated with the ALB Ingress Controller.

    cd ~/environment

    mkdir -p manifests/alb-ingress-controller && cd manifests/alb-ingress-controller

    # Final location
    /home/ec2-user/environment/manifests/alb-ingress-controller

Before deploying the AWS Load Balancer controller, we need to do some things. Because the controller operates over the worker node, you must make it accessible to AWS ALB/NLB resources through IAM permissions. IAM permissions can install IAM Roles for ServiceAccount or attach directly to IAM Roles on the worker node.

 1. First, create **IAM OpenID Connect (OIDC) identity provider** for the cluster. **IAM OIDC provider** must exist in the cluster(in this lab, *eks-demo*) in order for objects created by Kubernetes to use [**service account **](https://kubernetes.io/ko/docs/reference/access-authn-authz/service-accounts-admin/)which purpose is to authenticate to API Server or external services.

    eksctl utils associate-iam-oidc-provider \
        --region ${AWS_REGION} \
        --cluster eks-demo \
        --approve

[!] Let’s find out a little more here.

* The IAM OIDC identity provider you create can be found in Identity providers menu on IAM console or in the commands below.

* Check the OIDC provider URL of the cluster through the commands below.

    aws eks describe-cluster --name eks-demo --query "cluster.identity.oidc.issuer" --output text

Result from the command have the following format:

    https://oidc.eks.ap-northeast-2.amazonaws.com/id/8A6E78112D7F1C4DC352B1B511DD13CF

* Copy the value after **/id/** from the output above, then execute the command as shown below.

    aws iam list-open-id-connect-providers | grep 8A6E78112D7F1C4DC352B1B511DD13CF

* If the result appears, **IAM OIDC identity provider** is created in the cluster, and if no value appears, you must execute the creation operation again.

 1. Create an IAM Policy to grant to the AWS Load Balancer Controller.

    curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json

    aws iam create-policy \
        --policy-name AWSLoadBalancerControllerIAMPolicy \
        --policy-document file://iam_policy.json

 1. Create ServiceAccount for AWS Load Balancer Controller.

    eksctl create iamserviceaccount \
        --cluster eks-demo \
        --namespace kube-system \
        --name aws-load-balancer-controller \
        --attach-policy-arn arn:aws:iam::$ACCOUNT_ID:policy/AWSLoadBalancerControllerIAMPolicy \
        --override-existing-serviceaccounts \
        --approve

When deploying an EKS cluster, you can also add the IAM policy associated with the AWS Load Balancer Controller to the Worker node in the form of Addon. However, in this lab, we will conduct with the reference, [here ](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/deploy/installation/).

Also, please refer simple hands on lab about IAM roles for service accounts([IRSA ](https://aws.amazon.com/blogs/opensource/introducing-fine-grained-iam-roles-service-accounts/)) in [here ](https://aws.amazon.com/premiumsupport/knowledge-center/eks-restrict-s3-bucket/?nc1=h_ls).

[Add Controller to Cluster](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/60-ingress-controller/100-launch-alb#add-controller-to-cluster)

 1. Add AWS Load Balancer controller to the cluster. First, install [**cert-manager **](https://github.com/jetstack/cert-manager)to insert the certificate configuration into the Webhook. **Cert-manager** is an open source that automatically provisions and manages TLS certificates within a Kubernetes cluster.

    kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.12.0/cert-manager.yaml

 1. Download Load balancer controller yaml file.

    curl -Lo v2_5_4_full.yaml https://github.com/kubernetes-sigs/aws-load-balancer-controller/releases/download/v2.5.4/v2_5_4_full.yaml

 1. Run the following command to remove the **ServiceAccount** section in the manifest. If you don’t remove this section, the required annotation that you made to the service account in a previous step is overwritten.

    sed -i.bak -e '596,604d' ./v2_5_4_full.yaml

Replace cluster name in the **Deployment** spec section of the file with the name of your cluster by replacing my-cluster with the name of your cluster.

    sed -i.bak -e 's|your-cluster-name|eks-demo|' ./v2_5_4_full.yaml

 1. Deploy **AWS Load Balancer controller** file.

    kubectl apply -f v2_5_4_full.yaml

Download the IngressClass and IngressClassParams manifest to your cluster. And apply the manifest to your cluster.

    curl -Lo v2_5_4_ingclass.yaml https://github.com/kubernetes-sigs/aws-load-balancer-controller/releases/download/v2.5.4/v2_5_4_ingclass.yaml

    kubectl apply -f v2_5_4_ingclass.yaml

 1. Check that the deployment is successed and the controller is running through the command below. When the result is derived, it means success.

    kubectl get deployment -n kube-system aws-load-balancer-controller

In addition, the command below shows that **service account** has been created.

    kubectl get sa aws-load-balancer-controller -n kube-system -o yaml

Pods running inside the cluster for the necessary functions are called **Addon**. Pods used for add-on are managed by the Deployment, Replication Controller, and so on. And the namespace that this add-on uses is **kube-system**. Because the namespace is specified as kube-system in the yaml file, it is successfully deployed when the pod name is derived from the command above. You can also check the relevant logs with the commands below.

    kubectl logs -n kube-system $(kubectl get po -n kube-system | egrep -o "aws-load-balancer[a-zA-Z0-9-]+")

Detailed property values are available with the commands below.

    ALBPOD=$(kubectl get pod -n kube-system | egrep -o "aws-load-balancer[a-zA-Z0-9-]+")

    kubectl describe pod -n kube-system ${ALBPOD}

## Deploy Microservices

In this lab, you will learn how to deploy the backend, frontend to Amazon EKS, which makes up the web service. The order in which each service is deployed is as follows.

![](https://cdn-images-1.medium.com/max/2000/1*SnCdqA29dSUDBOW3fL5rmA.png)

* Download source code from git repository

* Create a repository for each container image in Amazon ECR

* Build container image from source code location, including Dockerfile, and push to repository

* Create and deploy Deployment, Service, Ingress manifest files for each service.

The figure below shows the order in which end users access the web service.

![](https://cdn-images-1.medium.com/max/2000/1*yJEqCtPM3BM_cV_yIqeglA.png)

## Deploy First Backend Service

### [Deploy flask backend](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/70-deploy-service/100-flask-backend#deploy-flask-backend)

To proceed with this lab, **Upload container image to Amazon ECR** part must be preceded.

 1. Move on to **manifests folder**(/home/ec2-user/environment/manifests).

* cd ~/environment/manifests/

 1. Create **deploy manifest**.

* cat <<EOF> flask-deployment.yaml --- apiVersion: apps/v1 kind: Deployment metadata: name: demo-flask-backend namespace: default spec: replicas: 3 selector: matchLabels: app: demo-flask-backend template: metadata: labels: app: demo-flask-backend spec: containers: - name: demo-flask-backend image: $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/demo-flask-backend:latest imagePullPolicy: Always ports: - containerPort: 8080 EOF

 1. Next, create **service manifest**.

* cat <<EOF> flask-service.yaml --- apiVersion: v1 kind: Service metadata: name: demo-flask-backend annotations: alb.ingress.kubernetes.io/healthcheck-path: "/contents/aws" spec: selector: app: demo-flask-backend type: NodePort ports: - port: 8080 targetPort: 8080 protocol: TCP EOF

 1. Finally, create **ingress manifest**.

* cat <<EOF> flask-ingress.yaml --- apiVersion: networking.k8s.io/v1 kind: Ingress metadata: name: "flask-backend-ingress" namespace: default annotations: alb.ingress.kubernetes.io/scheme: internet-facing alb.ingress.kubernetes.io/target-type: ip alb.ingress.kubernetes.io/group.name: eks-demo-group alb.ingress.kubernetes.io/group.order: '1' spec: ingressClassName: alb rules: - http: paths: - path: /contents pathType: Prefix backend: service: name: "demo-flask-backend" port: number: 8080 EOF

 1. Deploy the manifest created above in the order shown below. Ingress provisions Application Load Balancer(ALB).

* kubectl apply -f flask-deployment.yaml kubectl apply -f flask-service.yaml kubectl apply -f flask-ingress.yaml

 1. Paste the results of the following command into the Web browser or API platform(like Postman) to check:

* echo http://$(kubectl get ingress/flask-backend-ingress -o jsonpath='{.status.loadBalancer.ingress[*].hostname}')/contents/aws

It will take a few time for the ingress object to be deployed. Wait for the **Load Balancers** status to be active in [EC2 console ](https://console.aws.amazon.com/ec2/v2/home#LoadBalancers:).

The architecture as of now is shown below.

![](https://cdn-images-1.medium.com/max/2088/1*zS5fkRN_W8jF4V8THhlz2Q.png)

## Deploy Second Backend Service

### [Deploy Express backend](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/70-deploy-service/200-nodejs-backend#deploy-express-backend)

Deploy the express backend in the same order as the flask backend.

The lab below will deploy pre-built container images to skip the image build and repository push process conducted in **Upload container image to Amazon ECR**.

 1. Move on to **manifests folder**(/home/ec2-user/environment/manifests).

    cd ~/environment/manifests/

Create **deploy manifest** which contains built pre-built container image.

    cat <<EOF> nodejs-deployment.yaml
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: demo-nodejs-backend
      namespace: default
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: demo-nodejs-backend
      template:
        metadata:
          labels:
            app: demo-nodejs-backend
        spec:
          containers:
            - name: demo-nodejs-backend
              image: public.ecr.aws/y7c9e1d2/joozero-repo:latest
              imagePullPolicy: Always
              ports:
                - containerPort: 3000
    EOF

And then, create **service manifest** file.

    cat <<EOF> nodejs-service.yaml
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: demo-nodejs-backend
      annotations:
        alb.ingress.kubernetes.io/healthcheck-path: "/services/all"
    spec:
      selector:
        app: demo-nodejs-backend
      type: NodePort
      ports:
        - port: 8080
          targetPort: 3000
          protocol: TCP
    EOF

create **ingress manifest**.

    cat <<EOF> nodejs-ingress.yaml
    ---
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: "nodejs-backend-ingress"
      namespace: default
      annotations:
        alb.ingress.kubernetes.io/scheme: internet-facing
        alb.ingress.kubernetes.io/target-type: ip
        alb.ingress.kubernetes.io/group.name: eks-demo-group
        alb.ingress.kubernetes.io/group.order: '2'
    spec:
      ingressClassName: alb
      rules:
      - http:
            paths:
              - path: /services
                pathType: Prefix
                backend:
                  service:
                    name: "demo-nodejs-backend"
                    port:
                      number: 8080
    EOF

 1. Deploy the manifest files.

    kubectl apply -f nodejs-deployment.yaml
    kubectl apply -f nodejs-service.yaml
    kubectl apply -f nodejs-ingress.yaml

 1. Paste the results of the following command into the Web browser or API platform(like Postman) to check.

    echo http://$(kubectl get ingress/nodejs-backend-ingress -o jsonpath='{.status.loadBalancer.ingress[*].hostname}')/services/all

 1. The architecture as of now is shown below.

![](https://cdn-images-1.medium.com/max/2088/1*LeqLyJpCBg3I0sfWK9gGfQ.png)

## Deploy Frontend Service

### [Deploy React Frontend](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/70-deploy-service/300-frontend#deploy-react-frontend)

Once you have deployed two backend services, you will now deploy the frontend to configure the web page’s screen.

 1. Download the source code to be containerized through the command below.

    cd /home/ec2-user/environment
    git clone https://github.com/joozero/amazon-eks-frontend.git

 1. Through AWS CLI, create an image repository. In this lab, we will set the repository name to **demo-frontend**.

    aws ecr create-repository \
    --repository-name demo-frontend \
    --image-scanning-configuration scanOnPush=true \
    --region ${AWS_REGION}

 1. To spray two backend API data on the web screen, we have to change source code. Change the url values in **App.js** file and **page/upperPage.js** file from the frontend source code(location: /home/ec2-user/environment/amazon-eks-frontend/src).

![](https://cdn-images-1.medium.com/max/3396/0*klrK71B2p05PM6iu.png)

In the above source code, paste the values derived from the result (ingress addresses) below.

    echo http://$(kubectl get ingress/flask-backend-ingress -o jsonpath='{.status.loadBalancer.ingress[*].hostname}')/contents/'${search}'

![](https://cdn-images-1.medium.com/max/3396/0*u481SFStxMBIPYVw.png)

In the above source code, paste the values derived from the result (ingress addresses) below.

    echo http://$(kubectl get ingress/nodejs-backend-ingress -o jsonpath='{.status.loadBalancer.ingress[*].hostname}')/services/all

 1. Execute the following command in the location of the **amazon-eks-frontend** folder.

    cd /home/ec2-user/environment/amazon-eks-frontend
    npm install
    npm run build

[!] After **npm install**, if severity vulnerability comes out, perform the npm audit fix command and apply **npm run build**.

 1. Refer [Upload container image to Amazon ECR](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/40-container/200-eks) guide and proceed to create container image repository and push image. In this lab, set the image repository name to **demo-frontend**.

    docker build -t demo-frontend .

    docker tag demo-frontend:latest $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/demo-frontend:latest

    docker push $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/demo-frontend:latest

[!] During applying above CLI, if you receive message which said **denied: Your authorization token has expired. Reauthenticate and try again.**, then applying bottom command line and do this again.

    aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com

 1. Move to **manifests folder**. At this point, type image value to **demo-frontend** repository URI.

    cd /home/ec2-user/environment/manifests

    cat <<EOF> frontend-deployment.yaml
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: demo-frontend
      namespace: default
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: demo-frontend
      template:
        metadata:
          labels:
            app: demo-frontend
        spec:
          containers:
            - name: demo-frontend
              image: $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/demo-frontend:latest
              imagePullPolicy: Always
              ports:
                - containerPort: 80
    EOF

    cat <<EOF> frontend-service.yaml
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: demo-frontend
      annotations:
        alb.ingress.kubernetes.io/healthcheck-path: "/"
    spec:
      selector:
        app: demo-frontend
      type: NodePort
      ports:
        - protocol: TCP
          port: 80
          targetPort: 80
    EOF

    cat <<EOF> frontend-ingress.yaml
    ---
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: "frontend-ingress"
      namespace: default
      annotations:
        alb.ingress.kubernetes.io/scheme: internet-facing
        alb.ingress.kubernetes.io/target-type: ip
        alb.ingress.kubernetes.io/group.name: eks-demo-group
        alb.ingress.kubernetes.io/group.order: '3'
    spec:
      ingressClassName: alb
      rules:
        - http:
            paths:
              - path: /
                pathType: Prefix
                backend:
                  service:
                    name: "demo-frontend"
                    port:
                      number: 80
    EOF

 1. Deploy manifest file.

    kubectl apply -f frontend-deployment.yaml
    kubectl apply -f frontend-service.yaml
    kubectl apply -f frontend-ingress.yaml

 1. Copy and paste the result of the command below into the web browser.

    echo http://$(kubectl get ingress/frontend-ingress -o jsonpath='{.status.loadBalancer.ingress[*].hostname}')

 1. If the screen below comes out same, all the containers are working successfully.

![](https://cdn-images-1.medium.com/max/2000/0*HA1CUuGwbpFt6IXN.png)

### [The architecture as of now](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/70-deploy-service/300-frontend#the-architecture-as-of-now)

After deploying Ingress Controller and Service objects, the architecture configured is shown below.

![](https://cdn-images-1.medium.com/max/2088/1*JcXhiNyX-IpUfyOR-4F9Fg.png)

## AWS Fargate

## [AWS Fargate](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/80-fargate#aws-fargate)

![](https://cdn-images-1.medium.com/max/2000/1*a186Lf041Suf7bGnBnH_zA.png)

**AWS Fargate** is a serverless compute engine for containers that works with both Amazon Elastic Container Service (ECS) and Amazon Elastic Kubernetes Service (EKS). Fargate makes it easy for you to focus on building your applications. Fargate removes the need to provision and manage servers, lets you specify and pay for resources per application, and improves security through application isolation by design.

## Deploy service with AWS Fargate

### [Deploy pod with AWS Fargate](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/80-fargate/100-fargate-pod#deploy-pod-with-aws-fargate)

 1. To deploy pods to Fargate in a cluster, you must define at least one fargate profile that the pod uses when it runs. In other words, the fargate profile is a profile that specifies the conditions for creating pods with AWS Fargate type.

    cd /home/ec2-user/environment/manifests

    cat <<EOF> eks-demo-fargate-profile.yaml
    ---
    apiVersion: eksctl.io/v1alpha5
    kind: ClusterConfig
    metadata:
      name: eks-demo
      region: ${AWS_REGION}
    fargateProfiles:
      - name: frontend-fargate-profile
        selectors:
          - namespace: default
            labels:
              app: frontend-fargate
    EOF

For pods that meet the conditions listed in **selectors** in the yaml file above, it will be deployed as AWS Fargate type.

 1. Deploy fargate profile.

    eksctl create fargateprofile -f eks-demo-fargate-profile.yaml

 1. Check whether fargate profile was deployed successfully.

    eksctl get fargateprofile --cluster eks-demo -o json

    # Example of command result
    {
        "name": "frontend-fargate-profile",
        "podExecutionRoleARN": "arn:aws:iam::account-id:role/eksctl-eks-demo-test-farga-FargatePodExecutionRole-OLC3P21AD5DX",
        "selectors": [
            {
                "namespace": "default",
                "labels": {
                    "app": "frontend-fargate"
                }
            }
        ],
        "subnets": [
            "subnet-07e2d55650225419c",
            "subnet-0ac4a7fdbd803039c",
            "subnet-046a3dcfabce11b5f"
        ],
        "status": "ACTIVE"
    }

 1. In this lab, we will provision frontend pods to Fargate type. First, delete the existing frontend pod. Work with the command below in the folder where the yaml file is located.

    kubectl delete -f frontend-deployment.yaml

 1. Modify frontend-deployment.yaml file. Compared with previous yaml file, you can see that the value of label changed from **demo-frontend** to **frontend-fargate**. In step 1, when the pod meet the condition that key=app, value=frontend-fargate and namespace=default, eks cluster deploy pod to Fargate type.

    cd /home/ec2-user/environment/manifests

    cat <<EOF> frontend-deployment.yaml
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: demo-frontend
      namespace: default
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: frontend-fargate
      template:
        metadata:
          labels:
            app: frontend-fargate
        spec:
          containers:
            - name: demo-frontend
              image: $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/demo-frontend:latest
              imagePullPolicy: Always
              ports:
                - containerPort: 80
    EOF

 1. Modify frontend-service.yaml file.

    cat <<EOF> frontend-service.yaml
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: demo-frontend
      annotations:
        alb.ingress.kubernetes.io/healthcheck-path: "/"
    spec:
      selector:
        app: frontend-fargate
      type: NodePort
      ports:
        - protocol: TCP
          port: 80
          targetPort: 80
    EOF

 1. Deploy manifest file.

    kubectl apply -f frontend-deployment.yaml
    kubectl apply -f frontend-service.yaml

 1. With the command below, you can see that demo-frontend pods are provisioned at NOMINATED NODE.

    kubectl get pod -o wide

Or, you can check the list of Fargate worker nodes by following command.

    kubectl get nodes -l eks.amazonaws.com/compute-type=fargate

 1. You can also paste the results of the command below into the web browser to see the same screen as before.

    echo http://$(kubectl get ingress/frontend-ingress -o jsonpath='{.status.loadBalancer.ingress[*].hostname}')

## Explore Container Insights

## [Amazon CloudWatch Container Insight](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/90-monitoring#amazon-cloudwatch-container-insight)

![](https://cdn-images-1.medium.com/max/3692/0*oisKyz_OhDoDhcZ4.png)

Use **CloudWatch Container Insights** to collect, aggregate, and summarize metrics and logs from your containerized applications and microservices. Container Insights is available for Amazon Elastic Container Service (Amazon ECS), Amazon Elastic Kubernetes Service (Amazon EKS), and Kubernetes platforms on Amazon EC2. Amazon ECS support includes support for Fargate.

CloudWatch automatically collects metrics for many resources, such as CPU, memory, disk, and network. Container Insights also provides diagnostic information, such as container restart failures, to help you isolate issues and resolve them quickly. You can also set CloudWatch alarms on metrics that Container Insights collects.

## Explorer EKS CloudWatch Container Insights

In this lab, you will use [**Fluent Bit **](https://fluentbit.io/)to route logs. The lab order will install **CloudWatch Agent **to collect metric of the cluster and **Fluent Bit** to send logs to CloudWatch Logs in DaemonSet type.

![](https://cdn-images-1.medium.com/max/2296/0*K35iHBYwK8xVBwxz.png)

First, create a folder to manage manifest files.

    cd ~/environment
    mkdir -p manifests/cloudwatch-insight && cd manifests/cloudwatch-insight

### [Install CloudWatch agent, Fluent Bit](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/90-monitoring/100-build-insight#install-cloudwatch-agent-fluent-bit)

In [Create EKS Cluster with eksctl](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/50-eks-cluster/100-launch-cluster), **cloudWatch** related permissions were placed in the worker node.

 1. Create namespace named amazon-cloudwatch by following command.

    kubectl create ns amazon-cloudwatch

[!] If namespace created successfully, the namespace will exist in the list using the command below.

    kubectl get ns

 1. After specifying some settings values, install CloudWatch agent and Fluent Bit. Copy and paste one line at a time.

    ClusterName=eks-demo
    RegionName=$AWS_REGION
    FluentBitHttpPort='2020'
    FluentBitReadFromHead='Off'
    [[ ${FluentBitReadFromHead} = 'On' ]] && FluentBitReadFromTail='Off'|| FluentBitReadFromTail='On'
    [[ -z ${FluentBitHttpPort} ]] && FluentBitHttpServer='Off' || FluentBitHttpServer='On'

For **RegionName**, make sure it contains AWS Region code that you are currently working on. For instance, **ap-northeast-2** if you are working on in Seoul Region.

Then, through the command below, download yaml file.

    wget https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluent-bit-quickstart.yaml

And then, apply environment variables into this yaml file.

    sed -i 's/{{cluster_name}}/'${ClusterName}'/;s/{{region_name}}/'${RegionName}'/;s/{{http_server_toggle}}/"'${FluentBitHttpServer}'"/;s/{{http_server_port}}/"'${FluentBitHttpPort}'"/;s/{{read_from_head}}/"'${FluentBitReadFromHead}'"/;s/{{read_from_tail}}/"'${FluentBitReadFromTail}'"/' cwagent-fluent-bit-quickstart.yaml

Open this yaml file, find **DaemonSet** object which name is fluent-bit and add the values below the *spec*.

    affinity:
      nodeAffinity:
        requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: eks.amazonaws.com/compute-type
              operator: NotIn
              values:
              - fargate

The results of extracting some of the pasted ones are as follows. Take care indentation.

![](https://cdn-images-1.medium.com/max/2820/0*qpV6WWVydRXz_1CJ.png)

Deploy yaml file.

    kubectl apply -f cwagent-fluent-bit-quickstart.yaml

 1. Use the command below to check that the installation was succeeded. As a result, three cloudwatch-agent pods and three fluid-bit pods are available.

    kubectl get po -n amazon-cloudwatch

You can also check it through the command below. You can see that two Daemonsets are output.

    kubectl get daemonsets -n amazon-cloudwatch

### [Dive into the CloudWatch console](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/90-monitoring/100-build-insight#dive-into-the-cloudwatch-console)

* Log in [Amazon CloudWatch console ](https://console.aws.amazon.com/cloudwatch), click **Container Insights **under **Insights** menu in the left sidebar.

![](https://cdn-images-1.medium.com/max/4000/0*v6hJv0uYiCPZ3WqL.png)

* When you click **Map View** in the upper right corner, the cluster’s resources are displayed in tree form. You can also click on a particular object to see the associated metric values as shown below.

![](https://cdn-images-1.medium.com/max/4000/0*ric1KVMo7QG0e6Ua.png)

* Then select **Performance monitoring** in the above select bar. And if you click **EKS Services** at the top, you can see the metric values in terms of service as shown below.

![](https://cdn-images-1.medium.com/max/2654/0*n3cYFt-RcEDURQGc.png)

* Also, select a specific pod from **Pod performance** section, then click **View performance logs** in the dropbox to the right.

![](https://cdn-images-1.medium.com/max/2880/0*g2-oxRrP1rFG4yN0.png)

You will be redirected to the **CloudWatch Logs Insights** page as shown below. The query allows you to view the logs you want.

![](https://cdn-images-1.medium.com/max/2686/0*00BnJCCQ6zkM1Il0.png)

## Autoscaling Pod & Cluster

## [Kubernetes Auto Scaling](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/100-scaling#kubernetes-auto-scaling)

Auto scaling service means that the ability to automatically create or delete servers based on user-defined cycles and events. Auto scaling enables applications to respond flexibly to traffic.

Kubernetis has two main auto-scaling capabilities.

* HPA(Horizontal Pod AutoScaler)

* Cluster Autoscaler

HPA automatically scales the number of pods by observing CPU usage or custom metrics. However, if you run out of EKS cluster’s own resources to which the pod goes up, consider Cluster Autoscaler.

Applying these auto-scaling capabilities to a cluster allows you to configure a more resilient and scalable environment.

## Apply HPA

### [Applying Pod Scaling with HPA](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/100-scaling/100-pod-scaling#applying-pod-scaling-with-hpa)

The HPA(Horizontal Pod Autoscaler) controller allocates the number of pods based on metric. To apply pod scaling, you must specify the amount of resources required for the container and create conditions to scale through HPA.

![](https://cdn-images-1.medium.com/max/2000/1*hbvdRchoKNWhldNxqZ_ZUA.png)

 1. Create **metrics server**. **Metrics Server** aggregates resource usage data across the Kubernetes cluster. Collect metrics such as the CPU and memory usage of the worker node or container through kubelet installed on each worker node.

* kubectl apply -f [https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml](https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml)

 1. Use the command below to check that the metrics server is created successfully.

* kubectl get deployment metrics-server -n kube-system

 1. And then, modify flask deployment yaml file that you created in [Deploy First Backend Service](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/70-deploy-service/100-flask-backend). Change replicas to 1 and set the amount of resources required for the container.

* cd /home/ec2-user/environment/manifests cat <<EOF> flask-deployment.yaml --- apiVersion: apps/v1 kind: Deployment metadata: name: demo-flask-backend namespace: default spec: replicas: 1 selector: matchLabels: app: demo-flask-backend template: metadata: labels: app: demo-flask-backend spec: containers: - name: demo-flask-backend image: $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/demo-flask-backend:latest imagePullPolicy: Always ports: - containerPort: 8080 resources: requests: cpu: 250m limits: cpu: 500m EOF

1vCPU = 1000m(milicore)

 1. Apply the yaml file to reflect the changes.

* kubectl apply -f flask-deployment.yaml

 1. To set up HPA, create the yaml file below as well.

* cat <<EOF> flask-hpa.yaml --- apiVersion: autoscaling/v1 kind: HorizontalPodAutoscaler metadata: name: demo-flask-backend-hpa namespace: default spec: scaleTargetRef: apiVersion: apps/v1 kind: Deployment name: demo-flask-backend minReplicas: 1 maxReplicas: 5 targetCPUUtilizationPercentage: 30 EOF

Deploy yaml file.

    kubectl apply -f flask-hpa.yaml

You can set to this with simple kubectl command.

    kubectl autoscale deployment demo-flask-backend --cpu-percent=30 --min=1 --max=5

 1. After creating HPA, you can check the HPA status with the command below. If the target shows CPU usage as unknown, wait a moment and check.

* kubectl get hpa

 1. Perform a simple load test to check that the autoscaling functionality is working properly. First, enter the command below to understand the amount of change in the pod.

* kubectl get hpa -w

In addition, create additional terminals for load testing in AWS Cloud9. HTTP load testing through siege tool.

    sudo yum -y install siege

    export flask_api=$(kubectl get ingress/flask-backend-ingress -o jsonpath='{.status.loadBalancer.ingress[*].hostname}')/contents/aws

    siege -c 200 -i [http://$flask_api](http://$flask_api)

As shown in the screen below, you can load one side terminal and observe the amount of change in the pod accordingly on the other side. You can see that the **REPLICAS** value changes by up to 5 depending on the load.

![](https://cdn-images-1.medium.com/max/3832/0*VtDzh8zrAO1kpRmb.png)

## Apply Cluster Autoscaler

### [Applying Cluster Scaling with Cluster Autoscaler](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/100-scaling/200-cluster-scaling#applying-cluster-scaling-with-cluster-autoscaler)

Auto scaling was applied to the pod on the previous chapter. However, depending on the traffic, there may be insufficient Worker Node resources for the pod to increase. In other words, it’s full of Worker Nodes’ capacity and no more pod can’t be scheduled. At this point, what we use is **Cluster Autoscaler(CA)**.

![](https://cdn-images-1.medium.com/max/2002/1*6IEwAacuYf63_FftKzu0ug.png)

Cluster Autoscaler(CA) scales out the worker node if a pod in the pending state exists. Perform scale-in/out by checking utilization at intervals of a particular time. AWS also uses Auto Scaling Group to apply Cluster Autoscaler.

![](https://cdn-images-1.medium.com/max/2456/0*yupD3tsuJDgCy3kb.png)

[!] (Optional) To visualize the status of the current cluster, see [kube-ops-view](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/100-scaling/300-kube-ops-view).

 1. Use the command below to check **the value of ASG(Auto Scaling Group)** applied to the current cluster’s worker nodes.

* aws autoscaling \ describe-auto-scaling-groups \ --query "AutoScalingGroups[? Tags[? (Key=='eks:cluster-name') && Value=='eks-demo']].[AutoScalingGroupName, MinSize, MaxSize,DesiredCapacity]" \ --output table

In this lab, when deploying an EKS cluster, we performed the task of attaching IAM policies related to autoscaling. However, if you have not done so, click the hidden folder below to create the relevant IAM policy and attach it to the IAM role.

Creating an Auto Scaling IAM policy and attaching it to a worker nodes’ IAM Role

 1. In [Auto Scaling Groups ](http://console.aws.amazon.com/ec2/)page, click ASG applied in worker node, and update **Group details** value same as below.

![](https://cdn-images-1.medium.com/max/4000/0*CD817gQ7XQ2JoU7M.png)

![](https://cdn-images-1.medium.com/max/4000/0*ZE6YZSaO4YXlP29X.png)

![](https://cdn-images-1.medium.com/max/2352/0*cFHjH831YC7K2YG_.png)

 1. Return to your Cloud9 environment and download the deployment example file provided by the **Cluster Atuoscaler** project.

* cd /home/ec2-user/environment/manifests wget [https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml](https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml)

 1. Open the downloaded yaml file, set the cluster name from <YOUR CLUSTER NAME> to eks-demo and deploy it.

* ... command: - ./cluster-autoscaler - --v=4 - --stderrthreshold=info - --cloud-provider=aws - --skip-nodes-with-local-storage=false - --expander=least-waste - --node-group-auto-discovery=asg:tag=k8s.io/cluster-autoscaler/enabled,k8s.io/cluster-autoscaler/eks-demo ...

    kubectl apply -f cluster-autoscaler-autodiscover.yaml

 1. Perform a simple load test to check that the autoscaling functionality is working properly. First, enter the command below to understand the change in the number of worker nodes.

    kubectl get nodes -w

Then turn on the new terminal, and then perform a command to deploy 100 pods to increase the worker node.

    kubectl create deployment autoscaler-demo --image=nginx
    kubectl scale deployment autoscaler-demo --replicas=100

To check the progress of the pod’s deployment, perform the following command.

    kubectl get deployment autoscaler-demo --watch

 1. If **kube-ops-view** is installed, you can visually see the results below. This shows that two additional worker nodes were created and 100 pods were deployed.

![](https://cdn-images-1.medium.com/max/2000/0*-FebWXM-SuqhOckc.png)

 1. If you delete a previously created pods with the command below, you can see that the worker node will be scaled in.

    kubectl delete deployment autoscaler-demo

## Install Kubernetes Operational View

### [Install Kubernetes Operational View](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/100-scaling/300-kube-ops-view#install-kubernetes-operational-view)

[Kubernetes Operational View ](https://codeberg.org/hjacobs/kube-ops-view)is a simple web page that provides a visual view of the health of multiple Kubernetes clusters. Although not used for monitoring and operations management purposes, you can visually observe the process of scale-in/out during cluster autoscaling operations, such as **Cluster Autoscaler**.

In this lab, we deploy kube-ops-view via [**Helm **](https://helm.sh/). Helm is a tool for managing Kubernetes charts, which means a preconfigured Kubernetes resource package. The purpose of managing charts with Helm is to manage various manifest files easily.

[Install Helm](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/100-scaling/300-kube-ops-view#install-helm)

 1. Before configuring Helm, install the helm cli tool.

    curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

Check the current version through the command below.

    helm version --short

 1. Use the command below to add a stable repository.

    helm repo add stable https://charts.helm.sh/stable

    helm repo add k8s-at-home https://k8s-at-home.com/charts/

    helm repo update

 1. (Optional) Configure Bash completion for the helm command.

    helm completion bash >> ~/.bash_completion
    . /etc/profile.d/bash_completion.sh
    . ~/.bash_completion
    source <(helm completion bash)

[Install kube-ops-view](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/100-scaling/300-kube-ops-view#install-kube-ops-view)

 1. Install kube-ops-view through helm.

    helm install kube-ops-view k8s-at-home/kube-ops-view

 1. Check the chart was installed successfully.

    helm list

 1. Get the application URL by running these commands.

    export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=kube-ops-view,app.kubernetes.io/instance=kube-ops-view" -o jsonpath="{.items[0].metadata.name}")

    echo "Visit http://127.0.0.1:8080 to use your application"

    kubectl port-forward $POD_NAME 8080:8080

 1. In case of Cloud9, click the preview > preview running application which placed in the upper end. You can observe screen like below.

![](https://cdn-images-1.medium.com/max/3340/0*5jeM9nBfrgG97q1t.png)

## CI/CD for EKS cluster

## [CI/CD pipeline for EKS Cluster / Kubernetes Cluster](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/110-cicd#cicd-pipeline-for-eks-cluster-kubernetes-cluster)

Shape of CI/CD pipeline for either EKS or Kubernetes varies. This is because; 1/many different kinds of tools for CI/CD are out there, 2/each of dev/ops team’s culture that embraces and uses those tools is also diverse.

Given that situation, this tutorial amis to introduce the shape of CI/CD pipeline that is

* easy and speedy to implement

* to minimize manual task rather than automation

On top of that, the goal CI/CD pipeline in this tutorial will automatically detect application code changes in **Github**, and then trigger **Github Action** to integrate and buid the code changes. At the end, **ArgoCD** will be subsequently executed to deploy built artifacts to the target, EKS cluster. For pieces of block helping to automate this flow, we will introduce **Kustomize** that is tool to package kubernetes manifest up, **Checkov** and **Tryvy** for static analysis to secure EKS cluster running on.

* GitHub

* GitHub Actions

* Kustomize

* ArgoCD

* Checkov

* Trivy

The goal of CI/CD pipeline will like below, which is also called as **gitops** flow.

![](https://cdn-images-1.medium.com/max/2000/0*LTEN1pUTVQvUhQ-o.png)

## [CI/CD pipeline for EKS Cluster / Kubernetes Cluster with cdk helm](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/110-cicd#cicd-pipeline-for-eks-cluster-kubernetes-cluster-with-cdk-helm)

Shape of CI/CD pipeline for either EKS or Kubernetes varies. This is because; 1/many different kinds of tools for CI/CD are out there, 2/each of dev/ops team’s culture that embraces and uses those tools is also diverse.

Given that situation, this tutorial amis to introduce the shape of CI/CD pipeline that is

* easy and speedy to implement

* to minimize manual task rather than automation

On top of that, the goal CI/CD pipeline in this tutorial will automatically detect application code changes in **CodeCommit**, and then trigger **Codebuild** to integrate and buid the code changes. At the end, **ArgoCD** will be subsequently executed to deploy built artifacts to the target, EKS cluster. For pieces of block helping to automate this flow, we will introduce **Helm** that is tool to package kubernetes manifest up, **Checkov** and **Tryvy** for static analysis to secure EKS cluster running on.

* CodeCommit

* CodeBuild

* CodePipeline

* Helm

* ArgoCD

* Checkov

* Trivy

The goal of CI/CD pipeline will like below, which is also called as **gitops** flow.

![](https://cdn-images-1.medium.com/max/2000/0*nsbkZilyO6wM2flU.png)

## Create CI/CD pipeline

### [Build up CI/CD pipeline](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/110-cicd/100-cicd#build-up-cicd-pipeline)

Target CI/CD pipeline looks like this.

![](https://cdn-images-1.medium.com/max/2000/0*fApB9Yb-FsMVQAZ_.png)

**1. Create two git repository for application, kubernetes manifest each**

We need to have two github repository in place.

* ***front-app-repo***: located front-end application source code in

* ***k8s-manifest-repo***: located kubernetes manifest files in

**(1)** Create ***front-app-repo***

![](https://cdn-images-1.medium.com/max/2000/0*VDD6r0mYMoyFcsdk.png)

**(2)** initialize local code directory

You should change **“your-github-username”** to your own git user name.

    cd ~/environment/amazon-eks-frontend
    rm -rf .git
    export GITHUB_USERNAME=your-github-username

**(3)** Configure git remote repository locally

Push front-end source code to ***front-app-repo*** you just created.

    cd ~/environment/amazon-eks-frontend
    git init
    git add .
    git commit -m "first commit"
    git branch -M main
    git remote add origin https://github.com/$GITHUB_USERNAME/front-app-repo.git
    git push -u origin main

Confirm that it is all set.

![](https://cdn-images-1.medium.com/max/2000/0*71MtIg7TOCoV9VXp.png)

If you feel reluctant to submit username and password every single time you login, you can set cache to avoid it as below.

    git config --global user.name USERNAME
    git config --global user.email EMAIL
    git config credential.helper store
    git config --global credential.helper 'cache --timeout TIME YOU WANT'

If you are already using github MFA authentication, you are asked to use personal access token for password. To generate personal access token, you can follow [this github ](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token)guidance on it. Once you get the token from there, you can use the token when asked to submit password.

**2. Prepare least privilege IAM to use in CI/CD pipeline**

gitHub Action plays main role to build front-end application, build its docker container image and push it to ECR at the end. So, to make gitHub Action access ECR securly, we are strongly recommended to use seperate IAM with least privilege, and which will limit gitHub Action to access to ECR only.

**(1)** Create IAM user

    aws iam create-user --user-name github-action

**(2)** Create ECR policy

Make json file containg policy to ECR

    
    cd ~/environment
    cat <<EOF> ecr-policy.json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "AllowPush",
                "Effect": "Allow",
                "Action": [
                    "ecr:GetDownloadUrlForLayer",
                    "ecr:BatchGetImage",
                    "ecr:BatchCheckLayerAvailability",
                    "ecr:PutImage",
                    "ecr:InitiateLayerUpload",
                    "ecr:UploadLayerPart",
                    "ecr:CompleteLayerUpload"
                ],
                "Resource": "arn:aws:ecr:${AWS_REGION}:${ACCOUNT_ID}:repository/demo-frontend"
            },
            {
                "Sid": "GetAuthorizationToken",
                "Effect": "Allow",
                "Action": [
                    "ecr:GetAuthorizationToken"
                ],
                "Resource": "*"
            }
        ]
    }
    EOF

Using the file you made, create IAM policy. Policy name is ecr-policy recommended.

    aws iam create-policy --policy-name ecr-policy --policy-document file://ecr-policy.json

**(3)** Attach ECR policy to IAM user

Attach ecr-policy to IAM user you create previously.

    aws iam attach-user-policy --user-name github-action --policy-arn arn:aws:iam::${ACCOUNT_ID}:policy/ecr-policy

**3. Create githup secrets(AWS Credential, githup token)**

gitHub Action we make will store and use AWS credential, github token in githup secrets. This way, we can secure those secrets without being exposed unintentinally.

**(1)** Generate AWS Credential

When gitHub Action push the docker image of front-end application, it uses AWS credential. For this working, we have created github-action, IAM user with least privilege. Now, create Access Key, Secret Key for the IAM User.

    aws iam create-access-key --user-name github-action

Make a note of values of "SecretAccessKey", "AccessKeyId" out of output. Those will be used going forward in this tutorial.

    {
      "AccessKey": {
        "UserName": "github-action",
        "Status": "Active",
        "CreateDate": "2021-07-29T08:41:04Z",
        "SecretAccessKey": "***",
        "AccessKeyId": "***"
      }
    }

**(2)** Generate gitHub personal token

Once log in github.com, naviate **profile > Settings > Developer settings > Personal access tokens**. Finally, click on **Generate new token** in the top right corner.

Type access token for github action in Note, and then select **repo** in **Select scopes**. Finally, click **Generate token**

![](https://cdn-images-1.medium.com/max/2176/0*Tj-aWdoBe6l0bWWW.png)

Copy value of token in the output.

![](https://cdn-images-1.medium.com/max/2000/0*x7kdQTyPSUQghTkJ.png)

**(3)** Set up gitHub secret

Go back to **front-app-repo** repository and navigate **Settings > Secrets**. And click **New repository secret** in the top right corner.

As below screen shot, put ACTION_TOKEN, personal access token in **Name**, **Value** respectively.(*You must have copied personal access token in the previous step). Finally click **Add secret**

![](https://cdn-images-1.medium.com/max/2082/0*T3UzELwk8nV1I8B9.png)

Similar way, store both AccessKeyId and SecretAccessKey that github-action will use in gitHub secret. Note that **Name** of AccessKeyId and SecretAccessKey must be AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY each.

![](https://cdn-images-1.medium.com/max/2000/0*HDl_eEuMgLNaqATj.png)

![](https://cdn-images-1.medium.com/max/2000/0*KkHjpU7mf4IJYSpN.png)

**4. Make build script for gitHub Action to use**

**(1)** Make .github directory

    cd ~/environment/amazon-eks-frontend
    mkdir -p ./.github/workflows

**(2)** Make build.yaml for gitHub Action to use

gitHub Action will be running based on tasks that build.yaml contains. So we need to declare tasks that gitHub Action execute for us in build.yaml. build.yamlwill include checkout, build front-end application, make docker image, and push it to ECR.

Most eye-catching part in the script is procedure to dynamically put **docker image tag**. **We intend to have **$IMAGE_TAG **that is dynamically and randomly created to attach docker image built.**

    cd ~/environment/amazon-eks-frontend/.github/workflows
    cat > build.yaml <<EOF
    name: Build Front
    on:
      push:
        branches: [ main ]
    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout source code
            uses: actions/checkout@v2
          - name: Check Node v
            run: node -v
          - name: Build front
            run: |
              npm install
              npm run build
          - name: Configure AWS credentials
            uses: aws-actions/configure-aws-credentials@v1
            with:
              aws-access-key-id: \${{ secrets.AWS_ACCESS_KEY_ID }}
              aws-secret-access-key: \${{ secrets.AWS_SECRET_ACCESS_KEY }}
              aws-region: $AWS_REGION
          - name: Login to Amazon ECR
            id: login-ecr
            uses: aws-actions/amazon-ecr-login@v1
          - name: Get image tag(verion)
            id: image
            run: |
              VERSION=\$(echo \${{ github.sha }} | cut -c1-8)
              echo VERSION=\$VERSION
              echo "::set-output name=version::\$VERSION"
          - name: Build, tag, and push image to Amazon ECR
            id: image-info
            env:
              ECR_REGISTRY: \${{ steps.login-ecr.outputs.registry }}
              ECR_REPOSITORY: demo-frontend
              IMAGE_TAG: \${{ steps.image.outputs.version }}
            run: |
              echo "::set-output name=ecr_repository::\$ECR_REPOSITORY"
              echo "::set-output name=image_tag::\$IMAGE_TAG"
              docker build -t \$ECR_REGISTRY/\$ECR_REPOSITORY:\$IMAGE_TAG .
              docker push \$ECR_REGISTRY/\$ECR_REPOSITORY:\$IMAGE_TAG
    EOF

**(3)** Run gitHub Action workflow

Push code to *front-app-repo* to trigger gitHub Action workflow. gitHub Action workflow will automatically be triggered on push of code. gitHub Action workflow will run step-by-step based on build.yaml we’ve declared.

    cd ~/environment/amazon-eks-frontend
    git add .
    git commit -m "Add github action build script"
    git push origin main

Return to gitHub page and comfirm push is done. Also comfirm that gitHub Action workflow works exepectedly as below screen shots.

![](https://cdn-images-1.medium.com/max/2990/0*u3u7lIdLAaW5WNlR.png)

![](https://cdn-images-1.medium.com/max/3534/0*LF1bbojMoqNm9-uu.png)

If you confirmed the workflow finished successfully, go to ECR repository, demo-frontend to see if new docker imgage is pushed with new $IMAGE_TAG.

Check if the image tag contains part of sha value.

![](https://cdn-images-1.medium.com/max/2850/0*MaTf9VXsgxGKj3fm.png)

**5. Kustomize Overview**

In this tutorial, we are going to use Kustomize to simply inject same value of label, metadata, etc. into kubernetes Deployment objects. This helps us to avoid hassle job to modify mannually value in each of kubernetes objects. Most importantly, we are to use Kustomize to not only automatically, but also dynamically assign image tag to kubernetes Deployment.

Please discover more details on Kustomize here, [Kustomize official document](https://kustomize.io/)

**6. Structure directories for Kustomize**

**(1)** Make directories

Now that kubernetes manifest owns seperated github repository. On top of that, we are going to package it to deploy using Kustomize. For this we need to make directories that Kustomize can run accordingly. The struction of directories should follow predefined naming rule.

    cd ~/environment
    mkdir -p ./k8s-manifest-repo/base
    mkdir -p ./k8s-manifest-repo/overlays/dev
    cd ~/environment/manifests
    cp *.yaml ../k8s-manifest-repo/base
    cd ../k8s-manifest-repo/base
    ls -rlt

Outcome of directories has *base* and *overlays/dev* under *k8s-manifest-repo*.

* *base* : raw kubernetes manifest files are here. During kustomize build process, those files in here will be automatically modified along with customized content by users in **kustomize.yaml** under *overlays*.

* *overlays* : **customized content by users** is in **kustomize.yaml** under this directory. Also note that *dev* directory is to put all relevant files for deploying to dev environment. In this tutorial, we assume that we deploy to dev environment accordingly.

**(2)** Make Kustomize manifest files

Remember the goal of this tutorial is to make deployment pipeline for front-end application. So, we will change and replace some values in frontend-deployment.yaml and frontend-service.yaml with values we intend to inject during deployment step(e.g. image tag). These are values we are definitely to inject dynamically into associated kubernetes manifest files.

* metadata.labels**:** "env: dev" will be reflected to frontend-deployment.yaml, frontend-service.yaml

* spec.selector : "select.app: frontend-fargate" will be reflected to frontend-deployment.yaml, frontend-service.yaml

* spec.template.spec.containers.image : "image: " with newly created image tag will be reflected to frontend-deployment.yaml

![](https://cdn-images-1.medium.com/max/2000/0*MvO2chJTANv2VwtQ.png)

Make kustomize.yamlas below. Main purpose of this file is to define target files to be automatically injected by kustomize.

    cd ~/environment/k8s-manifest-repo/base
    cat <<EOF> kustomization.yaml
    apiVersion: kustomize.config.k8s.io/v1beta1
    kind: Kustomization
    resources:
      - frontend-deployment.yaml
      - frontend-service.yaml
    EOF

Next, it’s time to make files that contain what to inject into target files that kustomize.yamldefines in the previous step. First, make a file to inject into frontend-deployment.yaml.

    cd ~/environment/k8s-manifest-repo/overlays/dev
    cat <<EOF> front-deployment-patch.yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: demo-frontend
      namespace: default
      labels:
        env: dev
    spec:
      selector:
        matchLabels:
          app: frontend-fargate
      template:
        metadata:
          labels:
            app: frontend-fargate
    EOF

Also, make make a file to inject into frontend-service.yaml.

    cd ~/environment/k8s-manifest-repo/overlays/dev
    cat <<EOF> front-service-patch.yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: demo-frontend
      annotations:
        alb.ingress.kubernetes.io/healthcheck-path: "/"
      labels:
        env: dev
    spec:
      selector:
        app: frontend-fargate
    EOF

Lastly, improve kustomization.yaml to replace the name of image kubernetes deployment object refers to. The name of image will have to be with **image tag** that is randomly generated during build process of front-end application in gitHub Action workflow.

To be specific, value assigned to name will be replaced with value of a combination of newNameand newTag

Run this code.

    cd ~/environment/k8s-manifest-repo/overlays/dev
    cat <<EOF> kustomization.yaml
    apiVersion: kustomize.config.k8s.io/v1beta1
    kind: Kustomization
    images:
    - name: ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/demo-frontend
      newName: ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/demo-frontend
      newTag: abcdefg
    resources:
    - ../../base
    patchesStrategicMerge:
    - front-deployment-patch.yaml
    - front-service-patch.yaml
    EOF

As a result, content in XXXX-patch.yaml and value of images in kustomization.yaml will be automatically applied to kubernetes manifest on deployment to EKS cluster.

**7. Create gitHub repository for kubernetes manifest**

**(1)** gitHub repo for kubernetes manifest

Create respository in gitHub with name as ***k8s-manifest-repo***. This will persist kubernetes manifest files we’ve created so far.

![](https://cdn-images-1.medium.com/max/2060/0*WIsNvcDbKGCX08ph.png)

Push kubernetes manifest files to ***k8s-manifest-repo***

    cd ~/environment/k8s-manifest-repo/
    git init
    git add .
    git commit -m "first commit"
    git branch -M main
    git remote add origin https://github.com/$GITHUB_USERNAME/k8s-manifest-repo.git
    git push -u origin main

**8. Set up ArgoCD**

**(1)** Install ArgoCD in EKS cluster

Run this code to install ArgoDC in EKS cluster.

    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

ArgoCD also provides CLI to users. Install ArgoCD CLI, we are not using it in this tutorial going on though.

    cd ~/environment
    VERSION=$(curl --silent "https://api.github.com/repos/argoproj/argo-cd/releases/latest" | grep '"tag_name"' | sed -E 's/.*"([^"]+)".*/\1/')

    sudo curl --silent --location -o /usr/local/bin/argocd [https://github.com/argoproj/argo-cd/releases/download/$VERSION/argocd-linux-amd64](https://github.com/argoproj/argo-cd/releases/download/$VERSION/argocd-linux-amd64)

    sudo chmod +x /usr/local/bin/argocd

Basically ArgoCD is not directly exposed to external, so we need to set up ELB in front of ArgoCD for incoming transactions.=

    kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

It may take 3~4 mins to be reachable via ELB. Run this code to get the URL of ELB.

    export ARGOCD_SERVER=`kubectl get svc argocd-server -n argocd -o json | jq --raw-output .status.loadBalancer.ingress[0].hostname`
    echo $ARGOCD_SERVER

ArgoCD default username is admin. Get password against it with this command.

    ARGO_PWD=`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`
    echo $ARGO_PWD

Open $ARGOCD_SERVER in your local web browser with Username = adminand Password = $ARGO_PWD.

![](https://cdn-images-1.medium.com/max/2428/0*Ou27L4ConMaqfwcn.png)

**9. Configure ArgoCD**

**(1)** Configure ArgoCD

After logging in, Click **Applicaions** to configure in the top left corner.

![](https://cdn-images-1.medium.com/max/2000/0*2cxuINsCsnZHB89u.png)

Next, input basic information about target deployment of application. **Application Name** and **Project** should beeksworkshop-cd-pipeline and default each.

![](https://cdn-images-1.medium.com/max/2000/0*xZQDzLyN7QdtV61i.png)

**Repository URL** , **Revision**, **Path** in section of **SOURCE** must be **git address of **k8s-manifest-repo, main, and overlays/dev each.

![](https://cdn-images-1.medium.com/max/2000/0*HtC76dYjmAW4mduZ.png)

**Cluster URL** and **Namespace** in section of **DESTINATION** must be https://kubernetes.default.svc and default each. After input, click **Create**

![](https://cdn-images-1.medium.com/max/2000/0*6TXqIrMbDeMvPPO0.png)

Appropriate result will be named **eksworkshop-cd-pipeline** as below.

![](https://cdn-images-1.medium.com/max/2000/0*Al95YA1mZzSuM1Sr.png)

**10. Add Kustomize build step**

**(1)** Improve gitHub Action build script Add this code in build.yamlfor ***front-app-repo***. This code will update container image tag in kubernetes manifest files using kustomize. Afte that, it will commit and push those files to ***k8s-manifest-repo***.

When it successfully finishes, **ArgoCD** watching ***k8s-manifest-repo*** will catch the new update and start deployment process afterward.

    cd ~/environment/amazon-eks-frontend/.github/workflows
    cat <<EOF>> build.yaml
          - name: Setup Kustomize
            uses: imranismail/setup-kustomize@v1
          - name: Checkout kustomize repository
            uses: actions/checkout@v2
            with:
              repository: $GITHUB_USERNAME/k8s-manifest-repo
              ref: main
              token: \${{ secrets.ACTION_TOKEN }}
              path: k8s-manifest-repo
          - name: Update Kubernetes resources
            run: |
              echo \${{ steps.login-ecr.outputs.registry }}
              echo \${{ steps.image-info.outputs.ecr_repository }}
              echo \${{ steps.image-info.outputs.image_tag }}
              cd k8s-manifest-repo/overlays/dev/
              kustomize edit set image \${{ steps.login-ecr.outputs.registry}}/\${{ steps.image-info.outputs.ecr_repository }}=\${{ steps.login-ecr.outputs.registry}}/\${{ steps.image-info.outputs.ecr_repository }}:\${{ steps.image-info.outputs.image_tag }}
              cat kustomization.yaml
          - name: Commit files
            run: |
              cd k8s-manifest-repo
              git config --global user.email "github-actions@github.com"
              git config --global user.name "github-actions"
              git commit -am "Update image tag"
              git push -u origin main
    EOF

**(2)** Commit&push to front-app-repo

Commit and push newly improved build.yamlto ***front-app-repo*** to run gitHub Action workflow.

    cd ~/environment/amazon-eks-frontend
    git add .
    git commit -m "Add kustomize image edit"
    git push -u origin main

**(3)** Check github action

Check if gitHub Action workflow works fine.

![](https://cdn-images-1.medium.com/max/2402/0*Sxff2bvUFYMSZrK2.png)

**(4)** Check k8s-manifest-repo

Check if ***k8s-manifest-repo***’s latest commit is derived from **gitHub Action workflow of *front-app-repo***.

![](https://cdn-images-1.medium.com/max/2000/0*-jMbomxOf_AbPSJq.png)

**(5)** Check ArgoCD

Return to ArgoCD UI. Navigate **Applications > eksworkshop-cd-pipeline**. Now **CURRENT SYNC STATUS** is **Out of Synced**.

To run sync job automatically, we need to enable **Auto-Sync**. To do so, go to **APP DETAILS** and click **ENABLE AUTO-SYNC**.

![](https://cdn-images-1.medium.com/max/2260/0*4Kf-i8QjBORWvhOu.png)

As a result, ***k8s-manifest-repo***’s commit will be deployed to EKS Cluster.

![](https://cdn-images-1.medium.com/max/2190/0*-8YwhrBzgtGMKaAw.png)

To confirm that new image tag is deployed, check out ***k8s-manifest-repo*** commit history to get image tag information. And then, compare it with image tag that frontend-deploymentin ArgoCD used.

{{% notice info %}} You can see detail information when you click **pod** that starts with demo-frontend-. To get there, you might need to first navigate **Applications > eksworkshop-cd-pipeline >**. {{% /notice %}}

![](https://cdn-images-1.medium.com/max/2000/0*WymaKHxYvd0flFnj.png)

![](https://cdn-images-1.medium.com/max/2000/0*9mmKxj2-mus4v6o4.png)

From now on, on commit in ***k8s-manifest-repo***, ArgoCD automatically deploy the commit to EKS Cluster.

**11. Check CI/CD pipeline working from end to end**

Let’s test out whole gitops pipeline we’ve built by making code changes in front-end application.

**(1)** Change code

Go to **Cloud9** first, and then move to **amazon-eks-frontend/src/** and open App.js in folder tree of the left pane.

Replace code at **line 67** with EKS DEMO Blog version 1 and save it.

      return (
        <div className={classes.root}>
          <AppBar position="static" style={{ background: '#2E3B55' }}>
            <Toolbar>
              <IconButton edge="start" className={classes.menuButton} color="inherit" aria-label="menu">
                <CloudIcon />
              </IconButton>
              <Typography
                variant="h6"
                align="center"
                className={classes.title}
              >
                EKS DEMO Blog version 1
              </Typography>
              {new Date().toLocaleTimeString()}
            </Toolbar>
          </AppBar>
          <br/>

**(2)** Commit and push

Commit and push changed code to git repository.

    cd ~/environment/amazon-eks-frontend
    git add .
    git commit -m "Add new blog version"
    git push -u origin main

**(3)** Check CI/CD pipeline and application

Wait until ArgoCD sync job completes as below.

![](https://cdn-images-1.medium.com/max/2146/0*htJM71wc0nSz6J9h.png)

When it is all set, hit the application via URL from the below command.

    echo http://$(kubectl get ingress/frontend-ingress -o jsonpath='{.status.loadBalancer.ingress[*].hostname}')

It must show new code change as below.

![](https://cdn-images-1.medium.com/max/2472/0*jNx6VbqaxLuMaQPV.png)

## CI/CD with security

### [Improve CI/CD pipeline with security implementation](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/110-cicd/200-cicd-security#improve-cicd-pipeline-with-security-implementation)

Prior to deploying kubernetes manifest files to EKS Cluster, supplementary steps need to be added to prevent security and misconfiguration issue by using both [**Checkov **](https://github.com/bridgecrewio/checkov)and [**Trivy** ](https://github.com/aquasecurity/trivy). Also, we will use seperate ArgoCD account from admin user that we’ve used in the previous lab. This will follow ArgoCD RBAC rule to secure ArgoCD and EKS cluster ultimately.

For this, we will need to improve CD (Continuous Deploy) process as follows.

* On application code change, new docker image with new image tag is created

* Trivy inspects security vulnerability of the new image

* Kustomize starts making kubernetes manifest files with the new image information

* Checkov inspects security vulnerability and misconfiguration of kubernetes manifest files

* If no issue out there, ArgoCD starts sync job to deploy

Each of steps above is ran in the different gitHub Action workflow

* 1~2 : **github Action workflow of application repository**

* 3~5 : **github Action workflow of k8s manifest repository**

We will conduct followings to build it up.

* Improve **gitHub Action build script** in **frontend application repository**

* Improve **gitHub Action build script** in **k8s manifest repository**

* Deactivate **ArgoCD** ***AUTO_SYNC*** (***Manual***)

* Create new **ArgoCD** **account**

* Create ***auth-token*** for new **ArgoCD** **account**

* Configure **Argo RBAC** for new **ArgoCD** **account**

**1. Improve gitHub Action build script in frontend application repository**

We need additional step to ensure that newly created docker image has no security vulnerability prior to pushing it to ECR. For this we will modify build.yaml to add image scanning process using [**Trivy](https://github.com/aquasecurity/trivy)**

Run this code to change build.yaml for frontend application repo.

    cd ~/environment/amazon-eks-frontend/.github/workflows
    cat <<EOF> build.yaml
    name: Build Front
    
    on:
      push:
        branches: [ main ]
    
    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout source code
            uses: actions/checkout@v2
    
          - name: Check Node v
            run: node -v
    
          - name: Build front
            run: |
              npm install
              npm run build
    
          - name: Configure AWS credentials
            uses: aws-actions/configure-aws-credentials@v1
            with:
              aws-access-key-id: \${{ secrets.AWS_ACCESS_KEY_ID }}
              aws-secret-access-key: \${{ secrets.AWS_SECRET_ACCESS_KEY }}
              aws-region: $AWS_REGION
    
          - name: Login to Amazon ECR
            id: login-ecr
            uses: aws-actions/amazon-ecr-login@v1
    
          - name: Get image tag(verion)
            id: image
            run: |
              VERSION=\$(echo \${{ github.sha }} | cut -c1-8)
              echo VERSION=\$VERSION
              echo "::set-output name=version::\$VERSION"
    
          - name: Build, tag, and push image to Amazon ECR
            id: image-info
            env:
              ECR_REGISTRY: \${{ steps.login-ecr.outputs.registry }}
              ECR_REPOSITORY: demo-frontend
              IMAGE_TAG: \${{ steps.image.outputs.version }}
            run: |
              echo "::set-output name=ecr_repository::\$ECR_REPOSITORY"
              echo "::set-output name=image_tag::\$IMAGE_TAG"
              docker build -t \$ECR_REGISTRY/\$ECR_REPOSITORY:\$IMAGE_TAG .
    
          - name: Run Trivy vulnerability scanner
            uses: aquasecurity/trivy-action@master
            with:
              image-ref: '\${{ steps.login-ecr.outputs.registry}}/\${{ steps.image-info.outputs.ecr_repository }}:\${{ steps.image-info.outputs.image_tag }}'
              format: 'table'
              exit-code: '0'
              ignore-unfixed: true
              vuln-type: 'os,library'
              severity: 'CRITICAL,HIGH'
    
          - name: Push image to Amazon ECR
            run: |
              docker push \${{ steps.login-ecr.outputs.registry}}/\${{ steps.image-info.outputs.ecr_repository }}:\${{ steps.image-info.outputs.image_tag }}
    
          - name: Setup Kustomize
            uses: imranismail/setup-kustomize@v1
    
          - name: Checkout kustomize repository
            uses: actions/checkout@v2
            with:
              repository: $GITHUB_USERNAME/k8s-manifest-repo
              ref: main
              token: \${{ secrets.ACTION_TOKEN }}
              path: k8s-manifest-repo
    
          - name: Update Kubernetes resources
            run: |
              echo \${{ steps.login-ecr.outputs.registry }}
              echo \${{ steps.image-info.outputs.ecr_repository }}
              echo \${{ steps.image-info.outputs.image_tag }}
              cd k8s-manifest-repo/overlays/dev/
              kustomize edit set image \${{ steps.login-ecr.outputs.registry}}/\${{ steps.image-info.outputs.ecr_repository }}=\${{ steps.login-ecr.outputs.registry}}/\${{ steps.image-info.outputs.ecr_repository }}:\${{ steps.image-info.outputs.image_tag }}
              cat kustomization.yaml
    
          - name: Commit files
            run: |
              cd k8s-manifest-repo
              git config --global user.email "github-actions@github.com"
              git config --global user.name "github-actions"
              git commit -am "Update image tag"
              git push -u origin main
    
    EOF

Commit&push

    git add .
    git commit -m "Add Image Scanning in build.yaml"
    git push -u origin main

Afterwards, gitHub Action workflow will be executed and it shows the result of image scan.

![](https://cdn-images-1.medium.com/max/2540/0*aWcPGLi-tRC75Uuu.png)

We intended to have gitHub Action workflow move forward and complete even though image scan step fails. In real world, you might want to stop the workflow when image scan fails. For this you can set exit-code: '1'instead of exit-code: '0'in part of **trivy** step in build.yaml. For more details on this, please refer to [Trivy doc ](https://github.com/aquasecurity/trivy-action).

**2. Improve gitHub Action build script in k8s manifest repository**

    cd ~/environment/k8s-manifest-repo
    mkdir -p ./.github/workflows
    cd ~/environment/k8s-manifest-repo/.github/workflows
    cat <<EOF> build.yaml
    name: "ArgoCD sync"
    on: "push"
    
    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
    
          - name: Checkout source code
            uses: actions/checkout@v2
    
          - name: Setup Kustomize
            uses: imranismail/setup-kustomize@v1
    
          - name: Build Kustomize
            run: |
              pwd
              mkdir kustomize-build
              kustomize build ./overlays/dev > ./kustomize-build/kustomize-build-output.yaml
              ls -rlt
              cd kustomize-build
              cat kustomize-build-output.yaml
    
          - name: Run Checkov action
            id: checkov
            uses: bridgecrewio/checkov-action@master
            with:
              directory: kustomize-build/
              framework: kubernetes
    
          - name: Install ArgoCD and execute Sync in ArgoCD
            run: |
              curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
              chmod +x /usr/local/bin/argocd
              ARGO_SERVER=$ARGOCD_SERVER
              argocd app sync eksworkshop-cd-pipeline --auth-token \${{ secrets.ARGOCD_TOKEN }} --server $ARGOCD_SERVER --insecure
    
    EOF

**3. Deactivate ArgoCD *AUTO_SYNC *(*Manual*)**

Go to **Applicaiton > eksworkshop-cd-pipeline** and then click **APP DETAIL**. Next, change **SYNC_POLICY** with **DISABLE AUTO-SYNC**

![](https://cdn-images-1.medium.com/max/2244/0*PP0D0EkbqHC5R7If.png)

**4. Create new ArgoCD account**

To increase security of ArgoCD, we will use seperate ArgoCD account from admin user we used. Also we add role on top of the account.

New account name of ArgoCD for CI/CD pipeline is devops

ArgoCD allows us to add account via Configmap tha ArgoCD is using in the cluster.

Run this code.

    kubectl -n argocd edit configmap argocd-cm -o yaml

Next, add this code.

    data:
      accounts.devops: apiKey,login

Final code must be like this. (* createTimestamp, resourceVersion, etc differ depending on users environment)

    apiVersion: v1
    data:
      accounts.devops: apiKey,login
    kind: ConfigMap
    metadata:
      annotations:
        kubectl.kubernetes.io/last-applied-configuration: |
          {"apiVersion":"v1","kind":"ConfigMap","metadata":{"annotations":{},"labels":{"app.kubernetes.io/name":"argocd-cm","app.kubernetes.io/part-of":"argocd"},"name":"argocd-cm","namespace":"argocd"}}
      creationTimestamp: "2021-07-28T07:45:53Z"
      labels:
        app.kubernetes.io/name: argocd-cm
        app.kubernetes.io/part-of: argocd
      name: argocd-cm
      namespace: argocd
      resourceVersion: "153620981"
      selfLink: /api/v1/namespaces/argocd/configmaps/argocd-cm
      uid: a8bb80e7-577c-4f10-b3de-359e83ccee20

Finally, type :wq! to save and exit.

**5. Create *auth-token *for new ArgoCD account**

Let’s generate *auth-token* that new ArgoCD account, devops is using. This will be used for authentication token when we make api call to ArgoCD. So this is different credential from login password for ArgoCD UI.

Run this code and make a note of the output so that **we can continue to use it**

    argocd account generate-token --account devops

Failed to establish connection

If you are encountered “Failed to establish connection”,you should log in ArgoCD with following command.

    argocd login $ARGOCD_SERVER

To save token value from the output in **Secrets** of kubernetes maniffest repository, go to **Settings > Secrets**, and then click **New repository secret**. Finally, input ***ARGOCD_TOKEN*** and **saved token value** into **Name** and **Values** each, and then click **Add Secret**

![](https://cdn-images-1.medium.com/max/NaN/1*b31hiO4ynbDLRrXWEFF4aQ.png)

**6. Configure Argo RBAC for new ArgoCD account**

The new ArgoCD account we’ve created has no permission to make API call to sync. So, we need to grant it permission according to RBAC of ArgoCD.

To grant permission, run this code to modify argocd-rbac-cm, ArgoCD Configmap.

    kubectl -n argocd edit configmap argocd-rbac-cm -o yaml

Add this content. For lab purpose only, we intend to allow many of permissions. So please be mindful when you do this in production environment.

    data:
      policy.csv: |
        p, role:devops, applications, *, */*, allow
        p, role:devops, clusters, get, *, allow
        p, role:devops, repositories, get, *, allow
        p, role:devops, repositories, create, *, allow
        p, role:devops, repositories, update, *, allow
        p, role:devops, repositories, delete, *, allow
    
        g, devops, role:devops
      policy.default: role:readonly

Final content must be like this after adding the content. (* createTimestamp, resourceVersion, etc differ depending on users environment)

    apiVersion: v1
    data:
      policy.csv: |
        p, role:devops, applications, *, */*, allow
        p, role:devops, clusters, get, *, allow
        p, role:devops, repositories, get, *, allow
        p, role:devops, repositories, create, *, allow
        p, role:devops, repositories, update, *, allow
        p, role:devops, repositories, delete, *, allow
    
        g, devops, role:devops
      policy.default: role:readonly
    kind: ConfigMap
    metadata:
      annotations:
        kubectl.kubernetes.io/last-applied-configuration: |
          {"apiVersion":"v1","kind":"ConfigMap","metadata":{"annotations":{},"labels":{"app.kubernetes.io/name":"argocd-rbac-cm","app.kubernetes.io/part-of":"argocd"},"name":"argocd-rbac-cm","namespace":"argocd"}}
      creationTimestamp: "2021-07-28T07:45:53Z"
      labels:
        app.kubernetes.io/name: argocd-rbac-cm
        app.kubernetes.io/part-of: argocd
      name: argocd-rbac-cm
      namespace: argocd
      resourceVersion: "153629591"
      selfLink: /api/v1/namespaces/argocd/configmaps/argocd-rbac-cm
      uid: 1fe0d735-f3a0-4867-9357-7a9e766fef22

**7. Check new implementation working**

Commit and push the code

    cd ~/environment/k8s-manifest-repo
    git add .
    git commit -m "Add github action with ArgoCD"
    git push -u origin main

See if gitHub Action workflow completes and trigger ArgoCD deployment process.

gitHub Action workflow run into failure error as below. This is the result from **Checkov** ‘s static analysis on kubernetes manifest files. The result comes along with warning messages based on security best practice which is predefined in Checkov.

![](https://cdn-images-1.medium.com/max/2548/0*796tw6UxW3mXIiKl.png)

Since we confirmed **Checkov** works expectedly, we will narrow down the scope of analysis for lab purpose.

Run this code to scope **Checkov** analysis to CKV_K8S_17.

    cd ~/environment/k8s-manifest-repo/.github/workflows
    cat <<EOF> build.yaml
    name: "ArgoCD sync"
    on: "push"
    
    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
    
          - name: Checkout source code
            uses: actions/checkout@v2
    
          - name: Setup Kustomize
            uses: imranismail/setup-kustomize@v1
    
          - name: Build Kustomize
            run: |
              pwd
              mkdir kustomize-build
              kustomize build ./overlays/dev > ./kustomize-build/kustomize-build-output.yaml
              ls -rlt
              cd kustomize-build
              cat kustomize-build-output.yaml
    
          - name: Run Checkov action
            id: checkov
            uses: bridgecrewio/checkov-action@master
            with:
              directory: kustomize-build/
              framework: kubernetes
              check: CKV_K8S_17
    
          - name: Install ArgoCD and execute Sync in ArgoCD
            run: |
              curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
              chmod +x /usr/local/bin/argocd
              ARGO_SERVER=$ARGOCD_SERVER
              argocd app sync eksworkshop-cd-pipeline --auth-token \${{ secrets.ARGOCD_TOKEN }} --server $ARGOCD_SERVER --insecure
    
    EOF

Commit and push code.

    cd ~/environment/k8s-manifest-repo
    git add .
    git commit -m "Chage Checkov check scope"
    git push -u origin main

See if gitHub Action workflow completes and trigger ArgoCD deployment process. This time, ArgoCD will be successfully completed without interruption.

**8. Test out end-to-end pipeline working**

Let’s test out end-to-end pipeline working with code change on front-end application first.

**(1)** front-end application code change

Go to **Cloud9** first, and then move to **amazon-eks-frontend/src/** and open App.js in folder tree of the left pane.

Replace code at **line 67** with EKS DEMO Blog version 2 and save it.

      return (
        <div className={classes.root}>
          <AppBar position="static" style={{ background: '#2E3B55' }}>
            <Toolbar>
              <IconButton edge="start" className={classes.menuButton} color="inherit" aria-label="menu">
                <CloudIcon />
              </IconButton>
              <Typography
                variant="h6"
                align="center"
                className={classes.title}
              >
                EKS DEMO Blog version 2
              </Typography>
              {new Date().toLocaleTimeString()}
            </Toolbar>
          </AppBar>
          <br/>

**(2)** commit and push

Commit and push changed code to git repository.

    cd ~/environment/amazon-eks-frontend
    git add .
    git commit -m "Add new blog version 2"
    git push -u origin main

See if gitHub Action workflow completes and trigger ArgoCD deployment process. This time, ArgoCD will be successfully completed without interruption.

After end-to-end pipeline finishes successfully, open your local browser with URL of the application from this command.

    echo http://$(kubectl get ingress/backend-ingress -o jsonpath='{.status.loadBalancer.ingress[*].hostname}')

It must show you ***EKS DEMO Blog version 2*** at the top of page.

## Create CI/CD with HELM & CDK

## [Build up CI/CD pipeline](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/110-cicd/300-cicd#build-up-cicd-pipeline)

Below Image is a taget pipeline

![](https://cdn-images-1.medium.com/max/2000/0*4TdxDXNtltpT5ED9.png)

## [1. Deploy CDK stack](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/110-cicd/300-cicd#1.-deploy-cdk-stack)

Deploy pipeline with [AWS CDK ](https://aws.amazon.com/cdk/)for workshop. Below resources will be deployed.

* ***CodeCommit-App-Repo***: App Source repo

* ***CodeCommit-Helm-Repo***: Helm source

* ***CodeBuild-App***: Perform app build and push image to ecr

* ***CodeBuild-Helm***: Update image tag and push to helm repo

* ***CodePipeline***: Pipeline

* ***ECR-Repo***: Container Image Repo for save App Container Image

**(0)** Prerequisite

* To install AWS CDK, follow [this instruction ](https://docs.aws.amazon.com/cdk/v2/guide/getting_started.html#getting_started_install). Latest version is recommended.

* Check AWS Account

    aws sts get-caller-identity

* Set AWS Region

    export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')

If aws profile setted account and workshop account are different, please re-configure account id to workshop account

**(1)** Download CDK source for deploy infra and install python packages.

    curl 'https://static.us-east-1.prod.workshops.aws/public/d0e72c6e-904d-4933-beec-4c908d928217/static/images/110-cicd/code-pipeline-cdk.zip' --output cdk.zip
    unzip cdk.zip
    cd cdk
    pip install .

If you use Windows OS, please download source with followed link. [CDK Source](https://static.us-east-1.prod.workshops.aws/public/d0e72c6e-904d-4933-beec-4c908d928217/static/images/110-cicd/code-pipeline-cdk.zip)

**(2)** CDK Bootstrap and deploy

    cdk bootstrap
    cdk synth
    cdk deploy --require-approval never

**(3)** Check deployed resource.

    aws codecommit list-repositories --region ${AWS_REGION}
    aws codepipeline list-pipelines --region ${AWS_REGION}
    aws codebuild list-projects --region ${AWS_REGION}

## [2. Deploy Application](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/110-cicd/300-cicd#2.-deploy-application)

**(0)** Prerequirement for CodeCommit.

    pip install git-remote-codecommit

* If you use the Instance Profile in Cloud9, you can access codecommit through credential.helper.

    git config --global user.email "test@aaa.com" # put your email git config --global --replace-all credential.helper '!aws codecommit credential-helper $@' git config --global credential.UseHttpPath true

* If you access with [AWS IAM User ](https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-gc.html), get User name and password from IAM User’s Generate Credentials.

![](https://cdn-images-1.medium.com/max/4000/0*uTJMPm7Q8SSHLmCZ.png)

**(1)** Download application source and unzip the source.

    curl 'https://static.us-east-1.prod.workshops.aws/public/d0e72c6e-904d-4933-beec-4c908d928217/static/images/110-cicd/app.zip' --output app.zip
    unzip app.zip

If you use Windows OS, please download source with followed link. [App Source](https://static.us-east-1.prod.workshops.aws/public/d0e72c6e-904d-4933-beec-4c908d928217/static/images/110-cicd/app.zip)

**(2)** Push source codes into eks-workshop-app repository.

    export APP_CODECOMMIT_URL=$(aws codecommit get-repository --repository-name eks-workshop-app --region ${AWS_REGION} | grep -o '"cloneUrlHttp": "[^"]*'|grep -o '[^"]*$')

    git clone $APP_CODECOMMIT_URL
    cd eks-workshop-app

    cp -R ../app/* ./
    git add .
    git commit -m "init app Source"
    git push origin master

## [3. Deploy Helm](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/110-cicd/300-cicd#3.-deploy-helm)

**(1)** Download helm source and unzip the source.

    curl 'https://static.us-east-1.prod.workshops.aws/public/d0e72c6e-904d-4933-beec-4c908d928217/static/images/110-cicd/helm.zip' --output helm.zip
    unzip helm.zip

If you use Windows OS, please download source with followed link. [Helm Source](https://static.us-east-1.prod.workshops.aws/public/d0e72c6e-904d-4933-beec-4c908d928217/static/images/110-cicd/helm.zip)

**(2)** Helm CodeCommit Repo push

    export HELM_CODECOMMIT_URL=$(aws codecommit get-repository --repository-name eks-workshop-helm --region ${AWS_REGION} | grep -o '"cloneUrlHttp": "[^"]*'|grep -o '[^"]*$')
    cd helm
    git init
    git checkout -b master
    git add .
    git commit -m "init"
    git remote add origin $HELM_CODECOMMIT_URL
    git push origin master

## [4. Check Code Pipeline](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/110-cicd/300-cicd#4.-check-code-pipeline)

## [5. Install ArgoCD](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/110-cicd/300-cicd#5.-install-argocd)

**(1)** ArgoCD install

ArgoCD install to EKS cluster with below commands

    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

Below commands for argocd cli

    cd ~/environment
    VERSION=$(curl --silent "https://api.github.com/repos/argoproj/argo-cd/releases/latest" | grep '"tag_name"' | sed -E 's/.*"([^"]+)".*/\1/')

    sudo curl --silent --location -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/$VERSION/argocd-linux-amd64
    sudo chmod +x /usr/local/bin/argocd

Mac install command

    brew install argocd

Argocd is not recommanded expose public, but we expose argocd to public for workshop.

    kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

check argocd server host

    export ARGOCD_SERVER=`kubectl get svc argocd-server -n argocd -o json | jq --raw-output .status.loadBalancer.ingress[0].hostname`
    echo $ARGOCD_SERVER

Argocd username is admin and get pw from below command.

    ARGO_PWD=`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`
    echo $ARGO_PWD

open $ARGOCD_SERVER from browser and input admin and $ARGO_PWD

![](https://cdn-images-1.medium.com/max/2428/0*wBet61Btt9bUlnfY.png)

## [6. Configure ArgoCD](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/110-cicd/300-cicd#6.-configure-argocd)

**(0)** Argocd access iam user configure

Create user from IAM Console

Set Username and check Access Key ckeckbox

![](https://cdn-images-1.medium.com/max/2210/0*0J2lcosDDqM3zpQ5.png)

Choose AWSCodeCommitPoserUser Policy

![](https://cdn-images-1.medium.com/max/2334/0*vw2EBAi1mdpyfvRL.png)

And reaccess to IAM user console and click the Security credentials tab and generate credenitals from HTTPS Git credentials for AWS CodeCommit

![](https://cdn-images-1.medium.com/max/2000/0*k8ApDeohMT-ucFQU.png)

Download credentials or note

![](https://cdn-images-1.medium.com/max/2000/0*MutTdIsMUctlGK_U.png)

**(1)** Configure ArgoCD

Login to Argocd and clicked leftside settings icon and click the Repositories menu.

![](https://cdn-images-1.medium.com/max/3240/0*2INQ_jfB6t8Uu88B.png)

Click Connect Repo Button **Method** -> VIA HTTPS, **Project** -> default

**Repository URL** -> **CodeCommit Helm Repo**’s HTTPS Address, Put **UserName Password**

![](https://cdn-images-1.medium.com/max/2902/0*WA3vMxhzC2UJKR0p.png)

NewApp button click in Application tab **Application Name** -> random , **Project** -> default

![](https://cdn-images-1.medium.com/max/3194/0*rDrV3WS8Pe3hPeNb.png)

**Sync policy** -> AUTOMATIC, **RepoURL** -> before generated Repository, **PATH** -> . , **DESTINATION** section’s **Cluster URL** -> https://kubernetes.default.svc, **Namespace** -> default and click **Create**

![](https://cdn-images-1.medium.com/max/3512/0*tbcWRENiQYbJlFBJ.png)

**(5)** Check ArgoCD

Check deployment status in Argocd console

**Applications > APP Name**

![](https://cdn-images-1.medium.com/max/3392/0*92feoBOfpIH3bpJi.png)

Check new pipeline check image tag from ***Helm***’s commit history

And check Argocd console’s eks-workshop pod Image Tag

![](https://cdn-images-1.medium.com/max/2196/0*Bv1NEdEjXM7QRaJW.png)

Move to **Applications > eks-workshop >** and check eks-workshop **pod** click and check status

It will be continous synced **Helm Repo** and Argocd, when **Helm Repo** commit occurred.

## [7. Check Pipeline](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/en-US/110-cicd/300-cicd#7.-check-pipeline)

Commit Java code and check deployment app.

**(1)** Create source code

Create **app/src/main/java/com/aws/sample/HelloWorldController.java** from cloud9 console.

    package com.aws.samples;

    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.servlet.ModelAndView;

    @Controller

    public class HelloWorldController {

        @GetMapping("/hello-world")

        public ModelAndView hello() {

            ModelAndView modelAndView = new ModelAndView("hello-world");
            modelAndView.addObject("str", "Hello World");
            return modelAndView;
        }
    }

Create **app/src/main/resources/templates/hello-world.html**

    <!DOCTYPE html>
    <html lang="en"
          xmlns="http://www.w3.org/1999/xhtml"
          xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="utf-8">
        <meta content="width=device-width, initial-scale=1" name="viewport"/>
        <title>SampleApp</title>
        <link href="/favicon.ico" rel="icon">
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/css/bootstrap.min.css" rel="stylesheet">
        <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.css" rel="stylesheet"
              type="text/css"/>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"
                integrity="sha512-894YE6QWD5I59HgZOGReFYm4dnWc1Qt5NtvYSaNcOP+u1T9qYdvdihz0PPSiiqn/+/3e7Jo4EaG7TubfWGUrMQ=="
                crossorigin="anonymous" referrerpolicy="no-referrer"></script>
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/js/bootstrap.min.js"
                integrity="sha256-SyTu6CwrfOhaznYZPoolVw2rxoY7lKYKQvqbtqN93HI=" crossorigin="anonymous"></script>
    </head>
    <body>
    <div>
        <div class="container">
            <h1 style="text-align: center; margin-top: 10px" th:text=" ${str} + '&#127811;'"></h1>
        </div>
    </div>
    </body>
    </html>

**(2)** Commit and push

    cd app
    git add .
    git commit -m "Add hello-world page"
    git push origin master

**(3)** check result

Check Codepipeline status in AWS console.

Check ArgoCd console

Access to Sample App

    echo http://$(kubectl get ingress/eks-workshop-workshop-example-app -o jsonpath='{.status.loadBalancer.ingress[*].hostname}')

If the deployment successed, Hello-world menu print nomally

**Optional : IRSA setting for APP Cluster Menu**

**Policy Generate**

    cat <<EOF> eks-workshop-test-policy.json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "VisualEditor0",
                "Effect": "Allow",
                "Action": [
                    "eks:DescribeCluster",
                    "eks:ListClusters"
                ],
                "Resource": "*"
            }
        ]
    }
    EOF

create Policy

    aws iam create-policy \
        --policy-name EKSWorkshopTestPolicy \
        --policy-document file://eks-workshop-test-policy.json

check OIDC Url

    export OIDC_URL=$(aws eks describe-cluster --name [my-cluster] --query "cluster.identity.oidc.issuer" --output text)
    export ACCOUNT_ID=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.accountId')
    export FEDERATED=arn:aws:iam::$ACCOUNT_ID:oidc-provider/$OIDC_URL
    create Trust Policy
    cat >eks-workshop-test-trust-policy.json <<EOF
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Principal": {
                    "Federated": "$FEDERATED"
                },
                "Action": "sts:AssumeRoleWithWebIdentity",
                "Condition": {
                    "StringEquals": {
                        "oidc.eks.region-code.amazonaws.com/id/EXAMPLED539D4633E53DE1B71EXAMPLE:aud": "sts.amazonaws.com",
                        "oidc.eks.region-code.amazonaws.com/id/EXAMPLED539D4633E53DE1B71EXAMPLE:sub": "system:serviceaccount:default:eks-workshop-test-role"
                    }
                }
            }
        ]
    }
    EOF

Create IAM Role

    aws iam create-role \
      --role-name eks-workshop-test-role \
      --assume-role-policy-document file://"eks-workshop-test-trust-policy.json"

Attach policy to role

    aws iam attach-role-policy \
      --policy-arn arn:aws:iam::$ACCOUNT_ID:policy/EKSWorkshopTestPolicy \
      --role-name eks-workshop-test-role

Edit values yaml role-arn and push to helm repo

    serviceAccount:
      # Specifies whether a service account should be created
      create: true
      # Annotations to add to the service account
      annotations:
        eks.amazonaws.com/role-arn: arn:aws:iam::ACCOUNT_ID:role/eks-workshop-test-role
      name: workshop-example-app

**Pull recent commit from helm repo**

    cd helm
    git pull
    git add .
    git commit -m "update irsa role arn"
    git push origin master

If conflict occurred, solve the conflict.

    git pull
    git merge origin/master
    git push origin master

check deployment in argocd

## Clean up resources

At the end of this workshop, you need to delete used resources to avoid additional costs to your AWS account.

 1. Delete the Ingress resources. At this point, perform the command in the folder where the yaml file is located(/home/ec2-user/environment/manifests).

    cd ~/environment/manifests/

    kubectl delete -f flask-ingress.yaml
    kubectl delete -f nodejs-ingress.yaml
    kubectl delete -f frontend-ingress.yaml
    kubectl delete -f alb-ingress-controller/v2_5_4_full.yaml

 1. Delete EKS cluster.

    eksctl delete cluster --name=eks-demo

[!] Check that all related stacks have been deleted from AWS CloudFormation console.

 1. Remove Amazon ECR repository. With the command below, load the list of repository that you created.

    aws ecr describe-repositories

    aws ecr delete-repository --repository-name demo-flask-backend --force
    aws ecr delete-repository --repository-name demo-frontend --force

 1. Delete the collected metrics.

    aws logs describe-log-groups --query 'logGroups[*].logGroupName' --output table | \
    awk '{print $2}' | grep ^/aws/containerinsights/eks-demo | while read x; do  echo "deleting $x" ; aws logs delete-log-group --log-group-name $x; done

    aws logs describe-log-groups --query 'logGroups[*].logGroupName' --output table | \
    awk '{print $2}' | grep ^/aws/eks/eks-demo | while read x; do  echo "deleting $x" ; aws logs delete-log-group --log-group-name $x; done

 1. Delete the Cloud9 IDE environment you created.

## Challenges Faced and Solutions

**Challenge 1: Managing Kubernetes Configurations**

* **Solution**: Used eksctl and pre-configured YAML templates to manage and deploy configurations easily.

**Challenge 2: Monitoring Application and Cluster Performance**

* **Solution**: AWS Container Insights was critical in providing visibility into resource utilization and application health.

**Challenge 3: Automating Deployment without Downtime**

* **Solution**: CI/CD with blue-green deployment strategies helped ensure smooth transitions with minimal downtime during updates.

## Conclusion

This project demonstrates the power of Amazon EKS and its associated AWS services in delivering a scalable, efficient, and reliable web application infrastructure. By using Kubernetes on AWS, we’ve created a resilient environment suitable for high-demand applications. The integration of Fargate, ECR, Container Insights, and a CI/CD pipeline brings automation, visibility, and scalability to web application deployments on AWS.

## Code Repository

To explore and experiment with the project’s code and documentation, visit the [**GitHub Repository](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/8.%20Building%20Web%20Applications%20Using%20Amazon%C2%A0EKS)**. Here you can access the complete code, follow along with detailed instructions, and customize the applications to fit your own use cases.
>  ***Asif Khan — Aspiring Cloud Architect | Weekly Cloud Learning Chronicler***

[**LinkedIn](https://www.linkedin.com/in/asif0108/)**/[**Twitter](https://x.com/asif26073)**/[**GitHub](https://github.com/Dark-Cookie)**
