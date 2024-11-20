# Task 1 : Introduction

## Introduction to Infrastructure as Code (IaC)

Infrastructure as Code (IaC) is a fundamental pillar of DevOps and DevSecOps methodologies. It enables the automation, management, and provisioning of infrastructure through machine-readable configuration files, rather than manual processes. For individuals aiming to excel in DevOps or DevSecOps, mastering IaC concepts and tools is essential. 

Despite the wealth of information available on IaC, many resources skim over key concepts, leaving newcomers overwhelmed by technical terms and uncertain about the appropriate tools for specific tasks. This room provides a structured and paced introduction to IaC, offering foundational knowledge and clarity on its terms, lifecycle, and practical applications.

## Learning Prerequisites

Before diving into IaC, it is recommended that you have completed all prior DevSecOps modules. Key topics such as **Virtualization and Containers** and **Intro to Containerisation** (covered in Task 6 of the DevSecOps path) are especially relevant and provide necessary context for understanding IaC.

## Learning Objectives

By completing this room, you will achieve the following objectives:

- **Understand the Need for IaC**: Learn why IaC is crucial in modern infrastructure management.
  
- **Grasp IaC as a Concept**: Explore the principles of IaC, the tools available, and their unique characteristics.

- **IaC Lifecycle**: Understand the lifecycle of IaC, including the distinction between provisioning and configuration, and identify tools suitable for both stages.

- **Virtualization as a Foundation**: Recognize how virtualization serves as the backbone for implementing IaC effectively.

- **On-Premises vs. Cloud-Based IaC**: Differentiate between on-premises and cloud-based IaC approaches, understanding their benefits and use cases.

This foundational knowledge will equip you to navigate IaC with confidence and prepare for more advanced topics in subsequent modules.

# Task 2 : IaC-TheConcept Introduction to Infrastructure as Code (IaC)

Infrastructure as Code (IaC) is a fundamental pillar of DevOps and DevSecOps methodologies. It enables the automation, management, and provisioning of infrastructure through machine-readable configuration files, rather than manual processes. For individuals aiming to excel in DevOps or DevSecOps, mastering IaC concepts and tools is essential. 

Despite the wealth of information available on IaC, many resources skim over key concepts, leaving newcomers overwhelmed by technical terms and uncertain about the appropriate tools for specific tasks. This room provides a structured and paced introduction to IaC, offering foundational knowledge and clarity on its terms, lifecycle, and practical applications.

## Learning Prerequisites

Before diving into IaC, it is recommended that you have completed all prior DevSecOps modules. Key topics such as **Virtualization and Containers** and **Intro to Containerisation** (covered in Task 6 of the DevSecOps path) are especially relevant and provide necessary context for understanding IaC.

## Learning Objectives

By completing this room, you will achieve the following objectives:

- **Understand the Need for IaC**: Learn why IaC is crucial in modern infrastructure management.
  
- **Grasp IaC as a Concept**: Explore the principles of IaC, the tools available, and their unique characteristics.

- **IaC Lifecycle**: Understand the lifecycle of IaC, including the distinction between provisioning and configuration, and identify tools suitable for both stages.

- **Virtualization as a Foundation**: Recognize how virtualization serves as the backbone for implementing IaC effectively.

- **On-Premises vs. Cloud-Based IaC**: Differentiate between on-premises and cloud-based IaC approaches, understanding their benefits and use cases.

This foundational knowledge will equip you to navigate IaC with confidence and prepare for more advanced topics in subsequent modules.

### Your organisation is preparing to launch a new service called FlyNet. The DevSecOps team provisioned an infrastructure and tested this service in the dev environment. Which IaC characteristic will streamline the provisioning of this same infrastructure in staging and production?
Repeatable

### It's the day before launch, and the latest infra change has started producing strange errors, something about "destroying humanity". Weird! Which IaC characteristic allows us to go back to the last known working version? 
Versionable

### It's the day before launch, and the latest infra change has started producing strange errors, something about "destroying humanity". Weird! Which IaC characteristic allows us to go back to the last known working version? 
Scalable

