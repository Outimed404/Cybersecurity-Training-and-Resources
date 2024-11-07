# Task 1 : Introduction
In this room, we will look at Static Application Security Testing (SAST), one of the many tools used to detect vulnerabilities during the development lifecycle of an application. We will study its advantages and disadvantages and how it complements other methods to make apps secure early in development.

## Learning Objectives

- **Learn how SAST can be applied** to find weaknesses in real-world applications.
- **Understand the pros and cons** of using SAST.
- **Familiarise yourself with how SAST** can be implemented in a development lifecycle.

## Room Prerequisites

Before continuing with this room, it is important to have familiarity with:
- **Web vulnerabilities**, as shown in the OWASP Top 10 and the OWASP API Top 10 rooms.
- **Basic knowledge of the stages of the software development lifecycle (SDLC)**. Refer to the SDLC room if needed.

# Task 2 : Code Review

## Code Review for Secure Application Development

One of the methods used for developing secure applications is frequently testing the code for security bugs through a process known as **code review**. Code reviews involve examining the source code of an application to search for vulnerabilities using a white box approach. This method allows the reviewer to achieve greater coverage of the application's functionalities and reduces the time needed to find and fix bugs.

## Importance of Early Detection

If vulnerabilities introduced early in the development process are left unattended, they can propagate to the later stages of a project, making resolution more labor-intensive and costly. Code reviews aim to identify these vulnerabilities early when they are easier and cheaper to fix.

### Cost of Defects in Software

Resolving defects at an early stage is significantly less costly than addressing them at the end of a project. Early detection reduces the potential for expensive rework and mitigates security risks before they become critical.

## Manual vs Automated Code Review

**Code reviews** are often conducted using a combination of manual analysis and automated tools to achieve the best results. Each approach has its own benefits and considerations:

### Manual Code Review
- **Advantages**:
  - In-depth analysis performed by a human reviewer.
  - Greater precision in identifying complex vulnerabilities.
- **Disadvantages**:
  - Time-consuming, especially with large codebases.
  - Prone to human fatigue, which may result in missed issues.
  - Higher cost due to the extensive time required by reviewers.

### Automated Code Review
- **Advantages**:
  - Quick identification of common vulnerabilities.
  - Consistent performance regardless of codebase size.
  - Lower cost and faster execution compared to manual reviews.
- **Disadvantages**:
  - Limited to predefined rules; may miss vulnerabilities that don't match its configuration.
  - Less capable of identifying complex vulnerabilities that require human insight.

## Optimal Code Review Strategy

To achieve the best results:
- Run **automated tools** early in the development lifecycle to catch common vulnerabilities quickly and at a lower cost.
- Schedule **manual code reviews** periodically or at significant project milestones to address complex vulnerabilities that automated tools may overlook.

Combining both approaches helps ensure comprehensive security coverage and early defect resolution, ultimately resulting in a more secure and robust application.


### Are automated code reviews a substitute for manual reviewing? (yea/nay)
nay

### What type of code review will run faster? (Manual/Automated)
Automated

### What type of code review will be more thorough? (Manual/Automated)
Manual

# Task 3 : Manual Code Review

Before using Static Application Security Testing (SAST) tools, it’s important to first understand how manual code review is performed. This will help to see how these tools analyze code.

In this task, the goal is to find SQL injection vulnerabilities in a simple web application. While there are other vulnerabilities in the application, the focus is on SQL injections to demonstrate how traditional code reviews are done and how those steps can map to the analysis techniques used by SAST tools.

## Task Setup

1. **Setting Up the Virtual Machine (VM):**  
   - Before starting, ensure the VM is running by pressing the **Start Machine** button. The VM should be in split view, but if it’s not visible, click **Show Split View** at the top right.

2. **Accessing the Codebase:**  
   - The code used in this task can be found in the directory: `/home/ubuntu/Desktop/simple-webapp/`.

## Searching for Insecure Functions

