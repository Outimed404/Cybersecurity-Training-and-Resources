
# TwoMillion CTF Write-Up

# Challenge Title: TwoMillion

## Platform
Hack The Box

## Difficulty
Easy

## Description
TwoMillion is an Easy difficulty Linux box that was released to celebrate reaching 2 million users on HackTheBox. The box features an old version of the HackTheBox platform that includes the old hackable invite code. After hacking the invite code an account can be created on the platform. The account can be used to enumerate various API endpoints, one of which can be used to elevate the user to an Administrator. With administrative access the user can perform a command injection in the admin VPN generation endpoint thus gaining a system shell. An .env file is found to contain database credentials and owed to password re-use the attackers can login as user admin on the box. The system kernel is found to be outdated and CVE-2023-0386 can be used to gain a root shell.

## Tools and Techniques Used

- **Nmap**: Used to scan the target machine for open ports and services.

- **Browser Developer Tools**: Employed to analyze and inspect the JavaScript code on the `/invite` page, identifying the JavaScript file `inviteapi.min.js` and functions like `makeInviteCode` that provided insight into generating invite codes.

- **Burp Suite**: Utilized as an interception proxy to analyze and manipulate HTTP requests. It allowed detailed inspection of the request and response data, especially for API endpoint interactions and tracking HTTP POST data changes. This helped identify and decode invite codes, as well as interact with the `/api/v1/admin` endpoints to escalate privileges.

- **Base64 and ROT13 Decoding**: Used to decode encrypted and encoded data returned from the API endpoints. This allowed access to invite codes and further instructions embedded in the server responses.

- **Command Injection Exploitation**: Performed on the `/api/v1/admin/vpn/generate` endpoint to execute arbitrary commands on the server, leading to initial system access.

- **Environment (.env) File Enumeration**: The `.env` file on the server was examined for sensitive data like database credentials, which facilitated further privilege escalation by enabling SSH login as the `admin` user.

- **Kernel Exploitation**: Leveraged CVE-2023-0386, a vulnerability in the Linux OverlayFS, to escalate privileges from `admin` to `root` by maintaining metadata (such as SetUID bits) on moved files, providing root-level access on the target.

- **SSH**: After retrieving credentials, SSH was used to establish a connection to the target machine as the `admin` user, furthering exploitation efforts and allowing for the final privilege escalation steps to achieve root access.

## Step-by-Step Solution
**IP Addresses:**
- My IP address: 10.10.15.11
- Target machine IP address: 10.10.10.245

## Task 1: How many TCP ports are open ? 

2

![2MNMAP](https://github.com/user-attachments/assets/fba6ba34-882f-4596-be35-51211b21eac3)
![2MEditingETCHOST](https://github.com/user-attachments/assets/cd001967-f5ac-499f-baba-aca972ed6da1)
![2MWebSite](https://github.com/user-attachments/assets/c29bf222-a4e2-4f66-8c18-ed9a5428b49e)


## Task 2: What is the name of the JavaScript file loaded by the `/invite` page that has to do with invite codes?

inviteapi.min.js

![2MInvitePath](https://github.com/user-attachments/assets/96dfc556-5049-49bc-adf0-9458bc654bff)


### Task 3: What JavaScript function on the invite page returns the first hint about how to get an invite code? Don't include () in the answer.

makeInviteCode

![2MSOurceCodeInviteJS](https://github.com/user-attachments/assets/bb685e78-c703-4615-b283-61d979ddbec5)
![2MObfuscatedCode](https://github.com/user-attachments/assets/f82adcd9-e5b8-4328-bc64-74e9d72ec21a)
![2MUsingDE4JS](https://github.com/user-attachments/assets/4b5a07eb-7708-413f-9586-b98205367037)



### Task 4 : The endpoint in `makeInviteCode` returns encrypted data. That message provides another endpoint to query. That endpoint returns a `code` value that is encoded with what very common binary to text encoding format. What is the name of that encoding?

base64

![2MUsingBurpForChaginginPOST](https://github.com/user-attachments/assets/a88b9c7f-da59-4189-af7a-f5d3a715fc41)
![2MGeneratedInviteCode](https://github.com/user-attachments/assets/91ab5e54-ee7b-4aa2-9a1a-ed4c0d29fbf8)
![2MUnROT13](https://github.com/user-attachments/assets/ca965cbd-4d44-4b7b-b5ba-313f4d9bbeb1)
![2mUnbased64](https://github.com/user-attachments/assets/f4d80776-1fdb-4712-a86d-2cd0c62552ce)
![2MCreateAUser](https://github.com/user-attachments/assets/beaebed9-fa03-41ff-9b1e-e2cbc30fcf0f)
![2MDashBoard](https://github.com/user-attachments/assets/7fb2bdba-86de-4720-b108-f50f5da7dc7a)


### Task 5 : What is the path to the endpoint the page uses when a user clicks on "Connection Pack"?

/api/v1/user/vpn/generate


### Task 6: How many API endpoints are there under `/api/v1/admin`?

3

![2MListedAPIV1](https://github.com/user-attachments/assets/1ed47d8a-cf0a-4119-8b01-0523bccd819a)


### Task 7:  What API endpoint can change a user account to an admin account?

/api/v1/admin/settings/update
![2MEditingAccountSetting](https://github.com/user-attachments/assets/8cbd11da-f004-4342-808a-3ab5f52077f5)

### Task 8: What API endpoint has a command injection vulnerability in it?

/api/v1/admin/vpn/generate

![2MPhpEnvironmentVar](https://github.com/user-attachments/assets/59cc4ed7-f1c5-4c13-9e9e-ae62dbaee147)
![2MCredentials](https://github.com/user-attachments/assets/a1595ece-d671-43e2-add6-b8200e5b2fe4)
![2MSsh](https://github.com/user-attachments/assets/4c5629b9-945f-453b-9e85-0ed55c1ec7f9)


### ![[2MSsh.png]]


Task 9 : User flag

### Task 10 : What is the email address of the sender of the email sent to admin?

ch4p@2million.htb

### Task 11 : What is the 2023 CVE ID for a vulnerability in that allows an attacker to move files in the Overlay file system while maintaining metadata like the owner and SetUID bits?

CVE-2023-0386
![2MCVEUsage](https://github.com/user-attachments/assets/80610166-7a90-4f5c-9b17-75d8ed8f6c83)




### Task 12: Root flag
![2MRootFlag](https://github.com/user-attachments/assets/046e0b82-0e94-4119-a21f-8ace8cc575f0)
