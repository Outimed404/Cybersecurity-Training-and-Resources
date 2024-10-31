# Task 1 : Introduction
## Summary of Automation and Security in DevOps

Humans continuously seek simpler and more efficient methods to perform tasks. With the advent of programming and software development, the focus shifted towards automating processes. Today, automation is integral to the Software Development Life Cycle (SDLC) and DevOps practices, significantly enhancing development speed and deployment efficiency. However, this shift introduces new security risks. In manual processes, attackers typically target the individual's credentials or workstation. In contrast, automated processes allow attackers to exploit the pipeline itself.

### Learning Objectives

This module will cover the following concepts:

- Introduction to the DevOps pipeline
- Introduction to DevOps tools and automation
- Introduction to security principles for the DevOps pipeline

This introductory room will provide an overview of these concepts, which will be explored in greater detail in subsequent modules.


# Task 2 : DevOps Pipelines Explained

Before delving into automation security, it is essential to define the pipeline and identify where automation can be integrated. The following diagram illustrates a typical pipeline and the software that can be utilized for this purpose.

## Pipeline Components

In this module, we will explore the following aspects for each pipeline component:

- **Definition**: What each component is and its role in the pipeline.
- **Common Tools**: The tools frequently used for each component.
- **Security Overview**: An introduction to the security considerations related to each component.
- **Case Studies**: Examples of what can occur when security fails.

Each component will be examined in detail in the upcoming sections of this module.

### Where in the pipeline is our end product deployed?
Environments

# Task 3 : Source Code and Version Control


## Source Code Storage

The initial stage of the pipeline involves source code and version control, which requires a designated location for code storage. It is essential to maintain multiple versions of the code as improvements and features are continuously added. Key considerations for choosing a source code storage solution include:

- How to implement access control for the source code.
- Methods for tracking changes made to the code.
- Integration capabilities with development tools.
- Support for storing and using multiple versions of the code.
- Decision on whether to host the source code internally or use an external provider.

## Version Control

Version control is crucial for two main reasons:

1. **Feature Integration**: As modern development methodologies like Agile promote continuous updates, version control helps keep track of these changes.
2. **Team Collaboration**: With multiple developers working on the code, version control ensures that all changes can be effectively integrated.

Version control systems allow for maintaining various code versions, accommodating both individual developer versions and distinct application versions (minor and major).

### Common Tools

The two most prevalent source code storage and version control systems are:

- **Git**: A distributed version control system where each contributor has their own copy of the code.
- **SubVersion (SVN)**: A centralized version control system with a central repository.

GitHub is the largest provider for hosting Git repositories, while alternatives include self-hosted solutions like GitLab for Git and tools like TortoiseSVN and Apache SVN for SVN. Modern tools like GitLab offer functionalities that extend beyond simple storage and version control, supporting nearly the entire pipeline.

## Security Considerations

Source code often contains sensitive intellectual property, making authentication and access control vital. Changes and updates must be meticulously tracked, enabling rollback to previous versions if necessary. 

However, developers must avoid confusing source code storage with secret management. Sensitive information, like database credentials, should not be stored in the source code to prevent exposure, even in previous versions.

### Case Study: Git Never Forgets

The adage "Git never forgets" highlights the risks associated with version control. Once code is committed to a Git repository, it is permanently recorded, allowing access to historical commits. 

A common issue arises when developers inadvertently commit sensitive information (e.g., credentials) to a Git repo. If they delete these secrets in a subsequent commit, the historical record remains accessible. Tools like GittyLeaks can scan through all commits, potentially exposing sensitive information, regardless of its removal from the latest version.

### Who is the largest online provider of Git?
Github
### What popular Git product is used to host your own Git server?
Gitlab
### What tool can be used to scan the commits of a repo for sensitive information?
GittyLeaks

# Task 4 : Dependency Management
## Overview

While we may think we’re writing most of the code in our applications, much of it actually relies on pre-existing code in the form of libraries and software development kits (SDKs). These dependencies are an essential part of the development pipeline, covering both basic elements, like data types, to complex functionalities.

## External vs. Internal Dependencies