The first step in a manual code review is identifying insecure functions that could lead to vulnerabilities. Since the goal is to find SQL injection issues, we focus on functions that send raw SQL queries to the database. In PHP and MySQL, typical functions used for this are:

| Database Engine | Function            |
|-----------------|---------------------|
| MySQL           | `mysqli_query()`     |
|                 | `mysql_query()`      |
|                 | `mysqli_prepare()`   |
|                 | `query()`            |
|                 | `prepare()`          |

A practical approach for searching these functions is using the `grep` command. For instance, to search for instances of `mysqli_query()`, you can run the following command in the project's base directory:

```bash
user@machine$ cd /home/ubuntu/Desktop/simple-webapp/html/
user@machine$ grep -r -n 'mysqli_query('
db.php:18: $result = mysqli_query($conn, $query);
```
The `-r` option tells grep to recursively search all files under the current directory, and the `-n` option indicates that we want grep to tell us the number of the line where the pattern was found. In the above terminal output, we can see that a single instance of `mysqli_query()` was found on line `18`.

We have identified a line that could lead to SQL injections. However, just by looking at that line, we can't determine whether a vulnerability is present.

## Understanding the Context

Let's open `db.php` to analyse the context around the `mysqli_query()` function to see if we get a better idea of how it is used:

```js
function db_query($conn, $query){
    $result = mysqli_query($conn, $query);
    return $result;
}
```

Here we can see that `mysqli_query()` is wrapped into the `db_query()` function, and that the `$query` parameter is passed directly without modification. It is very common for functions to be nested into other functions, so simply analysing the local context of a function is sometimes not enough to determine if a vulnerability is present. We now need to trace the uses of the `db_query()` function throughout our code to identify potential vulnerabilities.

## Tracing User Inputs to Potentially Vulnerable Functions

We can use grep again to search for uses of `db_query()`:
Linux

```bash
user@machine$ grep -rn 'db_query('
    hidden-panel.php:7:$result = db_query($conn, $sql);
    hidden-panel.php:20:$result2 = db_query($conn, $sql2);
    hidden-panel.php:23:$result3 = db_query($conn, $sql3);
    db.php:17:function db_query($conn, $query){
```
        

Here we get three uses for `db_query()` on `hidden-panel.php`. Once again, we can analyse the context of each call, starting with the call on line 7:

```php
$sql = "SELECT id, firstname, lastname FROM MyGuests WHERE id=".$_GET['guest_id'];
$result = db_query($conn, $sql);
```

Congrats on finding an SQL injection! Whatever is passed in the `guest_id` parameter via the GET method will be concatenated to a raw SQL query without any input sanitisation, enabling the attacker to change the query.

If we do a similar process for the other two uses of `db_query()`, we will have the following code:

```php
$sql2 = "SELECT id, logtext FROM logs WHERE id='".preg_replace('/[^a-z0-9A-Z"]/', "", $_GET['log_id']). "'";
$result2 = db_query($conn, $sql2);
```

```php
$sql3 = "SELECT id, name FROM asciiart WHERE id=".preg_replace("/[^0-9]/", "", $_GET['art_id'], 1);
$result3 = db_query($conn, $sql3);
```
Again, we have some GET parameters being concatenated into SQL queries. This would give us the impression that we have two more vulnerabilities identified. Still, in both cases, the GET parameters are sanitised through `preg_replace()` using different regular expressions to replace any character that isn't allowed with an empty string. To decide if these lines of code are vulnerable, we will need to evaluate if the filters in place allow SQL injections to pass.

The query on `$sql2` is passed through a filter that only allows characters that are either alphanumeric or double-quotes. While double-quotes may seem like a possible vector for SQLi, they don't pose a threat in this case since the SQL sentence encloses the string passed through `$_GET['log_id']` between single-quotes ('). Since an attacker would have no way to escape from the string in the SQL sentence, we can be sure that this line isn't vulnerable.

