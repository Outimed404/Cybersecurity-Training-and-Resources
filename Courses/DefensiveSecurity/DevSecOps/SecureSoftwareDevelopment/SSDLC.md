# Task 1 : Introduction

This room focuses on the Secure Software Development Lifecycle (S-SDLC), its processes, and methodologies.

## Learning objectives

1. Understand  what SSDLC is and why it's important
2. Learn about the processes of SSDLC
3. Understand the Secure SDLC methodologies

# Task 2 : What is SSDLC?

Traditionally, security testing occurred late in the SDLC, leading to costly fixes and delayed vulnerability detection, often after deployment. The **Secure SDLC** (SSDLC) model addresses this by integrating security checks at every SDLC phase.

## Importance of SSDLC

Research by IBM’s Systems and Sciences Institute shows that fixing bugs found in the design phase is six times cheaper than fixing those found during implementation. Costs increase drastically, up to 100 times, if vulnerabilities are identified during maintenance. 

By integrating security early, SSDLC reduces costs, accelerates development, and lowers business risk. Examples include:

- **Design Phase:** Conduct architecture analysis for potential security flaws.
- **Development Stage:** Implement code reviews and use security scanners.
- **Pre-Deployment:** Perform security assessments, such as penetration testing.

In traditional waterfall models, security checks occurred at the end, often just before production, making remediation complex and costly. Agile methods enable “security by design,” integrating preventative discussions—like SQL injection mitigations—early in planning, saving time and reducing risk.

## Summary

- **Continuous Security:** Enhances software quality through ongoing security improvements.
- **Security Awareness:** Stakeholders understand security needs at each phase.
- **Early Detection:** Detects flaws before deployment, reducing risk and potential disruptions.
- **Cost Efficiency:** Early identification lowers remediation costs, minimizes business risk, and protects brand reputation.


### How much more does it cost to identify vulnerabilities during the testing phase?
15

# Task 3 : Implementing SSDLC
**Secure SDLC (SSDLC)** integrates security processes at every SDLC phase, from using security testing tools to defining security requirements alongside functional needs.

## Understanding Security Posture

To successfully implement SSDLC, start by assessing your current security posture:

- **Gap Analysis:** Identify existing activities and policies, their effectiveness, and alignment with security procedures.
- **Software Security Initiatives (SSI):** Define achievable goals with metrics, such as Secure Coding Standards or data handling playbooks, and track progress.
- **Formalize Processes:** Establish and operationalize security procedures within your SSI, allowing time for engineers to adapt and provide feedback.
- **Security Training:** Invest in training for engineers on both processes and tools to ensure smooth adoption.

## SSDLC Processes

With a clear security posture, integrate these key SSDLC processes into your development lifecycle:

- **Risk Matrix:**
  - **Risk Assessment:** Early SDLC phases should include security considerations to support a “security by design” approach, identifying potential issues when gathering requirements.
  - **Threat Modeling:** In the design phase, analyze potential threats by examining actions the software should prevent, reinforcing requirements with protective measures.

- **Code Scanning / Review:** Use automated or manual reviews, leveraging Static and Dynamic Security Testing (SAST/DAST) during development to identify vulnerabilities as code is written.

- **Security Assessments:**
  - **Vulnerability Assessments:** Automated scans that reveal application vulnerabilities without simulating attacks.
  - **Penetration Testing:** Conduct simulated attacks to validate and exploit vulnerabilities, ideally during the Operations & Maintenance phases.

## Next Steps

There are structured methodologies to guide you through risk assessment, threat modeling, scanning, testing, and operational assurance, which will be covered in detail in the following tasks.

### What should you understand before implementing Secure SDLC processes?
Security Posture
### During which stages should you perform a Risk Assessment?
Planning and Requirements
### What should be carried out during the design phase?
Threat Modelling

# Task 4 : Risk Assessment


