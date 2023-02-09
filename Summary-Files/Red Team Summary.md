# Red Team: Summary-of-Operations

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
- Unauthorized MySQL Database Access  
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

**Flag 1 & 2**

**Exploits Used: User Enumeration, Weak password, Unauthorized File Access**
  - Gaining access to the webserver using WPScan its possible to enumerate the Users and other information from the site
```bash
$ wpscan --url http://192.168.1.110/wordpress -eu
```  
  [![WPScan](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/WPScan.PNG)](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/WPScan.PNG)

  [![WPScan-findings](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/WPScan-Findings.PNG)](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/WPScan-Findings.PNG)

  [![WPScan-Users](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/WPScan-Users.PNG)](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/WPScan-Users.PNG)

  - Using the enumerated User **`Michael`** it was easy to discovered the username & password were identical by guessing the weak password 
  
   ```bash
  $ ssh michael@192.168.1.110
  ``` 
  [![SSH-via-Michael](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/SSH-via-user-michael.PNG)](https://github.com/srabbers/Final-Project/blob/57dcd6505154cc094521e6774a2725700501c7cb/Diagrams-and-Media/SSH-via-user-michael.PNG)
  
  - Once SSH connection was successful into **`Michael's`** shell their reconnaissance for the attack could continue 
    - This allows the attacker unauthorized access to **`Michael's`** entire account
  - While looking through /var/www/ the attacker managed to find `flag1 & flag2`
    - `flag1` was found in the /html/service.html file using grep to search the /var/www/ directory
    - `flag2` was found while searching the /var/www/ directory for hidden files & directories
   
- `flag1{b9bbcb33e11b80be759c4e844862482d}`
```bash
$ grep -R flag1
``` 
![Flag1](https://github.com/srabbers/Final-Project/blob/c5672d2a3cdf9f1e44b96f747d6e06aadb2caefa/Diagrams-and-Media/flag1.PNG)
  
- `flag2{fc3fd58dcdad9ab23faca6e9a36e581c}`
  
```bash
$ ls -lah
``` 
![Flag2](https://github.com/srabbers/Final-Project/blob/1038224e318f03276c352bcc7361ddf85e832bd0/Diagrams-and-Media/flag2.PNG)


**Flag 3**  
  
**Exploits Used: Unauthorized Access, Unauthorized MySQL Database Access, & MySQL Data Exfiltration**

  - Using the unauthorized access of **`Michael's`** account to look for the `MySQL database` config-file.php 
    - The config-file.php was found in the /var/www/html/wordpress directory 
    - Using the `cat` command to read the config-file.php file the root password for the `MySQL database` was discovered giving access to the `MySQL database`
    
![MySQL-config-file](https://github.com/srabbers/Final-Project/blob/a82bbc23f2e6292ef46ae1caa883eb9fde8b97f8/Diagrams-and-Media/MySQL-DB-config-file.PNG)

![MySQL-root-password](https://github.com/srabbers/Final-Project/blob/a82bbc23f2e6292ef46ae1caa883eb9fde8b97f8/Diagrams-and-Media/MySQL-DB-info.PNG)

![MySQL-access](https://github.com/srabbers/Final-Project/blob/772f596d6ecf7e99ad196c87b96dab3a7f39b835/Diagrams-and-Media/MySQL-Access.PNG)

  - `MySQL` was used to show & change databases, show tables, view tables, & display field data
    
  - `show databases;` is used to show the current MySQL databases
  - `use wordpress;` is used to change to the wordpress database
  - `show tables;` is used to show the tables of the wordpress database
    
  ![MySQL-WP-DB](https://github.com/srabbers/Final-Project/blob/772f596d6ecf7e99ad196c87b96dab3a7f39b835/Diagrams-and-Media/MySQL-wordpress-DB.PNG) 

  - `describe wp_users;` is used the show the field ID's of the wp_users table
  - `select user_login, user_pass from wp_users;` is used to view the data for the selected fields
      - The output of `select` gave the user_login names & user_pass hashes in plain text
      - These hashes were exfiltrated to the kali linux machine & put into wp_hashes.txt
  
  ![MySQL-WP-](https://github.com/srabbers/Final-Project/blob/772f596d6ecf7e99ad196c87b96dab3a7f39b835/Diagrams-and-Media/MySQL-wp_users.PNG)

  - `MySQL` was also used to find `flag3` in the table `wp_posts` 
    - `select * from wp_posts;`  was used to discover the flag 

  - `flag3{afc01ab56b50591e7dccf93122770cd2}`

```bash
mysql> select * from wp_posts;
``` 
  
  ![flag3](https://github.com/srabbers/Final-Project/blob/3dcaf420cedc6e31b1b4a6fc0a2da6a49d115be1/Diagrams-and-Media/flag3.PN)

**Flag 4** 

**Exploits used: John the ripper, Unauthorized access, & Misconfiguration of User Privileges/Privilege Escalation**

- `John the ripper` was used to crack the password hashes exfiltrated from the `MySQL database`
   - `John` was able to crack the password hash for the user `Steven` 
   - Discovering the password: `pink84`
```bash
john wp_hashes.txt
```
![John-cracked](https://github.com/srabbers/Final-Project/blob/133ba819bd790cd0d8e4837572bb43c5721b3259/Diagrams-and-Media/John-hashes.PNG)

```bash 
john --show wp_hashes.txt
``` 

![John-cracked](https://github.com/srabbers/Final-Project/blob/133ba819bd790cd0d8e4837572bb43c5721b3259/Diagrams-and-Media/John-cracked-hash.PNG)

- Using the cracked password `pink84` we can now SSH into the user `Steven`
  - Once SSH is successful we check the user for sudo priviledges 
```bash 
ssh steven@192.168.1.110
``` 
![SSH-Steven](https://github.com/srabbers/Final-Project/blob/8acf5482b55bbb213250dbb1d55fa18df0fe0863/Diagrams-and-Media/SSH-via-user-steven.PNG)

```bash 
sudo -l
``` 
![Sudo-check](https://github.com/srabbers/Final-Project/blob/17df8aad6c66989e284c2075a89d0d4a9da40bdb/Diagrams-and-Media/Elevated-privileges.PNG)

- Once we know the user has sudo access it is now possible for us to escalate our priviledges to `root` using python
  - Searched through root directory for `flag4`
   
```bash 
$ sudo python -c 'import os; os.system("/bin/sh")'
``` 

![root](https://github.com/srabbers/Final-Project/blob/17df8aad6c66989e284c2075a89d0d4a9da40bdb/Diagrams-and-Media/flag4.PNG)
