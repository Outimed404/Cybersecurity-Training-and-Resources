# Task 1 : Introduction


In the rapidly evolving world of software development, safeguarding source code is essential for maintaining the integrity and confidentiality of applications. One of the primary tools for managing and protecting source code is **version control**, which enables teams to collaborate efficiently, track changes, and maintain a comprehensive history of the codebase.

## Learning Objectives and Outcomes

In this module, we will cover the fundamentals of **source code storage and version control** with a particular focus on **Git**—a widely adopted version control system due to its flexibility, scalability, and robust feature set. While other systems like VCS and Mercurial also offer version control capabilities, Git’s popularity in the software development community makes it an ideal tool for our study.

### Key Topics Covered:
- **Introduction to Version Control**: Understanding the role of version control in DevSecOps (Development, Security, and Operations) and why it is a critical aspect of source code security.
- **Overview of Version Control Systems**: A look at various version control tools, with an emphasis on Git.
- **In-depth Git Exploration**: Commands, workflows, and best practices to help secure and manage a codebase effectively.

By the end of this module, you’ll have a comprehensive understanding of source code security and the essentials of version control with Git, equipping you to manage and secure your codebase with confidence.

Let’s get started!

# Task 2 : Git and Linus

Git’s creation arose from necessity, controversy, and innovation. The **Linux kernel**, a significant open-source software project, originally managed its development with patches and archived files until 2002, when it adopted a **Distributed Version Control System (DVCS)** called **BitKeeper**. However, in 2005, strained relations between the Linux community and BitKeeper’s creator, Larry McVoy, led to the withdrawal of the Linux community’s access to BitKeeper, disrupting development.

This conflict motivated **Linus Torvalds**, the creator of Linux, to develop a new DVCS, and in April 2005, he released the first version of **Git**. Designed to support large-scale, distributed development, Git allowed developers to work on projects from anywhere and easily integrate changes from multiple contributors. Torvalds drew on his experience with the Linux kernel and inspiration from other DVCS tools like BitKeeper, Subversion, and Monotone, focusing on Git’s ability to handle complex development demands.

Since its inception, Git has become one of the most widely used version control systems globally, with adoption across industries and by millions of developers.

### Key Benefits of Git
- **Open-source**: Available for contribution by anyone at no cost.
- **High Performance**: Git focuses on the contents of files rather than their names, optimizing performance.
- **Security through Hashing**: Git protects code integrity and change history using cryptographic hashing.
- **Widespread Adoption**: Many organizations have adopted Git as their primary version control system due to its robustness and flexibility.

### When was Git released?
2005

### What did Linux Kernel use for a DVCS previous to git?
BitKeeper

# Task 3 : 


Source code security is essential in software development to protect code from unauthorized access, tampering, and vulnerabilities. **Version control** plays a crucial role in securing source code, as it enables teams to track code changes, collaborate effectively, and maintain a full history of edits.

## How Version Control Works

Version control involves a **repository** (a database of changes) and a **working copy** (a developer’s personal copy of the project files). Here’s how it typically operates:
- Developers edit their working copies without impacting others.
- Once changes are finalized, they are **committed** to the repository, making them available to the team.

## Centralized vs. Distributed Version Control

Version control can be:
- **Centralized**: Uses a single repository; commits are immediately visible to all users.
- **Distributed**: Each user has a personal repository. Changes must be **pushed** to a central repository for others to see.

**Git, Mercurial, and Subversion** are popular version control systems. Distributed systems like **Git** and **Mercurial** offer improved performance, error resistance, and enhanced features, though they are more complex to master than centralized systems like **Subversion**.

## Focus on Git

As a leading version control system in the industry, **Git** provides robust features for source code security and collaboration. In the following sections, we will delve into Git’s key concepts, commands, and best practices to manage source code securely and streamline team collaboration in software development.

### What type of version control is Git?
Distributed


# Task 4 : Cloud Based Version Control


**Cloud-based version control** is a modern solution for managing source code in software development. It enables developers to store code and version history in the cloud, facilitating collaboration from any location. This approach is particularly beneficial for distributed teams, offering features like easy access, real-time collaboration, robust version history management, and seamless integration with other development tools. Overall, cloud-based version control provides a scalable, collaborative, and efficient method for managing source code in contemporary software development workflows.

## GitHub

**GitHub** is a cloud-based platform that allows developers to store and manage source code globally. Established in 2007 by Chris Wanstrath, P.J. Hyett, Tom Preston-Werner, and Scott Chacon, GitHub was built using Ruby on Rails. As the first open-source code repository on the web, it has become the dominant platform for code hosting and review.

