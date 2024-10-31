
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
![[Task2.png]]
## Task 3 : Are you able to get to other users' scans?
yes
![[Task3.png]]
## Task 4 : What is the ID of the PCAP file that contains sensative data?
0

## Task 5 : Which application layer protocol in the pcap file can the sensetive data be found in?
ftp

## Task 6 : We've managed to collect nathan's FTP password. On what other service does this password work?
ssh
![[Task4.png]]
## Task 7 : Submit the flag located in the nathan user's home directory.

![[Task5.png]]
## Task 8 : What is the full path to the binary on this machine has special capabilities that can be abused to obtain root privileges?
![[Task6UploadLinPeas.png]]
![[Task7.png]]
## Task 9 : 
![[Task8.png]]