**Risk** in the context of software development refers to the likelihood of a threat exploiting a vulnerability, which can lead to potential negative impacts. Factors like unpatched vulnerabilities, design flaws, or inadequately reviewed code can heighten risk. Integrating risk management throughout the SDLC helps mitigate these risks effectively.

## Purpose of Risk Assessment

A risk assessment helps identify potential threats, evaluate their severity, and determine how to control or mitigate them. This process is often followed by **threat modeling**, where specific threats are examined in more detail.

### Steps in Performing a Risk Assessment

1. **Assume Threats and Identify Motivations:** Begin with the assumption that attacks will occur. Evaluate the program’s data value, dependency on third-party resources, client profiles, and distribution reach to establish an acceptable risk level.
2. **Risk Evaluation:** Identify worst-case scenarios and simulate potential attacks to communicate risk impact. Evaluate factors like data sensitivity, attack complexity, and affected user count to prioritize risks.
3. **Access Evaluation:** Determine the level of accessibility, including network exposure and authentication requirements, to assess potential impacts on production versus test environments.

## Types of Risk Assessments

- **Qualitative Risk Assessment:** 
  - Classifies risks as "Low," "Medium," or "High" based on impact and probability. Uses a simple formula: **Risk = Severity x Likelihood**.
  - This approach is commonly used for its simplicity and provides a prioritized list without specific numerical values.

- **Quantitative Risk Assessment:**
  - Assigns numerical values to measure risk more precisely. For example, a high-impact bug in a critical service might be scored with points.
  - This assessment may involve creating a **Risk Matrix**, applying custom calculations based on the company’s operational needs, and quantifying potential financial losses.

## Timing of Risk Assessments in the SDLC

Risk assessments should occur early in the SDLC, during the planning and requirements stages, allowing teams to factor in the potential costs and prioritize appropriate security controls. Quantitative analysis, for example, can estimate **Annual Loss Expectancy (ALE)** to determine cost-effective security measures, balancing potential losses against control expenses.

---

By incorporating these risk assessments throughout the SDLC, teams can prioritize security efforts, communicate risks effectively, and make informed decisions about security investments.

### What is a formula to assign a Qualitative Risk level?
Severity x Likelihood

### Which type of Risk Assessment assigns numerical values to determine risk?
Quantitative Risk Assessment

# Task 5 : Threat Modelling

**Threat Modelling** is an essential process within the design phase of the SDLC, identifying and prioritizing potential security threats to protect assets and data, particularly those deemed valuable during risk assessment. Early integration of threat modelling allows teams to address vulnerabilities proactively, reducing long-term costs. 

## Key Threat Modelling Methodologies

Different threat modelling methodologies cater to various risk focuses, such as STRIDE for specific technical threats, DREAD for impact analysis, and PASTA for business-aligned threat modeling. These methods may be used independently or combined based on project needs.

### STRIDE

STRIDE, developed by Microsoft, categorizes threats based on six types aligned with the **CIA triad (Confidentiality, Integrity, and Availability)**:

- **Spoofing:** Impersonating users, violating authentication.
- **Tampering:** Unauthorized data modification, compromising data integrity.
- **Repudiation:** Actions that cannot be attributed to attackers, challenging accountability.
- **Information Disclosure:** Unapproved data access, breaching confidentiality.
- **Denial of Service (DoS):** Disrupting service availability.
- **Elevation of Privilege:** Unauthorized access escalation, violating authorization.

*Example Use:* A system data flow diagram (DFD) is created, and STRIDE is applied to each node to identify vulnerabilities across different threat categories.

### DREAD

DREAD complements STRIDE by providing a **threat scoring system** based on five components:

1. **Damage Potential:** Measures threat impact (0-10 scale; 0=no impact, 10=service outage).
2. **Reproducibility:** Indicates the ease of replicating the attack.
3. **Exploitability:** Assesses technical complexity for launching an attack.
4. **Affected Users:** Evaluates the number of affected users in case of an attack.
5. **Discoverability:** The likelihood of an attacker identifying the vulnerability.

