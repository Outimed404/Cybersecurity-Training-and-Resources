# Task 1 : Introduction

The Software Development Lifecycle (SDLC) framework is a systematic process used for planning, creating, testing, and deploying software applications. This framework is essential for ensuring the quality, security, and efficiency of software development. By understanding the SDLC, developers and stakeholders can better manage projects, mitigate risks, and deliver high-quality software products.

## Learning Objectives

In this room, you will learn the following:

1. **Understanding the SDLC Framework**:
   - Define the SDLC framework and its significance in software development.
  
2. **Phases of SDLC**:
   - Explore the different phases of the SDLC, including planning, design, development, testing, deployment, and maintenance.

3. **Instilling Security into the SDLC**:
   - Learn strategies and best practices for incorporating security measures at each stage of the SDLC to safeguard applications against vulnerabilities.

4. **Security SDLC Phases and Process**:
   - Understand the specific phases and processes within the Security SDLC, emphasizing how to ensure security is integrated into every aspect of software development.

# Task 2 : SDLC

## Summary of Software Development Lifecycle (SDLC)

The Software Development Lifecycle (SDLC) is a framework consisting of standardized practices that guide the development of software applications. It outlines specific tasks to be performed at each stage of software development, aiming to enhance the quality of both the software and the development process, thereby meeting customer expectations and adhering to deadlines and budget constraints. By measuring and analyzing each phase, SDLC helps maximize efficiency and reduce costs.

## Key Phases of SDLC

1. **Planning**: Involves project management aspects, including resource allocation, scheduling, and cost estimation.
2. **Requirements Definition**: Identifies the functionality and requirements of the application, such as user interactions in a social media app.
3. **Design & Prototyping**: Defines how the application will operate, including architecture, programming language, and communication methods.
4. **Software Development**: Focuses on building the application, including coding and documentation.
5. **Testing**: Ensures that all components function correctly and interact seamlessly, with an emphasis on performance.
6. **Deployment**: Makes the application available to users.
7. **Operations & Maintenance**: Addresses user-reported issues and plans for future enhancements.

The phases of SDLC can be adapted or merged based on the chosen software development model, such as Waterfall, Agile, or DevOps. For instance, testing can be integrated into the development phase when security measures are incorporated throughout the development process.

### How many phases can an SDLC have? (Format X-Y)
6-8

# Task 3 : SDLC Phase Part 1


### 1. Planning

In the Planning Phase, also known as the Feasibility Stage, the project's scope and purpose are defined. This phase ensures that the project remains focused on its original objectives while establishing boundaries. Key activities include:

- Identifying system problems and defining the scope.
- Outlining an effective plan for the development cycle to catch potential issues early.
- Determining necessary resources to achieve the plan, which is crucial for timely market readiness.

### 2. Requirements Definition

The Requirements Definition phase involves detailing the prototype ideas and gathering essential information. Activities include:

- Listing all requirements for the prototype system.
- Evaluating alternative prototypes.
- Identifying end-user needs through research and analysis.

For example, a social media application requires functionalities like connecting with friends. Documentation, such as a Software Requirement Specification (SRS), is typically produced to define these requirements.

### 3. Design and Prototyping

Developers outline the application's overall details, including:

- User interfaces and system interfaces.
- Network requirements and database instances.

SRS documents are converted into logical structures implemented in programming languages. Plans for system operation, training, and maintenance are established. Key elements include:

- Architectural specifications for coding practices and design.
- User interface definitions that dictate interaction with customers.
- Platform selection for running the software and communication methods for interacting with other systems.

An Architecture Design Review (ARD) document is usually created to align all teams involved in different aspects of the project.

### 4. Software Development

In the Software Development phase, code is written based on earlier design specifications. Developers benefit from clear instructions and documentation, including playbooks and guides. Tools such as compilers, debuggers, and interpreters are used, adhering to established coding guidelines. Incorporating secure coding practices and hygiene into playbooks enhances the overall security of the application.


### What phase focuses on determining the first idea for a prototype? 
Requirements Definition
### What stage is also known as the "Feasibility Stage"?
Planning Stage
### When do you outline the user interfaces and network requirements?
Design and Prototyping

# Task 4 : SDLC Phase Part 2

### 5. Testing

Testing is essential before releasing an application to users. During this phase, developers utilize automated tools, like source code scanners, to ensure the software meets quality standards defined earlier in the SDLC. Key components include:

- **Test Case Design and Development**: Testers create detailed test cases based on the test plan, ensuring they validate core functionalities within time and scope. Test cases should be simple, unique, identifiable, and repeatable to accommodate ongoing updates as new features are added. They may also require maintenance to validate both new and existing functionalities.

- **Test Environment Setup**: This involves establishing the parameters of the test environment, including hardware, software, test data, frameworks, configurations, and network settings. It's crucial for ensuring that the testing reflects actual user conditions.

- **Test Execution**: Testers execute all test cases, identifying and logging bugs while measuring system performance against requirements. Regression testing may be conducted to verify fixes and prevent new issues from arising.

### 6. Deployment