The query on `$sql3` is even more restrictive, allowing only numeric characters to be passed through `$_GET['art_id']`. However, notice that the `preg_replace()` function is called with a third parameter set to "1". If we refer to the manual page of the function, we will learn that the third parameter indicates the maximum number of replacements to be done. Since it is set to 1, only the first character that isn't a number will be replaced with an empty string. Any other characters will pass without being replaced, enabling SQL injections. This line is indeed vulnerable.

## Enough Manual Reviewing

We have used manual code reviewing to identify two SQL injection points in our application. While the example application is quite straightforward, a real application has many more lines of code. Tracing each instance of a potentially vulnerable function will be way more complicated. By now, it should be evident that manually reviewing large code bases can quickly become a tedious task. We will now move onto using SAST tools to perform the same analysis for us and analyse how well they perform.

## Bonus Exercise

Use manual code reviewing to find other types of vulnerabilities in the provided project. The following questions will guide you through it.

Local File Inclusion (LFI) attacks are made possible by the misuse of one of the following functions in PHP:
require()	include()	require_once()	include_once()

Answer the following questions using grep to search for LFI vulnerabilities only on the .php files in the html/ directory of the simple-webapp project.

### Which of the mentioned functions is used in the project? (Include the parenthesis at the end of the function name)
include()


### How many instances of the function found in question 2 exist in your project's code?
9

### What file contains the vulnerable instance?
view.php

### What line in the file found on the previous question is vulnerable to LFI?
22

# Task 4 : Automated Code Review

## Static Application Security Testing(SAST)

Static Application Security Testing (SAST) refers to using automated tools to analyze source code for vulnerabilities. It is designed to complement manual code reviews and other testing techniques like Dynamic Application Security Testing (DAST) and Software Composition Analysis (SCA), forming a holistic approach to application security during development.

While SAST tools help identify vulnerabilities early in the development process, they have their strengths and limitations.

## Pros of SAST

- Does not require a running instance of the application.
- Provides great coverage of the application's functionality.
- Runs faster than dynamic techniques.
- Reports exact locations of vulnerabilities in the code.
- Can easily be integrated into CI/CD pipelines.

## Cons of SAST

- The source code may not always be available, especially for third-party applications.
- Prone to false positives.
- Cannot identify vulnerabilities that are dynamic in nature.
- Mostly language-specific; tools can only check languages they understand.

## SAST Under the Hood

Most SAST tools perform two main tasks:

1. **Transform the Code into an Abstract Model:**  
   Tools ingest the source code and convert it into an abstract representation (e.g., Abstract Syntax Trees, AST) for further analysis. This step is crucial for ensuring the analysis is accurate and language-independent.
   
2. **Analyze the Abstract Model for Security Issues:**  
   Once the code is represented abstractly, various analysis techniques are applied to detect vulnerabilities.

In this section, we will focus on common analysis techniques used by SAST tools, which include:

### Semantic Analysis

Semantic analysis identifies insecure functions used in potentially vulnerable contexts, similar to how manual code reviews search for insecure functions. For example, it looks for functions like `mysqli_query()` where user input (such as `$_GET` or `$_POST` parameters) is directly concatenated into SQL queries.

### Dataflow Analysis

Dataflow analysis traces how data flows through the code, from user inputs (sources) to vulnerable functions (sinks). If data from a source reaches a sink without proper sanitization, a vulnerability exists. For instance, a `db_query()` function may be safe or unsafe depending on whether user input is sanitized before being passed to functions like `mysqli_query()`.

### Control Flow Analysis

Control flow analysis focuses on the order of operations in the code, looking for issues like race conditions, use of uninitialized variables, or resource leaks. For example, a piece of code may throw an exception if it tries to operate on a variable that hasn’t been initialized.

### Structural Analysis

Structural analysis checks whether the code follows best practices for the specific programming language. It may look for issues like dead code, improper use of cryptographic functions (e.g., weak encryption keys), or misuse of exception handling blocks.

### Configuration Analysis

