# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services
_TODO: Fill out the information below._

Nmap scan results for overall network:

```bash
$ [nmap 192.168.1.0-255]
```
[IP Network Results](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/nmapnetwork.png)

Nmap scan results for each Target 1 reveal the below services and OS details:

```bash
$ [nmap -sV 192.168.1.110]
```
[Target 1 Scan](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/nmaptarget1.png)

Nmap scan results for each Target 2 reveal the below services and OS details:

```bash
$ [nmap -sV 192.168.1.115]
```
[Target 2 Scan](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/nmaptarget2.png)


This scan identifies the services below as potential points of entry:
- Target 1
  - SSH (Port 22)
  - HTTP - Apache Web Server (Port 80)
  - Samba (Ports 139/445)
  - Wordpress Directory (traversal and/or enumeration)

- Target 2
  - SSH (Port 22)
  - HTTP - Apache Web Server (Port 80)

_TODO: Fill out the list below. Include severity, and CVE numbers, if possible._

The following vulnerabilities were identified on each target:
- Target 1
  - [Weak SSH Password: Username / Password "michael"](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/sshmichael.png) 
  - [Vital information unprotected - MySQL Database Config File](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/mysqlconfig.png)
  - [Password Hashes Accessible: MySQL Database](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/passwordhashes.png)       
  - [Samba Ports Subject to dDOS Attack: SMB-Vuln Scan](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/smbvuln.png)

_TODO: Include vulnerability scan results to prove the identified vulnerabilities._

### Exploitation
_TODO: Fill out the details below. Include screenshots where possible._

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: _TODO: Insert `flag1.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
  - `flag2.txt`: _TODO: Insert `flag2.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
  - `flag2.txt`: _TODO: Insert `flag2.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
  - `flag2.txt`: _TODO: Insert `flag2.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 2
  - `flag1.txt`: _TODO: Insert `flag1.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
  - `flag2.txt`: _TODO: Insert `flag2.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
  - `flag2.txt`: _TODO: Insert `flag2.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
  - `flag2.txt`: _TODO: Insert `flag2.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