### CI/CD with GitHub Actions

**Continuous Integration and Continuous Deployment (CI/CD)** is an automation methodology used in the software development lifecycle. It streamlines automation and continuous monitoring from integration to delivery and deployment, collectively forming a CI/CD pipeline. GitHub introduced **Actions** in 2018 to enhance developer automation. Actions consist of **Workflows**, allowing developers to define how to build code throughout the development pipeline. Developers can create automated activities for various triggers, such as push events, issue creation, or new releases—useful for tasks like building containers or deploying web services.

## GitLab

**GitLab** is another open-core platform that combines development, security, and operations in a single application. Founded in 2014 by Ukrainian developer Dmitriy Zaporozhets and Dutch developer Sytse Sijbrandij, GitLab became notable as the first partly Ukrainian unicorn, valued over $1 billion in 2018. Designed from the outset as a collaboration tool and code repository service, GitLab integrates CI/CD tools by default, eliminating the need for third-party services and providing a comprehensive all-in-one solution.

### CI/CD with GitLab

GitLab’s CI/CD functionality enables developers to automate their software delivery pipeline, encompassing building, testing, deploying, and monitoring. It employs **runners**—agents that execute jobs—to facilitate continuous integration and deployment. Developers can create custom workflows, define stages, and specify jobs for parallel or sequential execution, leading to efficient and reliable automation.

GitLab emphasizes reliability with features such as a built-in container registry, support for continuous deployment to Kubernetes, and **GitLab Pages** for hosting static websites. It also allows for the creation of multiple stable branches beyond the main branch, enhancing version control and release management.

In summary, GitLab’s built-in CI/CD capabilities, focus on reliability, and all-in-one DevOps platform position it as a robust solution for software development and deployment.

### When was Github founded? 
2007

### Where do Cloud-Based VCS store code?
repositories

# Task 5 : Insufficient Credential Hygiene

Maintaining good **credential hygiene** in CI/CD environments is crucial to mitigate potential security risks. Various systems and individuals within the engineering ecosystem rely on credentials—such as secrets and tokens—to deploy and access resources and perform other privileged actions. However, managing these credentials securely can be challenging due to the various contexts in which they are used and stored. Common issues include:

- Insecurely storing credentials in code repositories
- Improper usage during build and deployment processes
- Leaving credentials in container image layers
- Printing credentials to console output
- Neglecting to rotate credentials

All these issues can lead to significant security breaches.

## Risk Impact

Credentials are prime targets for attackers looking to access valuable resources and facilitate malicious activities. The engineering environments often present multiple opportunities for credential acquisition. Human errors and knowledge gaps in credential management further elevate the risk of credential exposure and compromise of critical resources.

## Recommendations

To mitigate risks associated with poor credential hygiene, organizations should adopt the following best practices:

1. **Least Privilege Principle**: Ensure credentials follow the principle of least privilege from code to deployment.
2. **Unique Credentials**: Avoid sharing the same credentials across multiple contexts to maintain accountability and simplify privilege management.
3. **Temporary Credentials**: Use temporary credentials whenever possible and establish procedures to rotate static credentials, periodically detecting stale credentials.
4. **Predefined Usage Conditions**: Limit credential usage to predefined conditions, such as specific IP addresses or identities.
5. **Secrets Detection**: Use IDE plugins, automatic scanning, and periodic repository commit scans to detect secrets pushed to and stored in code repositories.
6. **Prevent Console Output Exposure**: Utilize built-in vendor options or third-party tools to prevent secrets from being printed to console outputs during builds, ensuring that existing outputs do not contain secrets.
7. **Artifact Management**: Verify that secrets are removed from artifacts, such as container image layers and binaries.

Implementing these recommendations will help organizations safeguard against the risks posed by inadequate credential management.

## Environment Variables and Best Practices

**Environment variables** are an effective method for promoting credential hygiene during software development and deployment processes. They are commonly used to store and manage sensitive configuration information, such as API keys, passwords, and other credentials. Best practices for managing environment variables securely include:

1. **Avoid Hardcoding**: Do not hardcode sensitive information in code; instead, utilize environment variables.
2. **Regular Reviews and Rotation**: Regularly review and rotate credentials stored in environment variables.
3. **Access Control**: Limit access to environment variables to authorized personnel only.
4. **Principle of Least Privilege**: Set environment variables according to the principle of least privilege.
5. **Monitoring and Auditing**: Implement monitoring and auditing mechanisms to track changes to environment variables.
6. **Secrets Manager Solutions**: If using a secrets manager solution, review its encryption mechanisms to ensure compatibility with your development environments.