Configuration analysis focuses on potential flaws in the application's configuration files, rather than the source code itself. It checks for insecure configuration settings that may open the application to attack. For example, certain PHP settings like `allow_url_include` or `allow_url_fopen` may raise flags for vulnerabilities like Remote File Inclusion (RFI) or Server-Side Request Forgery (SSRF).

## Conclusion

Not all SAST tools implement every analysis technique discussed here, but having an understanding of these techniques allows you to better evaluate and compare different tools in the market.

### Does SAST require a running instance of the application for analysis? (yea/nay)
nay

### What kind of analysis would likely flag dead code segments?
structual analysis

### What kind of analysis would likely detect flaws in configuration files?
configuration analysis

### What kind of analysis is similar to grepping the code in search of flaws?
semantic analysis

# Task 5 : Rechecking our Application with SAST Tools

Now that we know how SAST tools work, we are ready to use a couple of them. We will use a simple PHP application as our target for this task. You can find the application's code in the simple-webapp folder on your desktop.

## Rechecking our Application with Psalm

We will first use Psalm (PHP Static Analysis Linting Machine), a simple tool for analysing PHP code. The tool is already installed as part of the application's project, so you won't need to install it, but you can find installation instructions on Psalm's online documentation if required.

Open a terminal and go to the project's directory. You will find a psalm.xml file at the root of the project. This is Psalm's configuration file, and it should look like this:

``` xml
<?xml version="1.0"?>
<psalm
    errorLevel="3"
    resolveFromConfigFile="true"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="https://getpsalm.org/schema/config"
    xsi:schemaLocation="https://getpsalm.org/schema/config vendor/vimeo/psalm/config.xsd"
    findUnusedBaselineEntry="true"
>
    <projectFiles>
        <directory name="html/" />
        <ignoreFiles>
            <directory name="vendor" />
        </ignoreFiles>
    </projectFiles>
</psalm>
```