# Task 3 : IaC - The Tools Part 1 

Infrastructure as Code (IaC) is a fundamental pillar of DevOps and DevSecOps methodologies. It enables the automation, management, and provisioning of infrastructure through machine-readable configuration files, rather than manual processes. For individuals aiming to excel in DevOps or DevSecOps, mastering IaC concepts and tools is essential. 

Despite the wealth of information available on IaC, many resources skim over key concepts, leaving newcomers overwhelmed by technical terms and uncertain about the appropriate tools for specific tasks. This room provides a structured and paced introduction to IaC, offering foundational knowledge and clarity on its terms, lifecycle, and practical applications.

## Learning Prerequisites

Before diving into IaC, it is recommended that you have completed all prior DevSecOps modules. Key topics such as **Virtualization and Containers** and **Intro to Containerisation** (covered in Task 6 of the DevSecOps path) are especially relevant and provide necessary context for understanding IaC.

## Learning Objectives

By completing this room, you will achieve the following objectives:

- **Understand the Need for IaC**: Learn why IaC is crucial in modern infrastructure management.
  
- **Grasp IaC as a Concept**: Explore the principles of IaC, the tools available, and their unique characteristics.

- **IaC Lifecycle**: Understand the lifecycle of IaC, including the distinction between provisioning and configuration, and identify tools suitable for both stages.

- **Virtualization as a Foundation**: Recognize how virtualization serves as the backbone for implementing IaC effectively.

- **On-Premises vs. Cloud-Based IaC**: Differentiate between on-premises and cloud-based IaC approaches, understanding their benefits and use cases.

This foundational knowledge will equip you to navigate IaC with confidence and prepare for more advanced topics in subsequent modules.


### In the scenario given, which type of IaC tool considers where you are on the map and gives instructions to reach the desired X point?
Declarative

# Task 4 : IaC - The Tools Part 2 
## Immutable vs. Mutable Infrastructure

Infrastructure can be categorized as **immutable** or **mutable**, depending on whether changes can be applied directly or require a complete rebuild. Here's a breakdown of both approaches:

---

## **Mutable Infrastructure**
- **Definition**: Infrastructure is modified in place without the need for a complete rebuild.
- **Advantages**:  
  - Efficient use of resources since changes are applied directly.  
  - Faster updates for tasks like regular maintenance.  
- **Disadvantages**:  
  - Risk of inconsistent states if an update fails partially (e.g., some servers may not fully update).  
  - Debugging issues can be complex, especially when different versions coexist.
- **Example Use Case**: Updating a critical database system where rebuilding from scratch introduces risks.  
- **Tools**: Chef, Puppet, Ansible (supports both mutable and immutable configurations).

---

## **Immutable Infrastructure**
- **Definition**: Infrastructure cannot be modified once provisioned. Changes require creating a new instance, applying updates, and replacing the old infrastructure.  
- **Advantages**:  
  - Ensures consistency across environments (e.g., version 2 is deployed without partial updates).  
  - Easier debugging since the previous version remains intact until the new one is fully functional.  
  - Failure recovery is simpler — the failed infrastructure can be torn down and recreated.  
- **Disadvantages**:  
  - Resource-intensive due to the need for parallel infrastructures during updates.  
  - Slower deployment for updates.  
- **Example Use Case**: Deploying a web application update to ensure consistency across all servers.  
- **Tools**: Terraform, AWS CloudFormation, Pulumi.

---

## **Provisioning vs. Configuration Management**

IaC tools can also be categorized based on their **purpose**:

## **Provisioning Tools**
- **Definition**: Tools that set up infrastructure, defining and creating resources (e.g., servers, databases, and networks).  
- **Examples**:  
  - Terraform  
  - AWS CloudFormation  
  - Google Cloud Deployment Manager  
  - Pulumi  

