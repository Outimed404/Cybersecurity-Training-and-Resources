# Introduction to TryHackMe Cargo Star Ship (THMCSS) Nostromo Challenge
Having a strong background in analyzing code and web application security will help you succeed in this mission.

## Challenge Title: Mother's Secret

## Platform
TryHackMe

## Difficulty
Easy

## Description
In this challenge, you will be investigating the **TryHackMe Cargo Star Ship (THMCSS) Nostromo**, owned by the **Weyland-TryHackMe Corps**, and its compromised computer system, **MU-TH-UR 6000**. Your objective is to uncover hidden secrets by exploiting vulnerabilities in the web application running on the **Nostromo server**. This challenge will test your **code analysis** and **exploitation** skills as you dive deep into identifying and exploiting vulnerabilities.

## Previous Experience

Before attempting this challenge, it is recommended to have completed the following rooms from the **DevSecOps path**:
- **SAST** (Static Application Security Testing)
- **DAST** (Dynamic Application Security Testing)

## Tools and Techniques Used

- **Burp Suite**: Utilized as an interception proxy to analyze and manipulate HTTP requests. It allowed detailed inspection of the request and response data, especially for API endpoint interactions and tracking HTTP POST data changes. 

- **Base64**: Used to decode encrypted and encoded data returned from the API endpoints. This allowed access to invite codes and further instructions embedded in the server responses.

## Step-by-Step Solution
**IP Addresses:**
- My IP address: 10.10.10.20
- Target machine IP address: 10.10.140.128

## Equipment Check

### Download the files attached to this task to review the code. 

    Explore the available endpoints of the Mother Server and try to find any clues that can reveal mother's secret.
    Search for a file that contains essential information about the ship's activities.
    Exploit the vulnerable code to download the secrets from the server. Can you spot the vulnerable code?
    Capture all the hidden flags you encounter during your exploration. Only Mother holds this secret. 

## Operating Manual

### Below are some sequences and operations to get you started. Use the following to unlock information and navigate Mother:

    Emergency command override is 100375. Use it when accessing Alien Loaders. 
    Download the task files to learn about Mother's routes.
    Hitting the routes in the right order makes Mother confused, it might think you are a Science Officer!

Can you guess what is /api/nostromo/mother/secret.txt?

### What is the number of the emergency command override?
On the text above, we got a clue "emergency command override" is 100375.
100375
### What is the special order number?
After some analysis on the code given for this challenge, we can found a way to call the api:

image1
image2

937
### What is the hidden flag in the Nostromo route ?

image3
Flag{X3n0M0Rph}
### What is the name of the Science Officier with permissions ?

We can unclock the name of the Science Officier if we are staying on the home page and performing the two past api called in a limited timed. 

image4
Ash
### What are the contents of the classified "Flag" box ?
For this question I found two ways to get the flag. 
The first one is on the webpage :
image 5.1

The second one is looking a bit closer to the javascript code located on the page:
image 5.2.1
5.2.2
5.2.3

THM_FLAG{0RD3R_937}

### Where is Mother's secret ?
To find the location of the Mother's secret, I made an api call using burp:

image 6

### What is Mother's secret ?
After finding the location, I edited the api request and I get the flag in response:
image7