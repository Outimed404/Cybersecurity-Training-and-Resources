# Task 1 : Introduction 
# Introduction

Explore the fundamentals of Infrastructure as Code (IaC) and its crucial role in modern cloud computing. We will focus on cloud-based IaC use cases using **AWS CloudFormation** and **Terraform**, enabling the creation and management of cloud resources in a scalable, efficient, and secure manner.

---

## Learning Prerequisites

From **Task 2** onwards, concepts and tool characteristics defined in the *Intro to IaC* room are referenced, so it would be a good idea to complete that first!

---

## Learning Objectives

- **Understanding Cloud-based IaC Concepts**
- **Secure IaC Practices**
- **Security Considerations in Cloud-based IaC**

# Task 2 : Terraform 101

## Terraform Overview

Terraform is an infrastructure as code (IaC) tool used for provisioning that allows the user to define both cloud and on-prem resources in a human-readable configuration file. These files can be versioned, reused, and distributed across teams. Terraform uses a consistent workflow to simplify and streamline infrastructure management. Subsequent tasks in this room will cover both Terraform's configuration and workflow. For now, let's define some key terms to understand how Terraform provisions and manages infrastructure.

---

## Terraform Provisioning and Architecture

Now that we know what Terraform does, let's examine *how* it does it. We'll take a closer look at **Terraform's Architecture**:

### Terraform Core

As the name suggests, **Terraform Core** is responsible for the core functionalities that allow users to provision and manage their infrastructure using Terraform. 

Terraform is a **declarative tool**, meaning that a desired state is defined, and the tool ensures the infrastructure meets that desired state. Users do not need to declare each step required to achieve this state. Terraform Core takes its input from two sources:

1. **Terraform Config Files**:  
   These files define the desired state of the infrastructure, specifying the resources that make up the desired environment.

2. **State**:  
   Terraform keeps a **state file** that tracks the current state of provisioned infrastructure.  
   - The Core component compares this state file to the desired state in the configuration files.  
   - If there are discrepancies (e.g., resources defined but not provisioned), Terraform creates a **plan** to align the current state with the desired state.  
   - By default, this state file is called `terraform.tfstate` and is stored in the same directory where Terraform is executed.

### Terraform Architecture Diagram

In the architecture diagram, Terraform Core takes its input from two sources:  
- User-defined **configuration files**  
- The **state file (`tfstate`)**

Should there be differences between the **desired state** (configuration files) and the **current state** (tfstate), Terraform will execute the required actions using a **provider**.

---

### Providers

The **plan** created by Terraform Core is executed using different **providers** based on the resources defined. Providers are used to interact with cloud providers, SaaS providers, and other APIs. Examples include:

- **AWS Provider**: For AWS resources like EC2 instances.
- **Kubernetes Provider**: For Kubernetes clusters.

---

### Provisioner

Terraform's architecture and tools offer many benefits, particularly for **DevSecOps engineers**. One of Terraform's standout features is the ability to use providers to provision and manage resources across multiple cloud platforms. This capability offers several advantages:

- **Multi-cloud Support**:  
  Manage infrastructure across multiple providers (e.g., AWS, Azure) from a single tool. This flexibility prevents vendor lock-in.

- **Declarative Configuration Language**:  
  - Terraform uses a human-readable, easy-to-understand configuration format to define resources.  
  - This format simplifies the declaration process.

- **Versioning and Change Tracking**:  
  - Terraform supports versioning, enabling tracking and collaboration during infrastructure development.  
  - Rollbacks to previous infrastructure states are easily manageable.

---

### Summary

Terraform's architecture and capabilities make it an invaluable tool for provisioning and managing infrastructure in a flexible, scalable, and efficient manner. The next task will dive deeper into how these resources are defined.

### Terraform Core takes its input from two sources; this source keeps track of what the infrastructure currently looks like. What is the name of this source?
State

### The other source defines what you want infrastructure to look like. What is the name of this source?
Terraform Config files

### If there is a difference between what the infrastructure currently looks like and what you want it to look like, this will require a change. This change will be actioned by a....?
Provider

# Task 3 : Terraform Configuration

## Configuration & Terraform

Now that we know what Terraform is and how it works, let's take a look at how we define the desired state using **Terraform config files**.  

