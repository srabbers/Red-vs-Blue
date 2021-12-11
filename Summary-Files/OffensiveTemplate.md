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
  - Nmap was also able to discover open ports allowing attacks to target specific services
    - WPScan was used to enumerate the users 
### Exploitation
_TODO: Fill out the details below. Include screenshots where possible._

The Red Team was able to penetrate **`Target 1`** and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: _TODO: Insert `flag1.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_
  - `flag2.txt`: _TODO: Insert `flag2.txt` hash value_
    - **Exploit Used**
      - _TODO: Identify the exploit used_
      - _TODO: Include the command run_