In the Deployment phase, the software’s design is finalized, integrating different modules into the main codebase. The application is prepared for market release, often utilizing automated deployment tools to streamline rollouts. Important aspects include:

- **Automation of Deployment**: Tools like Netlify and Argo CD facilitate automated software deployments, allowing for quick rollouts and rollbacks if issues arise.

### 7. Operations and Maintenance

Post-deployment, developers enter the maintenance phase to address issues reported by users and implement necessary changes. Key focus areas include:

- **Handling Bugs and Issues**: Developers resolve residual bugs or new issues that arise after launch. The emphasis shifts to maintaining software stability and uptime.

- **Collaboration with Operations Teams**: Operations teams support developers by promoting self-service and standardizing tools and processes. This collaboration enhances developer velocity and reduces friction between teams.

- **Automation of Operations Tasks**: DevOps teams automate routine tasks to improve consistency and efficiency. Tools like Terraform, Vagrant, and Ansible are used to manage operations through code, ensuring that operations are versioned, secured, and maintained.

### What phase focuses on handling issues or bugs reported by end-users?
Operations and Maintenance
### What phase involves releasing new versions of software?
Deployment
### What phase ensures software meets the standards defined in the requirements phase?
Testing

# Task 5 : Keep CALMS
The **CALMS** framework assesses a company's readiness for DevOps adoption. Coined by Jez Humble, co-author of *The DevOps Handbook*, CALMS stands for **Culture, Automation, Lean, Measurement, and Sharing**.

## Culture
DevOps is a shift in culture, not just methodology. To adopt DevOps successfully, organizations must move away from traditional waterfall models and embrace iterative, sprint-based development. This cultural shift affects all departments: development, QA, product management, and operations.

## Automation
Automation is essential in DevOps to streamline integration and deployment. As teams break projects into smaller components, automated processes ensure that feature integrations are reliable and repeatable. Initially, teams focus on continuous delivery, progressing to continuous integration and configuration as code, adapting builds based on the deployment environment. This approach minimizes errors and optimizes production-ready code.

## Lean
DevOps promotes a lean, iterative approach, delivering initial application versions sooner. This enables user feedback and continuous improvements, ensuring the product evolves over time rather than waiting for a "perfect" release.

## Measurement
Metrics are key in DevOps for tracking effectiveness and guiding incremental improvements. These metrics help identify areas to enhance, ensuring the process is both adaptable and constantly improving.

## Sharing
A strong DevOps pipeline relies on shared responsibility across all teams. When development, operations, and other teams collaboratively work toward the final solution, the result is a more cohesive and high-quality product for users.

### What does CALMS stand for?
Culture, Automation, Lean, Measurement, Sharing

# Task 6 : DevOps Metrics

Understanding DevOps metrics is essential for introducing security in a way that aligns with DevOps objectives. By building a common perspective, teams can instill security measures effectively.

## Key DevOps Metrics

Metrics provide insight into the health and efficiency of DevOps processes, helping to pinpoint areas for improvement. Essential metrics include:

- **Mean Time to Production (MTTP):** Measures the time from code commit to deployment. Shorter MTTP is achieved through test automation and small batch processing, giving developers quick feedback.
- **Deployment Frequency:** Tracks how often new code is deployed to production, reflecting team agility. High frequency requires an automated deployment pipeline with minimal human intervention.
- **Production Failure Rate:** Indicates the percentage of deployments requiring fixes post-production. Lowering failure rates helps ensure code stability and security compliance.
- **Mean Time to Recover (MTTR):** The time required to restore service after a failure, highlighting response and resolution efficiency. Continuous system monitoring and rapid rollback mechanisms help optimize MTTR.

## Tracking MTTP

MTTP is a critical metric that reveals how quickly code moves from development to deployment. Practices like pre-release testing, automation, and working in small batches improve MTTP, allowing faster detection and correction of issues.

## Reducing Failure Rates

Failure rates track production issues that need immediate remediation. Low MTTP correlates with reduced failure rates, making it easier to address bugs promptly, ensuring that new releases meet security standards.

## Deployment Frequency

Deployment frequency measures the speed of new code reaching production, often occurring multiple times per day in agile teams. Automated testing and feedback loops minimize manual interventions, supporting frequent, reliable deployments.

## Mean Time to Recover (MTTR)

MTTR tracks the speed of recovery from service interruptions, requiring effective monitoring, alerting, and rollback mechanisms. A low MTTR indicates that teams can address issues swiftly, minimizing impact.

## Communicating Risk

Metrics also serve to bridge the gap between DevOps and security perspectives. For DevSecOps, risk focuses on vulnerability impact, whereas DevOps views risk in terms of production stability. Understanding these perspectives fosters empathy and helps integrate security in ways that resonate across teams.

### What 2 metrics are used to measure deployment agility?
Deployment speed and frequency
### What is an essential rate for engineers in Production environments to know if code meets security requirements?
Failure rate
### What is the measurement for recovery time after a failure?
MTTR

# Task 7 : 

### What is the flag that you receive once you have doubled the empire's investment?
THM{Ruler.of.the.SDLC.Droids}