# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology


The following machines were identified on the network:
- VM 1: Kali
  - **Operating System**: LINUX
  - **Purpose**: Attacker
  - **IP Address**: 192.168.1.90
- VM 2: Capstone  
  - **Operating System**: LINUX
  - **Purpose**: Target
  - **IP Address**: 192.168.1.105
- VM 3: ELK Server  
  - **Operating System**: LINUX
  - **Purpose**: ELK logs
  - **IP Address**: 192.168.1.100
- VM 4: Target 1
  - **Operating System**: LINUX
  - **Purpose**: Apache web server
  - **IP Address**: 192.168.1.110
  - 
### Description of Targets

The target of this attack was: `Target 1` 192.168.1.110

Target 1 is an Apache web server and has SSH enabled, this allows ports 80 and 22 as possible ports of entry for attackers. As such, the following alerts have been implemented: Excessive HTTP Errors, CPU usage monitor, HTTP request size monitor

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Excessive HTTP Errors

![Excessive_HTTP](https://user-images.githubusercontent.com/88590862/145345457-a4a38a2a-0901-4e27-a0ac-4ee324ccea4b.PNG)

- Excessive HTTP Errors is implemented as follows:
  - **Metric**: WHEN count () GROUPED OVER top 5 'http.response.status_code'
  - **Threshold**: IS ABOVE 400 FOR THE LAST 5 minutes
  - **Vulnerability Mitigated**: Brute Force attacks
  - **Reliability**: High Reliability

#### HTTP Request Size Monitor

![HTTP_Request](https://user-images.githubusercontent.com/88590862/145345486-85b990f0-9e8d-4a80-9a0f-f5886ef209ed.PNG)

- HTTP Request Size Monitor is implemented as follows:
  - **Metric**: WHEN sum () OF http.response.bytes OVER all documents
  - **Threshold**: IS ABOVE 3500 FOR THE LAST 1 minute 
  - **Vulnerability Mitigated**: HTTP Request Smuggling
  - **Reliability**: High Reliability

#### CPU Usage Monitor

![CPU_Usage](https://user-images.githubusercontent.com/88590862/145345501-985418c5-c55b-475d-97b9-a26a5b64146c.PNG)

- CPU Usage Monitor is implemented as follows:
  - **Metric**: WHEN max () OF system.process.cpu.total.pct OVER all documents
  - **Threshold**: IS ABOVE .5 FOR THE LAST 5 minutes
  - **Vulnerability Mitigated**: Monitor for malicious scripts 
  - **Reliability**: High Reliability

### The three alerts shown above are meant to provide a quick alert to items that may be in need of attention. 
-Excessive HTTP errors could suggest that there is a  brute force attacking their way into a company secret folder for example. This alert is usually reliable since there is a spike in the requests happening during a brute force attack. 

-The HTTP Request size is meant to alert for the potential of a HTTP Request smuggle occuring on your server; where someone may be trying to bypass security controls, gain unauthorized access to sensitive data, and directly compromise other application users. 

-Lastly is the CPU usage alert. This was created to look out for any malicious scripts being run on the system or server. This alert allows us to potentially discover these malicious scripts or discover unoptimized code that is causing a CPU spike. 
