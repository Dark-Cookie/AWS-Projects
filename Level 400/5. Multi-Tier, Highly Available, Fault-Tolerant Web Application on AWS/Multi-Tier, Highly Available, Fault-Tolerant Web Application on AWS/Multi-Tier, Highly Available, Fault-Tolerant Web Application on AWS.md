
![](https://cdn-images-1.medium.com/max/3584/1*ejsbNco3ZS6_UzyX1poTpQ.png)

## AWS Project: Multi-Tier, Highly Available, Fault-Tolerant Web Application on AWS

### Introduction

In this project, I created a **multi-tier, highly available, and fault-tolerant web application** using AWS. This architecture is designed to maximize uptime, support scalability, and enhance security, making it ideal for real-world production environments. Through this project, I gained hands-on experience building robust cloud-based applications, leveraging **Amazon VPC**, **Amazon EC2**, **Amazon Aurora**, and **Amazon S3** to deliver high performance, reliability, and cost-efficiency.

### Tech Stack

* **Amazon VPC**: Provides isolated networking environments for secure data flow.

* **Amazon EC2**: Hosts scalable web and application server instances.

* **Amazon Aurora**: A managed, high-performance relational database with automated failover.

* **Amazon S3**: Stores and serves static content with durability and low latency.

### Prerequisites

* **AWS Account**: Required to access and configure all necessary AWS services.

* **AWS CLI**: For managing resources, configurations, and deployment tasks.

* **Basic Networking Knowledge**: Familiarity with networking concepts like subnets, load balancing, and security groups.

* **AWS Console Proficiency**: Experience using the AWS Console for deploying and configuring services.

### Problem Statement or Use Case

**Problem**: Traditional on-premises infrastructure often fails to meet the needs of applications requiring high availability and fault tolerance, especially under varying loads.

**Solution**: This project implements a **multi-tier architecture** with **high availability** and **fault tolerance** using AWS. By setting up a web application across multiple tiers (frontend, application logic, and database), the solution ensures seamless user experience, even in case of server failure or maintenance.

**Real-World Relevance**: The solution suits production environments where uptime is crucial, such as e-commerce platforms, content-driven websites, and customer-facing applications. The architecture can dynamically adjust resources to accommodate fluctuating traffic, making it scalable and cost-effective.

### Architecture Diagram

Below is a high-level overview of the architecture used:

![](https://cdn-images-1.medium.com/max/2560/1*u0QN5NlcGAMWuOdgjj6MnA.png)

### Component Breakdown

 1. **Amazon VPC**: Provides network isolation, enabling private and public subnets to securely route traffic between the internet and internal services.

 2. **Amazon EC2**: Hosts web and application server instances, with auto-scaling groups to dynamically adjust resources as traffic demands change.

 3. **Load Balancer**: Manages incoming requests and distributes them to healthy EC2 instances across different availability zones, ensuring high availability.

 4. **Amazon Aurora**: A managed relational database that automatically replicates data and performs failovers, providing a resilient storage solution.

 5. **Amazon S3**: Stores and delivers static content like images, CSS, and JavaScript files, reducing load on EC2 instances and improving performance.

### Step-by-Step Implementation

## Network — Amazon VPC

## Create VPC

**Amazon Virtual Private Cloud (Amazon VPC)** allows you to start AWS resources with a user-defined virtual network. This virtual network, along with the benefits of using AWS’s scalable infrastructure, is very similar to the existing network operating in the customer’s own data center.

## [Move on to VPC service](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/network/10-index#move-on-to-vpc-service)

 1. After logging in to the AWS console, select **VPC** from the service menu.

![](https://cdn-images-1.medium.com/max/2000/0*YrCPnUSKvkYQJ9ye.png)

If the screenshot below is **different** from the screen that you’re viewing, enable the **New VPC Experience toggle** to active.

![](https://cdn-images-1.medium.com/max/2000/0*mbUVpSqE_cAl7FqB.png)

## [Create VPC through VPC Wizard](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/network/10-index#create-vpc-through-vpc-wizard)

 1. Select **VPC Dashboard** and click **Create VPC** to create your own VPC.

![](https://cdn-images-1.medium.com/max/3216/0*9tpqi7SWoG86iu90.png)

 1. To create a space to provision AWS resources used in this lab, we will create a VPC and Subnets. Select **VPC and more** in **Resource to create** tab and change name tag to **VPC-Lab**. Leave the default setting for IPv4 CIDR block.

![](https://cdn-images-1.medium.com/max/3912/0*XxjluCOgQ-aLimxE.png)

It is a best practice to deploy resources across multiple Availability Zones for high availability and fault tolerance.

 1. To design high availability architecture, we create **2** subnet space and select **2a** and **2c** for **Customize AZs**. And set the CIDR value of the public subnet that can communicate directly with the Internet as shown in the screen below. Set the CIDR value of the private subnet as shown in the screen.

![](https://cdn-images-1.medium.com/max/2000/0*S6E-IxLAsMZHGxt3.png)

![](https://cdn-images-1.medium.com/max/2000/1*L2t4oYXhDGYTeMPARLONiw.png)

 1. You can use a **NAT gateway** so that instances in your private subnets can connect to services outside your VPC, but external services cannot initiate direct connections to these instances. In this lab, we will create a NAT gateway in only one Availability Zone to save cost. Also, for DNS options, **enable** both **DNS hostnames** and **DNS resolution**. After confirming the setting value, click the **Create VPC** button.

![](https://cdn-images-1.medium.com/max/2000/0*fx51n5W11S4jhbqP.png)

 1. As the VPC is created, you can see the process of creating network-related resources as shown in the screen below. For NAT Gateway, provisioning may take longer compared to other resources.

![](https://cdn-images-1.medium.com/max/2000/0*D1o31n3yCpcjSBjU.png)

 1. You can check the information of the created VPC. Check related information such as **CIDR** value, route table, network ACL, etc. Check that the values you just set are correct.

![](https://cdn-images-1.medium.com/max/2000/0*f-iRyPgm98NbXkiX.png)

## [Architecture Configured So Far](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/network/10-index#architecture-configured-so-far)

If VPC is completed through the VPC Wizard, the environment configured so far is as follows.

![](https://cdn-images-1.medium.com/max/2000/0*qaq8_WVNL1uKPYU1.png)

### Challenges Faced and Solutions

* **Cross-AZ Latency**: Replicating data across availability zones resulted in some latency in data access.

* **Solution**: Used Aurora’s automated cross-region replication to reduce latency while maintaining data consistency.

* **Auto Scaling Configuration**: Initially faced challenges with EC2 instances not scaling back down after load reduction.

* **Solution**: Adjusted **Auto Scaling policies** to ensure smoother scaling transitions, keeping resource usage cost-effective.

## Create VPC Endpoint

In this section, you create an endpoint for S3 to learn a VPC endpoint. Skip to do this step will not affect your progress to the next lab.

## [VPC Endpoint](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/network/20-index#vpc-endpoint)

 1. In **VPC Dashboard**, select **Endpoints**. Click **Create endpoint** button.

![](https://cdn-images-1.medium.com/max/2000/0*Uar_tA1VWdMGko0k.png)

 1. Type **s3 endpoint** for name and select **AWS services** in Service category tab. In the search bar below, type **s3** and select the list at the top.

![](https://cdn-images-1.medium.com/max/2000/0*jC8_3TPUZLvv_5C8.png)

 1. For S3 VPC endpoints, there are **gateway** types and **interface** types. For this lab, select the **gateway** type. And for the deployment location, select the **VPC-Lab-vpc** created in this lab.

![](https://cdn-images-1.medium.com/max/2000/0*95yWxrs8-mMUrpNN.png)

 1. Choose a route table to reflect the endpoint. Select the **two private subnets** as shown below. Additional routing information for using the endpoint is automatically added to the selected route table.

![](https://cdn-images-1.medium.com/max/2000/0*oG42ez2EGtXww9h2.png)

 1. You can also configure policies to control access to endpoints as shown below.

![](https://cdn-images-1.medium.com/max/2000/0*qvE3bcwGx_0orR-x.png)

You can use VPC endpoint policies to allow full access to AWS services or create custom policies. Check [Use VPC endpoint policies](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html#vpc-endpoint-policies)

 1. Confirm that the route to access Amazon S3 through the gateway endpoint has been automatically added to the **private route table** specified earlier.

![](https://cdn-images-1.medium.com/max/2620/0*HlYNcjndn-VfOL2J.png)

VPC endpoints are **communications within the AWS network** and have the **security and compliance** advantage of being able to control traffic through the endpoints. You can also optimize the **data processing cost** if you transfer your data through a VPC endpoint rather than a NAT gateway.

In this section, you created a S3 gateway endpoint to allow private S3 access from within the VPC without needing an internet gateway. This keeps S3 traffic private within the AWS network.

## Compute — Amazon EC2

## Launch a web server instance

This chapter starts with the default Amazon Linux instance and lets you automatically configure the Apache/PHP Web server during initial step.

## [Launch instance and connect to web service](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/launching#launch-instance-and-connect-to-web-service)

 1. In the AWS console search bar, type [EC2 ](http://console.aws.amazon.com/ec2)and select it. Then click **EC2 Dashboard **at the top of the left menu. Press the **Launch instance **button and select **Launch instance** from the menu.

![](https://cdn-images-1.medium.com/max/4000/0*HUD9xy3u3lzbI-qn.png)

 1. In **Name**, put the value **Web server for custom AMI**. And check the default setting in Amazon Machine Image below.

![](https://cdn-images-1.medium.com/max/3224/0*UxZHnr4e8rEiIHwI.png)

![](https://cdn-images-1.medium.com/max/2000/1*d6aW-UhBdgGlm_UB_YjJcw.png)

 1. Select t2.micro in **Instance Type**.

![](https://cdn-images-1.medium.com/max/2000/0*_V7fltjFCugNfmqF.png)

 1. For **Key pair**, choose **Proceed without a key pair**.

![](https://cdn-images-1.medium.com/max/2000/0*PqxnmoTaZGRAkfi6.png)

 1. Click the **Edit** button in **Network settings** to set the space where EC2 will be located.

![](https://cdn-images-1.medium.com/max/2000/0*Ryhof6wBVs4M05wG.png)

And choose the **VPC-Lab-vpc** created in the previous lab, and for the subnet, choose **public subnet**. **Auto-assign public IP** is set to **Enable**.

![](https://cdn-images-1.medium.com/max/2000/0*zd5sAWXDJrznIAQf.png)

![](https://cdn-images-1.medium.com/max/2000/1*Rkw3ereEQ8-YJDTDSJQjVg.png)

 1. Right below it, create **Security groups** to act as a network firewall. Security groups will specify the protocols and addresses you want to allow in your firewall policy. For the security group you are currently creating, this is the rule that applies to the EC2 that will be created. After entering Immersion Day - Web Server in **Security group name** and **Description**, select **Add Security group rule** and set ***Type*** to **HTTP**. Also allow TCP/80 for Web Service by specifying it. Select **My IP** in the source.

![](https://cdn-images-1.medium.com/max/2000/0*TgfpNFbuYYjmjvyJ.png)

![](https://cdn-images-1.medium.com/max/2000/0*MXUOeq8TtFFHCOj7.png)

It is a best practice to configure security groups following the principle of least privilege, allowing only the minimum required traffic.

 1. All other values accept the default values, expand by clicking on the **Advanced Details** tab at the bottom of the screen.

![](https://cdn-images-1.medium.com/max/2000/0*AXzt9InL1z50OS35.png)

Click the **Meta Data** version dropdown and select **V2 only (token required)**

![](https://cdn-images-1.medium.com/max/2820/0*7Jy1bDBs2-aAcgTo.png)

Enter the following values in the **User data** field and select **Launch instance**.

![](https://cdn-images-1.medium.com/max/2000/0*vhvEzdDHBFRPIOx9.png)

    #!/bin/sh
    ​
    #Install a LAMP stack
    dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel
    dnf install -y mariadb105-server
    dnf install -y httpd php-mbstring
    ​
    #Start the web server
    chkconfig httpd on
    systemctl start httpd
    ​
    #Install the web pages for our lab
    if [ ! -f /var/www/html/immersion-day-app-php7.zip ]; then
       cd /var/www/html
       wget -O 'immersion-day-app-php7.zip' 'https://static.us-east-1.prod.workshops.aws/public/2e449d3a-fc13-44c9-8c99-35a37735e7f5/assets/immersion-day-app-php7.zip'
       unzip immersion-day-app-php7.zip
    fi
    ​
    #Install the AWS SDK for PHP
    if [ ! -f /var/www/html/aws.zip ]; then
       cd /var/www/html
       mkdir vendor
       cd vendor
       wget https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip
       unzip aws.zip
    fi
    ​
    # Update existing packages
    dnf update -y

User Data is a user-defined initialization script that is executed when the first instance is created.

 1. Information indicating that the instance creation is in progress is displayed on the screen. You can view the list of EC2 instances by selecting **View Instances** in the lower right corner.

 2. After the instance configuration is complete, you can check the Availability Zone in which the instance is running, and externally accessible IP and DNS information.

![](https://cdn-images-1.medium.com/max/2342/0*HUG3UZ2hE1TUCbLN.png)

 1. Wait for the instance’s **Instance state** result to be **Running**. Open a new web browser tab and enter the **Public DNS or IPv4 Public IP** of your EC2 instance in the URL address field. If the page is displayed as shown below, the web server instance is configured normally.

 2. If you are using the Chrome web browser, when you attach the **Public IPv4 DNS** value to the web browser, if it does not run, https may be automatically added in front of the DNS value, so it may not run. Therefore, it is recommended to enter [http://.](http://.)

![](https://cdn-images-1.medium.com/max/2712/0*lZxS7zQ_ksLLOn1M.png)

## [Access the web service](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/launching#access-the-web-service)

 1. Go to the EC2 instance console. Select the instance you want to connect to and click the **Connect** button in the center.

![](https://cdn-images-1.medium.com/max/2000/0*_Oy2g3av1NMVJCpH.png)

 1. In the **Connect your instance** window, select the EC2 Instance Connect tab, then click the **Connect** button in the lower right corner.

![](https://cdn-images-1.medium.com/max/2000/0*xDdI1VEf3I1r1dOh.png)

 1. After a while, you can use the browser-based SSH console as shown below. Just close the window after the CLI test.

![](https://cdn-images-1.medium.com/max/3548/0*xru6kFeBXip7F-fv.png)

## [Connect to the Linux instance using Session Manager](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/launching#connect-to-the-linux-instance-using-session-manager)

You must click the **Access your Linux instance using Session Manager** link below to proceed with the exercise.

In the database lab to be followed, we connect to RDS database using the IAM role granted to the web server. Therefore, refer to [Accessing Linux instance using Session Manager ](https://catalog.workshops.aws/general-immersionday/en-US/basic-modules/10-ec2/ec2-linux/3-ec2-1)to assign IAM role to EC2 instance and connect to your Linux instance using Session Manager

![](https://cdn-images-1.medium.com/max/2000/0*Qmo6mNmD89ut263t.png)

## [Create a custom AMI](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/launching#create-a-custom-ami)

In the AWS EC2 console, you can create an Custom AMI to meet your needs. This can then be used for future EC2 instance creation. In this page, let’s create an AMI using the web server instance that we built earlier.

 1. In the EC2 console, select the instance that we made earlier in this lab, and click **Actions** > **Image and templates** > **Create Image**.

![](https://cdn-images-1.medium.com/max/2000/0*2mzjFpe7FwiMLerr.png)

 1. In the Create Image console, type as shown below and press **Create image** to create the custom image.

![](https://cdn-images-1.medium.com/max/2804/1*XQX0KC2QA0sqvmT9T5rqKA.png)

![](https://cdn-images-1.medium.com/max/2000/1*QoHie3Ww9wFn41L0waT1ow.png)

 1. Verify in the console that the image creation request in completed.

 2. In the left navigation panel, Click the **AMIs** button located under **IMAGES**. You can see that the **Status** of the AMI that you just created. It will show either **Pending** or **Available**.

![](https://cdn-images-1.medium.com/max/2160/0*idP3evogs7HUVO7j.png)

## [Terminate the instance](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/launching#terminate-the-instance)

**Custom AMI (Golden Image) creation has been completed for the auto scaling by using the EC2 instance you just created.** Therefore, the EC2 instance currently running is no longer needed, so let’s try to terminate it. ( In [Deploy auto scaling web service](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/launching/compute/auto-scaling), we will use custom AMI to create a new web server.)

Do **not** terminate the “Web server for custom AMI” Instance until the AMI creation process is fully completed. Ensure the AMI status shows as **Available** before proceeding.

 1. In the left navigation panel of the EC2 dashboard, select **Instances**. Then select the instance that should be deleted. From there, click **Instance state** -> **Terminate instance**.

![](https://cdn-images-1.medium.com/max/2000/0*bF4Cbt3yCr-_R1NW.png)

 1. When the alert message appears, click **Terminate** to delete.

![](https://cdn-images-1.medium.com/max/2000/0*r6Vgcju75EtMwsgE.png)

 1. The instance status changes to **Shutting down**. After that, the instance status turned to **terminated**. The instance deletion is complete. You may see the instance for a short period of time for deletion logging.

## [Architecture Configured So Far](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/launching#architecture-configured-so-far)

![](https://cdn-images-1.medium.com/max/2090/1*6H7np_Y7JcjeI8_fnzrX9w.png)

If you mark the resources that have been configured so far in conceptual terms, it is same with the picture below.

Congratulations! You have successfully created a Custom AMI (Golden Image) using the EC2 web server, which can be utilized for deploying an auto-scaling web service in the next section.

## Deploy auto scaling web service

Using the network infrastructure created in the Network- AMazon VPC lab, we will deploy a web service that can automatically scale out/in under load and ensure high availability. We use the web server AMI created in the previous chapter and the network infrastructure named VPC-Lab.

## [Configure Application Load Balancer](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/auto-scaling#configure-application-load-balancer)

AWS Elastic Load Balancer supports three types of load balancers: Application Load Balancer, Network Load Balancer, and Gateway Load Balancer. In this lab, you will configure and set up the Application Load Balancer to handle load balancing HTTP requests.

 1. From the **EC2 Management Console** in the left navigation panel, click **Load Balancers** under **Load Balancing**. Then click **Create Load Balancer**. In the Select load balancer type, click the **Create** button under **Application Load Balancer**.

![](https://cdn-images-1.medium.com/max/2072/0*ZnGv0Ussa-P4xhXw.png)

 1. Name the load balancer. In this case, name **Name** as Web-ALB. Leave the other settings at their default values.

![](https://cdn-images-1.medium.com/max/2206/0*6cgPgx7hDmqtOljf.png)

It is a best practice to deploy resources across multiple Availability Zones for fault tolerance and high availability.

 1. Scrolling down a little bit, there is a section for selecting availability zones. First, Select the VPC-Lab-vpc created previously. For Availability Zones select the 2 public subnets that were created previously. This should be **Public Subnet** for ap-northeast-2a and **Public Subnet C** for ap-northeast-2c.

![](https://cdn-images-1.medium.com/max/2196/0*_ZtzSFjYzgKkb7xZ.png)

 1. In the **Security groups** section, click the **Create new security group hyperlink**. Enter web-ALB-SG as the security group name and check the VPC information. Scroll down to modify the Inbound rules. Click the **Add rule** button and select **HTTP** as the Type and **Anywhere-IPv4** as the Source. And create a security group.

![](https://cdn-images-1.medium.com/max/2394/0*_5qlg2jo4h3MPCza.png)

 1. Return to the load balancer page again, click the refresh button, and select the **web-ALB-SG** you just created. **Remove the default security group.**

![](https://cdn-images-1.medium.com/max/2192/0*KiXd8DRIY2IK4cgl.png)

 1. In **Listeners and routing** column, click **Create target group**. Put Web-TG for Target group name and check all settings same with the screen below. After that click **Next** button.

![](https://cdn-images-1.medium.com/max/2412/0*QAK_mOcznbOvIcjQ.png)

![](https://cdn-images-1.medium.com/max/2000/0*-hfH6ef6GBs3SbwD.png)

 1. This is where we would register our instances. However, as we mentioned earlier, there are not instances to register at this moment. Click **Create target group**.

![](https://cdn-images-1.medium.com/max/2644/0*C_MK7LnNhvnODnaQ.png)

 1. Again, move into the Load balancers page, click refresh button and select Web-TG. And then Click **Create load balancer**.

![](https://cdn-images-1.medium.com/max/2008/0*ZCNjdwRwnJtHA-Ww.png)

![](https://cdn-images-1.medium.com/max/2434/0*uXFpjIxt9PT4cMXW.png)

## [Configure launch template](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/auto-scaling#configure-launch-template)

Now that ALB has been created, it’s time to place the instances behind the load balancer. To configure an Amazon EC2 instance to start with Auto Scaling Group, you can use **Launch Template**, **Launch Configuration**, or **EC2 Instance**. In this workshop, we will use the **Launch Template** to create an Auto Scaling group.

The launch template configures all parameters within a resource at once, reducing the number of steps required to create an instance. Launch templates make it easier to implement best practices with support for Auto Scaling and spot fleets, as well as spot and on-demand instances. This helps you manage costs more conveniently, improve security, and minimize the risk of deployment errors.

The launch template contains information that Amazon EC2 needs to start an instance, such as AMI and instance type. The Auto Scaling group refers to this and adds new instances when a scaling out event occurs. If you need to change the configuration of the EC2 instance to start in the Auto Scaling group, you can create a new version of the launch template and assign it to the Auto Scaling group. You can also select a specific version of the launch template that you use to start an EC2 instance in the Auto Scaling group, if necessary. You can change this setting at any time.

## [Create security group](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/auto-scaling#create-security-group)

Before creating a launch template, let’s create a security group for the instances created through the launch template to use.

 1. From the left navigation panel of the EC2 console, select **Security Groups** under the **Network & Security** heading and click **Create Security Group** in the upper right corner.

![](https://cdn-images-1.medium.com/max/2568/0*pITJHcx7J_GyIXhu.png)

![](https://cdn-images-1.medium.com/max/2000/1*vZHg3y78YxpcVN-WsqxyeA.png)

Scroll down to modify the Inbound rules. First, select the **Add rule** button to add the Inbound rules, and select HTTP in the **Type**. For **Source**, type ALB in the search bar to search for the security group created earlier Web-ALB-SG. This will **configure the security group to only receive HTTP traffic coming from ALB**.

![](https://cdn-images-1.medium.com/max/2302/0*27NbvcG2Oe0M-WfL.png)

![](https://cdn-images-1.medium.com/max/2000/1*izFBQaYxfh9HM3IG4iNYQA.png)

 1. Leave outbound rules’ default settings and click **Create Security Group** to create a new security group. This creates a security group that allows traffic only for HTTP connections (TCP 80) that enter the instance via ALB from the Internet.

## [Create launch template](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/auto-scaling#create-launch-template)

 1. In the EC2 console, select **Launch Templates** from the left navigation panel. Then click **Create Launch Template**.

![](https://cdn-images-1.medium.com/max/4000/0*jckPy7Uvv5SNGz97.png)

 1. Let’s proceed with setting up the launch template step by step. First, set **Launch template name** and **Template version description** as shown below, and select ***Checkbox*** for **Provide guidance** in Auto Scaling guidance. Select this checkbox to enable the template you create to be utilized by Amazon EC2 Auto Scaling.

![](https://cdn-images-1.medium.com/max/2000/0*B7rWQ7e_Mxnj4NGE.png)

![](https://cdn-images-1.medium.com/max/2000/1*u9xVA5UgDO5XdKwCOffqgw.png)

 1. Scroll down to set the launch template contents. In **Amazon Machine Image(AMI)**, set the AMI to Web Server v1, which was created in the previous EC2 lab. You can find it by typing Web Server v1 in the search section, or you can scroll down to find it in the My AMI section. Next, select t2.micro for the instance type. We are not going to configure SSH access because this is only for Web service server. Therefore, we do not use key pairs.

![](https://cdn-images-1.medium.com/max/2000/0*sK7RDBRiNFE1QRni.png)

 1. Leave the other parts as default. Let’s take a look at the **Network Settings** section. In security group dropdown, find and apply ASG-Web-Inst-SG created before.

![](https://cdn-images-1.medium.com/max/2000/0*DaEm7bmvpBn89ghh.png)

 1. Follow the Storage’s default values without any additional change. Go down and define the Instance tags. Click **Add tag** and Name for **Key** and Web Instance for **Value**. Select Resource types as **Instances and Volumes**.

![](https://cdn-images-1.medium.com/max/2000/0*gV8ABkcX8_D6dFLy.png)

![](https://cdn-images-1.medium.com/max/2000/1*2SBRx8uqpQ6tTKHhOumrkg.png)

 1. Finally, in the **Advanced details** tab, set the **IAM instance profile** to **SSMInstanceProfile**. If IAM role was not created earlier, refer [Create an IAM instance profile for Systems Manager ](https://catalog.workshops.aws/general-immersionday/en-US/basic-modules/10-ec2/ec2-linux/3-ec2-1#create-an-iam-instance-profile-for-systems-manager)to create the SSMInstanceProfile IAM role.

Leave all other settings as default, and click the **Create launch template** button at the bottom right to create a launch template.

![](https://cdn-images-1.medium.com/max/2000/0*Zr5zbI03xuH4VJhn.png)

 1. After checking the values set in **Summary** on the right, click **Create launch template** to create a template.

![](https://cdn-images-1.medium.com/max/2000/0*Xt3eux99MT5AvUg8.png)

## [Set Auto Scaling Group](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/auto-scaling#set-auto-scaling-group)

Now, let’s create the Auto Scaling Group.

 1. Enter the EC2 console and select **Auto Scaling Groups** at the bottom of the left navigation panel. Then click the **Create Auto Scaling group** button to create an *Auto Scaling Group*.

![](https://cdn-images-1.medium.com/max/3944/0*jeBu3z3VVoUBQYCY.png)

 1. In ***[Step 1: Choose launch template or configuration]***, specify the name of the Auto Scaling group. In this workshop, we will designate it as Web-ASG. Then select the launch template that you just created named Web. The default settings for the launch template will be displayed. Confirm and click the lower right **Next** button.

![](https://cdn-images-1.medium.com/max/2198/0*g2hSnUF4viOlsMcv.png)

![Set the network configuration with the Purging options and instance types as default. Choose VPC-Lab-vpc for **VPC**, select **Private subnet 1** and **Private subnet 2** for **Subnets**. When the setup is completed, click the **Next** button.](https://cdn-images-1.medium.com/max/2000/1*qD3gEC4JGDRXi21PQJ6FCQ.png)

![](https://cdn-images-1.medium.com/max/2000/0*NZfnbHMruz90N6hx.png)

 1. Next, proceed to set up load balancing. First, select Attach to an existing load balancer. Then in **Choose a target group for your load balancer**, select Web-TG created during in ALB creation. At the **Monitoring**, select Check box for **Enable group metrics collection within CloudWatch**. This allows CloudWatch to see the group metrics that can determine the status of Auto Scaling groups. Click the **Next** button at the bottom right.

![](https://cdn-images-1.medium.com/max/2000/0*ZwCP7vnaC9x69ywO.png)

![](https://cdn-images-1.medium.com/max/2000/0*gGKHOg3lzDeZD0OX.png)

 1. In the step of Configure group size and scaling policies, set scaling policy for Auto Scaling Group. In the **Group size** column, specify **Desired capacity** and **Minimum capacity** as 2 and **Maximum capacity** as 4. Keep the number of the instances to 2 as usual, and allow scaling of at least 2 and up to 4 depending on the policy.

![](https://cdn-images-1.medium.com/max/2000/0*MOTq0B_DaAq2iOMV.png)

 1. In the Scaling policies section, select **Target tracking scaling policy** and type 30 in **Target value**. This is a scaling policy for adjusting the number of instances based on the CPU average utilization remaining at 30% overall. Leave all other settings as default and click the **Next** button in the lower right corner.

![](https://cdn-images-1.medium.com/max/2000/0*VBTIQs73KJxS_bBZ.png)

 1. We will not **Add notifications**. Clcik the **Next** button to move to the next step. In the Add tags step, we will simply assign name tag. Click **Add tag**, type Name in **Key**, ASG-Web-Instance in **Value**, and then click **Next**.

![](https://cdn-images-1.medium.com/max/2000/0*kkst4xieD-x8fNn_.png)

![](https://cdn-images-1.medium.com/max/2000/1*79qguydZAKlE_2DXmfHCgA.png)

 1. Now we are in the final stage of review. After checking the all settings, click the **Create Auto Scaling Group** button at the bottom right.

 2. Auto Scaling group has been created. You can see the Auto Scaling group created in the Auto Scaling group console as shown below.

![](https://cdn-images-1.medium.com/max/2250/0*NdW7U9_k0Wop8gff.png)

 1. Instances created through the Auto Scaling group can also be viewed from the EC2 Instance menu.

![](https://cdn-images-1.medium.com/max/2876/0*g-4O7bHUvjEnZWKw.png)

## [Architecture Configured So Far](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/auto-scaling#architecture-configured-so-far)

Now, we’ve built a web service that is high available and automatically scales under load! The configuration of the services we have created so far is as follows.

![](https://cdn-images-1.medium.com/max/2000/1*eQB9f2xnbqehe8aeF4Stww.png)

Congratulations! You successfully deployed a scalable and highly available web service using an Application Load Balancer, security groups, launch template, and Auto Scaling group.

## Check web service and test

Now, let’s test the service you have configured for successful operation. First, let’s check whether you can access the website normally and whether the load balancer works, and then load the web server to see if Auto Scaling works.

## [Check web service and load balancer](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/test-service#check-web-service-and-load-balancer)

 1. To access through the Application Load Balancer configured for the web service, click the **Load Balancers** menu in the EC2 console and select the Web-ALB you created earlier. Copy ***DNS name*** from the basic configuration.

![](https://cdn-images-1.medium.com/max/2454/0*pbA8XAknaYpJBGH5.png)

 1. Open a new tab in your web browser and paste the **copied DNS name**. You can see that web service is working as shown below. For the figure below, you can see that the web instance placed in **ap-northeast-2a** is running this web page.

![](https://cdn-images-1.medium.com/max/2000/0*B-GAIy_PkL_axVOh.png)

 1. If you click the refresh button here, you can see that the host serving the web page has been replaced with **an instance of another availability zone area** (ap-northeast-2c) as shown below. This is because routing algorithms in ALB target groups behave **Round Robin** by default.

![](https://cdn-images-1.medium.com/max/2796/0*VXjSoSfrEWAtEgGl.png)

 1. Currently, in the the Auto Scaling group, scaling policy’s baseline has been set to 30% CPU utilization for each instance.

* If the average ***CPU utilization of an instance is less than 30%***, Reduce the number of instances.

* If the average ***CPU utilization of an instance is over 30%***, Additional instances will be deployed, load will be distributed, and adjusted to ensure that the average CPU utilization of the instances is 30%.

 1. Now, let’s test load to see whether Auto Scaling works well. On the web page above, click the **LOAD TEST** menu. The web page changes and the applied load is visible. Click on the logo at the top left of the page to see that each instance is under load.

* Before load:

![](https://cdn-images-1.medium.com/max/2000/0*JbzBDB3tcuEo-vHR.png)

* After load:

![](https://cdn-images-1.medium.com/max/2000/0*ZjrEHUEASQQbDr5M.png)

The principle that causes CPU load is that when the CPU Idle value is over 50, the PHP code operates every five seconds to create, compress, and decompress arbitrary files. Traffic is distributed and operated by the ALB, so the load is applied to other instances continuously.

 1. Enter **Auto Scaling Groups** from the left side menu of the EC2 console and click the **Monitoring** tab. Under **Enabled metrics**, click **EC2** and set the right time frame to **1 hour**. If you wait for a few seconds, you’ll see the **CPU Utilization (Percent)** graph changes.

![](https://cdn-images-1.medium.com/max/2474/0*zATKYHflJR3GaTJp.png)

 1. Wait for about 5 minutes (300 seconds) and click the **Activity** tab to see the additional EC2 instances deployed according to the scaling policy.

![](https://cdn-images-1.medium.com/max/2316/0*ncVbGv1KjofdgIyq.png)

 1. When you click on the **Instance management** tab, you can see that two additional instances have sprung up and a total of four are up and running.

![](https://cdn-images-1.medium.com/max/2390/0*BEU_0tmSdirzjAT3.png)

 1. If you use the ALB DNS that you copied earlier to access and refresh the web page, you can see that it is hosting the web page in two instances that were not there before. The current CPU load is 0% because it is a new instance. It can also be seen that each of them was created in a different availability zone. If it’s not 0%, it can look more than 100% because it’s a constant load situation.

So far, we’ve checked that Auto Scaling group is working through a load test on the web service. If the page that causes the CPU load is working, close the page to prevent additional load.

## Database — Amazon Aurora

## Create VPC security group

The RDS service uses the same security model as EC2. The most common usage format is to provide data as a database server to an EC2 instance operating as an applicatiojn server within the same VPC, or to configure it to be accessible to the DB Application client outside of the VPC. The VPC Security Group must be applied for proper access control.

In the previous [Compute — Amazon EC2](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute) lab, we created web server EC2 instances using Launch Template and Auto Scaling Group. These instances use Launch Template to apply the security group **ASG-Web-Inst-SG** . Using this information, we will create a security group so that only web server instances within the Auto Scaling Group can access RDS instances.

 1. On the left side of the VPC dashboard, select **Security Groups** and then select **Create Security Group**.

![](https://cdn-images-1.medium.com/max/2582/0*rGG26eRKM3oQYLTr.png)

 1. Enter **Security group name** and **Description** as shown below. Choose the **VPC** that was created in the first lab. It should be named VPC-Lab.

![](https://cdn-images-1.medium.com/max/2426/0*_0wD9Iz688WkFQv-.png)

![](https://cdn-images-1.medium.com/max/2000/1*Ey3tvR4DHZ3S4lPktv_w1Q.png)

Following the principle of least privilege, it is a best practice to allow inbound traffic to your database only from trusted sources, such as your application servers.

 1. Scroll down to the Inbound rules column. Click Add rule to create a security group policy that allows access to RDS from the EC2 Web servers that you previously created through the Auto Scaling Group. Under **Type**, select **MySQL/Aurora** The port range should default to **3306**. The protocol and port ranges are automatically specified. The **Source type** entry can specify the IP band (CIDR) that you want to allow acces to, or other security groups that the EC2 instances to access are already using. Select the security group(named ***ASG-Web-Inst-SG*** ) that is applied to the web instances of the Auto Scaling group in the [Compute — Amazon EC2](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute)

![](https://cdn-images-1.medium.com/max/2318/0*a7esnJ0DiW2NlTEZ.png)

 1. When settings are completed, click **Create Security Group** at the bottom of the list to create this security group.

![](https://cdn-images-1.medium.com/max/2260/0*RTofi-4BFnp9He5V.png)

## Create RDS instance

Since the security group that RDS will use has been created, let’s create an instance of **RDS Aurora (MySQL compatible)**.

 1. In the AWS Management console, go to the [RDS (Relational Database Service) ](https://console.aws.amazon.com/rds).

![](https://cdn-images-1.medium.com/max/2000/0*e_FrPO3bWw2mkyoT.png)

 1. Select **Create Database** in dashboard to start creating a RDS instance.

![](https://cdn-images-1.medium.com/max/2094/0*EUIatlZDNin8jr2V.png)

 1. You want to select the RDS instances’ database engine. In Amazon RDS, you can select the database engine based on open source or commercial database engine. In this lab, we will use **Amazon Aurora with MySQL-compliant database engine**. Select **Standard Create** in the choose a database creation method section. Set ***Engine type*** to **Aurora (MySQL Compatible)**, Set ***Version*** to **Aurora (MySQL 5.7) 2.11.4**.

![](https://cdn-images-1.medium.com/max/2000/0*38Xm_MYgBWsgrX7L.png)

 1. Select **Production** in ***Template***. Under **Settings**, we want to specify administrator information for identifying the RDS instances. Enter the information as it appears below.

![](https://cdn-images-1.medium.com/max/2000/0*cdykGGkRs8dVsMs4.png)

![](https://cdn-images-1.medium.com/max/2000/1*kVrMX625dkS-c00ZMm1_7g.png)

For production workloads, it is a best practice to enable high availability and fault tolerance by creating read replicas in different Availability Zones.

 1. Under **DB instance size** select **Memory Optimized class**. Under **Availability & durability** select **Create an Aurora Replica or reader node in a different AZ**. Select **db.r5.large** for instance type.

![](https://cdn-images-1.medium.com/max/2000/0*Da5CeoZNdchxE3y7.png)

It is a best practice to deploy databases within a private subnet of a VPC for better security and network isolation

 1. Set up network and security on the **Connectivity** page. Select the VPC-Lab that you created earlier in the Virtual private cloud (VPC) and specify the subnet that the RDS instance will be placed in, public access, and security groups. Enter the information as it appears below.

![](https://cdn-images-1.medium.com/max/2000/0*6rMx_dmnq5gj-4Ru.png)

![](https://cdn-images-1.medium.com/max/2000/1*TlKq-kgDImQHGbHZqe6yGA.png)

 1. Scroll down and click **Additional configuration**. Set database options as shown below. Be aware of the uppercase and lowercase letters of **Initial database name**.

![](https://cdn-images-1.medium.com/max/2000/0*0km1Wq2V7BmvTJ6m.png)

![](https://cdn-images-1.medium.com/max/2000/1*GxZhEXR8pMWo5kDEgGpc0Q.png)

 1. Subsequent items such as **Backup**, **Entry**, **Backtrack**, **Monitoring**, and **Log exports** all accept the default values, and press **Create database** to create a database.

 2. A new RDS instance is now creating. This may take more than 5 minutes. You can use an RDS instance when the DB instance’s status changed to ***Available***.

![](https://cdn-images-1.medium.com/max/2886/0*wio7qlq6hprUI9eq.png)

## [Architecture Configured So Far](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/database/create-rds#architecture-configured-so-far)

The configuration of the services we have created so far is as follows.

![](https://cdn-images-1.medium.com/max/2086/1*JC2E5XSe5I2pgFqcXhwoQw.png)

## Connect RDS with Web App server

The Web Server instance that you created in the previous computer lab contains code that generates a simple address book to RDS. The Endpoint URL of the RDS must be verified first in order to use the RDS on the EC2 Web Server.

## [Storing RDS Credentials in AWS Secrets Manager](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/database/connect-app#storing-rds-credentials-in-aws-secrets-manager)

The web server we built includes sample code for our address book. In this lab, you specify which database to use in the sample code and how to connect it. We will store that information in AWS Secrets Manager.

In this chapter, we will create a secret containing data connection information. Later, we will give the web server the appropriate permission to retrieve the secret.

 1. In the console window, open AWS Secrets Manager ([https://console.aws.amazon.com/secretsmanager/ ](https://console.aws.amazon.com/secretsmanager/)) and click the **Store a new secret** button.

![](https://cdn-images-1.medium.com/max/2000/0*jt7oFdco4EpSk09Q.png)

It is a best practice to store database credentials and other sensitive information securely using AWS Secrets Manager, instead of hard-coding them in application code.

 1. Under **Secret Type**, choose **Credentials for Amazon RDS database**. Write down the user name and password you entered when creating the database. And under **Database** select the database you just created. Then click the **Next** button.

![](https://cdn-images-1.medium.com/max/2000/1*kgGtilp49z7O35_Li2wo0A.png)

![](https://cdn-images-1.medium.com/max/2000/0*4-59fWZ82wYRBAoA.png)

 1. Name your secret, mysecret. The sample code is written to ask for the secret by this specific name. Click **Next**.

![](https://cdn-images-1.medium.com/max/2000/0*xD8xGzjLs9kOk80O.png)

 1. Leave Secret rotation at default values. Click Next.

![](https://cdn-images-1.medium.com/max/2000/0*x_psQRHMhVYs8bUx.png)

 1. Review your choices. Click **Store**.

![](https://cdn-images-1.medium.com/max/2000/0*_qUvDgboWfPKQiBN.png)

 1. You can check the list of secret values with the name **mysecret** as shown below.

![](https://cdn-images-1.medium.com/max/2306/0*LihfEoC1WOSZyooM.png)

 1. Click **mysecret** hyperlink and find **Secret value** tab. And click **Retrieve secret value** button.

![](https://cdn-images-1.medium.com/max/2166/0*m9rc0emqKATFIXWZ.png)

 1. Click **Edit** button, and check whether there is **dbname** and **immersionday** in key/value section. If they were not, click **Add** button, fill out the value and click **save** button.

![](https://cdn-images-1.medium.com/max/3090/0*C6jejklj2xJdpyJ9.png)

## Access RDS from EC2

Now that you have created a secret, you must give your web server permission to use it. To do this, we will create a Policy that allows the web server to read a secret. We will add this policy to the Role you previously assigned to the web server.

## [Allow the web server to access the secret](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/database/update-asg#allow-the-web-server-to-access-the-secret)

To follow the principle of least privilege, it is a best practice to grant the minimum required permissions to resources. In this case, you will grant permissions for the web server instances to access the specific secret containing the database credentials.

 1. Sign in to the AWS Management Console and open the [IAM console ](https://console.aws.amazon.com/iamv2/home#/home). In the navigation pane, choose **Policies**, and then choose **Create Policy**.

![](https://cdn-images-1.medium.com/max/2206/0*2s_XMJdOPbbTxbsn.png)

 1. Click **Choose a service**.

![](https://cdn-images-1.medium.com/max/2000/0*coA0QieAl5gKpRM3.png)

 1. Type **Secrets Manager** into the search box. Click **Secrets Manager**.

![](https://cdn-images-1.medium.com/max/2000/0*1aaMv_bEjNOrDDKs.png)

 1. Under **Access level**, click on the carat next to **Read** and then check the box by **GetSecretValue**.

![](https://cdn-images-1.medium.com/max/2000/0*vktJYT4O6xweNTI6.png)

 1. Click on the carat next to **Resources**. For this lab, select **All resources**. Click **Next: Tags**.

For the lab, we’re allowing EC2 to access all secrets. With a real workload, you should consider allowing access to specific secrets.

![](https://cdn-images-1.medium.com/max/2000/0*krbckWWmQ5bQIAN4.png)

 1. Click **Next: Review**.

![](https://cdn-images-1.medium.com/max/2000/0*iAQck6-jgtjZA94c.png)

 1. On the **Review Policy** screen, give your new policy the name **ReadSecrets**. Click **Create policy**.

![](https://cdn-images-1.medium.com/max/2000/0*B4FLUCL0gcEDXsel.png)

 1. In the navigation pane, choose **Roles** and type **SSMInstanceProfile** into the search box. This is the role you created previously in [Connect to your Linux instance using Session Manager](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/basic-modules/10-ec2/ec2-linux/3-ec2-1). Click **SSMInstanceProfile**.

![](https://cdn-images-1.medium.com/max/2000/0*4WfVN2lfwQ0fpKhi.png)

 1. Under **Permissions policies**, click **Attach policies**.

![](https://cdn-images-1.medium.com/max/2376/0*Rd14cEIvYI1m3Oe-.png)

 1. Search for the policy you created called **ReadSecrets**. Check the box and click **Attach policy**.

![](https://cdn-images-1.medium.com/max/2306/0*iqruqhYtiPDqI0eH.png)

 1. Under **Permissions policies**, verify that **AmazonSSMManagedInstanceCore** and **ReadSecrets** are both listed.

![](https://cdn-images-1.medium.com/max/2000/0*o7gS6awme6LQIxjL.png)

## [Try the Address Book](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/database/update-asg#try-the-address-book)

 1. Access the [EC2 Console] ([https://console.aws.amazon.com/ec2/v2/home?instanceState=running ](https://console.aws.amazon.com/ec2/v2/home?instanceState=running)) window and click **load balancer**. After copying the **DNS name** of the load balancer created in the compute lab, open a new tab in your browser and paste it.

![](https://cdn-images-1.medium.com/max/2914/0*9W1FqPvL8b1rRq5T.png)

 1. After connecting to the web server, go to the RDS tab.

![](https://cdn-images-1.medium.com/max/2000/0*m98HcLXVGg4H7vLS.jpg)

 1. Now you can check the data in the database you created.

![](https://cdn-images-1.medium.com/max/2000/0*ib9HWg5TBMIav-IZ.png)

This is a very basic exercise in interacting with a MySQL database managed by AWS. RDS can support much more complex relational database scenarios, but hopefully this simple example will make the point clear. You are free to add/edit/delete content from the RDS database using the **Add Contact, Edit** and **Remove** links in the address book.

## [Architecture Configured So Far](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/database/update-asg#architecture-configured-so-far)

Now, with the work done so far, you have built a web service with guaranteed high availability. The infrastructure architecture we have constructed so far is as follows.

![](https://cdn-images-1.medium.com/max/2088/1*zL_zkbmpaap7AeDjL5gCeA.png)

## RDS Management Features

In multiple AZ deployments, Amazon RDS automatically provisions and maintains synchronous spare replicas in different availability zone. The default DB instance is synchronized from the availability zone to the spare replica to provide data redundancy.

## [RDS Failover Tests](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/database/manage-rds#rds-failover-tests)

When multiple AZs are enabled, Amazon RDS automatically switches to a spare replica in another availability zone if the DB instance has a planned or unplanned outage. The amount of time that failover takes to complete depends on the database activity and other conditions when the default DB instance becomes unavailable. The time required for failover is typically 60–120 seconds. However, if the transaction is large or the recovery process is complex, the time required for failover can be increased. When failover is complete, the RDS console UI takes additional time to reflect in the new availability zone.

 1. From the RDS management console, select **Databases**, select the instance that you want to proceed with the failover, and click **Failover** in the task menu.

![](https://cdn-images-1.medium.com/max/2682/0*5krOyr90NUJYrzZz.png)

 1. A message asking whether you’re going to failover the rdscluster. Press the **Failover** button.

![](https://cdn-images-1.medium.com/max/2000/0*agNKKnyjZA_xgNbT.png)

 1. The **refresh** button changes the status of **rdscluster** in the DB identifier to **Failing-over**. In a few minutes, press the **Refresh** button to see **Reader and Writer roles changed**. The failover is complete.

![](https://cdn-images-1.medium.com/max/2468/0*71lf9ngbmmbL3veh.png)

## [Create RDS Snapshot](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/database/manage-rds#create-rds-snapshot)

Let’s take a snapshot of the RDS in production. Snapshot can be created at any frequency for backup to database instances, and the database can be restored at any time based on the snapshots created.

 1. From the RDS management console, select **Databases**, and **Select the instance** on which you want to perform the snapshot operation. Select **Actions** > **Take snapshot** in the upper right corner.

![](https://cdn-images-1.medium.com/max/2058/0*MHYWRlH1GiH4PwdG.png)

 1. Type the name you want to use for the snapshot as immersionday-snapshot. Press the **Take Snapshot** button to complete the creation.

![](https://cdn-images-1.medium.com/max/2000/0*PkF9Z4tzoopsZ4H9.png)

 1. From the left RDS menu, select **Snapshots** and check the creation status of the snapshot. The state of the snapshot is the first ***creating*** state, and you can use that snapshot to restore the database when state become ***available***. To restore, select the **snapshot** and select **Actions** to see what you can do with that snapshot. **Restore Snapshot** allows you to create RDS instances with the same data based on snapshots taken. ***This lab will not perform a restore.***

![](https://cdn-images-1.medium.com/max/2094/0*9p5WjYxbMgHkex5O.png)

## [Change RDS Instance Type](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/database/manage-rds#change-rds-instance-type)

Scale-Up/Scale-Down of RDS instances can be done very simply through the RDS Management Console.

 1. Let’s change the specification of the RDS instance by **selecting the instance** you want to change and clicking **Modify**.

![](https://cdn-images-1.medium.com/max/2000/0*wvuTMy8T5XCU17RU.png)

 1. You can select the specification of the instance that you want to change by selecting the list box of **instance classes**. Let’s choose **db.r6g.large** here.

![](https://cdn-images-1.medium.com/max/2000/0*1XQWUTZrD87cNEYD.png)

 1. Scroll to the bottom and select **Continue** to go to the page where you check the instance’s current value and new value and select when to apply.

![](https://cdn-images-1.medium.com/max/2000/0*gAtkZyZjbGz7Q0he.png)

 1. Select **Apply immediately**. In this case, RDS changes its instance immediately after perform a back up task. Then click **Modify DB Instance**. Depending on the type of instance and the amount of data to back up, it can take several minutes. Therefore, you should expect a certain amount of downtime for RDS services(Redundant configuration minimizes downtime).

![](https://cdn-images-1.medium.com/max/2000/0*hHKulBpWbAoYPywA.png)

In case of selecting **Apply during the next scheduled maintenance window**, make the change in the user’s Maintenance Window, which is specified on a weekly basis.

 1. You can see that the status of the instance has changed to ***Modifying***.

![](https://cdn-images-1.medium.com/max/2046/0*Nu2PXw0bS0V4ORlt.png)

 1. When you click refresh button again, you can see that the Writer instance has changed. This is because the instance you selected earlier for the size change was the Writer instance. RDS minimizes downtime through failover before resizing. If you wait a moment, you will see that the change to ***Available*** status has been completed as shown below.

![](https://cdn-images-1.medium.com/max/2048/0*_DgNowN5rIXi2xu6.png)

![](https://cdn-images-1.medium.com/max/2064/0*zDdHBEK96rph_V48.png)

RDS can change the size of the instance at any time. However, the size of the database does not support shrink after scaling up.

## Connect RDS Aurora

Let’s try to make an RDS connection through MySQL CLI, which is used for general database management/operation.

To do this,

 1. Create an EC2 instance with the AMI created in Public Subnet within the VPC-Lab. The networking option should allow Public IP.

 2. Changes the security group settings for RDS Aurora. Configure the newly created EC2 instance to accept security groups as sources.

 3. Log in to the EC2 instance you just created with SSH, and connect to RDS Aurora through the MySQL Client. The EC2 web server already has MySQL client installed during EC2 deployment.

Organizing the above items will be a challenge. Once the setup is successful, you can connect to the CLI environment and perform mysql commands as shown below.

    $ ssh -i AWS-ImmersionDay-Lab.pem ec2-user@”EC2 Host FQDN or IP”
    Last login: Sun Feb 18 14:41:59 2018 from 112.148.83.236
    
           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    
    https://aws.amazon.com/amazon-linux-ami/2017.09-release-notes/
    
    
    $ mysql -u awsuser -pawspassword -h awsdb.ccjlcjlrtga1.ap-northeast-2.rds.amazonaws.com
    
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 34
    Server version: 5.6.10 MySQL Community Server (GPL)
    
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | immersionday       |
    | mysql              |
    | performance_schema |
    +--------------------+
    4 rows in set (0.01 sec)
    
    mysql> use immersionday;
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A
    
    Database changed
    mysql> show tables;
    +------------------------+
    | Tables_in_immersionday |
    +------------------------+
    | address                |
    +------------------------+
    1 row in set (0.01 sec)
    
    mysql> select * from address;
    +----+-------+--------------+---------------------+
    | id | name  | phone        | email               |
    +----+-------+--------------+---------------------+
    |  1 | Bob   | 630-555-1254 | bob@fakeaddress.com |
    |  2 | Alice | 571-555-4875 | alice@address2.us   |
    +----+-------+--------------+---------------------+
    2 rows in set (0.00 sec)
    
    mysql>

## Storage — Amazon S3

## Create Bucket on S3

All objects in Amazon S3 are stored within a bucket. You must create a Bucket before storing data on Amazon S3.

## [Create Bucket](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/storage/create-bucket#create-bucket)

 1. From the AWS Management Console, connect to [S3 ](https://console.aws.amazon.com/s3). Press **Create bucket** to create a bucket.

![](https://cdn-images-1.medium.com/max/2662/0*Ugn3MszWeHTxNFw_.png)

It is a best practice to use S3 Bucket Policy and Access Control Lists (ACLs) to control access to your S3 buckets and objects, following the principle of least privilege.

 1. Enter a unique bucket name in the ***Bucket name*** field. For this lab, type immersion-day-user_name, substituiting user-name with your name. ***All bucket names in Amazon S3 have to be unique and cannot be duplicated***. In the **Region** drop-down box, specify the region to create the bucket. In this lab, select the region closest to you. The images will show the **Asia Pacific (Seoul)** region. **Object Ownership** change to **ACLs enabled**. Bucket settings for Block Public Access use default values, and select **Create bucket** in the lower right corner.

![](https://cdn-images-1.medium.com/max/2000/0*jj2wfP2Ur_Tgay-F.png)

Bucket names must comply with these rules:

* Can contain lowercase letters, numbers, dots (.), and dashes (-).

* Must start with a number or letter.

* Can be specified from a minimum of 3 to a maximum of 255 characters in length.

* Cannot be specified in the format like the IP address (e.g., 265.255.5.4).

There may be additional restrictions depending on the region in which the bucket is created. The name of the bucket cannot be changed once it is created and is included in the URL to specify objects stored within the bucket. Please make sure that the bucket you want to create is named appropriately.

 1. A bucket has been created on Amazon S3.

![](https://cdn-images-1.medium.com/max/2030/0*qgkHd-DJgwEAbopi.png)

There are no costs incurred for creating bucket. You pay for storing objects in your S3 buckets. The rate you’re charged depends on the region you are using, your objects’ size, how long you stored the objects during the month, and the storage class. There are also per-request fees. [Click for more information](https://aws.amazon.com/s3/pricing/)

## Adding objects to buckets

If the bucket has been created successfully, you are ready to add the object. Objects can be any kind of file, including text files, image files, and video files. When you add a file to Amazon S3, you can include information about the permissions and access settings for that file in the metadata.

## [Adding objects for static Web hosting](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/storage/put-object#adding-objects-for-static-web-hosting)

This lab hosts static websites through S3. The static website serves as a redirect to an instance created by the VPC Lab when you click on a particular image. Therefore, prepare one image file, one HTML file, and an ALB DNS name.

 1. Download the image file [aws.png ](https://static.us-east-1.prod.workshops.aws/public/2e449d3a-fc13-44c9-8c99-35a37735e7f5/static/common/s3_advanced_lab/aws.png)and save it as aws.png.

 2. Write index.html using the source code below.

    <html>
        <head>
            <meta charset="utf-8">
            <title> AWS General Immersion Day S3 HoL </title>
        </head>
        <body>
            <center>
            <br>
            <h2> Click image to be redirected to the EC2 instance that you created </h2>
            <img src="{{Replace with your S3 URL Address}}" onclick="window.location='DNS Name'"/>
            </center>
        </body>
    </html>

 1. Upload the aws.png file to S3. Click ***S3 Bucket*** that you just created.

![](https://cdn-images-1.medium.com/max/2030/0*THHEqNi_pJkWbI_h.png)

 1. Click the **Upload** button. Then click the **Add files** button. Select the pre-downloaded aws.png file through File Explorer. Alternatively, place the file in Drag and Drop to the screen.

![](https://cdn-images-1.medium.com/max/2000/0*zpbu8cQTQ13lLtma.png)

![](https://cdn-images-1.medium.com/max/2000/0*ZCv60Sk9Xm6VOcLv.png)

 1. Check the file information named aws.png to upload, then click the **Upload** button at the bottom.

![](https://cdn-images-1.medium.com/max/2000/0*j2aqhsGupGDEyScQ.png)

 1. Check the URL information to fill in the image URL in index.html file. Select the uploaded aws.png file and copy the ***Object URL*** information from the details on the right.

![](https://cdn-images-1.medium.com/max/2418/0*WZeC3tKINS9BMPdO.png)

 1. Paste ***Object URL*** into the image URL part of the index.html. Then specify the ***ALB DNS Name*** of the load balancer created by [Deploy auto scaling web service](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/compute/auto-scaling) to redirect to ALB when you click on the image.

![](https://cdn-images-1.medium.com/max/4000/0*79kA-SBdikQFKXGJ.png)

 1. Upload the index.html file to S3 following the same instructions as you did to upload the image.

![](https://cdn-images-1.medium.com/max/2000/0*hegLvEkLi7_AYY_K.png)

 1. If you check the objects in your S3 bucket, you should see 2 files.

![](https://cdn-images-1.medium.com/max/2446/0*zLPmiEkZp6ojS46q.png)

Congratulations! You have successfully created an S3 bucket and uploaded objects into it.

## View objects

Now that you’ve added an object to your bucket, let’s check it out in your web browser.

## [View Objects](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/storage/view-object#view-objects)

 1. In the Amazon S3 Console, please ***click the object*** you want to see. You can see detailed information about the object as shown below.

![](https://cdn-images-1.medium.com/max/2152/0*1bAlVtYdYLUCk00q.png)

By default, all objects in the S3 bucket are owner-only(Private). To determine the object through a URL of the same format as [***https://{Bucket}.s3.{region}.amazonaws.com/{Object}***,](https://{Bucket}.s3.{region}.amazonaws.com/{Object},) you must grant ***Read*** permission for external users to read it. Alternatively, you can create a signature-based Signed URL that contains credentials for that object, allowing unauthorized users to access it temporarily.

 1. Return to the previous page and select the **Permissions** tab in the bucket. To modify the application of **Block public access (bucket settings)**, press the right Edit button.

![](https://cdn-images-1.medium.com/max/2156/0*_vZhn0Z7wrEEnOua.png)

 1. **Uncheck box** and press the **Save changes** button.

![](https://cdn-images-1.medium.com/max/2000/0*YANG98kAl_ju7kh_.png)

 1. Enter confirm in the bucket's Edit Block public access pop up window and press the **Confirm** button.

![](https://cdn-images-1.medium.com/max/2000/0*Mn0UgR1Gz5n2vXYO.png)

 1. Click the **Objects** tab, select the uploaded **files**, click the **Action** drop-down button, and press the **Make public** button to set them to public.

![](https://cdn-images-1.medium.com/max/2644/0*MyfsYviX7q46SbXS.png)

 1. When the confirmation window pops up, press the **Make public** button again to confirm.

![](https://cdn-images-1.medium.com/max/2000/0*-Hs8aNHqrD0cDKis.png)

It is a best practice to periodically review and audit the permissions and access settings for your S3 buckets and objects to ensure they align with your security requirements and the principle of least privilege.

 1. Return to the bucket page, select index.html, and click the ***Object URL*** link in the Show Details entry.

![](https://cdn-images-1.medium.com/max/2218/0*Tiq6Zx9ez8GqFXMH.png)

 1. When you access the HTML object file object URL, the following screen is printed.

![](https://cdn-images-1.medium.com/max/2000/0*d1MppYp0fEFICXix.png)

 1. When you click on an image, it is redirected to the instance’s web page you created.

![](https://cdn-images-1.medium.com/max/2000/0*MrEDxuD5rzF-4ttW.png)

## Enable Static Web Site Hosting

You can use Amazon S3 to host static websites.

## [Static Web Site Settings](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/storage/static-web-hosting#static-web-site-settings)

A static website refers to a website that contains static content (HTML, image, video) or client-side scripts (Javascript) on a web page. In contrast, dynamic websites require server-side processing, including server-side scripts such as PHP, JSP, or ASP.NET. Server-side scripting is not supported on Amazon S3. If you want to host a dynamic website, you can use other services such as EC2 on AWS.

 1. In the S3 console, select the bucket you just created, and click the **Properties** tab. Scroll down and click the Edit button on **Static website hosting**.

![](https://cdn-images-1.medium.com/max/2000/0*P9Yt_qRCgSDolqZv.png)

![](https://cdn-images-1.medium.com/max/2000/0*8RkJnnj65ulskone.png)

 1. Activate the static website hosting function and select the hosting type and enter the index.html value in the Index document value, then click the **save changes** button.

![](https://cdn-images-1.medium.com/max/2000/0*Aozf0RvFdM78NPy3.png)

 1. Click **Bucket website endpoint** created in the **Static website hosting** entry to access the static website.

![](https://cdn-images-1.medium.com/max/2000/0*VnQKxSOEqnICJpRi.png)

 1. This allows you to host static websites using Amazon S3.

![](https://cdn-images-1.medium.com/max/4000/0*iNnLVhBR9RperRux.png)

## Move objects

You have seen the ability to add objects to buckets and verify them so far. Now, let’s see how we can move objects to different buckets or folders.

## [Move Objects](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/storage/move-object#move-objects)

 1. Create a temporary bucket for moving objects between buckets (Bucket name: immersion-day-myname-target). Substitute **myname** with your name. Rememeber the naming rules for the bucket. **Block all public access** ***Uncheckbox*** for quick configuration.

![](https://cdn-images-1.medium.com/max/2000/0*74wIGd2Wmuq5p9Cb.png)

 1. Check the notification window below and select **Create bucket**.

![](https://cdn-images-1.medium.com/max/2000/0*JOCBaAt5yH_w-fKW.png)

 1. In the Amazon S3 Console, select the bucket that contains the object (the first bucket you created) and click the checkbox for the object you want to move. Select the **Actions** menu at the top to see the various functions you can perform on that object. Select **Move** from the listed features.

![](https://cdn-images-1.medium.com/max/2098/0*o07wQ1D9eVtUbDzP.png)

 1. Select the destination as **bucket**, then click the **Browse S3** button to find the new bucket you just created.

![](https://cdn-images-1.medium.com/max/2000/0*XTL0i3G5Sn9QRYBO.png)

 1. Click the bucket name in the pop-up window, then select the destination (arrival) bucket. Click the **Choose destination** button.

![](https://cdn-images-1.medium.com/max/2000/0*QMllVh7-UwU5xTIf.png)

![](https://cdn-images-1.medium.com/max/2000/0*RyKGA7hqpZ0D6So_.png)

 1. Check that the object has moved to the target bucket.

![](https://cdn-images-1.medium.com/max/2000/0*VBQNoRoL1FCoqg5j.png)

Even though you move an object, its existing permissions remain intact.

## Enable Bucket versioning

You can use ***Bucket Versioning*** if you want to update existing files to the latest version within the same bucket, but still want to keep the existing version.

It is a best practice to enable versioning on your S3 buckets to protect against accidental deletion or overwrites of objects, and to maintain a history of changes to your data.

## [Enable versioning](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/storage/enable-versioning#enable-versioning)

 1. In the Amazon S3 Console, select the first S3 bucket we created. Select the **Properties** menu. Click the Edit button in **Bucket Versioning**.

![](https://cdn-images-1.medium.com/max/2000/0*oQ4LS5Qj0Bns6dcQ.png)

 1. Click the enable radio button on **Bucket Versioning**, then click **Save changes**.

![](https://cdn-images-1.medium.com/max/2000/0*_5MkFQfPxNf5BFcZ.png)

 1. In this lab, the index.html file will be modified and re-uploaded with the same name. Make some changes to the **index.html** file. Then upload the modified file to the same S3 bucket.

 2. When the changed file is completely uploaded, click the object in the S3 Console. You can view ***current version*** information by clicking the **Versions** tab on the page that contains object details.

![](https://cdn-images-1.medium.com/max/2208/0*xFd85yZbQAQCqOdk.png)

Congratulations on your progress! You’ve successfully learned how to add and verify objects in Amazon S3 buckets, move objects between buckets or folders, and utilize bucket versioning to update files while preserving existing versions.

## Deleting objects and buckets

You can delete unnecessary objects and buckets to avoid unnecessary costs.

 1. In the Amazon S3 Console, select the **Bucket** that you want to delete. Then click **Delete**. A dialog box appears for deletion.

![](https://cdn-images-1.medium.com/max/2020/0*FDU8vw63E54ljZDb.png)

 1. There is a warning that buckets cannot be deleted because they are not empty. Select **empty bucket configuration** to empty buckets.

![](https://cdn-images-1.medium.com/max/2000/0*AZ0Slr_aLdeCqg3U.png)

 1. **Empty bucket** performs a one-time deletion of all objects in the bucket. Confirm by typing **permanently delete** in the box. Then click the **Empty** button.

![](https://cdn-images-1.medium.com/max/2000/0*GRfLniJEJxOZfrjg.png)

 1. Now the bucket is empty. Perform task 1 again. **Enter a bucket name** and press the **Delete bucket** button.

![](https://cdn-images-1.medium.com/max/2000/0*NqUxleAwmJHgvXpu.png)

***Congratulations!! You have completed all the workshop. Thank you for your efforts.***

## Clean up resource

If you participated in an AWS event using an AWS-provisioned account, no cleanup is necessary. However, if you completed this workshop with **your own account**, we strongly recommend following this guide to delete the resources and avoid incurring costs

Delete the resources you created for the lab in reverse order.

## [Database](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/cleanup#database)

**Delete an Amazon RDS Cluster**

 1. After accessing to the Amazon RDS console, select **DB Instances**.

![](https://cdn-images-1.medium.com/max/4000/0*tWWrGdfxWo_EqnPy.png)

 1. By default, an **Amazon RDS cluster** has delete protection enabled to prevent accidental deletions. To disable it, select the **Cluster** and click the **Modify** button.

![](https://cdn-images-1.medium.com/max/4000/0*2N6dyXM5wAVpHMCp.png)

 1. Uncheck the **Enable deletion protection** button and click the **Continue** button.

![](https://cdn-images-1.medium.com/max/3120/0*ndzFN3UTaFkTdc7q.png)

 1. For immediate deletion, select **Apply immediately** and click the **Modify cluster** button.

![](https://cdn-images-1.medium.com/max/3132/0*_4SwezLdK9FKslpz.png)

 1. In order to delete a DB Cluster, you must first delete the DB instances included in the cluster. They can be deleted in any order, but we will delete the **Writer instance** first. Select the **Writer instance**, and click the **Delete** button on the **Actions** menu.

![](https://cdn-images-1.medium.com/max/4000/0*9I434nKfpQ4WWorV.png)

 1. Type **delete me** in the blank and click the **Delete** button.

![](https://cdn-images-1.medium.com/max/2956/0*gsYp5gsHQb6TFl52.png)

 1. This time, we will delete the **Reader instance**. Select the **Reader instance** and click the **Delete** button on the **Actions** menu.

![](https://cdn-images-1.medium.com/max/3720/0*R2YCjE3YEBL9fyiX.png)

 1. Type **delete me** in the blank and click the **Delete** button.

![](https://cdn-images-1.medium.com/max/2488/0*B-hNkn9inFoddDGp.png)

 1. Lastly, we will delete the **DB Cluster**. Click the **Delete** button on the **Actions** menu.

![](https://cdn-images-1.medium.com/max/4000/0*Iiv2Yu9EsE_rHza3.png)

 1. Uncheck the **Take a final snapshot** button, check the **I acknowledge that automatic backups, including system snapshots and point-in-time recovery, are no longer available when I delete an instance** button, and type **delete me** in the blank. Click **Delete DB Cluster** and the DB cluster will be deleted.

![](https://cdn-images-1.medium.com/max/2464/0*a7hXG85_JKTKACmF.png)

**Delete a Amazon RDS Snapshot**

 1. To delete the snapshot of the DB Cluster created during the lab, select **immersionday-snapshot** and click the **Delete snapshot** button on the **Actions** menu.

![](https://cdn-images-1.medium.com/max/4000/0*wx70oo56BN7dworn.png)

 1. Click the **Delete** button.

![](https://cdn-images-1.medium.com/max/2976/0*YiN5q7vELPWYJf-u.png)

**Delete a secret in AWS Secrets Manager**

 1. We’re going to delete the secret that stored a **RDS credential** during the lab. Type Secrets Manager in the AWS console search bar and then select it.

![](https://cdn-images-1.medium.com/max/4000/0*H7VBedh9B7eNbBas.png)

 1. Select **mysecret**.

![](https://cdn-images-1.medium.com/max/4000/0*oLRjHZInOt-xf66z.png)

 1. Click **Delete secret** on the **Actions** menu.

![](https://cdn-images-1.medium.com/max/4000/0*Zj3RohbpfRrU7-sK.png)

 1. To prevent accidental deletion of secrets, AWS Secrets Manager has a deletion wait time of **minimum 7 days** and **maximum 30 days**. Enter the minimum time of 7 days and press the **Schedule deletion** button.

![](https://cdn-images-1.medium.com/max/3864/0*HOgRxQ1oAfb-t2tv.png)

## [Compute](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/cleanup#compute)

**Delete an Auto Scaling Group**

 1. We’re going to delete the **Auto Scaling Group** that we used during the lab. Type **EC2** in the AWS Console search bar and select it. Select **Auto Scaling Groups** from the left menu. Select the **Web-ASG** that we created in the lab and click the **Delete** button on the **Actions** menu.

![](https://cdn-images-1.medium.com/max/4000/0*64edaw2shcaTIwOe.png)

 1. Type **delete** in the blank and click the **Delete** button.

![](https://cdn-images-1.medium.com/max/3424/0*FlFFVwVFqL_rhRPr.png)

**Delete an Application Load Balancer**

 1. Next, we’re going to delete the **Application Load Balancers**. Select **Load Balancers** from the left menu. Then select the **Web-ALB** that we created in the lab and click the **Delete load balancer** button in the **Actions** menu.

![](https://cdn-images-1.medium.com/max/4000/0*EOZQDabZ-eT_kmr3.png)

 1. Type **confirm** in the blank and click the **Delete** button.

![](https://cdn-images-1.medium.com/max/3440/0*tLvD3MMz9-GyVAiv.png)

**Delete a Target Group**

 1. We’re going to delete the Target Group we created when we created the Application Load Balancer. Select Target Groups from the left menu. Select the Target Group we created in the lab, web-TG, and click the Delete button on the Actions menu.

![](https://cdn-images-1.medium.com/max/4000/0*SiMtcFgbaQq5_ldK.png)

 1. Click the **Yes, delete** button.

![](https://cdn-images-1.medium.com/max/2440/0*ONyCfh2uTtfwpZuR.png)

**Delete EC2 AMIs**

 1. Select **AMIs** from the left menu. Select the AMI named **Web Server v1** that you created in the lab. Click the **Deregister AMI** button on the **Actions** menu.

![](https://cdn-images-1.medium.com/max/2694/0*jgmwx8WaRsXuGh66.png)

 1. Click the **Deregister AMI** button.

![](https://cdn-images-1.medium.com/max/2000/0*CMvj5BtzsIySfF1K.png)

**Delete EC2 Snapshots**

 1. You’ve just deleted an AMI, but this action doesn’t automatically remove the associated snapshot. So you need to remove it manually. From the left menu, choose **Snapshots**. Be sure to note the snapshot’s creation date. Then, select the snapshot you created in the lab, and click the **Delete snapshot** button on the **Actions** menu.

![](https://cdn-images-1.medium.com/max/2684/0*pqU-2m92KXWfkCMf.png)

 1. Click the **Delete** button.

![](https://cdn-images-1.medium.com/max/2000/0*lV7qRMgn5p90uuEB.png)

 1. Select **Launch Templates** from the left menu. Select the template named **Web** that you created in the lab. Click the **Delete template** button on the **Actions** menu.

![](https://cdn-images-1.medium.com/max/2724/0*7EpjYwkJC9S9aXFO.png)

 1. Type **Delete** in the blank and click the **Delete** button.

![](https://cdn-images-1.medium.com/max/2000/0*gCV6yKQBCnV08v9n.png)

**(Optional) Delete an EC2 instance**

 1. If you went through the [(Optional) Connect RDS Aurora](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/database/challenge-aurora) section during the database lab, you need to delete the EC2 instance you created in the lab. Select **Instances **from the left menu. Select the EC2 instance you created during the lab, and click the **Terminate instance **button on the **Instance state** menu.

![](https://cdn-images-1.medium.com/max/4000/0*jyRpYc3tbBRjQisA.png)

 1. Click the **Terminate** button.

![](https://cdn-images-1.medium.com/max/3316/0*EgmkQO1xWBuO4kQP.png)

## [Network](https://catalog.us-east-1.prod.workshops.aws/workshops/869a0a06-1f98-4e19-b5ac-cbb1abdfc041/en-US/advanced-modules/cleanup#network)

**Delete VPC endpoints**

 1. You’re almost there. Type **VPC** in the AWS Console search bar and select it. Select **Endpoints** from the left menu. Select **S3 endpoint**, the endpoint you created in the lab, and click the **Delete VPC endpoints** button on the **Actions** menu.

![](https://cdn-images-1.medium.com/max/2800/0*5Ghx5MsycD8XG8NB.png)

 1. Type **delete** in the blank, and click the **Delete** button.

![](https://cdn-images-1.medium.com/max/2188/0*a2RZyUu1NWPzNLCP.png)

**Delete a NAT gateway**

 1. Select **NAT gateways** from the left menu and select **VPC-Lab-nat-public** you created during the lab. Click the **Delete NAT gateway** button on the **Actions** menu.

![](https://cdn-images-1.medium.com/max/2808/0*z0_EwyTYHkcfNe2x.png)

 1. Type **delete** in the blank and click the **Delete** button.

![](https://cdn-images-1.medium.com/max/2000/0*GTziKPZOr6WVYqm_.png)

**Delete an Elastic IP**

 1. You’ve just deleted the NAT gateway, but this action doesn’t automatically delete the Elastic IP that the NAT gateway used, so you need to remove it manually. Select **Elastic IPs** from the left menu, and select **VPC-Lab-eip-ap-northeast-2a**. (The name after VPC-Lab-eip may vary depending on your region.) Click the **Release Elastic IP addresses** button on the **Actions** menu. If it says it is still associated with the NAT gateway and cannot be deleted, refresh the webpage and try again.

![](https://cdn-images-1.medium.com/max/2800/0*oZZhVveNIHvSnIZc.png)

 1. Click the **Release** button.

![](https://cdn-images-1.medium.com/max/2026/0*f1MkXdaB_2Fw8Z5a.png)

**Delete a Security Group**

 1. We’re going to delete the **Security Group** you created during the lab. Select **Security Groups** from the left menu. Select **Immersion Day — Web Server and DB-SG** first, and then click the **Delete security groups** button on the **Actions** menu. The reason for not deleting all security groups at once is that some security groups reference other security groups in their inbound rules. A security group that is being referenced cannot be deleted until the security group that is referencing it is deleted. Therefore, delete the security groups in the following order: **Immersion Day — Web Server, DB-SG -> ASG-Web-Inst-SG -> web-ALB-SG**.

![](https://cdn-images-1.medium.com/max/4000/0*cpxYXyLyTb6VtkmB.png)

 1. Type **delete** in the blank and click the **Delete** button.

![](https://cdn-images-1.medium.com/max/4000/0*gQ-1b55K9wz6AE1n.png)

 1. Select **ASG-Web-Inst-SG** and click the **Delete security groups** button on the **Actions** menu.

![](https://cdn-images-1.medium.com/max/4000/0*9j6oN1XJEXSpQnNl.png)

 1. Click the **Delete** button.

![](https://cdn-images-1.medium.com/max/4000/0*3pBGtGNub087tD08.png)

 1. Select **web-ALB-SG** and click the **Delete security groups** button on the **Actions** menu.

![](https://cdn-images-1.medium.com/max/4000/0*rcXAdIjHd9bittuK.png)

 1. Click the **Delete** button.

![](https://cdn-images-1.medium.com/max/4000/0*wRXTYIiOIGTncjrf.png)

**Delete a VPC**

 1. Finally, select **Your VPCs** from the left menu, and select the **VPC-Lab-vpc** that you created during the lab. Click the **Delete VPC** button in the **Actions** menu.

![](https://cdn-images-1.medium.com/max/2802/0*_kYoUJ9u5mXnAEFG.png)

 1. Type **delete** in the blank and click the **Delete** button.

![](https://cdn-images-1.medium.com/max/2000/0*g3_lB5r2cPfxFc1G.png)

We strongly recommend that you double-check to make sure you haven’t missed anything, as some resources that weren’t cleared may incur costs.

### Conclusion

This project demonstrates how to build a **fault-tolerant, multi-tier web application on AWS** that provides high availability and efficient resource utilization. By leveraging **AWS VPC**, **EC2**, **Aurora**, and **S3**, the architecture ensures reliable performance, even under high traffic or unexpected failures, with scalable backend resources and optimized delivery for static content.

This setup is highly applicable for production-grade web applications and offers an ideal framework for other high-availability, fault-tolerant projects.

## Code Repository

To explore and experiment with the project’s code and documentation, visit the [**GitHub Repository](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/5.%20Multi-Tier%2C%20Highly%20Available%2C%20Fault-Tolerant%20Web%20Application%20on%C2%A0AWS/Multi-Tier%2C%20Highly%20Available%2C%20Fault-Tolerant%20Web%20Application%20on%C2%A0AWS)**. Here you can access the complete code, follow along with detailed instructions, and customize the applications to fit your own use cases.
>  ***Asif Khan — Aspiring Cloud Architect | Weekly Cloud Learning Chronicler***

[**LinkedIn](https://www.linkedin.com/in/asif0108/)**/[**Twitter](https://x.com/asif26073)**/[**GitHub](https://github.com/Dark-Cookie)**