## **Configuration Management Tools**
- **Definition**: Tools that manage the software and settings within provisioned infrastructure, such as installing, configuring, or updating software.  
- **Examples**:  
  - Ansible  
  - Chef  
  - Puppet  
  - SaltStack  

### **Example Workflow**
A company deploying a new web application could:  
1. Use **Terraform** to provision the servers, databases, and networks.  
2. Use **Ansible** to install and configure the necessary application software and monitoring agents.

---

## **IaC Tools Summary**

| Tool                 | Paradigm          | Agent Type  | Infrastructure Type | Primary Use         |
|----------------------|-------------------|-------------|----------------------|---------------------|
| **Terraform**        | Declarative       | Agentless   | Immutable           | Provisioning        |
| **Ansible**          | Hybrid            | Agentless   | Mutable/Immutable   | Configuration Mgmt. |
| **Pulumi**           | Declarative       | Agentless   | Immutable           | Provisioning        |
| **AWS CloudFormation** | Declarative     | Agentless   | Immutable           | Provisioning        |
| **Chef**             | Imperative        | Agent-based | Mutable             | Configuration Mgmt. |
| **Puppet**           | Declarative       | Agent-based | Mutable             | Configuration Mgmt. |

---

# **Static Site Scenario**

**The Year**: 2029  
**The Problem**: Sentient AI "FlyNet" plans to alter history by sending machines back to 1984 to eliminate Ron Connor's mother, preventing his birth.  
**Your Task**: Use IaC tools to bypass FlyNet's employee training module and locate their time-travel technology.

- **Step 1**: Provision the infrastructure for your mission using **Terraform** or **Pulumi**.  
- **Step 2**: Install critical reconnaissance software with **Ansible**.  
- **Step 3**: Retrieve the location of the time-travel tech before FlyNet intercepts.  

What could go wrong? Just a rogue AI trying to rewrite history!

### Can you retrieve the location and retrieve the flag?
thm{l4b_C0mpl3x_co0rds}


# Task 5 : Infrastructure as Code Lifecycle

## **The Infrastructure as Code Lifecycle (IaCLC)**

The Infrastructure as Code Lifecycle (IaCLC) defines a systematic process for managing infrastructure provisioning, configuration, and ongoing maintenance. This lifecycle is not a linear sequence but a continual and repeatable cycle, ensuring infrastructure remains secure, reliable, and aligned with evolving requirements.

---

## **Phases of the IaC Lifecycle**

### **Continual (Best Practice) Phases**
These phases are ongoing and applied consistently to maintain and enhance infrastructure throughout its lifecycle.

1. **Version Control**
   - Ensure all infrastructure definitions and configurations are versioned.
   - Enables rollbacks to previous states if changes introduce errors.
   - Tools: Git, SVN.

2. **Collaboration**
   - Foster teamwork by maintaining transparency and communication.
   - Helps prevent mismatches in modular components and confusion over infrastructure state.

3. **Monitoring/Maintenance**
   - Continuously monitor infrastructure for:
     - Performance issues.
     - Security events or breaches.
     - Failure events.
   - Automate maintenance tasks (e.g., backups, disk cleanup).

4. **Rollback**
   - In case of failures, restore the infrastructure to the last known good state using version-controlled definitions.

5. **Review + Change**
   - Regularly assess infrastructure for:
     - New business needs.
     - Security vulnerabilities.
     - Optimization opportunities.
   - Apply updates to improve performance, compliance, and scalability.

---

### **Repeatable (Infra Creation + Config) Phases**
These phases are executed during infrastructure setup or when significant changes are required.

1. **Design**
   - Plan the infrastructure architecture, considering:
     - Security (e.g., access controls, encryption).
     - Scalability.
     - Business requirements.
   - Example: Design scaling policies to handle peak traffic loads securely.

2. **Define**
   - Convert the design into code using a declarative or imperative approach.
   - Tools: Terraform, AWS CloudFormation, Pulumi.

3. **Test**
   - Validate the code using:
     - **Linters**: Catch syntax errors and logical inconsistencies.
     - **Staging Environments**: Deploy the infrastructure in a test environment to ensure it works as intended.