The DREAD model is effective for **prioritizing threats** by calculating risk scores, helping guide response urgency and remediation efforts.

### PASTA

PASTA, or **Process for Attack Simulation and Threat Analysis**, is a strategic, risk-centric model that aligns security requirements with business objectives. PASTA uses seven stages:

1. **Define Objectives:** Establish goals and define asset scope.
2. **Define Technical Scope:** Create architectural diagrams (physical and logical) to understand dependencies.
3. **Decomposition & Analysis:** Map trust boundaries and threat vectors for each asset.
4. **Threat Analysis:** Identify specific application vulnerabilities based on threat intelligence.
5. **Vulnerabilities & Weakness Analysis:** Identify and mitigate known vulnerabilities.
6. **Attack/Exploit Enumeration & Modelling:** Map attack surfaces and simulate threat vectors.
7. **Risk Impact Analysis:** Finalize risk analysis, recommend mitigations, and document residual risks.

*Example Use:* PASTA could be used for a customer-facing app to map and simulate potential DoS attacks, identifying preventative actions aligned with business impact.

---

## Integrating Threat Modelling in SDLC

When integrated into the design phase of SDLC, threat modelling provides a proactive means to **identify, categorize, and mitigate security threats** before code is written, contributing to a secure, resilient system by design. Each methodology offers unique insights, allowing developers to tailor their approach based on project complexity and business risk.

### What threat modelling methodology assigns a rating system based on risk probability?
DREAD
### What threat modelling methodology is built upon the CIA triad?
STRIDE
### What threat modelling methodology helps align technical requirements with business objectives?
PASTA

# Task 6 : Secure Coding

Ensuring robust application security begins with a **secure code review** process within the SDLC, especially during the implementation phase. According to a 2020 Verizon report, **43% of breaches targeted web applications**, highlighting the critical need for early detection of vulnerabilities through secure code reviews. This process allows developers to identify and address security flaws before deployment, significantly reducing the risk of post-launch vulnerabilities and the associated costs of retroactive fixes.

## Secure Code Review Approaches

Secure code review methods can be **manual** or **automated**, and each offers distinct advantages:

- **Manual Code Review**: Security experts review code line-by-line to identify vulnerabilities, often collaborating with developers to understand the application’s functions. Manual reviews are valuable for discovering nuanced, context-specific issues but can be time-consuming.
  
- **Automated Code Analysis**: Automated tools such as **SAST** and **DAST** accelerate the review process, often detecting vulnerabilities overlooked in manual review, and providing continuous scanning capabilities to streamline secure development.

## Types of Code Analysis

### 1. Static Application Security Testing (SAST)

**SAST** is a **white-box testing** technique analyzing the application source code without running it. SAST can be implemented early in the SDLC and integrated into CI/CD pipelines, enabling developers to detect vulnerabilities before code integration.

**How SAST Works**:
- Analyzes code pre-compilation, identifying weaknesses like SQL injection or cross-site scripting (XSS).
- Helps find vulnerabilities through **White Box Testing**, giving testers insight into code structure and helping them identify exploitable paths.
- Best suited for early SDLC phases, flagging potential issues before they are deeply embedded.

### 2. Software Composition Analysis (SCA)

**SCA** is used alongside SAST to assess vulnerabilities within open-source components and libraries within a codebase. It is crucial for modern applications that often rely on third-party packages, helping teams stay compliant with security and licensing standards.

**Key Features of SCA**:
- Scans dependencies for known vulnerabilities.
- Alerts developers about any open-source component that may expose the application to risk.
- Ensures compliance with license requirements and governance policies throughout the SDLC.

### 3. Dynamic Application Security Testing (DAST)

**DAST** is a **black-box testing** technique for evaluating an application at runtime to discover vulnerabilities like configuration issues and unexpected response behaviors that could be exploited. DAST simulates external attacks, making it ideal for late-stage testing in staging or pre-production environments.