> **Note**: Implementing environment variables does not guarantee immunity from compromise. Adhering to best practices is essential to ensure they are implemented securely and effectively.

### What is a solution to store secrets securely without revealing them?
environment variables
### Does using environment variables mean you are free from secrets being compromised (Yes or No)?
No

# Task 6 : The Git, The Branch and The Ugly


Here we will cover basic commands to interact with projects in GitLab.

To fetch a project from GitLab, Git uses a command called `clone`. The syntax looks something like this:

```textarea
git clone <repository_url>
```


It’s used to copy (clone) an existing repository (repo) at another location in a new directory (repositories). Local filesystems or remote machines accessible by supported protocols can host the original repository. Working copies of Git repositories have their history, manage their files, and are isolated from their source repositories. The repository you create on your computer contains all the files and directories of your project. You can think of a branch as different versions of your repository or as an independent development line. Various versions of a repository can be contained in multiple branches.

```textarea
git clone -b name_of_branch <repository_url>
```

The above example would clone only the specified `name_of_branch` from the remote Git repository.

## Understanding Git’s Branching Model

To understand Git’s branching model, it’s essential to know how Git stores its data. Git does not store changesets or differences; instead, it keeps snapshots of the content. When you make a commit in Git, it creates a commit object that includes a pointer to the snapshot of the content, along with information like author details, commit message, and parent commit(s) pointer(s).

### Branch and Repo Structure

#### Branching

Git provides the `git branch` command to create, list, and delete branches. As mentioned, branches in Git represent isolated lines of development. When you create a local branch from a remote branch, tracking branches are created automatically by Git, which are local branches linked to remote branches. You can list remote-tracking branches using the `-r` option with the `git branch` command and local and remote branches with the `-a` option.

In Git, local branches exist on your local machine and remote branches are stored in a remote location. Comparing the differences between local and remote branches can be helpful. Here are the steps to do it:

1. Update remote-tracking branches by running the command: 

```bash
git fetch
```


2. List both local and remote branches using the command: 

``` bash
git branch -a
```


This will show the branches with an asterisk indicating the currently checked-out branch.

3. To specifically list remote branches, use the following command: 

```bash
git branch -r
```

### Adding Code

- **Add Changes**: To add changes made to a file to the staging area, use the `git add` command followed by the file name. This tells Git to include the changes made to the file in the next commit.


```bash
git add <filename>
```

- **Commit Changes**: To save changes to the staging area, use the commit command. This creates a new commit in the Git history with the changes made. Use the commit message to describe the changes, which is important to track progress.


```bash
git commit -m "commit message"
```

- **Push Changes**: This will update the remote repository with the changes you made locally. It’s the final command to upload the changes from your local repository to the remote. Add the name of the branch you are working on. Note: you can pass “origin.” “Origin” refers to the repository from which a project was initially cloned.


```bash
git push <branch name>
```



### How does Git refer to isolated lines of development?
branches

### What term does Git use to refer to the original repository you cloned from?
origin

### What command can you use to "copy" the contents in a remote repository?
git clone

# Task 7 : USCSS Nostromo


In the depths of space, aboard the **USCSS Nostromo**, a team of developers was on a mission to explore a distant planet. As they journeyed through the void of space, they encountered a strange and terrifying creature - an alien unlike any they had ever seen!

As they frantically tried to defend themselves against the monster's attacks, they realized something else was even more dangerous than the alien: the ship's code was riddled with security holes!

One of the most alarming issues was that sensitive authentication credentials were hard-coded directly into the source code of the ship's software. This meant that anyone who gained access to the code would have access to these critical credentials, putting the entire mission at risk.

Thankfully, the team had **DevSecOps Engineers** who knew just what to do. They decided to use **environment variables** to store the sensitive information so that the credentials would not be directly visible in the source code. This way, they could prevent unauthorized access to their systems by protecting their credentials.

## Getting Started

