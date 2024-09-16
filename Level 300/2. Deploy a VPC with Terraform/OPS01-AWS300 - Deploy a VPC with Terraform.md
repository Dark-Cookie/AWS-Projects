# OPS01-AWS300 - Deploy a VPC with Terraform

## Cloud Service Provider
- Amazon Web Services

## Difficulty
- Level 300 (Advanced)

## Project's Author(s)
- [Markus Mabson ](https://www.linkedin.com/in/markus-mabson-86917a133/)

## Objectives

### You need to complete the following:
- Download terraform binaries [Terraform binaries](https://www.terraform.io/downloads.html)
- Initialize terraform working directory 
- Document your environment (Draw.io)
- Create the following with Terraform deployment
    - Virtual Private Cloud (VPC)
    - Route Tables (public/private)
    - Route Table Associations
    - Internet Gateway
    - Elastic IP
    - Nat Gateway 
    - 1 EC2 Instance in public/ Private Subnet  (Amazon  Linux 2 AMI)
    - Security Groups
- Verify connectivity of public instance / private instance 
- Delete the Terraform deployment
- Verify resources are deleted

### Architectural Diagram
![Architecture Diagram](Image.png)

### You need to answer the following:
### ***What is Infrastructure as Code (IaC)?***

**Infrastructure as Code (IaC)** is the practice of managing and provisioning computing infrastructure (such as servers, networks, databases, etc.) through machine-readable configuration files, rather than using manual processes or interactive configuration tools. It allows developers and operators to describe the desired infrastructure in code, which can then be version-controlled, automated, and executed in a repeatable manner.

Key benefits of IaC include:

- **Automation:** Reduces the need for manual intervention.
- **Consistency:** Ensures that infrastructure is consistently configured across environments (development, testing, production).
- **Version Control:** Infrastructure changes can be tracked, reviewed, and reverted like application code.
- **Scalability:** Large-scale environments can be easily managed and modified.

### ***What is Terraform?***

**Terraform** is an open-source IaC tool developed by HashiCorp that allows users to define, provision, and manage infrastructure across various cloud providers (e.g., AWS, Azure, Google Cloud) and other platforms. Terraform uses a declarative language called HCL (HashiCorp Configuration Language) to describe the desired state of infrastructure.

Key features of Terraform include:

- **Provider Agnostic:** Supports multiple cloud providers and platforms, allowing multi-cloud strategies.
- **Declarative Language:** Users describe the desired infrastructure state, and Terraform handles the steps to achieve it.
- **State Management:** Terraform tracks the infrastructure changes using state files to ensure the real infrastructure matches the desired configuration.
- **Plan and Apply:** Users can generate a "plan" before applying changes, which helps avoid mistakes.

### ***What is a state file used for?***

A **state file** in Terraform is a critical component that keeps track of the current state of the infrastructure as Terraform manages it. It serves as a source of truth for the resources that have been created and their associated configurations.

Key purposes of the state file:

- **Mapping Real Resources to Configuration:** The state file helps Terraform understand which resources in the real world correspond to the ones described in the code.
- **Performance Optimization:** It allows Terraform to quickly determine the necessary changes without querying all the resources from the provider.
- **Tracking Dependencies:** The state file tracks relationships between resources, ensuring they are created, modified, or destroyed in the correct order.
- **Collaboration:** In shared environments, the state file helps teams collaborate, ensuring consistent infrastructure management.

State files are typically stored locally or remotely (e.g., in an S3 bucket) to enable collaboration and ensure that all users are working from the same state.

### ***In what cases would you use Infrastructure as Code?***

You would use Infrastructure as Code in:


**1. Cloud Resource Management:**
   - Provisioning and managing cloud infrastructure (e.g., VMs, storage, networking) on platforms like AWS, Azure, or Google Cloud.

**2. Consistent Multi-Environment Deployment:**
   - When you need to ensure that environments such as development, testing, and production are identical, IaC ensures that configuration drift is minimized.

**3. Automating Infrastructure:**
   - For large-scale or dynamic environments, IaC allows you to automate the deployment, scaling, and management of resources without manual effort.

**4. Disaster Recovery:**
   - In cases of disaster recovery, IaC can quickly recreate infrastructure from code, significantly reducing downtime.

**5. Version Control and Auditing:**
   - If you need to track changes over time, IaC allows you to use version control systems (e.g., Git) to audit who made changes and when.

**6. Collaboration in Teams:**
   - In large organizations where multiple teams work on infrastructure, IaC provides a structured and consistent approach to managing and sharing infrastructure changes.

**7. Infrastructure Testing and Validation:**
   - IaC allows you to test infrastructure code before applying it, reducing the chances of errors in production.

**8. Hybrid or Multi-Cloud Environments:**
   - IaC can simplify the management of hybrid or multi-cloud environments by providing a unified way to define and manage infrastructure across different providers.

## References
- [Terraform](https://www.terraform.io/)
- [Terraform module registry](https://registry.terraform.io/)

## Costs
- Included in the Free Tier

## Estimated time to complete
- 30 minutes

## Tips
- Indent your code to make it concise and readable
Some resources may need to wait for another resource to be created. 
- It may be useful to create a key_pair in the management UI before creating the EC2 instances

## Output
![alt text](Image.gif)
