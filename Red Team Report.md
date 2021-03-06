# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

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

The following vulnerabilities were identified on each target:
- Target 1 
  - [Weak SSH Password: Username / Password "michael"](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/sshmichael.png) 
  - [Vital information unprotected - MySQL Database Config File](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/mysqlconfig.png)
  - [Password Hashes Accessible: MySQL Database](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/passwordhashes.png)       
  - [Samba Ports Subject to dDOS Attack: SMB-Vuln Scan](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/smbvuln.png)
  - [Vulnerable User with Sudo Access](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/rootescalation.png)
  - [Username Enumeration of Wordpress](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/wpscanusers.png)

- Target 2
  - [Directory Enumeration (using GoBuster)](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/T2Gobuster.png)
  - [Storing Critical Information (Flag 1) Within Accessible Directory](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/Target2Flag1.png)
  - [Subject to Backdoor code execution, initiating a reverse shell](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/ReverseShell.png)
  - [Using the reverse shell to exploit a vulnerability in the mysql database to gain Root Access](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/Target2MySqlExploit.png)

### Exploitation

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1 - [Raven One](https://blog.barradell-johns.com/index.php/2018/12/17/raven-one-writeup/)
  - `flag1.txt`: `b9bbcb33e11b80be759c4e844862482d`  [Target 1 Flag 1](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/target1flag1.png)
    - **Exploit Used**
      - Vulnerable information stored unprotected
      - grep "flag"
  - `flag2.txt`: `fc3fd58dcdad9ab23faca6e9a36e581c` [Target 1 Flag 2](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/target1flag2.png)
    - **Exploit Used**
      - Very low strength password allowing immediate ssh into target machine.
      - ssh michael@192.168.1.110 password: michael
  - `flag3.txt`: `afc01ab56b50591e7dccf93122770cd2` [Target 1 Flag 3](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/target1flag3.png)
    - **Exploit Used**
      - Unsecure database credentials
      - mysql --username=root --password=R@v3nSecurity
  - `flag4.txt`: `715dea6c055b9fe3337544932f2941ce` [Target 1 Flag 4](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/target1flag4.png)
    - **Exploit Used**
      - Sudo python access for lower level user
      - sudo python -c 'import pty;pty.spawn("/bin/bash")'

The Red Team was able to penetrate `Target 2` and retrieve the following confidential data:
- Target 2 - [Raven 2](https://www.secjuice.com/raven-2-write/) 
  - `flag1.txt`: `a2c1f66d2b8051bd3a5874b5b6e43e21` [Target 2 Flag 1](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/Target2Flag1.png)
    - **Exploit Used**
      - Directory enumeration using GoBuster
      - gobuster -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt dir -u http://192.168.1.115  
  - `flag2.txt`: `6a8ed560f0b5358ecf844108048eb337` [Target 2 Flag 2](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/Target2Flag2.png)
    - **Exploit Used**
      - Backdoor bash script reverse shell
      - bash [exploit.sh](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/exploitsh.png)
  - `flag3.txt`: `a0f568aa9de277887f37730d71520d9b` [Target 2 Flag 3](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/Target2Flag3.png)
    - **Exploit Used**
      - Unsecure image file
      - scp image back to kali machine to open in viewer
  - `flag4.txt`: `df2bc5e951d91581467bb9a2a8ff4425` [Target 2 Flag 4](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/Target3Flag4.png) 
    - **Exploit Used**
      - MySQL database exploit 1518.c
      - [Instructions](https://github.com/GPKnight/Final-Project-Columbia-Cybersecurity/blob/main/Images/RootInstructions.png)