**How DAST Works**:
- Conducts simulated attacks to test application behavior and security response.
- Identifies vulnerabilities that may not be apparent in static code, such as runtime issues.
- Typically executed after code acceptance, providing insights on how the application handles potential exploitation.

### 4. Interactive Application Security Testing (IAST)

**IAST** combines aspects of SAST and DAST to deliver **grey-box testing**, allowing for real-time vulnerability detection while an application runs. It is deployed alongside the application on the server and often used in staging or testing environments.

**How IAST Works**:
- Monitors data flow and interactions within the application during execution.
- Detects vulnerabilities based on how data interacts across code paths.
- Informs developers of specific vulnerable code lines for faster remediation.

### 5. Runtime Application Self-Protection (RASP)

**RASP** is integrated directly into the application in production, monitoring traffic and blocking threats in real time. It identifies threats at runtime, making it a powerful tool for protecting deployed applications.

**How RASP Works**:
- Monitors inbound and outbound application traffic to detect malicious behavior.
- Takes protective actions such as blocking or isolating threats to protect against security incidents.
- Complements other testing methods by continuously monitoring runtime vulnerabilities and reporting anomalous behaviors.

## Implementing a Security Testing Timeline

To maximize security, it’s essential to implement each of these tools at different stages in the SDLC:

1. **Development Phase**: Integrate **SAST** to detect code-level vulnerabilities early, and **SCA** to manage open-source component risks.
2. **Pre-Production**: Introduce **DAST** and **IAST** to test the application in a staging environment, ensuring runtime vulnerabilities are identified and mitigated before release.
3. **Production**: Deploy **RASP** for continuous monitoring, allowing the application to self-detect and defend against emerging threats in real time.

By using a combination of these tools, organizations can better secure their applications across the SDLC, improving resilience against attacks and reducing security risks in production.

###  Is it recommended to use SAST analysis at the beginning of the SDLC? (y/n)
y
### Which type of code analysis uses the black-box method?
DAST
### Which type of code analysis uses the white-box method?
SAST

# Task 7 : Security Assessment


Security assessments are a core component of securing software development and should be integrated into all SDLC phases whenever feasible. Security assessments comprehensively evaluate applications for vulnerabilities and attack vectors and are often performed in the **Operations and Maintenance** phase to ensure security controls encompass the entire application. This ensures robust coverage of potential threats, especially as the application reaches production with fully integrated components.

There are two primary types of security assessments:

1. **Vulnerability Assessment**
2. **Penetration Testing**

Generally, external testers are engaged to perform these assessments legally, simulating real-world attack scenarios to improve security posture.

---

## Types of Security Assessments

### 1. Vulnerability Assessment

**Purpose**: A Vulnerability Assessment identifies potential security weaknesses across an organization's systems and network infrastructure without verifying or attempting to exploit them. 

- **Process**: Automated tools scan for vulnerabilities by probing system ports, services, and configurations, then cross-reference findings with known vulnerability databases.
- **Examples of Tools**: OpenVAS, Nessus (Tenable), and ISS Scanner.
- **Output**: Generates a report listing vulnerabilities, each assigned a **severity classification** (e.g., High/Medium/Low) or a **CVSS score**.

**Advantages**:
- Quickly identifies potential vulnerabilities.
- More budget-friendly than penetration testing.

**Disadvantages**:
- Results in a broad list of vulnerabilities, which may be challenging to prioritize.
- Results vary significantly based on the capabilities of the chosen tools.
- Does not simulate attack scenarios, so certain vulnerabilities may only be exploitable under specific conditions (e.g., behind a proxy or through social engineering).
- Low-severity vulnerabilities may serve as entry points for more impactful attacks.

### 2. Penetration Testing

**Purpose**: Penetration Testing, or **Pentesting**, involves an in-depth assessment of vulnerabilities, with testers actively attempting to exploit weaknesses to simulate real-world attack scenarios.

- **Process**: Builds upon Vulnerability Assessment by testing vulnerability exploitability and impact. It may involve additional tactics, such as **privilege escalation**, to assess the extent of potential harm.
- **Output**: A detailed report highlighting exploitable vulnerabilities with specific **countermeasures** to mitigate them.

