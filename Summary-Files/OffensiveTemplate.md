# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

## Exposed Services

Netdiscover results identify the IP addresses of Targets on the Network:
```bash
$ netdiscover -r 192.168.1.255/24
```
[![Net Discovery](/Diagrams-and-Media/Network-Discovery.PNG)](https://github.com/srabbers/Final-Project/blob/main/Diagrams-and-Media/Network-Discovery.PNG)

Nmap scan results for each machine reveal the services and OS details:

```bash
$ nmap -sV 192.168.1.1-115
```
[![Nmap Scans](https://github.com/srabbers/Final-Project/blob/b449f8140afd116352dfcbdbb37690b17afc84aa/Diagrams-and-Media/nmap-services-scan1.PNG)](https://github.com/srabbers/Final-Project/blob/b449f8140afd116352dfcbdbb37690b17afc84aa/Diagrams-and-Media/nmap-services-scan1.PNG)  


[![Nmap Scans](https://github.com/srabbers/Final-Project/blob/b449f8140afd116352dfcbdbb37690b17afc84aa/Diagrams-and-Media/nmap-services-scan2.PNG)](https://github.com/srabbers/Final-Project/blob/b449f8140afd116352dfcbdbb37690b17afc84aa/Diagrams-and-Media/nmap-services-scan2.PNG)

Nmap scan results for **`Target 1`** reveal the services and OS details below:

   - Hostname: **`Target 1`**
   - Operating System: **`Linux`**
   - Role on Network: **`Blue Team Machine`** 
   - IP Address: **`192.168.1.110`**
 
 ```bash
$ nmap -sV 192.168.1.110
```
[![Nmap scan Target1](https://github.com/srabbers/Final-Project/blob/main/Diagrams-and-Media/nmap-Services-OS.PNG)](https://github.com/srabbers/Final-Project/blob/main/Diagrams-and-Media/nmap-Services-OS.PNG)

The following vulnerabilities were identified on each target:
- **`Target 1`**
  - List of
  - Critical
  - Vulnerabilities

_TODO: Include vulnerability scan results to prove the identified vulnerabilities._

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