4. **Provision**
   - Use provisioning tools to set up the infrastructure in the production environment.
   - Tools: Terraform, Pulumi.

5. **Configure**
   - Apply necessary settings and software to the provisioned resources.
   - Tools: Ansible, Chef, Puppet.

---

## **Lifecycle Workflow**

1. **Activity: Provisioning New Infrastructure**
   - **Continual Phases**: Version Control, Collaboration, Review + Change.
   - **Repeatable Phases**: Design → Define → Test → Provision → Configure → Monitor.

2. **Activity: Infrastructure Changes**
   - Triggered by: Business requirements, performance issues, or discovered vulnerabilities.
   - **Continual Phases**: Monitoring/Maintenance, Review + Change, Rollback (if needed).
   - **Repeatable Phases**: Define → Test → Provision → Configure.

---

## **Key Practices**

- Adopt **CI/CD Pipelines** for infrastructure to streamline testing, provisioning, and configuration.
- Ensure frequent **code reviews** to identify potential issues early.
- Use **automated monitoring tools** (e.g., Prometheus, CloudWatch) for real-time insights.

By following the IaCLC, teams can maintain robust, secure, and scalable infrastructure, aligning with both technical and business needs.

### A DevSecOps Engineer at CyberMyne is looking for guidance on developing their next infrastructure. What type of phases provide guidance during the development or configuration of an infrastructure?
Repeatable

### What type of phases ensure best practices throughout infrastructure development and management?
Continual

### The 'Monitoring/Maintenance' continual phase can trigger which other continual phase?
Rollback

# Task 6 : Virtualisation & IaC

## **Virtualisation and Infrastructure as Code (IaC)**

Virtualisation is a cornerstone technology that enhances the power and efficiency of Infrastructure as Code (IaC). By abstracting physical resources, virtualisation enables scalability, consistency, and flexibility in managing infrastructure. This overview will explore key virtualisation levels, their relationship with IaC, and specific technologies like Kubernetes that combine virtualisation with IaC to unlock unparalleled efficiency.

---

## **Virtualisation Levels**

### 1. **Hypervisor-Level Virtualisation**
- **Definition**: Virtualises entire machines, allowing multiple virtual machines (VMs) to run on a single physical server.
- **Example**: VMware.
- **Features**:
  - Each VM can run a different OS.
  - Resource-intensive but offers high flexibility and isolation.

### 2. **Container-Level Virtualisation**
- **Definition**: Virtualises at the operating system level, sharing the same OS kernel across containers.
- **Example**: Docker.
- **Features**:
  - Lightweight and fast deployment.
  - Ideal for microservices and scalable applications.

---

## **Virtualisation Use Cases in IaC**

### **1. Scalability**
- IaC enables scaling by defining policies that trigger the creation or removal of virtual machines or containers based on demand.
- Example: Automatically adding instances during peak loads.

### **2. Resource Isolation**
- Virtualisation allows precise resource allocation.
- Using IaC, resource limits (CPU, memory) can be defined, ensuring one component's overuse doesn't affect others.

### **3. Testing, Snapshots, and Rollbacks**
- **Testing**: Virtualised environments provide consistent replicas for development and testing.
- **Snapshots**: Capture the state of VMs/containers for easy rollback during deployment failures.
- **Rollback**: Revert to a previous stable state in case of errors.

### **4. Templates**
- Create reusable templates or blueprints for virtualised environments.
- IaC provisions new instances from these templates, ensuring uniformity and reducing setup time.

### **5. Multi-Tenancy**
- Host multiple tenants (customers) on the same infrastructure with isolated resources.
- IaC manages multi-tenant setups while ensuring security and efficiency.

### **6. Portability**
- Virtualisation facilitates moving infrastructure across cloud providers or data centers.
- IaC simplifies recreating environments for hybrid cloud setups or disaster recovery.

---

## **Virtualisation Technologies in IaC**

