# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

## Exposed Services

**Netdiscover results identify the IP addresses of Targets on the Network:**
```bash
$ netdiscover -r 192.168.1.255/24
```
[![Net Discovery](/Diagrams-and-Media/Network-Discovery.PNG)](https://github.com/srabbers/Final-Project/blob/main/Diagrams-and-Media/Network-Discovery.PNG)

**Nmap scan results for each machine reveal the services and OS details:**

```bash
$ nmap -sV 192.168.1.1-115
```
[![Nmap Scans](https://github.com/srabbers/Final-Project/blob/b449f8140afd116352dfcbdbb37690b17afc84aa/Diagrams-and-Media/nmap-services-scan1.PNG)](https://github.com/srabbers/Final-Project/blob/b449f8140afd116352dfcbdbb37690b17afc84aa/Diagrams-and-Media/nmap-services-scan1.PNG)  


[![Nmap Scans](https://github.com/srabbers/Final-Project/blob/b449f8140afd116352dfcbdbb37690b17afc84aa/Diagrams-and-Media/nmap-services-scan2.PNG)](https://github.com/srabbers/Final-Project/blob/b449f8140afd116352dfcbdbb37690b17afc84aa/Diagrams-and-Media/nmap-services-scan2.PNG)

**Nmap scan results for **`Target 1`** reveal the services and OS details below:**

   - Hostname: **`Target 1`**
   - Operating System: **`Linux`**
   - Role on Network: **`Blue Team Defense`** 
   - IP Address: **`192.168.1.110`**
 
 ```bash
$ nmap -sV 192.168.1.110
```
[![Nmap scan Target1](https://github.com/srabbers/Final-Project/blob/main/Diagrams-and-Media/nmap-Services-OS.PNG)](https://github.com/srabbers/Final-Project/blob/main/Diagrams-and-Media/nmap-Services-OS.PNG)

**This nmap scan identifies the following services as potential points of entry:**


   - Port 22/tcp open ssh (service) OpenSSH 6.7p1 Debian 5+deb8u4
   - Port 80/tcp open http (service) Apache httpd 2.4.10 ((Debian))
   - Port 111/tcp open rpcbind (service) 2-4 (RPC #100000)
   - Port 139/tcp open netbios-ssn (services) Samba smbd 3.X - 4.X
   - Port 445/tcp open netbios-ssn (services) Samba smbd 3.X - 4.X

**The following service vulnerabilities were identified on **`Target 1`**:**

 - [CVE-2021-28041 open SSH](https://nvd.nist.gov/vuln/detail/CVE-2021-28041)  
  - [CVE-2017-15710 Apache https 2.4.10](https://nvd.nist.gov/vuln/detail/CVE-2017-15710)
  - [CVE-2017-8779 exploit on open rpcbind port could lead to remote DoS](https://nvd.nist.gov/vuln/detail/CVE-2017-8779)  
  - [CVE-2017-7494 Samba NetBIOS](https://nvd.nist.gov/vuln/detail/CVE-2017-7494)  
### Critical Vulnerabilities 
**The following critical vulnerabilities were identified on **`Target 1`**:**
- Network Mapping and User Enumeration (WordPress site)
  - Nmap was used to discover the Network Topology.  
  - Nmap was able to discover open ports and services running allowing attackers to target specific services
    - WPScan was used to enumerate the usernames of the wordpress site
- Weak User Password
  -  A user had a weak password that was easily figured out by guessing
     -  The guessed password was then used to SSH into the web server allowing the attacker access to confidential data
- MySQL Database Access  
  - The attacker was able to discover a file on a users machine containing login information for the      MySQL database  
    - They were able to use the login information to gain access to the MySQL database  
- MySQL Data Exfiltration  
  - By browsing through the various tables in the MySQL database the attacker was able to discover password hashes of all the users.  
    - The attacker was able to exfiltrate the password hashes data
- Unsalted User Password Hash (WordPress database) 
  - The attacker was able to use the MySQL wordpress database to acquire & exfiltrate unsalted password hashes 
   - These unsalted hashes are easily cracked using John the Ripper
   - This allowed access to an account for the attacker to escalate their privileges 
- Misconfiguration of User Privileges/Privilege Escalation  
  - The attackers noticed that Steven had sudo privileges 
    - This allowed the attacker to run a sudo python command to gain access to a root shell
  
### Exploitation
The Red Team was able to penetrate **`Target 1`** and retrieve the following confidential data:
## Flag 1 
 `flag1.txt: flag1{b9bbcb33e11b80be759c4e844862482d}`
- **Exploit Used: User Enumeration & Weak password**
  - The attacker first needs to gain access to the webserver, since they know that the target is running a wordpress site on the server they can use WPScan to enumerate the Users and other information from the site
  
```bash
root@Kali:~# wpscan --url http://192.168.1.110/wordpress -eu
```  
  [![WPScan](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/WPScan.PNG)](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/WPScan.PNG)

  [![WPScan-findings](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/WPScan-Findings.PNG)](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/WPScan-Findings.PNG)

  [![WPScan-Users](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/WPScan-Users.PNG)](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/WPScan-Users.PNG)

  - Using the enumerated User **`Michael`** the attacker discovered the username & password were identical by guessing the password allowing for SSH connection
   ```bash
    ssh michael@192.168.1.110
  ``` 
  [![SSH-via-Michael](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/SSH-via-user-michael.PNG)](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/SSH-via-user-michael.PNG)
  
  - Once we have SSH connection into **`Michael's`** account we able to look through his files for any confidential data to help further the attack
  - 






  -
  - `flag2.txt`: _TODO: Insert `flag2.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
      -