Terraform configuration files are written in a declarative language called **HCL (HashiCorp Configuration Language)**, which, as discussed, is both easy to understand and human-readable. The primary purpose of this language is to declare **resources**, which represent infrastructure objects. The combined definition of these resources forms the **desired infrastructure state**.

---

### Example: Defining a Simple VPC with Terraform

As a DevSecOps engineer, Terraform configuration files might be used to define resources for a project. For instance, consider the scenario where we want to define a simple **VPC (Virtual Private Cloud)** using the **AWS provider**.

This is just one of the many ways Terraform simplifies infrastructure provisioning by allowing you to clearly define the desired state of your cloud resources in an organized, efficient, and secure manner.


```t
provider "aws" { 
 region = "eu-west-2" 
}

# Create a VPC
resource "aws_vpc" "flynet_vpc" { 
 cidr_block = "10.0.0.0/16" 
 tags = { 
  Name = "flynet-vpc"
 }
}

```

**What does the code do?** After defining our provider (which in this case is AWS), this resource block is used to define a VPC. We first define the resource by stating the resource type, which in this case is an "aws_vpc", and then what we will call this resource. This example is "flynet_vpc". Then begins our resource block, where we define our resource arguments. The arguments given will depend on what type of resource is being defined. For example, an aws_vpc will need a CIDR block (method for allocating IP addresses). We also define tags which can be used in AWS to help with the automation of resources, billing, access control, etc.

## Resource Relationships

Sometimes, resources can depend on other resources. When defining a resource with dependencies, the resources it depends on will be referenced. For example, if you were assigned a task to define an AWS security group ingress rule, allowing SSH access from any source within the VPC, you would add the following code block to your configuration file.

```t
resource "aws_security_group" "example_security_group" {
 name = "example-security-group"
 description = "Example Security Group"
 vpc_id = aws_vpc.flynet_vpc.id #Reference to the VPC created above (format: resource_type.resource_name.id)

 # Ingress rule allowing SSH access from any source within the VPC
 ingress {
  #Since we are allowing SSH traffic , from port and to port should be set to port 22
  from_port = 22
  to_port = 22
  protocol = "tcp"
  cidr_blocks = [aws_vpc.flynet_vpc.cidr_block]
 }
}
```

**What does the code do?** Here, you can see we have a resource block that defines an AWS security group (helps secure AWS environments by controlling how traffic is allowed into EC2 machines), which allows SSH access from any source within the VPC. You can see in the commented lines how this is configured and how resources can be referenced from within another resource block.

## Infrastructure Modularisation

As a DevSecOps engineer, you will likely work with a large, complex infrastructure. Another benefit of Terraform is that it allows for the modularisation of an infrastructure, that is, for the infrastructure to be broken down and defined as modular components rather than in a single place. This is common practice in larger organisations as it simplifies the management of complex infrastructure configurations. Let's take a look at what the above example configuration would look like. Below is a representation of a directory /tfconfig and the files within it, with a brief comment explaining the purpose of each file:

```bash
tfconfig/
 -flynet_vpc_security.tf #Like mentioned, instead of having all resources defined in one file, resources can be paired up and defined in separate modular files
 -other_module.tf
 -variables.tf #some values will be used across two or many infrastructure modules, so instead of declaring these repeatedly in each .tf file it makes sense to paramaterise them in a file called variables.tf. These variables can then be directly referenced in the .tf file.
 -main.tf #main.tf acts as the central configuration file where the defined modules are all referenced in one place
 ```
 If we consider the example given above, if we were to define a vpc using this structure, it could be configured like so: 

 ```bash
variable "vpc_cidr_block" {
 description = "CIDR block for the VPC"
 type = string #Set the type of variable (string,number,bool etc)
 default = "10.0.0.0/16" # Can be changed as needed
}
 ```

 We are defining a VPC the same way as we did at the beginning of this task, except now we are storing it in a variables.tf file. We are doing this so we can reuse/reference it. This could be referenced in any module.tf file, for example, below, we have a code block for a module named flynet_vpc_security:

 ```bash
cidr_block = var.vpc_cidr_block 
#Variable can be referenced using var. followed by variable name
 ```

 Finally, this module (and all other module tf files) would be collected and referenced in the main.tf file like so:

 ```bash
module "flynet_vpc_security" {
 source = "./flynet_vpc_security.tf" 
 #where module is defined (relative to current file)
}

module "other_module" {
 source = "./Other_module.tf"
}
 ```

 ### When defining the VPC (in the first example) before the resource block, what was defined?
 provider

 ### When modulating your infrastructure, what file acts as the central configuration file? 
 main.tf

 ### Instead of repeatedly defining values across multiple infrastructure modules these values can be collected in one file and referenced. What is the name of this file? 
 variables.tf