### **1. Kubernetes (K8s)**
- **Purpose**: Container orchestration for deploying, scaling, and managing applications.
- **Key Components**:
  - **Master Node**: Manages the cluster.
  - **Worker Nodes**: Run containerised applications.
- **IaC Integration**:
  - Tools like Terraform automate provisioning of Kubernetes clusters and components.
  - IaC defines scaling policies for pods/nodes, automating resource adjustments.

### **Example Workflow**:
- Use Terraform to:
  1. Create a Kubernetes cluster.
  2. Deploy additional resources (e.g., Nginx server, load balancer).
  3. Define Kubernetes deployments, services, and configurations as reusable modules.

### **Benefits**:
- Declarative state management ensures the infrastructure matches the desired configuration.
- Dependency management prevents deployment issues by resolving order and requirements.

---

## **Other Virtualisation Technologies with IaC**

1. **Vagrant**
   - Spins up VMs or containers for local development.
   - IaC ensures consistency in environment setup.

2. **OpenStack**
   - Open-source platform for private cloud environments.
   - IaC automates resource provisioning and management within these clouds.

---

## **Conclusion**

Virtualisation and IaC are symbiotic technologies that amplify each other's capabilities. Hypervisor and container virtualisation provide the foundation for scalable, consistent, and portable infrastructure. Combined with IaC tools like Terraform and Kubernetes, they enable efficient provisioning, configuration, and management of modern infrastructure. To delve deeper, explore topics like **Virtualization and Containers** or **Intro to Containerisation**!


### CyberMine is deploying the latest machine model E-1000. This model requires virtualisation at an operating system level to allow for lightweight and rapid deployment behind the scenes! What level of virtualisation would be needed for this?
Containerisation
 
### CyberMine's E-100 Model is still very popular for all your extermination needs, this model requires multiple OS to run on a single machine. Which level of virtualisation would be needed for this?
Hypervisor

### The new E-1000 model has a feature that allows it to pass through physical objects. Wild! This new feature, however, is very resource-intensive. Which 'Use of IaC' will ensure that this resource consumption won't affect the performance of the machine's other components?
Resource Isolation

### Due to the resource consumption of this new feature, it requires rapid scaling of resources. Which container orchestration software can be used to automate this process?
Kubernetes

# Task 7 : On-Prem IaC vs. Cloud-Based IaC

## **On-Premises vs. Cloud-Based Infrastructure as Code (IaC)**

Infrastructure as Code (IaC) has two primary use cases: **on-premises** and **cloud-based**. The choice between these depends on factors such as control, scalability, cost, and the specific needs of the organization. This guide outlines key differences across five categories and highlights the unique benefits of each approach.

---

## **Key Differences**

### **1. Location**
- **On-Premises**:  
  - Infrastructure resides physically within an organization’s premises or in rented data centers.
  - Managed and maintained directly by the organization.
- **Cloud-Based**:  
  - Utilizes infrastructure hosted in the cloud by providers like AWS, Microsoft Azure, or GCP.
  - Resources are virtual and provisioned on demand.

---

### **2. Technology**
- **On-Premises**:  
  - Common tools: **Ansible**, **Chef**, **Puppet**.
  - These tools manage physical servers, network devices, and storage systems.
- **Cloud-Based**:  
  - Common tools: **Terraform**, **AWS CloudFormation**, **Azure Resource Manager (ARM)**, **Google Cloud Deployment Manager**.
  - Designed to exploit the elasticity of cloud computing and leverage CSP-specific features.

---

### **3. Resources**
- **On-Premises**:  
  - Deals with physical hardware and related challenges:
    - Hardware compatibility.
    - Physical maintenance and upgrades.
  - Often preferred by organizations with legacy systems or specific regulatory requirements.
- **Cloud-Based**:  
  - Interacts with virtual resources managed by the CSP:
    - Virtual machines, databases, storage, and networks.
  - The underlying infrastructure is abstracted and maintained by the CSP, freeing up user resources.

---

