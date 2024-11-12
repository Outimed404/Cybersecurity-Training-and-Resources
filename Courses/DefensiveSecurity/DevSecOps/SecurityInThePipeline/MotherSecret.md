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
![Mother1](https://github.com/user-attachments/assets/3258cc71-6732-4093-9531-56af4b971808)

![Mother2](https://github.com/user-attachments/assets/abd72ab9-e154-4191-95cc-e104e36004f3)


937
### What is the hidden flag in the Nostromo route ?

![Mother3](https://github.com/user-attachments/assets/7b13cf61-479a-4c8f-9575-8d4ad25c4611)


Flag{X3n0M0Rph}
### What is the name of the Science Officier with permissions ?

We can unclock the name of the Science Officier if we are staying on the home page and performing the two past api called in a limited timed. 

![Mother4](https://github.com/user-attachments/assets/68c1041b-9e2c-4e93-9aa7-c117bd55b93f)


Ash
### What are the contents of the classified "Flag" box ?
For this question I found two ways to get the flag. 
The first one is on the webpage :

![Mother5 1](https://github.com/user-attachments/assets/ac7dae7a-572b-47d5-8e3c-8cba63f4ff1e)


The second one is looking a bit closer to the javascript code located on the page:
![Mother5 2 1](https://github.com/user-attachments/assets/dce068f0-fa9f-40f8-90d6-30d2a8ac22db)
![Mother5 2 2](https://github.com/user-attachments/assets/26d08977-7755-41f5-81d5-37259c030170)
![Mother5 2 3](https://github.com/user-attachments/assets/7f4c64c8-c477-4106-8a1b-0252391fbbcf)



THM_FLAG{0RD3R_937}

### Where is Mother's secret ?
To find the location of the Mother's secret, I made an api call using burp:

![Mother6](https://github.com/user-attachments/assets/3af47902-5857-439f-a848-5927dbb71ad2)

/opt/m0th3r

### What is Mother's secret ?
After finding the location, I edited the api request and I get the flag in response:
![Mother7](https://github.com/user-attachments/assets/99fb4371-4282-4bb6-9257-d1fbbc265a1b)

Flag{Ensure_return_of_organism_meow_meow!}
