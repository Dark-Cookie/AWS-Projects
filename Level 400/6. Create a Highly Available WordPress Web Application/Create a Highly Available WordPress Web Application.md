
![](https://cdn-images-1.medium.com/max/8192/1*XSMCMgzlfWFCUKlFaflG2g.jpeg)

## AWS Project: Create a Highly Available WordPress Web Application

### Introduction

This project focuses on creating a highly available, scalable WordPress web application on AWS using key services such as **Amazon VPC**, **Amazon RDS**, **Amazon EFS**, and **EC2** with **Auto Scaling** and **Application Load Balancer** (ALB). The primary goal of this project is to build a reliable, resilient architecture for WordPress that can handle fluctuating traffic while providing seamless user experience.

By deploying WordPress in a multi-tier architecture, I learned how to leverage AWS infrastructure to meet the needs of high-traffic applications, ensuring both availability and fault tolerance.

### Tech Stack

* **Amazon VPC**: Provides isolated network environments and security controls.

* **Amazon RDS**: Hosts a reliable, highly available MySQL database for WordPress.

* **Amazon EFS**: A scalable, shared storage solution for dynamic content, enabling multiple EC2 instances to access the same data.

* **Amazon EC2**: Hosts WordPress instances and dynamically scales with Auto Scaling groups.

* **Application Load Balancer (ALB)**: Distributes incoming traffic across multiple instances for enhanced availability.

### Prerequisites

* **AWS Account**: Required to access and configure AWS services.

* **AWS CLI**: For resource management and deployment tasks.

* **Basic AWS Networking Knowledge**: Understanding of VPCs, subnets, and security groups.

* **WordPress Setup Knowledge**: Familiarity with WordPress installation and configuration.

### Problem Statement or Use Case

**Problem**: WordPress applications often face challenges around **scalability** and **availability**, especially with traditional hosting environments where capacity may not automatically adjust to traffic demands.

**Solution**: Using AWS, this project demonstrates how to set up WordPress in a highly available, scalable architecture that can handle fluctuations in traffic and ensure minimal downtime. AWS’s managed services enable the environment to scale automatically, handle failover, and improve the user experience.

**Real-World Relevance**: This approach is ideal for production-grade WordPress applications with high traffic, such as e-commerce sites, news platforms, and corporate blogs. By implementing this architecture, companies can reduce manual management, lower costs, and improve their WordPress application’s reliability and speed.

## Step-by-Step Implementation

## Configure the network

## [Create a new Virtual Private Cloud (VPC)](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/foundations/lab1#create-a-new-virtual-private-cloud-(vpc))

As a starting point for the workshop you will need to login to your AWS account, select the region of your choice and create a new VPC.

To do this click on **Your VPCs** on the left hand side of the console and click **Create VPC**. Enter wordpress-workshop as name for your VPC and a CIDR range such as the one below. When you're fiinished click **Create**.

![](https://cdn-images-1.medium.com/max/2000/0*arp_R5B4zAa3eHNb.png)

After creating the VPC, on the VPC details page click on **Actions** and then select **Edit VPC Settings**.
Make sure to enable both **DNS resolution** and **DNS hostnames** under **DNS Settings** and click **Save**.

![](https://cdn-images-1.medium.com/max/2000/0*AbInTljU2pEuhIqq.png)

## [Create public and private subnets in the new VPC](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/foundations/lab1#create-public-and-private-subnets-in-the-new-vpc)

Once the VPC has been created, the next step is to create the subnets that will be used to host the application across two different Availability Zones. We are going to create six subnets in total, three for each AZ, as shown in the following diagram:

![](https://cdn-images-1.medium.com/max/2000/0*JH-nb0XsibL5EnFb.png)

The first pair of subnets, *Public*, will be accessible from the Internet and contain load balancers and NAT gateways. The second pair, *Application*, will contain application servers and your shared EFS filesystem. Your application servers will be able to communicate with the Internet via the NAT gateways but will only be addressable from the load balancers. Finally the *Database* pair of subnets will hold your active / passive relational database. It will be accessible to other resources in the VPC but will have no access to the Internet and cannot be addressed by the Internet or the load balancers.

To create each of the six subnets please select **Subnets** on the left of the AWS VPC console, then click on **Create subnet** and use the details in the table below to define the characteristics of each of your subnets. Make sure to always select the **Wordpress-workshop** VPC when creating the subnets.

![](https://cdn-images-1.medium.com/max/2000/0*7jbQLwxQATPSn65N.png)

The screenshots in this lab were taken from a deployment in the **Ireland (eu-west-1)** region, if you are building in a different AWS region please just ensure that you create your subnets in 2 different availability zones in the same region, such as *us-west-2a* and *us-west-2b*.

For each subnet specify a name and a CIDR range for the subnet. Be sure and create a public, application, and data subnet in each of two availability zones as detailed in the table below.

![](https://cdn-images-1.medium.com/max/2000/0*iIoesWqwBX2_TqA0.png)

At this point all the correct subnets have been created and they can route network traffic between them. In the next set of steps you will create an Internet Gateway, allowing communication between your VPC and the Internet. You will also configure your routing tables to only allow Internet communication with your public subnets and not the private application or data subnets.

## [Create an Internet Gateway and set up routing](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/foundations/lab1#create-an-internet-gateway-and-set-up-routing)

The following steps will allow connectivity from the Internet to the public subnets and also connectivity from the private subnets to the Internet via NAT gateways.

First you need to create a new Internet Gateway (IGW) from your VPC dashboard and attach it to the wordpress-workshop VPC. Start by clicking **Internet gateways** on the left hand side of the VPC console and then click the **Create Internet gateway** button. Enter a name for your IGW such as WP Internet Gateway and click **Create Internet gateway**.

![](https://cdn-images-1.medium.com/max/2000/0*m3lxtDcdlecqA4w5.png)

After the IGW has been created you need to associate it with your VPC by attaching it to your VPC.
Select **Attach to VPC** from the **Actions** drop-down menu then select the wordpress-workshop VPC from drop-down list of available VPCs and click on **Attach Internet gateway**.

![](https://cdn-images-1.medium.com/max/4000/0*26dKAmAw4shfd2eD.png)

The gateway will be used by instances and services running in the public subnets (e.g. Public Subnet A and Public Subnet B) to communicate to the Internet.

Once the gateway is created you will need to create a new routing table and associate it with the public subnets.
Create a new route table by selecting **Route tables** in the left-hand menu of the console and then clicking on the **Create route table** button.
Give it a **Name** and select the wordpress-workshop from the drop-down, then click on **Create route table**.

![](https://cdn-images-1.medium.com/max/2000/0*2aKpH7JygC6yaIGi.png)

After creating the route table, select it from the **Route tables** section of your VPC dashboard, then click on **Actions** -> **Edit routes** and add a default route via the Internet Gateway created in the previous step and click on **Save changes**.

![](https://cdn-images-1.medium.com/max/4000/0*gadljJGabSln299-.png)

Finally, you need to associate the newly created route table with the public subnets. To do that, click on the public route table, then click on **Subnet Associations**, edit by clicking on **Edit subnet associations** and select the two public subnets created earlier and click on **Save associations**.

![](https://cdn-images-1.medium.com/max/3222/0*lzmZPtrv3R-ExX1B.png)

## [Create one NAT gateway in each public subnet](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/foundations/lab1#create-one-nat-gateway-in-each-public-subnet)

The Wordpress instances will need to be able to connect to the Internet and download application and OS updates. To avoid dependencies across Availability Zones, you are going to create two NAT gateways, one for each Availability Zone where the application is deployed.

To do this, you will create one NAT Gateway in each Availability Zone, then create one route table for each application subnet, update the route table with a default route through the NAT gateway in the same AZ, and then associate the route table to the respective application subnet.

![](https://cdn-images-1.medium.com/max/2000/0*MoNxkL2o7mfqYlvx.png)

Go to the VPC dashboard in your account, select **NAT gateways** and create one gateway in each of the two public subnets (i.e. Public Subnet A and Public Subnet B) Always make sure you have selected the correct public subnet when creating the gateway.

![](https://cdn-images-1.medium.com/max/2000/0*sD5LCz0P-xvq2Mq0.png)

Now we need to create route tables for each of the two Application subnets and use the NAT gateways created earlier as the default gateway:

![](https://cdn-images-1.medium.com/max/2000/0*p2nPAtNgArjmBHfZ.png)

Edit the route table and add the default route via the NAT gateway in Application subnet A:

![](https://cdn-images-1.medium.com/max/4000/0*LFdtttYVbhIKp6LG.png)

Associate the route table with Application Subnet A:

![](https://cdn-images-1.medium.com/max/3720/0*8TkFo0JeKTwbxx4S.png)

Repeat the last three steps to also create a route table for Application Subnet B which uses the NAT gateway deployed in the second availability zone.

## [Verify your configuration](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/foundations/lab1#verify-your-configuration)

You have now created a virtual private cloud network across two availability zones within an AWS region. You have created six subnets, three in each availability zone, and have configured a route so that the Internet can communicate with resources in the public subnets and vice versa. The application subnets have been configured, via routing table, to communicate with the Internet via NAT gateways in the public subnets, and the data subnets can only communicate with resources in the six subnets, but not the Internet.

Please note that the information below is based on a VPC deployed in the *Ireland (eu-west-1)* region. If you had choosen a different region for your setup you need to adjust the region name accordingly.

You can compare your own configuration based on the screenshot below and move along when you have verified your setup.

Check the **Resource map** section of your VPC which shows your VPC, subnets, route tables, Internet gateways, NAT gateways which helps you visualise the resources.

![](https://cdn-images-1.medium.com/max/2948/0*6hJWfh2C_V9fFYv-.png)

## Building the Data Tier

## Set up the RDS database

## [Create database security groups](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/datatier/lab2#create-database-security-groups)

You will create 2 security groups:

* WP Database Clients will be attached to the EC2 instances running the web servers

* WP Database will be attached to the RDS DB instance

Visit the [Amazon VPC console ](https://console.aws.amazon.com/vpc/home?#SecurityGroups)and create 2 security groups.

First, create the WP Database Clients security group:

* Click on **Create security group**

* Fill in the *Security group name* and *Description* fields

* Select the wordpress-workshop from the drop-down

* Scroll to the bottom of the page and click on **Create security group**

![](https://cdn-images-1.medium.com/max/2598/0*jeepMw3cJCvwUQro.png)

Now create the WP Database security group:

* In the **Inbound Rules** section, click on **Add rule**

* Select Type **MySQL/Aurora** which allows traffic on port 3306 from **Custom** source *WP Database Clients* security group.

![](https://cdn-images-1.medium.com/max/2580/0*LBqnNXN-wVgpQFjf.png)

Please note, that you can search for the security group by name in the source field of the security group rule.

Now you are ready to create your RDS database.

## [Create an RDS subnet group](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/datatier/lab2#create-an-rds-subnet-group)

Amazon RDS is an easy to manage relational database service. When you use Amazon RDS to deploy a database in a highly available setup, it will create 2 instances in 2 different availability zones. To do this, when you create a database you specify a subnet group which tells RDS in which subnets it can deploy your database instances.

To create a DB subnet group browse to the [Amazon RDS console ](https://console.aws.amazon.com/rds/home), click on **Subnet groups **in the panel on your left, click on the **Create DB Subnet Group** button and use the following details:

* Name: Aurora-Wordpress

* Description: RDS subnet group used by Wordpress

* VPC: wordpress-workshop

![](https://cdn-images-1.medium.com/max/2000/0*h4V28k942sjPZnRY.png)

Scroll down and add the two **Data subnets** created earlier (one for each AZ) to your new subnet group and click **Create**.

![](https://cdn-images-1.medium.com/max/2000/0*S4asBXJt8yZnTM4V.png)

Please note, in order to get the ID of the data subnets, you can open a second tab and navigate to the [**Subnets **section ](https://console.aws.amazon.com/vpcconsole/home?#subnets:)of the VPC console. From the list of subnets select the one you are interested in. On the bottom of the screen you will then be able to copy the **Subnet ID** by clicking the Copy to clipboard icon beside the id.

## [Create the Aurora database instance](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/datatier/lab2#create-the-aurora-database-instance)

Once the subnet group has been created you are ready to launch the RDS-managed database.
Go to the [Amazon RDS Console ](https://console.aws.amazon.com/rds/home), select **Databases **from the menu on the left and click **Create database**.

Enter the following details:

* Database creation method: **Standard create**

* Engine options: **Aurora (MySQL Compatible)**

* Keep the default Engine Version

![](https://cdn-images-1.medium.com/max/2000/0*0Xx7q68GpEftCsJd.png)

* Use wpadminas **Master username**

* When prompted for the Master password, you can either click on Auto generate a password or create your own — in either case please make sure you write down the password as it will be required a few steps later when setting up the connectivity of the Wordpress instances to the database.

![](https://cdn-images-1.medium.com/max/2000/0*wKbxY2q6c4EyiwQ4.png)

Select the DB instance size together with a Multi-AZ deployment, required for high availability. To keep costs low, for this workshop we recommend using a burstable instance class (db.t4g.medium or similar). Burstable instances might not be suited for production environments.

![](https://cdn-images-1.medium.com/max/2000/0*UUh-STqwnSv4Tbqi.png)

In the connectivity section, make sure you select the wordpress-workshop VPC, together with the aurora-wordpress DB subnet group, and WP Database security group created earlier:

![](https://cdn-images-1.medium.com/max/2000/0*Qxhyh7t_huFpaRne.png)

![](https://cdn-images-1.medium.com/max/2000/0*y2NZTjMjCazyuF9A.png)

In the **Monitoring** section, uncheck **Turn on DevOps Guru**

![](https://cdn-images-1.medium.com/max/2000/0*069e8pUaGW2hTPji.png)

Expand **Additional configuration** section and specify an **Initial database name** of *wordpress*.

![](https://cdn-images-1.medium.com/max/2000/0*tZcrL9SkLHo99crq.png)

Finally, click on **Create database** to start building the cluster.
The database will take a few minutes to be provisioned and made available.

## [Verify your configuration](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/datatier/lab2#verify-your-configuration)

The active / passive database should now be available and running in two different availability zones, waiting for connections from any EC2 resource with the client security group associated to it.
Please compare your own configuration based on the screenshots below and move along when you have verified your setup.

**Security Groups**

![](https://cdn-images-1.medium.com/max/2000/0*37wtUvdVSZvQNw-g.png)

**Subnet group**

![](https://cdn-images-1.medium.com/max/3114/0*xJWA8DsTiDUd-kXO.png)

**Database setup**

![](https://cdn-images-1.medium.com/max/3112/0*TWzX41Rbe45cX39Y.png)

## Create the shared filesystem

Amazon Elastic File System (Amazon EFS) provides a simple, scalable, elastic file system for general purpose workloads for use with AWS Cloud services and on-premises resources. In this lab you will create an EFS cluster that will provide a shared filesystem for your Wordpress content.

## [Create filesystem security groups](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/datatier/lab3#create-filesystem-security-groups)

When using Amazon EFS, you specify Amazon EC2 security groups for your EC2 instances and security groups for the EFS mount targets associated with the file system. A security group acts as a firewall, and the rules that you add define the traffic flow.

In this workshop, you will create 2 security groups:

* WP EFS Clients will be attached to the EC2 instances running the web servers

* WP EFS will be attached to the EFS mount targets

Visit the [Amazon VPC console ](https://console.aws.amazon.com/vpc/home?#SecurityGroups)to create the 2 security groups.

First, create the WP EFS Clients security group:

* Click on **Create security group**

* Fill in the *Security group name* and *Description* fields

* Select the wordpress-workshop VPC from the drop-down

* Scroll to the bottom of the page and click on **Create security group**

![](https://cdn-images-1.medium.com/max/2740/0*09H6o2O3xttvXS3y.png)

Amazon EFS creates a shared file system and exposes it as a NFS share. The security group attached to the EFS mount points will need to allow inbound connections on the NFS TCP port 2049.

Create the WP EFS security group:

* In the **Inbound Rules** section, click on **Add rule**

* Select Type **NFS** and specify **Custom** source *WP EFS Clients* security group.

![](https://cdn-images-1.medium.com/max/2756/0*3X4iVTHfkT9iOqMb.png)

Please note, that you can search for the security group by name in the source field of the security group rule.

Now you are ready to create the EFS file system.

## [Create the EFS file system](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/datatier/lab3#create-the-efs-file-system)

To create an EFS file system visit the [Amazon EFS console ](https://console.aws.amazon.com/efs/home)and click **Create file system**.

Enter Wordpress-EFSin the **Name** field.
From the VPC drop down select the wordpress-workshop VPC and click **Customize**.

![](https://cdn-images-1.medium.com/max/2000/0*OV728OGFdwehapPc.png)

On the creation page, uncheck **Enable automatic backups** to avoid backing up the contents of the file system. It’s recommended to keep it enabled when deploying a production environment.

Keep all other settings unchanged and click **Next**.

![](https://cdn-images-1.medium.com/max/2128/0*LlRhIQn2k3aZ0mZg.png)

On the **Network access** page, under **Mount targets**, choose the two subnets created for the Data tier (Data subnet A and B). On the right side, under **Security groups**, associate the WP EFS security group created above to each mount target and remove the association with the *Default* security group.
Click **Next**.

![](https://cdn-images-1.medium.com/max/2470/0*CLj9Ny2MHOXDHCxg.png)

Accept the defaults on the next screen for **File system policy**

![](https://cdn-images-1.medium.com/max/2452/0*pbohCLPQnCNSPLhP.png)

Click **Next**, review and confirm the file system creation by clicking **Create**.

This will create two mount targets in the Data subnets and after a few moment the file system will become *Available*.

![](https://cdn-images-1.medium.com/max/3566/0*WfOZIsXgBsKM04AP.png)

## Build the Application Tier

## Create the load balancer

To distribute traffic across your Wordpress application servers you will need a load balancer. In this lab you will create an Application Load Balancer.

Application Load Balancer operates at the request level (layer 7), routing traffic to targets (EC2 instances, containers, IP addresses, and Lambda functions) based on the content of the request

## [Create load balancer and application security groups](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/application/lab4#create-load-balancer-and-application-security-groups)

Visit the [Amazon VPC console ](https://console.aws.amazon.com/vpc/home?#SecurityGroups)to create the security groups.

First, create the WP Load Balancer security group:

* Click on **Create security group**

* Fill in the *Security group name* and *Description* fields

* Select the wordpress-workshop from the drop-down

* In the **Inbound Rules** section, click on **Add rule**

* Select Type **HTTP** which allows traffic on port 80 from **My IP** source to limit access to your current public IP.

* Scroll to the bottom of the page and click on **Create security group**

Outside of a workshop environment you would likely want to modify the security group to allow access from any IP address. To learn more about security in and of the cloud please visit the [AWS Cloud Security ](https://aws.amazon.com/security/)website.

![](https://cdn-images-1.medium.com/max/2752/0*QHFrX5gG4qQ46oNY.png)

Now create the WP Web Servers security group:

* In the **Inbound Rules** section, click on **Add rule**

* Select Type **HTTP** which allows traffic on port 80 from **Custom** source *WP Load Balancer* security group.

![](https://cdn-images-1.medium.com/max/2750/0*Hui6vZM9Bnk87AWg.png)

Please note, that you can search for the security group by name in the source field of the security group rule.

## [Create a load balancer](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/application/lab4#create-a-load-balancer)

A load balancer distributes incoming application traffic across multiple targets, such as EC2 instances, in multiple Availability Zones, increasing the availability of the Wordpress platform.

From the [EC2 console ](https://console.aws.amazon.com/ec2/home)click **Load Balancers **on the left-hand menu and then click **Create load balancer**.
Click **Create **under **Application Load Balancer**.

![](https://cdn-images-1.medium.com/max/2000/0*kgQcV7OFeHSvWugA.png)

Give your load balancer a name and under **Network mapping** select the wordpress-workshop VPC.
Then tick the checkbox for both availability zones and select the public subnets created in the first lab.

![](https://cdn-images-1.medium.com/max/2230/0*GMLm7IU-FljgiV9C.png)

![](https://cdn-images-1.medium.com/max/2200/0*Bj82X3m1cktAPFGh.png)

Under the **Security groups** select the **WP Load Balancer** created earlier and remove any default security group.

![](https://cdn-images-1.medium.com/max/2200/0*fpNB_B9LJ45rF8uK.png)

Under **Listeners and routing** click on the link **Create target group**.

![](https://cdn-images-1.medium.com/max/2000/0*ObnR-4N3qY5ENL00.png)

This opens a new window for you to create a new target group. Use the following details

* Wordpress-TargetGroup as **Traget group name**

* wordpress-workshopas **VPC**

![](https://cdn-images-1.medium.com/max/2000/0*iZ1M8HCijqpmKHpM.png)

Then, in the **Health checks** section, enter the following path:

* /phpinfo.php In the next lab you will create the Launch Template for the web application servers which will create the phpinfo.php as part of the UserData script execution at instance boot. If health checks fail the User Data script has most likely failed.

![](https://cdn-images-1.medium.com/max/2000/0*-J06RMpKnBvu80rZ.png)

Click on **Next** and then on **Create target group** without defining any targets.

![](https://cdn-images-1.medium.com/max/3220/0*-rohVM1jezX9lRcK.png)

Close the window, refresh the Listener to choose the created target group.

![](https://cdn-images-1.medium.com/max/2184/0*YgJ5a761BqF9i1Zo.png)

In the **Summary** section, review and click **Create load balancer** to create the load balancer.

![](https://cdn-images-1.medium.com/max/2192/0*q3PH9ER4EHJ5yIRY.png)

![](https://cdn-images-1.medium.com/max/3270/0*Sl5Z2L-Lm3ErkVUP.png)

Make a note of the **DNS name** created for your load balancer as you will need this in the following steps.

## Create a launch Template

You have created a software-defined network across multiple fault-isolated Availability Zones, deployed a highly-available Aurora MySQL database and an EFS file system for shared storage. In this lab you will define the templates for the application servers running PHP as part of a scalable Wordpress installation.

## [Create a launch template for the Auto Scaling Groups (ASG)](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/application/lab5#create-a-launch-template-for-the-auto-scaling-groups-(asg))

A launch template is an instance configuration information, that allows you to create a saved instance configuration that can be used to launch instances at a later time. It includes the ID of the Amazon Machine Image (AMI), the instance type, a key pair, security groups, and other parameters used to launch EC2 instances. Additionally, it allows you to have multiple versions of a launch template.

Select **Launch Templates** on the left panel of your [EC2 console ](https://console.aws.amazon.com/ec2/home), then click on **Create launch template**.

* Give the Launch template the name WP-WebServers-LT

![](https://cdn-images-1.medium.com/max/2000/0*s_vM4qnAPJ8s-iVA.png)

* Choose the **Amazon Linux** AMI after selecting **Quick Start** in the **Application and OS Images (Amazon Machine Image)** section

![](https://cdn-images-1.medium.com/max/2000/0*egP3rlxN5QBYXjUL.png)

* Select the t3.micro instance type:

![](https://cdn-images-1.medium.com/max/2000/0*7RRPnjXPw6p-SUje.png)

* Don’t include a key pair and select the following **Security groups** to attach to the instances launched from this Launch Template:

* WP Web Servers to allow connections from the Application Load Balancer

* WP Database Clients to allow instances to connect to the Aurora MySQL DB

* WP EFS Clients to allow instances to mount the NFS export of the EFS file system

![](https://cdn-images-1.medium.com/max/2000/0*FIesWoNtPyIOC3Tk.png)

Expand **Advanced details** and use the script below to populate the User Data field as text.

![](https://cdn-images-1.medium.com/max/2000/0*5sM-mw5KwAoJz0mT.png)

Update the variables in the Bash script below with the values from your environment.

* **EFS_FS_ID**

* This should be set to the file system ID of the Elastic Filesystem deployed in the previous lab. To obtain the file system ID visit the [EFS console ](https://console.aws.amazon.com/efs/home?#/file-systems).

* **DB_NAME**

* This is the name of the database which Wordpress should use to store its data. If you entered the default values in Lab 2 this should be a value of wordpress. To confirm visit the details page for your RDS database and look for *DB name* under *Configuraiton*.

* **DB_HOST**

* This is the hostname of your database Writer instance. To obtain this visit the details page for your RDS database and look under *Connectivity & Security*. Use the *Writer* type instance hostname, a value such as wordpress-workshop.cluster-ctdnyvvewl6s.eu-west-1.rds.amazonaws.com.

* **DB_USERNAME**

* This will be the database username you specified in Lab 2. It can be found as *Master username* under *Configuration* on the details page for your RDS instance.

* **DB_PASSWORD**

* This is the password for the database user created in Lab 2.

    #!/bin/bash
    
    DB_NAME="wordpress"
    DB_USERNAME="wpadmin"
    DB_PASSWORD=""
    DB_HOST="wordpress-workshop.cluster-xxxxxxxxxx.eu-west-1.rds.amazonaws.com"
    EFS_FS_ID="fs-xxxxxxxxx"
    
    dnf update -y
    
    #install wget, apache server, php and efs utils
    dnf install -y httpd wget php-fpm php-mysqli php-json php amazon-efs-utils
    
    #create wp-content mountpoint
    mkdir -p /var/www/html/wp-content
    mount -t efs $EFS_FS_ID:/ /var/www/html/wp-content
    
    #install wordpress
    cd /var/www
    wget https://wordpress.org/latest.tar.gz
    tar -xzf latest.tar.gz
    cp wordpress/wp-config-sample.php wordpress/wp-config.php
    rm -f latest.tar.gz
    
    #change wp-config with DB details
    cp -rn wordpress/* /var/www/html/
    sed -i "s/database_name_here/$DB_NAME/g" /var/www/html/wp-config.php
    sed -i "s/username_here/$DB_USERNAME/g" /var/www/html/wp-config.php
    sed -i "s/password_here/$DB_PASSWORD/g" /var/www/html/wp-config.php
    sed -i "s/localhost/$DB_HOST/g" /var/www/html/wp-config.php
    
    #change httpd.conf file to allowoverride
    #  enable .htaccess files in Apache config using sed command
    sed -i '/<Directory "\/var\/www\/html">/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/httpd/conf/httpd.conf
    
    # create phpinfo file
    echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php
    
    # Recursively change OWNER of directory /var/www and all its contents
    chown -R apache:apache /var/www
    
    systemctl restart httpd
    systemctl enable httpd

Review the final configuration under **Summary** and click **Create launch template**. You can disregard warnings about being able to SSH into the server and can also choose *Proceed without keypair* as you will not need to remotely access these servers.

## Create the app server

In this lab you will use the load balancer and launch configuration from the previous 2 labs to create an auto scaling fleet of Wordpress application servers.

## [Create the ASG for the back-end web servers](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/application/lab6#create-the-asg-for-the-back-end-web-servers)

Once you have created the launch configuration you can proceed to creating the Autoscaling group for the Wordpress web servers.
To do that select **Auto Scaling Groups** in the EC2 console, click on **Create an Auto Scaling Group** specify a name and select the previously created launch template:

![](https://cdn-images-1.medium.com/max/2000/0*kDI0vbj2pDsVTYxh.png)

In the next screen make sure that the wordpress-workshop VPC is selected, together with the Application Subnet A and Application Subnet B subnets for the web servers:

![](https://cdn-images-1.medium.com/max/2000/0*C4tTTmchtvbKWjiu.png)

In the next screen **Configure advanced options** choose the option **Attach to an existing load balancer** and choose the target group you created earlier from the **Existing load balancer target groups** list.

![](https://cdn-images-1.medium.com/max/2000/0*C016olT7pCKqPyCI.png)

In the **Health checks** section, make sure to **Turn on Elastic Load Balancing health checks**.

![](https://cdn-images-1.medium.com/max/2000/0*GqmvgJzqwbMJmo5r.png)

Click **Next** to configure group size and scaling policies with the following values:

* In the **Group size** section, enter 2 in the **Desired capacity** field.

* In the **Scaling** section, enter 2 as **Min desired capacity** and 4 as **Max desired capacity**

* Select **Target tracking scaling policy** and enter 80 as **Target value** for the **Average CPU utilization**

![](https://cdn-images-1.medium.com/max/2000/0*J7brAybTFnN1jS4F.png)

Click through and accept the remaining defaults to complete the creation of Auto Scaling Group by clicking on **Create Auto Scaling group**.

The autoscaling group will now begin creating the desired number of EC2 instances based on the launch template you created. As the systems come online, the target group is updated with the instance details for your EC2 instances and the load balancer will begin distributing traffic across the instances. As instances are added or removed, the autoscaling group and load balancer will work in concert with one another to ensure that only healthy instances receive traffic.

![](https://cdn-images-1.medium.com/max/3272/0*_aYuvK7xVVkeBHdI.png)

When your targets are deemed healthy in your target group you can open the DNS name for your Application Load Balancer in your web browser to view your newly created Wordpress installation.

![](https://cdn-images-1.medium.com/max/2476/0*eXzfY0Or6s3O-Dce.png)

## [Next steps](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/application/lab6#next-steps)

You have now created a highly-available auto-scaling deployment of Wordpress that will scale in and out in response to client traffic hitting the website.

## Clean Up

If you used your own account to follow this workshop, please perfom the actions below to remove all resources you created and stop incurring charges for them.

## [Delete the Auto Scaling Group](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/summary/clean-up#delete-the-auto-scaling-group)

Go to the ***Auto Scaling Groups*** section of the EC2 Console, select the Autoscaling Group created in [Lab 6](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/application/lab6.md), open the **Actions **menu and select **Delete**. To confirm deletion, type delete in the text field of the dialog that will open and click **Delete**.

![](https://cdn-images-1.medium.com/max/3656/0*BGJ94-VbnSJJYTok.png)

## [Delete the Load Balancer](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/summary/clean-up#delete-the-load-balancer)

On the EC2 Console, go the **Load Balancers** section, select the Application Load Balancer created in [Lab 4](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/application/lab4.md), open the **Actions **menu and select **Delete load balancer**. To confirm deletion, type confirm in the text field of the dialog that will open and click **Delete**.

![](https://cdn-images-1.medium.com/max/4000/0*Orpth62zRXpf-c_Q.png)

## [Delete the Target Group](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/summary/clean-up#delete-the-target-group)

Go to the **Target Groups** section, select the Target Group created in [Lab 4](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/application/lab4.md), open the **Actions **menu and select **Delete**. Click on **Yes, delete** on the dialog that will open.

![](https://cdn-images-1.medium.com/max/4000/0*J4Dlsu1aYqAy7ZwO.png)

## [Delete the Launch Template](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/summary/clean-up#delete-the-launch-template)

Move to the **Launch Templates** section, select the Launch Template created in [Lab 5](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/application/lab5.md), open the **Actions menu **and select **Delete template**.
To confirm deletion, type Delete in the field and click **Delete**

![](https://cdn-images-1.medium.com/max/4000/0*i_iuYlqruRqSXps-.png)

## [Verify instances](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/summary/clean-up#verify-instances)

Once you delete the Auto Scaling Group, the instances will begin shutting down and eventually they will be terminated. Verify that all instances launched by the Auto Scaling Group have been terminated correctly.
You can use the **aws:autoscaling:groupName** attribute to filter instances launched by the Auto Scaling Group created in [Lab 6](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/application/lab6.md).

![](https://cdn-images-1.medium.com/max/2000/0*Y7cyAUrY0Tv544jv.png)

## [Delete the Aurora cluster](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/summary/clean-up#delete-the-aurora-cluster)

 1. Go to [RDS console ](https://console.aws.amazon.com/rds/home?#databases:), select the **wordpress-workshop ***Regional cluster *and click **Modify**.

 2. Scroll to the bottom of the page, unckeck **Enable deletion protection** and click on **Continue**

![](https://cdn-images-1.medium.com/max/2000/0*odD2nb1mYF_cH9y_.png)

 1. Select the option **Apply immediately** and click **Modify cluster**

![](https://cdn-images-1.medium.com/max/2000/0*aKdQRB5tx4XoF0vu.png)

 1. On the [RDS console ](https://console.aws.amazon.com/rds/home?#databases:), select the *Reader instance*, go to the **Actions **menu and select **Delete**.
To confirm deletion, type delete me into the field and click **Delete**.

![](https://cdn-images-1.medium.com/max/2364/0*GjQ-vlfkzYPKBJQX.png)

 1. Now select the *Writer instance, go to the **Actions** menu and select **Delete**.
To confirm deletion, type delete me into the field and click **Delete**.

 2. You can delete the *Regional cluster* now. Select it, go to the **Actions** menu and select **Delete**.
Make sure to

* uncheck **Create final snapshot**

* check the acknowledgement

To confirm deletion, type delete me into the field and click on **Delete DB cluster**

![](https://cdn-images-1.medium.com/max/2000/0*CX05H-Z9Zoqcgu6O.png)

## [Verify RDS Snapshots](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/summary/clean-up#verify-rds-snapshots)

Once the RDS Aurora Cluster has been completely deleted, move to the [RDS Snapshots ](https://console.aws.amazon.com/rds/home?#snapshots-list:tab=automated)page, select the **System** tab and make sure no automated snapshots are present for the Aurora cluster created during the workshop.

## [Delete EFS filesystem](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/summary/clean-up#delete-efs-filesystem)

Go to the [EFS Console ](https://console.aws.amazon.com/efs/home?#/file-systems), select the file system created in [Lab 3](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/datatier/lab3.md) and click **Delete**.
Confirm the deletion by entering the file system’s ID in the dialog that will appear and click **Confirm**.

![](https://cdn-images-1.medium.com/max/2382/0*gLCtL_6fNBvOaZsQ.png)

## [Delete VPC](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/summary/clean-up#delete-vpc)

### [Delete NAT Gateways](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/summary/clean-up#delete-nat-gateways)

Go to the [NAT gateways ](https://console.aws.amazon.com/vpcconsole/home?#NatGateways:)page of the VPC Console, select one NAT Gateway at a time, go to the **Actions **menu and select **Delete NAT gateway**.
To confirm deletion, type delete in the field and click **Delete**

![](https://cdn-images-1.medium.com/max/2952/0*02dI5g9YOZ2AkA3A.png)

### [Delete VPC](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/summary/clean-up#delete-vpc)

Select the wordpress-workshop VPC from [Your VPCs](https://console.aws.amazon.com/vpcconsole/home?#vpcs:) page in the VPC Console. Go to the **Actions **menu and select **Delete VPC**.

![](https://cdn-images-1.medium.com/max/2896/0*0uPuIepGHeXbG0ze.png)

You will be able to delete the VPC only if no network interfaces are still present in any of the subnets of the VPC. The dialog that will appear when you click on **Delete VPC** will show any remaining ENIs.

![](https://cdn-images-1.medium.com/max/2000/0*s4ii2QJEI52Gln7P.png)

Click on the **network interfaces** link to check which services are still using the VPC and take appropriate actions.

Once all ENIs have been removed, you will be able to delete the VPC.
The dialog that will appear shows which resources will be deleted once you delete the VPC.
To confirm deletion, type delete in the field and click **Delete**

![](https://cdn-images-1.medium.com/max/2000/0*4e7xDAbnvUV_PQlj.png)

### [Release Elastic IPs](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/summary/clean-up#release-elastic-ips)

Go to the [Elastic IPs ](https://console.aws.amazon.com/vpcconsole/home?#Addresses:)page of the VPC Console, select all unassociated Elastic IPs (*Association ID *value is -), open the **Actions **menu and select **Release Elastic IP addresses**.

![](https://cdn-images-1.medium.com/max/2532/0*-fQq2clqskImB1Vh.png)

## [Remove the IAM User](https://catalog.us-east-1.prod.workshops.aws/workshops/3de93ad5-ebbe-4258-b977-b45cdfe661f1/en-US/summary/clean-up#remove-the-iam-user)

If you created the workshop-user IAM User to follow the workshop labs, make sure to delete it from the [IAM Console](https://us-east-1.console.aws.amazon.com/iam/home?#/users) logging in with a IAM User or IAM Role with the appropriate permissions.

![](https://cdn-images-1.medium.com/max/2474/0*psK8JrGVjViW-5yz.png)

### Challenges Faced and Solutions

* **Database Connection Issues**: When setting up RDS, there were some configuration issues with access control.

* **Solution**: Adjusted VPC security groups and ensured that EC2 instances had the correct permissions to access RDS.

* **WordPress File Management**: Managing WordPress media files on multiple instances was initially challenging.

* **Solution**: Implemented Amazon EFS as a shared file system, enabling seamless media management across instances.

### Conclusion

This project demonstrates the creation of a **highly available, fault-tolerant WordPress application on AWS**, featuring automated scaling, traffic distribution, and shared storage. Leveraging AWS’s managed services, the architecture provides a robust solution for production-ready applications where uptime and scalability are essential. By combining services such as **VPC**, **EC2**, **EFS**, **RDS**, and **ALB**, this setup ensures efficient resource management, optimized cost, and minimal manual intervention.

This architecture can be applied to any high-traffic WordPress deployment to achieve a resilient and scalable environment with easy content management.

## Code Repository

To explore and experiment with the project’s code and documentation, visit the [**GitHub Repository](https://github.com/Dark-Cookie/AWS-Projects/tree/main/Level%20400/6.%20Create%20a%20Highly%20Available%20WordPress%20Web%20Application)**. Here you can access the complete code, follow along with detailed instructions, and customize the applications to fit your own use cases.
>  ***Asif Khan — Aspiring Cloud Architect | Weekly Cloud Learning Chronicler***

[**LinkedIn](https://www.linkedin.com/in/asif0108/)**/[**Twitter](https://x.com/asif26073)**/[**GitHub](https://github.com/Dark-Cookie)**