# Task 4 : Terraform Workflow

## Understanding the Terraform Workflow

The Terraform workflow generally follows four key steps: **Write**, **Initialise**, **Plan**, and **Apply**. To understand how this workflow benefits DevSecOps engineers in provisioning infrastructure, let’s explore how Terraform is used at three distinct points in time:

- **Day 1**: The infrastructure does not exist yet and needs to be provisioned from scratch.
- **Day 2+**: The infrastructure exists, but some changes need to be made.
- **Day N**: An undefined point in time when the infrastructure is no longer needed.

---

## Day 1: Provisioning Infrastructure from Scratch

On **Day 1**, you start with an empty environment and need to define and provision your infrastructure. Here’s how the Terraform workflow operates:

### 1. Write  
Define the desired state of your infrastructure in a Terraform configuration file. These files specify the components and their relationships.

### 2. Initialise  
Prepare your workspace by running the `terraform init` command.  
This command performs tasks such as downloading dependencies (e.g., the `aws` provider plugin if AWS is defined in the configuration).

### 3. Plan  
Generate an execution plan using `terraform plan`.  
This plan compares the current state (empty environment) to the desired state and outlines the required actions (e.g., adding resources). The user is shown what will be added or destroyed.

### 4. Apply  
Apply the changes using `terraform apply`.  
Terraform determines the order of resource creation based on dependencies. For example, if a security group references a VPC, Terraform will create the VPC first.

---

## Day 2+: Making Changes to Existing Infrastructure

From **Day 2+**, changes are made to an already provisioned infrastructure. For example, you may want to add a new component. The workflow is similar but adapted for updating:

### 1. Initialise  
Run `terraform init` to initialise the workspace, especially after changes to the configuration.

### 2. Plan (Optional but Recommended)  
Run `terraform plan` to generate an updated execution plan.  
This step is crucial for existing infrastructure to ensure changes are understood before they are applied. For example, the plan will note if a component needs to be added, while confirming that existing components match the desired state.

### 3. Apply  
Run `terraform apply` to execute the changes.  
If the planning phase is skipped, Terraform will still generate an execution plan for approval before applying changes. The state file is then updated to reflect the new current state.

---

## Day N: Decommissioning Infrastructure

At **Day N**, the infrastructure is no longer needed (e.g., a test environment or decommissioned application). Terraform makes it easy to destroy resources:

### Destroy  
Run `terraform destroy`.  
This command generates a destroy plan, compares it to the current infrastructure, and tears down all resources. It works similarly to `terraform apply`, but instead of provisioning, it removes resources entirely.

---

## Summary of Terraform Workflow

1. **Day 1**: Use Terraform to provision infrastructure from scratch.
2. **Day 2+**: Modify existing infrastructure by adding or updating components.
3. **Day N**: Decommission unneeded infrastructure with the `terraform destroy` command.

By following this workflow, DevSecOps engineers can ensure efficient, reliable, and secure infrastructure management at every stage of development.


### What command would you run to action the steps outlined to take your infrastructure from the desired state to the actual state?
terraform apply

### What command would you run to prepare your workspace?
terraform init
 
### What command, when run, would provide you with a series of actions that would be required to take your infrastructure from the desired state to the actual state?
terraform plan

# Task 5 : Cloudformation 101

## CloudFormation Overview
CloudFormation is an Amazon Web Services (AWS) IaC tool for automated provision and resource management. Instead of manually configuring resources through the AWS Management Console or using the AWS Command Line Interface (CLI), you can use CloudFormation templates to describe your infrastructure in a "declarative" manner.

### Declarative Infrastructure as Code
With CloudFormation, you express the desired state of your infrastructure using a JSON or YAML template. This template defines the resources, their configurations, and the relationships between them. CloudFormation provides and manages these resources, making managing and replicating your infrastructure easier.

