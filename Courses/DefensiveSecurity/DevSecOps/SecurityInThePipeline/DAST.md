# Task 1 : Introduction

## Summary of Dynamic Application Security Testing (DAST)

Dynamic Application Security Testing (DAST) is a method used to evaluate the security of an application by analyzing it from an external perspective, similar to how an attacker would approach it. This technique focuses on identifying vulnerabilities in a running instance of the application.

## Room Objectives

- **Applying DAST to Identify Weaknesses**: Learn how DAST can be effectively utilized to uncover security flaws in real-world applications.
- **Understanding Pros and Cons**: Gain insights into the advantages and limitations of DAST compared to other security testing methods.
- **Familiarization with DAST Tools**: Get acquainted with various tools that are commonly employed for DAST.

DAST provides a practical approach for testing live applications, making it a valuable tool for proactive security assessment.


# Task 2 : Dynamic Application Security Testing (DAST)

## What is DAST?
Dynamic Application Security Testing (DAST) involves testing a live web application for vulnerabilities using an approach similar to that of an external attacker. This "black-box" method detects vulnerabilities by attempting to exploit them through manual or automated means.

DAST evaluates the application from an outsider’s perspective, abstracting away from its internal code to focus on vulnerabilities visible to an attacker. The results often highlight critical vulnerabilities that do not require prior knowledge of the system.

DAST complements other security testing methods and does not replace them. A comprehensive secure development lifecycle typically integrates multiple techniques for better coverage.

## Manual vs. Automated DAST

- **Manual DAST**: Conducted by security engineers who manually test the application for vulnerabilities, allowing for deeper insights and detection of issues that automated tools may miss.
- **Automated DAST**: Uses tools to scan applications for vulnerabilities, offering efficiency and speed. Automated scans are well-suited for frequent code changes, but may lack the nuanced understanding of manual testing.

### Benefits of Combining Both Approaches
Using both manual and automated DAST within the Software Development Lifecycle (SDLC) can yield better results. Automated DAST is ideal for rapid feedback during development, while manual DAST can be performed periodically for thorough analysis.

## DAST in the SDLC
Automated DAST is often integrated during test phases for quick feedback. It helps catch simple, common vulnerabilities early. Manual DAST, with its more comprehensive scan profiles, can be conducted on a weekly basis or tailored to project needs. Before production release, a full web application pentest is recommended for an extensive review.

**Note**: Automated DAST is often referred to simply as DAST, while manual DAST may be considered part of traditional web application penetration testing.

## Pros and Cons of DAST

### Pros
- Identifies vulnerabilities during runtime, including those related to the deployment process.
- Detects complex vulnerabilities like HTTP request smuggling and parameter pollution, often missed by SAST.
- Programming language-agnostic due to black-box nature.
- Produces fewer false positives compared to SAST.
- May detect certain business logic flaws, depending on the tool.

### Cons
- Limited code coverage; may miss specific use-case vulnerabilities.
- Some vulnerabilities are harder to find compared to static analysis.
- Challenges in crawling modern, client-heavy applications.
- Lacks detailed remediation guidance due to absence of code insight.
- Some scans may be time-consuming.
- Requires a running application to test.

## Ready to DAST?
DAST typically involves two main tasks:

1. **Spidering/Crawling**: Navigates the web app to map pages and parameters.
2. **Vulnerability Scanning**: Launches attack payloads to test identified pages and parameters. Users can customize attack types based on application needs.

For this demonstration, we will use the ZAP proxy as a DAST tool. While enterprise tools often come with added features and higher costs, the fundamental principles remain the same.

### Is DAST a replacement for SAST or SCA? (Yea/Nay)
Nay


### What is the process of mapping an application's surface and parameters usually called?
Spidering/Crawling


### Does DAST check the code of an application for vulnerabilities? (Yea/Nay)
Nay


# Task 3 : Spiders and Crawlers 

## Spidering an App with ZAP