- **External Dependencies**: Public libraries and SDKs hosted on platforms like PyPi (Python), NuGet (.NET), and Gems (Ruby). These are openly available for integration but come with security concerns as they are outside direct control.
  
- **Internal Dependencies**: Custom libraries and SDKs developed and maintained within an organization, such as an in-house authentication library. These dependencies allow for specific functionality to be reused across multiple applications within the organization but also present unique security challenges.

### Security Concerns

**Internal Dependencies**:
- Internal libraries can become outdated if they no longer receive updates or if the original developer leaves the organization.
- Security for the package manager is the organization’s responsibility when it comes to internal dependencies.
- Vulnerabilities in an internal library can affect multiple applications within the organization if that library is widely used.

**External Dependencies**:
- Since the organization does not have full control over external dependencies, each library must be thoroughly vetted for security risks.
- Compromises in external package managers or content distribution networks (CDNs) can lead to supply chain attacks.
- External libraries are a common target for attackers who may search for zero-day vulnerabilities. If such vulnerabilities are discovered, multiple organizations using the same library could be affected simultaneously.


## Common Tools

To manage dependencies, package managers (or dependency managers) are used:
- For **external dependencies**: Common package managers include PyPi, NuGet, and Gems.
- For **internal dependencies**: Tools like JFrog Artifactory and Azure Artifacts help with the management of internally maintained libraries.

## Security Considerations

Dependencies often involve code outside our direct control, which introduces potential security risks. Tracking vulnerabilities in these dependencies can be challenging due to the sheer number of external libraries used in modern development. Unresolved vulnerabilities in dependencies may translate into vulnerabilities in the application itself.

## Case Study: Log4Shell

In 2021, a zero-day vulnerability was found in **Log4j**, a Java-based logging utility, called Log4Shell. This critical flaw allowed unauthenticated attackers to execute remote code on affected systems using Log4j, impacting countless applications worldwide due to its widespread use.

The vulnerability’s reach was immense, showcasing the scale of impact a single compromised dependency can have on multiple products. Lists of affected products even had to be split alphabetically due to the high volume, highlighting the real security risks associated with dependency management.
### What do we call the type of dependency that was created by our organisation? (Internal/External)
Internal
### What type of dependency is JQuery? (Internal/External)
External
### What is the name of Python's public dependency repo?
PyPi
### What dependency 0day vulnerability set the world ablaze in 2021?
Log4j

# Task 5 : Automated Testing

Automated testing has greatly enhanced the efficiency of testing processes, once a manual and tedious task. In modern pipelines, automated testing helps to ensure the stability and functionality of applications.

## Types of Automated Testing

### Unit Testing
Unit tests focus on small sections of the application to verify specific functionality. These tests can be integrated into the CI/CD pipeline as quality gates, stopping a build if test cases fail. However, unit testing generally does not cover security aspects.

### Integration Testing
Integration testing examines how various parts of an application work together. Regression testing is a subset that ensures new features do not negatively impact existing ones. Like unit testing, integration tests focus on functionality and usually do not cover security.

## Automated Security Testing

### Static Application Security Testing (SAST)
SAST tools analyze source code to identify vulnerabilities and can be integrated into CI/CD as security gates, halting the pipeline if vulnerabilities are detected. This type of testing flags potential security issues early in the development process.

### Dynamic Application Security Testing (DAST)
DAST tools perform runtime testing by executing the code, allowing them to detect vulnerabilities that SAST tools might miss. They often test for vulnerabilities like Cross-Site Scripting (XSS) by tracking data flow from sources to sinks within the application. DAST tools can also serve as security gates in the CI/CD pipeline.

### Penetration Testing
Manual penetration testing remains critical as automated tools (including SAST and DAST) may struggle with complex, context-dependent vulnerabilities, such as bypassing parts of a process flow (e.g., payment validation) or identifying business logic flaws. While automated tools have improved, manual testing remains a cost-effective approach for detecting these complex vulnerabilities.

## Common Tools
Popular tools for automated testing include:
- **SAST Tools**: GitHub and GitLab offer built-in SAST tools; others like Snyk and SonarQube are widely used for both SAST and DAST.