1. **Start the Machine**: Press the **Start Machine** button at the top of this task. You may access the VM using the **AttackBox** or your **VPN connection**.
2. **Access the GitLab Server**: Open your web browser and navigate to [https://10-10-114-132.p.thmlabs.com](https://10-10-114-132.p.thmlabs.com) (VM takes approximately **3-5 minutes** to boot). Note: As a free user using the AttackBox, access the VM via [http://10.10.114.132](http://10.10.114.132).

### Log In

Log in to the GitLab server using the provided credentials:
- **Username:** `TryHackMe`
- **Password:** `TryHackMe!`

Search for the **USCSS-Nostromo** project template in the GitLab search bar.

## Set-Up: Adding an SSH Key

To interact with a project, you need to add an SSH key to your user account to clone the project. Follow these steps:

1. Open a new terminal window.
2. Generate a new SSH key with the command:

   ```bash
   ssh-keygen -t ed25519 -C "try@hackme.com"
   ```

3. Press **Enter** to accept the default file name and location.
4. Optionally enter a passphrase for added security or leave it blank.
5. View the public key with:

   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

6. Copy the public key to your clipboard.

### Add the SSH Key to GitLab

1. Navigate to your profile settings in GitLab and click **"Edit profile"**.
2. Go to the **"SSH Keys"** tab.
3. Paste the public key into the **"Key"** field and give it a title (e.g., **"My GitLab SSH Key"**).
4. Click the **"Add Key"** button to save it.

### Cloning the Project

1. Familiarize yourself with the project.
2. Clone it to your local machine using:

   ```bash
   git clone <project-URL>
   ```

   Replace `<project-URL>` with the same URL used to log in. Enter the login credentials when prompted.

3. If using SSH, add the hostname to your hosts file:

   ```bash
   echo '10.10.114.132 gitlab.tryhackme.loc' >> /etc/hosts
   ```

4. Get the SSH URL under the Clone drop-down and navigate into the project directory:

   ```bash
   cd uscss-nostromo
   ```

5. Create a new branch:

   ```bash
   git checkout -b <branch-name-of-your-choice>
   ```

## Environment Variables

Environment Variables are key-value pairs set in the operating system that can be accessed by programs. They are used to manage software configuration and store sensitive information, such as authentication credentials.

### Implementing Environment Variables

1. Identify the plaintext credentials in **nostromo.go**.
2. Replace hard-coded credentials with environment variables using `os.Getenv`:

   ```go
   import (
       "fmt"
       "net/http"
       "os"
   )
   
   func init () {
       apiURL = "https://example.com"
       username = os.Getenv("GITLAB_USERNAME")
       password = os.Getenv("GITLAB_PASSWORD")
   }
   ```

3. Commit your changes:

   ```bash
   git commit -a -m "Fixed credential hygiene by using environment variables"
   ```

4. Push changes to the branch:

   ```bash
   git push -u origin <branch-name-chosen-earlier>
   ```

5. Verify that the changes are reflected in the main branch by checking the **nostromo.go** file for your updated credentials.

### Security Improvements

In the updated codebase, the following enhancements were made:
- Used the `os.Getenv()` function to read environment variables for sensitive information.
- Improved security and maintainability by allowing credentials to be updated without modifying the code directly.

By following these steps, the team aboard the **USCSS Nostromo** can protect their mission from both external threats and internal vulnerabilities.

### What is the name of the package that you need to import to make use of os.getenv?
os

### What is the hidden flag?
THM-3LL3N-RIPL3Y 

# Task 8 : Secret Management


Environment variables are crucial for securing digital authentication credentials such as passwords, keys, APIs, and tokens in applications, services, and sensitive IT components. Proper implementation of a secret management solution is necessary to utilize these environment variables effectively. Inadequate secrets management can lead to security vulnerabilities and auditing challenges, potentially resulting in security breaches. This tutorial focuses on managing secrets with GitLab.

## Managing Secrets in GitLab

Once environment variables are added to a GitLab project, you can use GitLab's secret management features to securely store and access these variables. Here’s how to do it:

1. Go to your GitLab project and navigate to the **Settings** page.
2. Click on the **CI/CD** tab and then click on **Variables**.
3. Click on **Add variable** and enter the name and value of your environment variable. Ensure that the variable is marked as **Protected**.
4. Click **Add variable** to save it.

In your `nostromo.go` file, you can use the following syntax to access the environment variable's value:

```go
func init () {
    apiURL = "https://example.com"
    username = os.Getenv("GITLAB_USERNAME")
    password = os.Getenv("GITLAB_PASSWORD")
}
```

This allows for secure storage and access of credentials through environment variables.
## Bonus

Notice the presence of a **.gitlab-ci.yml** file? If there are no pipelines, workflows, or "status checks," consider researching its purpose at GitLab CI/CD Documentation. While it may be outside the scope of this tutorial, it's worth exploring in future tasks related to "Build System Security."

###  What is the hidden flag?
THM_S3CUr3_4L13NS
### What do you need to keep source code secure besides environment variables?
secret management
### Which file handles the configuration to run CI/CD jobs?
.gitlab-ci.yml