The goal is to map out all resources in a target web application using ZAP's spidering module to identify the scope of analysis. The spider explores the website starting from a specified page and follows all detectable links.

## Spidering

To begin spidering a website:

1. Go to **Tools -> Spider**.
2. Enter the website's URL as the starting point (leave Context and User boxes empty).
3. Optional settings:
   - **Recurse**: Follow links recursively.
   - **Spider Subtree Only**: Limit spidering to subfolders of the starting URL.
   - **Show Advanced Options**: Enable advanced settings for fine-tuning.

### Spider Window

- Point to `http://10.10.141.230:8082/` and press **Start Scan**.
- After scanning, the **Sites tab** will display all discovered URLs.

### Sites Tab

- The **Sites list** populates with URLs found during spidering.
- URLs outside the starting point's URL are considered out-of-scope.
- Example: A link like `/nospiders-gallery.php` may not appear if it is dynamically generated by JavaScript.

## Spider Limitations

- ZAP’s regular spider does not process JavaScript, so it misses links generated by scripts (e.g., `/nospiders-gallery.php`).
- Since ZAP does not have a JavaScript engine, it cannot detect dynamically generated links.

## More Spiders

To overcome these limitations, ZAP can use a real browser (e.g., Firefox or Chrome) to process JavaScript and retrieve the resulting HTML code. This is done using the **AJAX Spider**.

### Using the AJAX Spider

1. Go to **Tools -> AJAX Spider**.
2. Configure the URL and other settings.
3. Select **Firefox** as the browser.
4. Click **Start Scan**.
5. The AJAX Spider will use Firefox to browse the website and process JavaScript.
6. After completion, the **Sites tab** will show dynamically generated URLs (e.g., `/nospiders-gallery.php`).

Leveraging a real browser helps ZAP capture the full page rendering, ensuring the spider results match what a user sees.

### ZAP can run an AJAX spider by using browsers without a Graphical User Interface(GUI). What are these browsers called?
Headless

### Analysing the Sites tab, what HTTP parameters can be passed to login.php using the POST method? (In alphabetical order and separated by commas)
pass, user


### What other .php resource, besides nospiders-gallery.php was found by the AJAX spider but not by the regular spider?
/view.php


# Task 4 : Scanning for Vulnerabilities

## Configuring a Scan Policy in ZAP

Customizing the scan policy is crucial in Dynamic Application Security Testing (DAST) to optimize the scanning process and avoid irrelevant tests. Tailoring the scanning profile to the specific web application reduces scan time by eliminating unnecessary payloads.

### Example Application Setup:
- **Operating System**: Linux
- **Web Server**: Apache 2.4
- **Programming Language**: PHP (No frameworks used)
- **Databases**: None

In this example, the application doesn't use a database. If the default scanning profile is used, tests like SQL injections will be performed, which are irrelevant in this case. This can result in unnecessary scan time, especially for larger applications.

## Creating and Modifying Scan Policies

To create or modify a scan policy in ZAP:

1. Go to **Analyse -> Scan Policy Manager**.
2. Click **Add** to create a new scan policy.

### Scan Policy Parameters:

For each category in the policy, you can configure the following parameters:

- **Threshold**: Controls the verbosity of the tests.
  - Lower threshold: More findings, higher chance of false positives.
  - Higher threshold: Fewer findings, more confidence in results but may miss some vulnerabilities (false negatives).
  - Set to **OFF** to disable a category.
  
- **Strength**: Controls how many tests are run for each category.
  - Higher strength: More tests, but longer scan times.

These parameters should be customized based on the application and iteratively refined over time.

### Disabling Irrelevant Tests

For the given example application:
- Disable tests related to databases and XML processing (since they are not used).
- Disable **Cross Site Scripting (DOM Based)** tests to save resources and speed up the scan.

## Running the First Scan

After customizing the scan policy, initiate a vulnerability scan by:

1. Going to **Tools -> Active Scan**.
2. Select the starting point (e.g., `http://10.10.141.230:8082/`) and check **Recurse** to scan pages beyond the starting point.
3. Click **Start Scan**.

### Reviewing Scan Results

After the scan, results will appear in the **Alerts** section, where you can:
- See descriptions of the findings.
- Review the specific request and response used by ZAP to detect vulnerabilities.
- Mark any false positives by right-clicking the finding and selecting **Mark as False Positive**.

By customizing the scan policy and reviewing findings, the scanning process becomes more efficient and tailored to the specific application.


### Will disabling some test categories help speed up the scanning phase? (Yea/Nay)
Yea

### There should be two high-risk alerts in your scan results. One is Path Traversal. What's the name of the other one?
Cross Site Scripting (Reflected)

# Task 5 : Authenticated Scans
## Dealing with Logins in ZAP

To scan areas of a web application that require authentication, we must teach ZAP how to log in and maintain an active session. This is done by recording the authentication process and using a ZEST script to replicate it during scans.

## Disabling ZAP HUD

Before starting, make sure to disable the ZAP HUD from the toolbar, as it might interfere with some tasks.

## Recording Authentication

1. Click **Record a New ZEST Script** from the toolbar.
2. Set the script type to **Authentication** and specify the base URL.
3. Click **Start Recording**.
4. Open a browser by clicking **Open Browser** and navigate to the login page (`http://10.10.141.230:8082/login.php`).
5. Manually log in with the credentials:
   - **Username**: nospiders
   - **Password**: nospiders
6. Stop recording the script by clicking the **Record a New ZEST Script** button again.
7. Close ZAP’s browser and delete unnecessary requests from the recorded script.
8. Test the recorded script by clicking the **Run** button in the Script Console.

### Result of Authentication Test

- If successful, ZAP will detect a redirection to `cowsay.php` after the login POST request.

## Creating a Context

A **Context** defines a group of URLs where authentication will be applied. For this simple application:

1. Right-click the base URL in the **Sites tab** and select **Include in Context -> New Context**.
2. Under the **Authentication** tab, select **Script-based Authentication** and load the recorded ZEST script.
3. Even though the user is embedded in the ZEST script, ZAP requires a user definition for authentication to work. Create a user in the **Users** section.

Once done, the resources in the site will be marked with a target icon, indicating they are part of the defined Context.

## Re-spidering the Application

1. Rerun the spider with the correct **Context** and **User**.
2. This allows ZAP to discover previously undetected resources that require authentication.

### Checking the Results

- After re-spidering, check the **Added Nodes** tab for any newly discovered resources.
- ZAP may find additional PHP scripts, such as the logout functionality (`logout.php`), which could interfere with authentication during scanning.

## Avoiding Logouts

1. Right-click the `logout.php` script in the **Sites tab** and exclude it from the Context to prevent ZAP from logging out during scans.
2. Identify **Logged-in** and **Logged-out indicators**:
   - **Logged-in indicator**: Look for elements (e.g., the logout link) that appear when logged in. Flag them as the **Authentication Logged-in indicator**.
   - **Logged-out indicator**: Look for elements (e.g., the login link) that appear when logged out. Flag them as the **Authentication Logged-out indicator**.

## Verification Strategy

1. Use the **Poll the Specified URL** strategy to verify if the session is still active by checking for the logged-in/logged-out indicators.
2. Set ZAP to poll `/aboutme.php` every 60 requests to ensure the session is still valid.

## Rescanning the Application

1. After configuring authentication and verification strategies, initiate a new scan by going to **Tools -> Active Scan**.
2. Select the **Context** and **User** you created.
3. ZAP will now scan the authenticated sections of the application and potentially uncover new vulnerabilities.

### Final Scan Results

ZAP should now detect additional vulnerabilities, including high-risk issues that were previously inaccessible due to authentication requirements.

### Which type of script was used to record the authentication process to our site in ZAP?
ZEST script

