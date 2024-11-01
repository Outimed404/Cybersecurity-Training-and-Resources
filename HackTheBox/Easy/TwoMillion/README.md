
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


## Step-by-Step Solution
**IP Addresses:**
- My IP address: 10.10.15.11
- Target machine IP address: 10.10.10.245

## Task 1: How many TCP ports are open ? 

2
![[2MNMAP.png]]

![[2MEditingETCHOST 1.png]]
![[2MWebSite.png]]
## Task 2: What is the name of the JavaScript file loaded by the `/invite` page that has to do with invite codes?

inviteapi.min.js

![[2MInvitePath.png]]

### Task 3: What JavaScript function on the invite page returns the first hint about how to get an invite code? Don't include () in the answer.

makeInviteCode

![[2MSOurceCodeInviteJS.png]]
![[2MObfuscatedCode.png]]

![[2MUsingDE4JS.png]]



### Task 4 : The endpoint in `makeInviteCode` returns encrypted data. That message provides another endpoint to query. That endpoint returns a `code` value that is encoded with what very common binary to text encoding format. What is the name of that encoding?

base64

![[2MUsingBurpForChaginginPOST.png]]
![[2MGeneratedInviteCode.png]]

![[2MUnROT13.png]]
![[2mUnbased64.png]]
![[2MCreateAUser.png]]

![[2MDashBoard.png]]


### Task 5 : What is the path to the endpoint the page uses when a user clicks on "Connection Pack"?

/api/v1/user/vpn/generate


### Task 6: How many API endpoints are there under `/api/v1/admin`?

3

![[2MListedAPIV1.png]]


### Task 7:  What API endpoint can change a user account to an admin account?

/api/v1/admin/settings/update

![[2MEditingAccountSetting.png]]
### Task 8: What API endpoint has a command injection vulnerability in it?

/api/v1/admin/vpn/generate
![[2MPhpEnvironmentVar.png]]

![[2MCredentials.png]]
### ![[2MSsh.png]]


Task 9 : User flag

### Task 10 : What is the email address of the sender of the email sent to admin?

ch4p@2million.htb

### Task 11 : What is the 2023 CVE ID for a vulnerability in that allows an attacker to move files in the Overlay file system while maintaining metadata like the owner and SetUID bits?

CVE-2023-0386
![[2MCVEUsage.png]]
### Task 12: Root flag

![[2MRootFlag.png]]