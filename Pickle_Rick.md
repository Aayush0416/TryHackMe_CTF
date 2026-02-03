# TryHackMe – Pickle Rick Write-Up 🥒

## Room Type
Beginner | Web Exploitation | Linux Privilege Escalation

---

## Overview
Pickle Rick is a beginner TryHackMe room designed to teach **basic web enumeration**,  **command execution**, and **privilege escalation through misconfigured sudo permissions**.  
The goal is to locate **three secret ingredients** hidden on the target system.

---

## Tools Used
- Nmap  
- Gobuster  
- Firefox (TryHackMe AttackBox)  
- Linux command-line utilities  

---

## Step 1: Enumeration

### Port Scanning
An initial Nmap scan was performed to identify open services.

```bash
nmap <TARGET_IP>
```

Results:

22/tcp – SSH

80/tcp – HTTP

Since HTTP was available, the attack focused on web enumeration.

---

## Step 2: Web Enumeration
### Source Code Review
The website was accessed via the browser.
Inspecting the page source revealed a username inside an HTML comment, indicating
information disclosure.

### Directory Enumeration
Gobuster was used to discover hidden directories and files.

```bash
gobuster dir -u http://<TARGET_IP> -w /usr/share/wordlists/dirb/common.txt
```

###Notable findings:

```bash
/robots.txt

/assets/

```

---

## Step 3: Information Disclosure
### robots.txt
Accessing robots.txt revealed a string that was later used as the password.

This highlights how robots.txt can unintentionally expose sensitive data.

---

## Step 4: Authentication & Command Execution
Using:

**Username** from HTML source

**Password** from robots.txt

Successful login was achieved.

After login, the application provided a command execution panel, allowing system
commands to be executed on the server.

---

## Step 5: Ingredient Collection
###Ingredient 1
Listing files revealed the first ingredient.

```bash
ls

less Sup3rS3cretPickl3Ingred.txt
```
The cat command was restricted, but less was used to read the file.

## Ingredient 2
Exploring user directories led to the second ingredient.

```bash
ls /home

ls /home/rick

less "/home/rick/second ingredients"
```

---

##Step 6: Privilege Escalation
###Checking sudo Permissions
```bash
sudo -l
```
Output:

(ALL) NOPASSWD: ALL
This configuration allows execution of any command as root without authentication,
which is a critical security misconfiguration.

###Root Access & Final Ingredient
```bash
sudo ls /root

less /root/3rd.txt
```
The third and final ingredient was retrieved from the root directory.

##Conclusion
This room demonstrates how information disclosure, weak access controls, and
misconfigured sudo permissions can lead to complete system compromise.