###  What additional high-risk vulnerability was found on the site after running the authenticated scan?
Remote OS Command Injection

# Taks 6 : Checking APIs with ZAP

## Dealing with APIs in ZAP

Web applications increasingly rely on APIs to perform tasks. Unlike traditional web applications that can be easily spidered, APIs require a different approach for security testing because their endpoints do not expose information about each other, making discovery harder. Rather than spidering APIs, we will rely on the development team to provide a specification of the available API endpoints and the parameters that can be sent to them.

## API Descriptions

APIs are typically documented using standards like OpenAPI (formerly Swagger), SOAP, or GraphQL. These specifications provide the necessary information to test the security of the API endpoints. Here's a breakdown of how to interact with APIs using ZAP:

- **OpenAPI (Swagger)**: This standard is commonly used to define API endpoints, parameters, and responses.
- **SOAP**: An older standard for APIs, typically used in enterprise systems.
- **GraphQL**: A newer API query language that allows clients to request exactly the data they need.

The specification file for an API usually contains a section called `paths`, which lists all the available endpoints and their parameters. For example, the file for the API at `http://10.10.141.230:8081/swagger.json` lists an endpoint `/asciiart/{art_id}` that uses the GET method and accepts an `art_id` parameter in the URL.

Some APIs also provide a user-friendly UI to test and explore the endpoints. For example, the API at `http://10.10.141.230:8081/swagger-ui/` allows quick testing of the available endpoints.

> **Note**: In production environments, API definitions might be hidden to prevent attackers from knowing how to interact with the API. You can request these definitions from the development team if needed.

## Importing OpenAPI Definitions in ZAP

ZAP supports two methods for importing API definitions:
1. **Offline file**: You can upload an API definition file directly.
2. **URL**: You can provide a URL to download the API definition.

For example, to import the OpenAPI definition from the URL `http://10.10.141.230:8081/swagger.json`, follow these steps:

1. Go to **Import -> Import an OpenAPI definition from a URL**.
2. Specify the URL `http://10.10.141.230:8081/swagger.json`.
3. Click **Import**.

Once the API definition is imported, ZAP will display all available endpoints and parameters in the **Sites tab**.

## Scanning the API

After importing the API definition:
1. ZAP will automatically perform passive scanning on the API endpoints.
2. To run an active scan against the API, right-click on its URL in the **Sites tab** and select **Attack -> Active Scan**.

ZAP will then scan the API and identify potential vulnerabilities. You can review the scan results and use them to address security issues in the API.

### Final Notes
- Once the scan is complete, vulnerabilities will be reported in the **Alerts** tab.
- The API is treated as any other web application by ZAP, and you can use the same scanning tools to analyze it.

### What high-risk vulnerability was found on the /asciiart/generate endpoint?
Remote OS Command Injection

### Read the details on the Path Traversal vulnerability detected. Based solely on the information presented by the scanner, would you categorise this finding as a false positive? (yea/nay)
Yea

# Task 7 : Integrating DAST into the development pipeline

## Implementing DAST in the Development Pipeline

**Dynamic Application Security Testing (DAST)** is typically used to automate vulnerability scanning within the development pipeline, in contrast to manual penetration testing. Implementing DAST involves several considerations:

- **Deciding when to run scans**: Choose at which stages of development scans will be run.
- **Determining triggers**: Scans can be triggered by each commit or on a scheduled basis.
- **Choosing scan intensity**: Full vulnerability scans can be time-consuming, so lighter scans might be preferred to avoid slowing down development.

To integrate DAST into the development pipeline without disrupting the development team's workflow, collaboration with the development team is crucial.

## Review of the CI/CD Pipeline

