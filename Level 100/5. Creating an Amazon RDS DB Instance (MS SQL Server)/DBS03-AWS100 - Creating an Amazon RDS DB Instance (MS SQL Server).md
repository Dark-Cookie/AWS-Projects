# DBS03-AWS100 - Creating an Amazon RDS DB Instance (MS SQL Server)

## Cloud Service Provider

- Amazon Web Services


## Difficulty

- Level 100 (Introductory)


## Project's Author(s)

- [Jagan](https://twitter.com/JAG2wt)

## Objectives

### You need to complete the following:


- Setup a database instance on Amazon RDS
- Create a MS SQL Server database instance on RDS
- Connect to RDS database instance from your local

### You need to answer the following:

### ***What is RDS?***

Amazon RDS (Relational Database Service) is a fully managed service by AWS that simplifies the process of setting up, operating, and scaling relational databases in the cloud. RDS supports various database engines, including:

- Amazon Aurora
- MySQL
- PostgreSQL
- MariaDB
- Oracle
- Microsoft SQL Server

With RDS, AWS handles the underlying infrastructure, such as backups, patching, and replication, allowing users to focus on database management and application development. It also offers features like automated backups, multi-AZ deployments for high availability, read replicas for scalability, and security using encryption at rest and in transit.

### ***How to create a database instance on RDS?***

To create a database instance on Amazon RDS, follow these steps:

**Step 1: Sign in to the AWS Management Console**

1. Navigate to the Amazon RDS Console.
2. From the dashboard, click on Create database.

**Step 2: Configure the Database Engine**

1. Choose a database creation method:
     - **Standard create:** Offers complete customization.
     - **Easy create:** Offers pre-configured settings for beginners.

2. Select a database engine (**SQL Server Express Edition**).
3. Choose a version for the engine (Latest).

**Step 3: Set up the Database Instance**

1. **Instance Class:** Choose the size of the instance (`db.t3.micro`).
2. **Storage:** Specify the storage type and allocated space (SSD).
3. **VPC and Subnet:** Select the VPC and subnet where you want to deploy the RDS instance.
4. **Database identifier:** Provide a name for your database instance.

**Step 4: Configure Database Credentials**

1. Set the master username and password for your database.
2. Choose **Public accessibility** for the database to be publicly accessible.

**Step 5: Advanced Settings**

1. **Backup:** Configure automatic backups and retention period.
2. **Encryption:** Enable encryption for the database instance (optional).
3. **Maintenance:** Configure maintenance windows, parameter groups, and more.

**Step 6: Review and Launch**
1. Review your configurations.
2. Click **Create Database**.

Once the RDS instance is launched, it will take a few minutes for the instance to be available.

### ***How to connect to RDS database?***

To connect to an RDS database:

**Step 1: Get the Endpoint and Port**

- Go to the RDS Console.
- Select the RDS instance you want to connect to.
- Copy the endpoint and port from the Connectivity & Security section.

**Step 2: Configure Security Groups and Networking**

- Ensure that the security group attached to your RDS instance allows inbound connections on the database's port (e.g., 1433 for SQL Server Express Edition).
- If the RDS instance is in a private subnet, you will need a VPN or an EC2 instance within the same VPC to act as a bastion host for the connection.

**Step 3: Connect via Client Tools**
- SQL Server: Use SQL Server Management Studio (SSMS) and connect with the endpoint, port, **username**, and **password**.

## References
- [What is RDS?](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html)
- [Setting Up for Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_SettingUp.html)
- [Creating an Amazon RDS DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html)
- [Connecting to a DB Instance Running the Microsoft SQL Server Database Engine](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ConnectToMicrosoftSQLServerInstance.html)
- [Running SQL Server Databases on Amazon RDS for SQL Server - AWS Virtual Workshop](https://youtu.be/twOglkIFbXU)
- [Amazon RDS Free Tier](https://aws.amazon.com/rds/free/)

## Costs

- Included in the Free Tier


## Estimated time to complete
- 15 minutes


## Tips
- Having database knowledge is an added advantage
- 750 hours of Amazon RDS Single-AZ `db.t2.micro` included in free tier

# Output
![alt text](Image.png)