### **4. Scalability**
- **On-Premises**:  
  - Scaling is slow and requires manual intervention:
    - Procurement and physical installation of new hardware.
    - Planning for peak loads in advance.
  - Example: Universities preparing for traffic spikes during exam seasons.
- **Cloud-Based**:  
  - Leverages the elastic nature of the cloud:
    - Auto-scaling policies dynamically adjust resources based on demand.
    - Ideal for applications with fluctuating resource needs, such as online games during events.
  - Example: Retailers scaling up for Black Friday traffic and scaling down afterward.

---

### **5. Cost**
- **On-Premises**:  
  - High initial investment for physical hardware and ongoing maintenance costs.
  - Cost is fixed regardless of usage.
  - Often chosen for reasons other than cost, such as control or compliance.
- **Cloud-Based**:  
  - Pay-as-you-go pricing:
    - Only pay for resources used.
    - Enhanced efficiency with auto-scaling to avoid overprovisioning.
  - Example: Streaming services scaling resources during a popular release and scaling back later.

---

## **Benefits**

### **On-Premises IaC**
1. **Complete Control**:  
   - Full authority over the infrastructure, hardware, and data management.
   - Necessary for compliance with strict security and regulatory requirements (e.g., data sovereignty).  
   - Example: Large financial institutions or government agencies managing sensitive data.
2. **Customization**:  
   - Infrastructure can be tailored to meet highly specific business needs.  

### **Cloud-Based IaC**
1. **Scalability and Flexibility**:  
   - Rapidly scale resources up or down to meet fluctuating demand.
   - Ideal for fast-growing businesses or services with variable traffic patterns.  
   - Example: Online gaming or e-commerce platforms handling seasonal spikes.
2. **Global Reach**:  
   - Deploy infrastructure across multiple regions to reduce latency and improve user experience.
   - Example: Streaming services providing low-latency content worldwide.
3. **Cost Efficiency**:  
   - Avoids large upfront investments.
   - Pay-as-you-go pricing model ensures costs align with usage.

---

## **When to Choose On-Premises vs. Cloud-Based IaC**

| **Factor**            | **On-Premises IaC**                                    | **Cloud-Based IaC**                                    |
|------------------------|--------------------------------------------------------|--------------------------------------------------------|
| **Control Needs**      | Critical for data sovereignty and compliance.          | CSPs can handle many security and compliance aspects.  |
| **Scalability**        | Limited by physical hardware.                          | Elastic and automated scaling with minimal delays.     |
| **Cost Structure**     | High initial costs; fixed operational expenses.        | Pay-as-you-go; cost savings through scaling policies.  |
| **Speed of Deployment**| Slower, due to hardware constraints.                   | Faster; provisioning resources within minutes.         |
| **Use Case Example**   | Banks, government, or regulated industries.            | Startups, global services, or businesses with spikes.  |

By understanding these differences and benefits, you can choose the IaC approach that best fits your organizational needs.


### Cloud-based resources are provisioned/configured in a cloud environment. Who handles the underlying infrastructure?
cloud service provider
### What category does on-prem infrastructure struggle with due to hardware limitations when facing increased traffic?
scalability

# Task 8 : IaC - The Final Push

## Time to Fight

The technology required to send Sergeant Miles Creese back to 1984 is being held in a lab complex. All that's left now is to capture it and retrieve the tech. There's only one issue: it's located in a heavily fortified exterminator base, and the resistance doesn't have the resources to take them on. Lucky for the resistance, though, one piece of technology they do have their hands on is their 'Infrastructure at a Click' weapons system. This allows resistance fighters to deploy a weapon infrastructure (requiring a network and server components so the weapons can communicate with each other) to take on the machine army! After all, weapons can be reprovisioned; resistance fighters cannot! Simply define how many weapons you want using provision points and upgrade those weapons using configure points to defeat the incoming machines! Just don't let the machines take down your server, or it's game over! 

### Can you get the flag using your infrastructure as code skills?
thm{1Nfr4StrUctUr3_Pr0}