## Templates and Stacks
A CloudFormation template is a text file that serves as a blueprint for your infrastructure. It contains sections that describe various AWS resources like EC2 instances, S3 buckets, and more. When you use a template to create resources, it forms a CloudFormation stack. Stacks are the fundamental units of CloudFormation, and they represent a collection of AWS resources that are created, updated, and deleted together.

Example:

```yml
# CloudFormation Template
AWSTemplateFormatVersion: '2010-09-09'  # Version of the CloudFormation template syntax

Description: 'A simple CloudFormation template'  # Description of the template

Resources:  # Section where AWS resources are defined
  MyEC2Instance:  # Logical name of the resource
    Type: 'AWS::EC2::Instance'  # Type of AWS resource being created

    Properties:  # Properties specific to the resource type
      ImageId: 'ami-12345678'  # ID of the Amazon Machine Image (AMI) for the EC2 instance
      InstanceType: 't2.micro'  # Type of EC2 instance
      KeyName: 'my-key-pair'  # SSH key pair for accessing the instance

  MyS3Bucket:  # Another resource example
    Type: 'AWS::S3::Bucket'
    Properties:
      BucketName: 'my-s3-bucket'  # Name of the S3 bucket

Outputs:  # Section to define outputs for the template
  EC2InstanceId:  # Logical name for the output
    Description: 'ID of the EC2 instance'  # Description of the output
    Value: !Ref MyEC2Instance  # Reference to the EC2 instance resource
```

### Key Template Components

- **`AWSTemplateFormatVersion`**: Specifies the CloudFormation template version.
- **`Description`**: Provides a brief description of the template.
- **`Resources`**: Defines the AWS resources to be created, such as EC2 instances or S3 buckets.  
  - Each resource has a logical name (e.g., `MyEC2Instance`, `MyS3Bucket`).  
  - **`Type`**: Indicates the AWS resource type.  
  - **`Properties`**: Holds configuration settings for the resource.  
- **`Outputs`**: Defines the output values displayed after creating the stack.  
  - Contains a logical name, description, and a reference to a resource using `!Ref`.

**Notes**: CloudFormation Designer is a service for visually creating/validating these templates. [Read more here](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/working-with-templates-cfn-designer.html).

---

## CloudFormation Architecture

CloudFormation, similar to Terraform, follows a workflow for provisioning and managing AWS resources. Understanding its architecture and workflow is essential for effective use.

### Main and Worker Architecture

CloudFormation employs a **main-worker architecture**:  
- **Main Node**: The CloudFormation service running in AWS interprets and processes the template. It orchestrates stack creation, updates, and deletions.  
- **Worker Nodes**: Distributed across AWS regions, they handle the actual provisioning of resources as instructed by the main node.

---

### Template Processing Flow