We won't go too deep into the configuration options, but here you can see the errorLevel parameter, which indicates how strict Psalm will be when reporting issues. The lower the value, the more issues will be reported. There's also a section called projectFiles, where the files to be scanned are indicated. Only the html directory will be scanned for this project, and the vendor directory will be ignored (as we don't want to test third-party dependencies).

With the configuration file set, you can run Psalm using the following command from within the project's directory:
Linux
```bash
user@machine$ cd /home/ubuntu/Desktop/simple-webapp/
user@machine$ ./vendor/bin/psalm --no-cache


ERROR: TypeDoesNotContainType - html/hidden-panel.php:10:5 - Operand of type 0 is always falsy (see https://psalm.dev/056)
  if ($result->num_rows = 0) {
```
        

By default, Psalm will only run some structural analysis over the code and show us programming errors that need our attention. In the previous example, Psalm reports that the condition on the if clause is always false. In this case, the programmer mistakenly used the assignment operator (=) instead of the comparison operator (==). Since the assignment operator always returns the assigned value (0 in this case), the condition will always evaluate as false (0 will be automatically cast to false in PHP).

The default tests will search for issues with variable types, variable initialisation and other safe coding patterns. While these issues are not vulnerabilities, Psalm will make recommendations so your code follows coding best practices, which is a good start to avoid runtime errors.

Psalm also offers the possibility to run dataflow analysis on our code using the --taint-analysis flag. The output for this command will be much more interesting as it will pinpoint possible security issues. The output of the command will look like this:
Linux

```bash
user@machine$ cd /home/ubuntu/Desktop/simple-webapp/
user@machine$ ./vendor/bin/psalm --no-cache --taint-analysis

ERROR: TaintedInclude - html/view.php:22:9 - Detected tainted code passed to include or similar (see https://psalm.dev/251)
  $_GET
    <no known location>

  $_GET['img'] - html/view.php:22:28
include('./gallery-files/'.$_GET['img']);

  concat - html/view.php:22:9
include('./gallery-files/'.$_GET['img']);
```
        

For each error reported, Psalm will show you a complete trace of the data flow from the source ($_GET) to the sink function (include()). Here we have a typical Local File Inclusion (LFI) vulnerability, as the GET parameter img is concatenated directly into the filename passed to the include() function.

Dataflow analysis usually gets the most interesting findings from a security standpoint. However, other structural findings are equally important to fix, even if they don't directly translate into an exploitable vulnerability. Structural flaws can often lead to business logic errors that may be hard to track or some unpredictable behaviours in your app.

Dealing With False Positives and False Negatives

No matter what SAST tool you use, errors will always be present. Just as a quick reminder, we will generally be concerned with the two following error types:

- False positives: The tool reports on a vulnerability that isn't present in the code.
- False negatives: The tool doesn't report a vulnerability that is present in the code.

These errors might present themselves because the tool cannot correctly assess the target code, but they can also be introduced if we, as users, don't use the tool correctly.

As a quick example, when we manually reviewed the code before, we found three possible instances of SQL injection. Still, we discarded one of them as character filtering was being applied to it effectively, leaving us with two confirmed SQL injection vulnerabilities. If we check Psalm's output, we will notice only a single instance of SQL injection is reported (the first one in lines 6-7 of hidden-panel.php):
Linux
```bash

user@machine$ ./vendor/bin/psalm --no-cache --taint-analysis

ERROR: TaintedSql - html/db.php:18:32 - Detected tainted SQL (see https://psalm.dev/244)
  $_GET
    <no known location>

  $_GET['guest_id'] - html/hidden-panel.php:6:65
$sql = "SELECT id, firstname, lastname FROM MyGuests WHERE id=".$_GET['guest_id'];

  concat - html/hidden-panel.php:6:8
$sql = "SELECT id, firstname, lastname FROM MyGuests WHERE id=".$_GET['guest_id'];
```

        

Just as an experiment, let's comment out lines 6 and 7 in hidden-pannel.php and see what happens:
```bash
// $sql = "SELECT id, firstname, lastname FROM MyGuests WHERE id=".$_GET['guest_id'];
// $result = db_query($conn, $sql);
```
In theory, we should have no more errors reporting TaintedSQL instances. However, if we execute Psalm once again, it will now report a different TaintedSQL error corresponding to one of the other two SQL queries:
Linux
```bash
user@machine$ ./vendor/bin/psalm --no-cache --taint-analysis

ERROR: TaintedSql - html/db.php:18:32 - Detected tainted SQL (see https://psalm.dev/244)
  $_GET
    <no known location>

  $_GET['log_id'] - html/hidden-panel.php:19:87
$sql2 = "SELECT id, logtext FROM logs WHERE id='".preg_replace('/[^a-z0-9A-Z"]/', "", $_GET['log_id']). "'";

  call to preg_replace - html/hidden-panel.php:19:87
$sql2 = "SELECT id, logtext FROM logs WHERE id='".preg_replace('/[^a-z0-9A-Z"]/', "", $_GET['log_id']). "'";
```
        

By now, you are probably wondering why Psalm didn't detect this vulnerability the first time. The reason for this can be found if you check the first line of both errors. Both show line 18 of db.php as the source of the problem. For Psalm, both vulnerabilities are the same and related to how you call mysqli_query() on line 18 of db.php. In other words, Psalm expects you to apply a fix there, as it doesn't understand that the code defines db_query() as a wrapper to any database queries.

Before continuing, make sure to uncomment back lines 6-7 in hidden-panel.php:
```php
$sql = "SELECT id, firstname, lastname FROM MyGuests WHERE id=".$_GET['guest_id'];
$result = db_query($conn, $sql);
```
To better help Psalm understand the situation, we can use some annotations to give more context. We can specify that db_query() should be considered a sink, so we get an error if any tainted input reaches the function via the $query parameter. To do so, add the following comment on top of the db_query() function definition in db.php:

```php
/**
 * @psalm-taint-sink sql $query
 * @psalm-taint-specialize
 */
function db_query($conn, $query){
    $result = mysqli_query($conn, $query);
    return $result;
}
```
The @psalm-taint-sink annotation tells Psalm to check the $query parameter for tainted inputs of type sql. Now Psalm will issue a TaintedSQL error every time a tainted input reaches db_query().

We also need to add the @psalm-taint-specialize annotation to tell Psalm that each invocation of the function should be treated as a separate issue and that the function's taintedness depends entirely on the inputs it receives. This way, we will get both issues separately.

After re-running Psalm, we should now get all instances of TaintedSQL errors as expected:
Linux
```bash
user@machine$ ./vendor/bin/psalm --no-cache --taint-analysis

ERROR: TaintedSql - html/db.php:21:26 - Detected tainted SQL (see https://psalm.dev/244)
  $_GET
    <no known location>

  $_GET['guest_id'] - html/hidden-panel.php:6:65
$sql = "SELECT id, firstname, lastname FROM MyGuests WHERE id=".$_GET['guest_id'];

  concat - html/hidden-panel.php:6:8
$sql = "SELECT id, firstname, lastname FROM MyGuests WHERE id=".$_GET['guest_id'];

  $sql - html/hidden-panel.php:6:1
$sql = "SELECT id, firstname, lastname FROM MyGuests WHERE id=".$_GET['guest_id'];

  call to db_query - html/hidden-panel.php:7:27
$result = db_query($conn, $sql);

ERROR: TaintedSql - html/db.php:21:26 - Detected tainted SQL (see https://psalm.dev/244)
  $_GET
    <no known location>

  $_GET['log_id'] - html/hidden-panel.php:19:87
$sql2 = "SELECT id, logtext FROM logs WHERE id='".preg_replace('/[^a-z0-9A-Z"]/', "", $_GET['log_id']). "'";

  call to preg_replace - html/hidden-panel.php:19:87
$sql2 = "SELECT id, logtext FROM logs WHERE id='".preg_replace('/[^a-z0-9A-Z"]/', "", $_GET['log_id']). "'";

  preg_replace#3 - html/hidden-panel.php:19:87
$sql2 = "SELECT id, logtext FROM logs WHERE id='".preg_replace('/[^a-z0-9A-Z"]/', "", $_GET['log_id']). "'";

  preg_replace - vendor/vimeo/psalm/stubs/CoreGenericFunctions.phpstub:1182:10
function preg_replace($pattern, $replacement, $subject, int $limit = -1, &$count = null) {}

  concat - html/hidden-panel.php:19:9
$sql2 = "SELECT id, logtext FROM logs WHERE id='".preg_replace('/[^a-z0-9A-Z"]/', "", $_GET['log_id']). "'";

  concat - html/hidden-panel.php:19:9
$sql2 = "SELECT id, logtext FROM logs WHERE id='".preg_replace('/[^a-z0-9A-Z"]/', "", $_GET['log_id']). "'";

  $sql2 - html/hidden-panel.php:19:1
$sql2 = "SELECT id, logtext FROM logs WHERE id='".preg_replace('/[^a-z0-9A-Z"]/', "", $_GET['log_id']). "'";

  call to db_query - html/hidden-panel.php:20:28
$result2 = db_query($conn, $sql2);

        
```
- Note: You will get a third error repeating one of the previous ones. This is because mysqli_query is still considered a valid sink (Psalm's has a built-in list of default sinks), so you should still have the original alert on top of the new ones.

Finally, if you compare Psalm's results with our manual review, you should notice that there are some differences in the results:
| Code  | Manual Review  | Psalm Review  | Verdict       |
|-------|----------------|---------------|---------------|
| $sql  | Vulnerable     | Vulnerable    | OK            |
| $sql2 | Not Vulnerable | Vulnerable    | False Positive|
| $sql3 | Vulnerable     | Not Vulnerable| False Negative|

A certain level of false positives and false negatives is always expected when using SAST tools. As with any other tool, you must manually check the report to search for any errors. SAST is an excellent complement to manual reviews but should never be considered as their replacement.

### What type of error occurs when the tool reports on a vulnerability that isn't present in the code?
false positive

### How many errors are reported after annotating the code as instructed in this task and re-running Psalm?
9

# Task 6 : SAST in the Development Cycle
![sast](https://github.com/user-attachments/assets/72e5e3d1-f0bb-4ce2-a8f4-8d47b90061e1)



SAST is one of the first tools to appear during the development lifecycle. It is often implemented during the coding stage, as there's no need to have a functional application to use it.

## SAST in the Development Cycle

Depending on each case, SAST can be implemented in one of the following ways:

- **CI/CD integration**: Each time a pull request or a merge is made, SAST tools will check the code for vulnerabilities. Checking pull requests ensures that the code that makes it to merges has undergone at least a basic security check. On some occasions, instead of checking every single pull request, executing SAST scans only at merges may help prevent slowdowns in the development pipeline, as it avoids making all developers wait for full SAST scans.
- **IDE integration**: SAST tools can be integrated into the developers' favourite IDEs instead of waiting for a pull request or merge to occur. This way, the code can be fixed as early as possible, saving time further ahead in the project.

Note that you may want SAST running on both the developers' IDEs and your CI/CD pipeline, which would be valid. For example, IDE tools may run basic structural checks and enforce secure coding guidelines, while CI/CD integrations may run more time-consuming dataflow/taint analysis. Remember that your specific setup will depend on the requirements of each project and should be evaluated by the team carefully.

If you want to learn how security tools can be integrated into a CI/CD pipeline, check the DAST room for an example of integrating a DAST tool into a Jenkins pipeline. The process with SAST tools is pretty similar, so we won't repeat it here.

## Integrating SAST in IDEs

Your virtual machine has the VS Code editor installed and already configured to use a couple of SAST tools:

- **Psalm**: The tool we've been using supports IDE integration by installing the Psalm plugin into VS Code directly from the Visual Studio Marketplace. This plugin will check anything you type in real-time and show you the same alerts as the console version directly into your code. Taint analysis won't be available, so you'll only see the structural problems reported on a basic Psalm analysis.

- **Semgrep**: Yet another SAST tool that can be installed into VS Code directly from the Visual Studio Marketplace. Just as with Psalm, it will show inline alerts directly in your code. Semgrep even allows us to build custom rules if needed. You can check the rules that are loaded for this project on the `semgrep-rules` directory inside the project's directory. The specifics on how to build Semgrep rules will be left for a future room, so don't worry about them now. You can check the rules in the directory, but this won't be required for this room.

Both tools will work similarly by showing any problems detected directly inline in your code. The only notable difference is that Semgrep will run when you start VS Code and will show you the problems it detects on all of the files, while Psalm will only show you issues related to the file you are currently editing. The plugins will add the following information to your IDE screen:

### SAST in VS Code

For each line where issues are detected, you can get additional information by hovering the mouse pointer over it. Here's what you would get by hovering over line 46 of `login.php`:

#### Inline Problems in VS Code

You can also check the complete list of issues by clicking the Errors indicator at the bottom of VS Code's screen:

#### Problems Panel

Having your IDE check for problems allows you to produce cleaner code as a developer, contributing significantly to the overall security of your final application.

Using both tools in your IDE, answer the questions at the end of the task.

### How many problems in total are detected by Semgrep in this project?
27

### How many problems are detected in the showrecipe.inc.php file?
8

Open showrecipe.inc.php. There are two types of problems being reported by Semgrep in this file. One is identified as "tainted-sql-string" and refers to possible SQL injections.

### What other problem identifier is reported by Semgrep in this file? (Write the id reported by Semgrep)
echoed-request

### What type of vulnerability is associated with the problem identifier on the previous question?
cross-site scripting