**Advantages**:
- Demonstrates real-world attack potential.
- Quantifies true risk and provides actionable insights.
- Provides clear, actionable insights for stakeholders and customers.

**Disadvantages**:
- More resource-intensive than a Vulnerability Assessment due to the detailed work involved.
- Requires extensive planning and can take significant time to execute thoroughly.

### Which form of assessment is more budget-friendly and takes less time?
Vulnerability Assessment

### Which type of assessment identifies vulnerabilities and attempts to exploit them?
Penetration Testing

### When do you typically carry out Vulnerability Assessments or Pentests?
Operations & Maintenance

# Task 8 : SSDLC Methodologies

Integrating security into the Software Development Life Cycle (SDLC) is essential, regardless of the chosen development methodology (Agile, DevOps, Extreme Waterfall, etc.). Key considerations include:

- **Build with Security in Mind**
- **Introduce Testing Focused on Security**

## Common Methodologies for Integrating Security in SDLC

### 1. Microsoft's Security Development Lifecycle (SDL)

**Principles of SDL**:
- **Secure by Design**: Security is a fundamental quality attribute throughout the software lifecycle.
- **Security by Default**: Systems are built to minimize potential harm from attacks.
- **Secure in Deployment**: Deployment is accompanied by tools and guidance for users.
- **Communications**: Developers are prepared for threats through open communication with users and administrators.

SDL consists of mandatory security activities corresponding to traditional development phases, emphasizing understanding security vulnerabilities' causes and effects. 

**Key Practices**:
- **Provide Training**: Educate engineers and managers on security basics to ensure secure software development.
- **Define Security Requirements**: Update security requirements continually to reflect functionality changes and evolving threats.
- **Define Metrics and Compliance Reporting**: Establish acceptable security quality levels to hold teams accountable.
- **Perform Threat Modeling**: Document and assess security implications in planned designs.
- **Establish Design Requirements**: Ensure secure features are well-engineered and consistently applied.
- **Define and Use Cryptography Standards**: Protect sensitive data through proper encryption practices.
- **Manage Third-Party Components**: Understand and inventory third-party components to mitigate security risks.
- **Use Approved Tools**: Maintain a list of approved security tools and their checks.
- **Perform Security Testing (SAST, DAST, IAST)**: Conduct thorough code and runtime analyses.
- **Perform Security Assessments**: Utilize Vulnerability Assessments and Penetration Testing to uncover potential vulnerabilities.
- **Establish a Standard Incident Response Process**: Prepare an Incident Response Plan for addressing emerging threats.

### 2. OWASP Secure Software Development Life Cycle (S-SDLC)

**Principles of OWASP S-SDLC**:
- Mandatory security activities grouped by traditional SDLC phases.
- Metrics are collected for training effectiveness, process compliance, and post-release improvements.
- Emphasis on understanding security vulnerabilities' cause and effect.

The OWASP S-SDLC emphasizes building "security quality gates" throughout the development pipeline by dedicating sprints to security practices (e.g., code reviews, authentication, authorization, input validation). 

**Influences**:
- **Software Assurance Maturity Model (SAMM)**: An open framework to help organizations formulate and implement tailored software security strategies.
- **Building Security In Maturity Model (BSIMM)**: A study of real-world software security initiatives that reflects the current state of software security, serving as a comparison for organizations to assess their security posture.

For further information, you can explore the [OWASP SAMM](https://owasp.org/www-project-samm/) and [BSIMM](https://www.bsimm.org/) links.

### What methodology follows a set of mandatory procedures embedded in the SDLC?
Microsoft SDL
### What Maturity Model helps you measure tailored risks facing your organisation?
SAMM
### What maturity model acts as a measuring stick to determine your security posture?
BSIMM


# Task 9 : Secure Space Lifecycle

### What is the flag?
THM{D0-A-Barr3l-R011}