![ImageTemplateProcessingFlow](https://tryhackme-images.s3.amazonaws.com/user-uploads/61a7523c029d1c004fac97b3/room-content/10cd2efae7292cd42ba523ba3625c3c8.svg)

#### Step-by-Step Workflow:

1. **Template Submission**:  
   Users submit a CloudFormation template (JSON or YAML format) to the CloudFormation service.  
   - Templates can be stored in an S3 bucket, for example.  

2. **Template Validation**:  
   The CloudFormation service validates the template to ensure:  
   - Syntax is correct.  
   - It follows AWS resource specifications.  

3. **Processing by the Main Node**:  
   The main node processes the template and creates a set of instructions for resource provisioning.  
   - Determines the order of resource creation based on dependencies.  

4. **Resource Provisioning**:  
   The main node communicates with worker nodes across AWS regions, which provision resources as per the instructions.  

5. **Stack Creation/Update**:  
   Resources are provisioned in the specified order to form a **stack**.  
   - A stack represents the complete set of provisioned resources.

---

### Event-Driven Model

CloudFormation operates on an **event-driven model**, generating events during:  
- Stack creation.  
- Stack updates.  
- Stack deletion.  

These events are logged and can be monitored to track progress or troubleshoot issues.

---

### Rollback and Rollback Triggers

- **Rollback**: If an error occurs during stack creation or update, CloudFormation can automatically revert the stack to its previous state.  
- **Rollback Triggers**: Conditions defined in the template that specify when a rollback should occur.

---

### Cross-Stack References

CloudFormation supports **cross-stack references**, enabling:  
- Resources in one stack to reference resources in another.  
- Management of complex applications and dependencies spanning multiple stacks.

---

Understanding CloudFormation's architecture provides insights into how it efficiently manages the orchestration and provisioning of AWS resources, ensuring reliable and consistent infrastructure deployment.

### What is generated during stack creation?
events
### Can you define rollback triggers in a template? (yay or nay)
yay
### What allows you to refer to resources in another stack?
Cross-Stack References

# Task 6 : CloudFormation Configuration & Use Cases

## Configuration and Concepts

### Template Structure

CloudFormation templates consist of the following key sections:

- **`AWSTemplateFormatVersion`**: Specifies the AWS CloudFormation template version.
- **`Description`**: Describes the template.
- **`Parameters`**: Allows you to input custom values when creating or updating a stack.
- **`Resources`**: Defines the AWS resources to be created or managed.
- **`Outputs`**: Describes the values that can be queried once the stack is created or updated.

---

### Intrinsic Functions

CloudFormation templates support **intrinsic functions**, which enable operations like:  

- **Referencing Resources**: Use references to link resources within the template.  
- **Performing Calculations**: Conduct basic calculations to derive values dynamically.  
- **Conditional Inclusion of Resources**: Add or exclude resources based on conditions defined in the template.

```yml
Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: ami-12345678
      InstanceType: t2.micro

Outputs:
  InstanceId:
    Value: !Ref MyInstance

  PublicDnsName:
    Value: !GetAtt MyInstance.PublicDnsName

  SubstitutedString:
    Value: !Sub "Hello, ${MyInstance}" 
```
### Intrinsic Functions

CloudFormation provides several intrinsic functions to simplify and enhance templates:

- **`Fn::Ref`**: References the value of the specified resource.
- **`Fn::GetAtt`**: Retrieves the value of an attribute from a resource in the template.
- **`Fn::Sub`**: Performs string substitution.

---

### Resource Dependencies

Resources in CloudFormation often have dependencies.  
- For example, an EC2 instance might depend on a VPC being created first.  
- **Automatic Dependency Handling**: CloudFormation determines and handles the correct order for resource creation and updates based on these dependencies.

---

### Change Sets

Before applying changes to a stack, CloudFormation provides a **Change Sets** feature:  
- **Preview Changes**: Understand the impact of modifications before applying them.  
- **Risk Reduction**: Helps prevent unintended consequences by providing insights into proposed changes.

---

### Use Cases

CloudFormation is versatile and supports a wide range of use cases:

#### 1. **Infrastructure Provisioning and Management**  
- Simplifies the process of creating and managing AWS resources.  
- Ensures consistency and repeatability in infrastructure deployments.  
- Reduces the risk of configuration errors.

#### 2. **Application Lifecycle Management**  
- Manages the complete lifecycle of applications.  
- Includes resource provisioning, application deployment, and updates or rollbacks.

#### 3. **Multi-Environment Deployments**  
- Facilitates deploying the same infrastructure across multiple environments (e.g., dev, test, production).  
- The same template can be reused with different parameter values for each environment.

#### 4. **Resource Scaling**  
- Supports scaling infrastructure as workloads grow.  
- Includes modifying templates or leveraging auto-scaling features for dynamic growth.

---

### Summary

CloudFormation offers a robust and scalable approach to managing AWS resources as code.  
- It provides benefits such as **automation**, **consistency**, and **collaboration** for cloud infrastructure setup and maintenance.


### Does CloudFormation support intrinsic functions? (yay or nay)
yay

### What feature helps you understand the impact of modifications before they are applied?
change sets

# Task 7 : Terraform vs. CloudFormation

## CloudFormation vs. Terraform

### **CloudFormation**

- **Vendor Lock-In**:  
  AWS CloudFormation is tightly integrated with AWS services, making it the natural choice for AWS-centric environments.  
- **Native Integration**:  
  Provides seamless integration with AWS services, enabling direct resource references and a unified management experience.  
- **Service Coverage**:  
  Comprehensive coverage of AWS services, often supporting new services shortly after their release.  
- **Learning Curve**:  
  Easier for individuals already familiar with AWS services.

---

### **Terraform**

- **Cloud-Agnostic**:  
  Supports multiple cloud providers (e.g., AWS, Azure, Google Cloud), making it ideal for hybrid or multi-cloud deployments.  
- **Community and Modules**:  
  A large, active community contributes reusable and shareable modules, speeding up development and encouraging best practices.  
- **State Management**:  
  Uses a state file to track the current state of infrastructure, offering clear visibility of deployed resources.  
- **Language Flexibility**:  
  Written in HashiCorp Configuration Language (HCL), which is highly readable and more flexible than JSON or YAML.

---

### **Use Cases**

#### **Terraform Use Cases**

1. **Multi-Cloud Environments**  
   - **Scenario**: A company operates infrastructure across multiple cloud providers (e.g., AWS, Azure, GCP).  
   - **Why Terraform**:  
     - Terraform provides providers for various cloud platforms, making multi-cloud management seamless.  
     - A single Terraform configuration can span multiple cloud providers, ensuring consistency across infrastructure.  

2. **Community Modules and Providers**  
   - **Scenario**: An organisation relies heavily on community-contributed modules and providers.  
   - **Why Terraform**:  
     - Terraform’s active community contributes reusable modules and providers for diverse services and platforms.  
     - These community-driven resources accelerate development and implement best practices.

---

#### **AWS CloudFormation Use Cases**

1. **Deep AWS Integration**  
   - **Scenario**: An enterprise operates primarily in AWS and utilises many AWS-specific services.  
   - **Why CloudFormation**:  
     - Provides native integration with AWS services, making it an ideal choice for managing AWS-centric infrastructure.  
     - Features like **StackSets** and **Change Sets** enhance resource management and deployment workflows.  

2. **Managed Service Integration**  
   - **Scenario**: An organisation uses AWS-managed services (e.g., Elastic Beanstalk, OpsWorks).  
   - **Why CloudFormation**:  
     - Offers seamless provisioning and management of AWS-managed services.  
     - Includes specific resource types for AWS-managed services, simplifying templates and enhancing abstraction.

---

### Summary

| **Feature**               | **CloudFormation**                  | **Terraform**                            |
|---------------------------|-------------------------------------|------------------------------------------|
| **Cloud Provider Scope**  | AWS-specific                       | Multi-cloud                              |
| **Integration**           | Deep native AWS integration        | Broad with many cloud providers          |
| **Language**              | JSON or YAML                       | HCL (flexible and readable)              |
| **Community Support**     | Moderate                          | Large and vibrant                        |
| **Best for**              | AWS-centric environments           | Multi-cloud and hybrid cloud deployments |


### Is CloudFormation cloud agnostic? (yay or nay)
nay

### Which IaC tool has deep AWS integration?
CloudFormation

### Which IaC tool has accessible community-driven modules?
Terraform

# Task 8 : Secure IaC
## Secure Cloud IaC Best Practices

### General Best Practices for Both CloudFormation and Terraform

1. **Version Control**  
   - Use version control systems like Git to store IaC code.  
   - Enables tracking of changes, collaboration, and maintaining a history of updates.

2. **Least Privilege Principle**  
   - Assign the minimum required permissions for credentials and IaC tools.  
   - Restrict permissions to only what is necessary for the actions being performed.

3. **Parameterise Sensitive Data**  
   - Avoid hardcoding sensitive data (e.g., credentials, API keys) in IaC code.  
   - Use parameterisation to handle sensitive information securely.

4. **Secure Credential Management**  
   - Use cloud-native credential management solutions (e.g., AWS Secrets Manager, Azure Key Vault).  
   - Store and handle sensitive data securely using dedicated vaults or services.

5. **Audit Trails**  
   - Enable logging and monitoring features to maintain a record of IaC changes.  
   - Periodically review logs to detect and address potential issues.

6. **Code Reviews**  
   - Implement collaborative code reviews to ensure adherence to security best practices.  
   - Identify and fix potential vulnerabilities early in the development process.

**Additional Resources**: Visit the [Source Code Security Room](#) to learn more about secure coding practices.

---

### Best Practices Specific to **AWS CloudFormation**

1. **Use IAM Roles**  
   - Assign IAM roles with minimal permissions to CloudFormation stacks.  
   - Avoid using long-term access keys to improve security.

2. **Secure Template Storage**  
   - Store CloudFormation templates in encrypted Amazon S3 buckets.  
   - Restrict access to authorised users or roles to prevent unauthorised access.

3. **Stack Policies**  
   - Implement stack policies to define conditions for resource updates.  
   - Protect critical resources from unintended changes during stack updates.

---

### Best Practices Specific to **Terraform**

1. **Backend State Encryption**  
   - Enable encryption for the Terraform state file to protect sensitive data.

2. **Use Remote Backends**  
   - Store the Terraform state file in remote backends like Amazon S3 or Azure Storage.  
   - Enhances collaboration and secures the state file.

3. **Variable Encryption**  
   - Use tools like HashiCorp Vault to encrypt sensitive variables.  
   - Alternatively, leverage secure key management solutions for safe handling.

4. **Provider Configuration**  
   - Securely configure provider credentials using environment variables or encrypted files.  
   - Avoid embedding sensitive information directly in configuration files.

---

### Summary

By adhering to these best practices, organisations can ensure secure, consistent, and reliable infrastructure management using CloudFormation or Terraform. Leveraging built-in security features and following strict access and data handling protocols helps mitigate risks and safeguard cloud environments.


### What can you do instead of hardcoding secrets in IaC code?
 Parameterise Sensitive Data
### What collaborative process can catch security issues early?
Code Reviews
### What policies can you implement in CloudFormation for update controls?
Stack Policies

# Task 9 : Pratical

# Practical DevSecOps Demonstration with Terraform and AWS CloudFormation

We’ve explored cloud-based Infrastructure as Code (IaC) concepts using Terraform and AWS CloudFormation. Now, it’s time to experience what a typical day might look like as a DevSecOps engineer using these tools.  

In this static site, you’ll encounter a block of text representing your **infrastructure as code**. Initially, you’ll be presented with an **insecure, barebones infrastructure**. Your task is to enhance its security by selecting resource blocks or modifying variables.  

Once the infrastructure is secured and your role as a DevSecOps engineer is fulfilled, the **flag is yours!**  

To start your cloud-based adventure, click the green **"View Site"** button in the top right of this task.  

---

## 1. Introduction Menu

Upon launching the static site, you’ll see the **Introduction Menu**, which provides options such as:  

- **Resetting Infrastructure**: Start fresh by resetting the exercise.  
- **Accessing Instructions**: View guidelines and rules for the exercise.  
- **Building Infrastructure**: Begin the interactive task.  

Choose your desired option to proceed.  

---

## 2. IaC Screen

After selecting an option from the Introduction Menu, you’ll be taken to the **IaC Config Screen**.  
This screen simulates the stages of Terraform:  
- **Init**  
- **Plan**  
- **Apply**  

---

## 3. Instruction Model

The static site serves as an **instructional model** for learning YAML configuration fundamentals.  

Inspired by tools like **Scratch** and **Code Warrior**, it provides a user-friendly experience.  
This approach allows you to focus on key concepts without worrying about intricate coding details.  

---

## 4. Interactive Parts

During the exercise, you’ll interact with **clickable blanks** in a YAML configuration file.  

- These blanks represent infrastructure components that need to be added or modified to improve security.  
- Select the correct options to enhance your infrastructure step by step.  

---

## 5. Criteria for the Flag

The exercise consists of three phases:  

1. **Review**  
   - Analyse a YAML config file for security vulnerabilities.  
   - Click **"Next"** when you’re ready to proceed.  

2. **Implement**  
   - Fill in blanks to enhance infrastructure security.  
   - Use the Terraform workflow to complete the changes.  

3. **Destroy**  
   - After successfully securing the infrastructure, destroy it by clicking the **"Terraform Destroy"** button.  

**Note**: Successfully completing each phase will light up a **diamond** at the bottom of the screen.  

---

## 6. Messages

You’ll encounter various messages throughout the exercise:  

- **Welcome Message**: A greeting to start your journey.  
- **Review Phase Message**: Analyse the YAML config file for gaps in security.  
- **Implement Phase Message**: Enhance the infrastructure by filling in blanks and following instructions.  
- **Destroy Phase Message**: Securely destroy the infrastructure after implementation.  

---

## 7. Config Files

The provided YAML configuration file represents the infrastructure.  

- During the **Implement Phase**, interact with the clickable blanks using dummy options to build a secure infrastructure.  

### Can you use your Cloud-based IaC knowledge to get the flag?
THM{c10uD-b@z3d-1@SeE}