## Case Study: Performance and Integration Challenges in Automated Security Testing

One challenge in implementing SAST and DAST tools is ensuring they are properly integrated into the pipeline without impacting performance. When introducing new security gates, several key considerations include:
- **Performance Impact**: SAST/DAST tools require substantial resources to scan code, which can slow down operations, especially before major releases.
- **Integration Points**: Ensure integration aligns with pipeline flow to avoid bottlenecks.
- **Result Calibration**: Calibrate the tool for accuracy to minimize false positives or overlooked vulnerabilities.
- **Quality and Security Gates**: Configure gates to halt the pipeline only for valid security concerns to avoid unnecessary delays.

A Proof-of-Concept (PoC) for new security tools should be carefully planned, ideally outside of peak hours, to assess both performance costs and integration impacts without disrupting the pipeline.

### What type of tool scans code to look for potential vulnerabilities?
SAST

### What type of tool runs code and injects test cases to look for potential vulnerabilities?
DAST

### Can SAST and DAST be used as a replacement for penetration tests? (Yea,Nay)
Nay


# Task 6 : Continuous Integration and Delivery


Modern software pipelines automate the process of compiling, building, integrating, and deploying software updates. This continuous integration and delivery process, known as **CI/CD**, supports Agile development and accelerates software delivery.

### Evolution of CI/CD Terminology

- **Continuous Integration and Continuous Development**: Initially focused on Agile development practices while still deploying final releases on a waterfall model.
- **Continuous Integration and Continuous Deployment**: Evolved to Agile deployment, integrating development within the CI phase.
- **Continuous Integration and Continuous Delivery**: Expanded to include all elements of solution delivery, including post-deployment monitoring and feedback.

These terms are often used interchangeably, though they all refer to the same goal of frequent, automated integration and delivery.

## Components of a CI/CD Pipeline

A CI/CD pipeline typically includes these elements:

1. **Starting Trigger**: An action, such as a code push to a branch, that starts the pipeline.
2. **Building Actions**: The process of building the project and its new features.
3. **Testing Actions**: Verifying that new features work without disrupting existing functionality.
4. **Deployment Actions**: Specifies deployment targets if tests pass, e.g., pushing to a Testing environment.
5. **Delivery Actions**: Goes beyond deployment, focusing on the delivery and monitoring of the deployed solution.

### Build Infrastructure

CI/CD pipelines require infrastructure, known as **build orchestrators** and **agents**:
- **Build Orchestrators** direct agents in performing CI/CD tasks.
- **Agents** execute specific pipeline tasks.

These pipelines automate substantial parts of the SDLC, representing a significant attack surface and an area prone to misconfiguration.

## Common CI/CD Tools

Popular tools for CI/CD include:
- **GitHub** and **GitLab**: Provide CI/CD capabilities with build agents (GitHub) and GitLab Runner applications (GitLab).
- **Jenkins**: A widely-used build orchestrator suited for complex CI/CD builds.

## Case Study: Dev and Prod Misconfiguration

A typical misconfiguration in CI/CD involves sharing build agents between **Development (DEV)** and **Production (PROD)** environments. This setup poses risks:
- Developers typically have access to DEV triggers but not PROD.
- If a DEV user account is compromised, an attacker can trigger malicious builds on a shared DEV/PROD agent.
- By persisting on the agent, the attacker can inject malicious code during a PROD build, compromising the application at release.

The best practice is to separate DEV and PROD build agents to prevent cross-environment contamination.

### What does CI in CI/CD stand for?
Continuous Integration
### What does CD in CI/CD stand for?
Continuous Delivery
### What do we call the build infrastructure element that controls all builds?
Build Orchestrator
### What do we call the build infrastructure element that performs the build?
Build Agent

# Task 7 : Environments

Modern CI/CD pipelines include multiple environments with specific roles, stability levels, and security postures. Each environment progressively increases in stability and security as it nears production (PROD), ensuring that the software meets quality and security standards throughout the development lifecycle.

### Core Environments