Our development pipeline for the web application and API consists of two main components:
- **Gitea Repository**: The code for both applications is stored on [http://10.10.141.230:3000/](http://10.10.141.230:3000/).
- **Jenkins CI/CD Instance**: Jenkins is responsible for building and deploying the code. You can access Jenkins at [http://10.10.141.230:8080/](http://10.10.141.230:8080/).

### How It Works:
- Each time a commit is made in Gitea, Jenkins retrieves the code and builds it into a Docker instance.
- The built Docker instances are the ones that have been scanned.

You can use the following credentials for Gitea or Jenkins:
- **Username**: thm
- **Password**: thm

## Integrating ZAP into the Pipeline

To integrate ZAP into the pipeline, we use **zap2docker**, a dockerized version of ZAP designed for automation. ZAP will run automated scans on each commit.

### Steps:
1. **Explore Jenkins**: 
   - Upon logging into Jenkins, you will find the **thm** organization with repositories for both the web application and API.
   - Each repository contains a branch called `main` with two stages:
     - **Build the Docker image**: This builds the Docker container using the `Dockerfile`.
     - **Deploy the Docker image**: This deploys the container.

2. **Add ZAP Stage**:
   - The `Jenkinsfile` for the web application defines the build and deploy stages. To integrate ZAP, add a third stage for running the scan.
   - The scan is configured as a **Baseline Scan** to minimize pipeline delays. This scan checks for common, easily discoverable vulnerabilities (e.g., missing security headers, insecure cookies) but does not perform heavy scans like SQL Injection or XSS, which require more time.

3. **Test the Pipeline**:
   - After modifying the `Jenkinsfile`, commit the changes and trigger a build. The **Scan with OWASP ZAP** stage will run after the Docker image is built.
   - The scan will take around **5 minutes**, and it is expected that the build will fail due to discovered vulnerabilities.
   - To view the scan results, open the build in Jenkins, navigate to **Workspaces**, and download the report from the **zap-reports** folder.

> **Note**: Right-click to save the report before opening it to avoid formatting issues.

4. **Repeat for the API**: 
   - Follow the same process to enable ZAP scanning for the **simple-api** repository.
   - Use the generated report to analyze and answer the questions at the end of the task.

By integrating DAST into the pipeline, you can quickly detect vulnerabilities during development, ensuring early visibility without disrupting the development process.

## Running Automated Scans With zap2docker

To integrate ZAP in our pipeline, we will use zap2docker, a dockerized version of ZAP proxy built with automation as its primary purpose. The full documentation for zap2docker can be found here.

Note: The commands in this section are provided for reference only. You don't need to run them manually, as we will have Jenkins do it for us later.

To install zap2docker, you can pull it from Docker Hub using the following command:

```bash
docker pull owasp/zap2docker-stable
```
You can run ZAP from the docker instance using one of the packaged scan scripts, which will allow you to run one of the following scan profiles:

Baseline Scan: ZAP will spider the target website for a maximum time of 1 minute. No active scan will be performed. You can optionally run an AJAX spider if desired.

```bash
docker run -t owasp/zap2docker-stable zap-baseline.py -t https://www.example.com
```
Full Scan: ZAP will run a spider with no time limits, followed by an active scan.

```bash
docker run -t owasp/zap2docker-stable zap-full-scan.py -t https://www.example.com
```
API Scan: ZAP will perform an active scan against an API. You will need to provide a URL to an API description file (OpenAPI, GraphQL or SOAP).
```bash
docker run -t owasp/zap2docker-stable zap-api-scan.py -t https://www.example.com/swagger.json -f openapi
```
In any of the scans, the results for passive scans will be limited to 10 alerts. In addition to the base commands, you can add the -j switch to perform an AJAX scan on the baseline and full scans.

# Task 8 : Conclusion

In this room, we have covered the basics of how DAST works and introduced ZAP proxy as a tool that can perform DAST in manual and automated ways. DAST is only one of many ways to check for applications' vulnerabilities and should be used in tandem with other types of testing like SAST, SCA, penetration tests and others to guarantee a reasonable security level for our applications. As with any other technique, DAST won't be a silver bullet solution but will contribute to an application's overall security throughout the software development lifecycle.