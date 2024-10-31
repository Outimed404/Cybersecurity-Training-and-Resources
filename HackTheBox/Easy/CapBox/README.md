
# Cap CTF Write-Up

# Challenge Title: Cap

## Platform
Hack The Box

## Difficulty
Easy

## Description
Cap is an easy difficulty Linux machine running an HTTP server that performs administrative functions including performing network captures. Improper controls result in Insecure Direct Object Reference (IDOR) giving access to another user's capture. The capture contains plaintext credentials and can be used to gain foothold. A Linux capability is then leveraged to escalate to root.

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
- My IP address: 10.10.15.11
- Target machine IP address: 10.10.10.245

## Task 1: How many TCP ports are open ? 
3

```bash
nmap -p- -A 10.10.10.245
```
Port 21,22,80
## Task 2 : After running a "Security Snapshot", the browser is redirected to a path of the format `/[something]/[id]`, where `[id]` represents the id number of the scan. What is the `[something]`?
data
![Task2](https://github.com/user-attachments/assets/ff029d0a-573c-4961-acc0-0669c3b0a4c6)

## Task 3 : Are you able to get to other users' scans?
yes
![Task3](https://github.com/user-attachments/assets/14c0e703-275a-46fe-ad60-882e432d84ca)

## Task 4 : What is the ID of the PCAP file that contains sensative data?
0

## Task 5 : Which application layer protocol in the pcap file can the sensetive data be found in?
ftp
![Task4](https://github.com/user-attachments/assets/26ab7b37-488c-4f07-be50-29553e60daf2)

## Task 6 : We've managed to collect nathan's FTP password. On what other service does this password work?
ssh

## Task 7 : Submit the flag located in the nathan user's home directory.
![Task5](https://github.com/user-attachments/assets/b02d7da9-2eca-4f6f-b0f3-bcf90b40ed76)


## Task 8 : What is the full path to the binary on this machine has special capabilities that can be abused to obtain root privileges?

![Task7](https://github.com/user-attachments/assets/d1c85366-fba6-40ac-a700-ac8691d82146)
![Task6UploadLinPeas](https://github.com/user-attachments/assets/20080c02-efb3-43fb-9ef8-e2ed56c0568e)

## Task 9 : 
![Task8](https://github.com/user-attachments/assets/485ee9e6-8cbe-4c83-86cc-556e10c1630f)