1. **DEV (Development)**
   - **Purpose**: Developer workspace for coding and testing.
   - **Stability**: Unstable due to frequent updates and changes.
   - **Security**: Lowest security posture, with relaxed access controls.
   - **Customer Data**: No.

2. **UAT (User Acceptance Testing)**
   - **Purpose**: Testing specific features and overall application functionality.
   - **Stability**: Semi-stable with some security controls.
   - **Security**: Improved from DEV but not yet production-grade.
   - **Customer Data**: No.

3. **PreProd (Pre-Production)**
   - **Purpose**: Last testing environment before production, ideally mirroring PROD.
   - **Stability**: Stable environment to ensure smooth PROD transition.
   - **Security**: High security, similar to PROD but may vary by implementation.
   - **Customer Data**: No.

4. **PROD (Production)**
   - **Purpose**: Serves live users or customers with the current application version.
   - **Stability**: Must remain stable; updates require strict change management.
   - **Security**: Strongest security posture, limited access.
   - **Customer Data**: Yes.

5. **DR/HA (Disaster Recovery/High Availability)**
   - **Purpose**: Ensures continuous operation in case of production failures.
   - **Stability**: Fully mirrors PROD stability and availability.
   - **Security**: Matches PROD, ensuring operational integrity.
   - **Customer Data**: Yes.

### Additional Deployment Strategies

- **Blue/Green Environments**: Parallel PROD instances, one with the current version (Blue) and one with the new version (Green). Allows rapid switch between versions if issues arise.
- **Canary Environments**: Gradual deployment method that migrates small user segments to the new environment, minimizing risk by monitoring stability with incremental traffic increases.

### Modern Environment Management Tools

Advances in virtualization and containerization enable flexible, efficient environment management using tools like:
- **Vagrant**, **Terraform**: Create virtual environments.
- **Docker**, **Kubernetes**: Deploy containerized environments.

These tools support **Infrastructure as Code (IaC)** for consistent and repeatable environment setup.

### Security Considerations

As environments progress toward PROD, they must undergo thorough security hardening to reduce attack surface, which includes:
- Disabling unnecessary services.
- Regularly updating hosts and applications.
- Enforcing firewall configurations to block unused ports.

### Case Study: Developer Bypasses in PROD

**Issue**: Developer bypasses intended for DEV (e.g., hardcoded OTPs, MFA bypasses) can accidentally persist into PROD if not properly sanitized, creating security vulnerabilities.

**Solution**: Enforce segregation between environments and implement security gates to prevent these bypasses from progressing through pipeline stages.
### Which environment usually has the weakest security configuration?
DEV
### Which environment is used to test the application?
UAT
### Which environment is similar to PROD but is used to verify that everything is working before it is pushed to PROD? 
PrePROD
### What is a common class of vulnerabilities that is discovered in PROD due to insecure code creeping in from DEV?
Developer Bypasses

# Task 8 : Challenge

### What is the flag received after successfully building your pipeline?
THM{Pipeline.Automation.Is.Fun}

# Task 9 : Conclusion 

Automation has transformed SDLC processes, enabling faster and more efficient creation and deployment of updates in applications. However, the introduction of automation also brings new security challenges. With increased automation comes an expanded attack surface, as attackers can target the CI/CD pipeline itself to compromise applications indirectly.

### Importance of Secure Automation

To prevent automation from becoming a vulnerability point, secure automation practices are crucial. A secure automated pipeline aims to:
- Minimize risk of compromise by protecting automated workflows.
- Safeguard application integrity by ensuring that automation steps themselves are secure.
- Ensure that each component of the pipeline—from build triggers to deployment and monitoring—maintains security.

### Course Scope

This module explores each element of the pipeline in-depth, focusing on security best practices for a robust, automated pipeline. Topics covered include:
- **Trigger and Build Security**: Safeguarding pipeline initiation points and build integrity.
- **Testing and Security Gates**: Incorporating security checks throughout the testing stages.
- **Deployment and Monitoring**: Maintaining security even in live environments with continuous monitoring.

By understanding the security requirements at each stage of the pipeline, developers can build not only faster but more resilient applications.
