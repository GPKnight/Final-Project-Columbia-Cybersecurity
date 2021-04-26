# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

The following machines were identified on the network:
- Name of VM 1 : Host Machine
  - **Operating System**: Microsoft Windows
  - **Purpose**: Provide the virtual environment for hypervisor to manage all the lab virtual machines.
  - **IP Address**: 192.168.1.1
  - **Open Ports**: 22 - OpenSSH 7.61pl Ubuntu 4 (Ubuntu Linux; protocol 2.0)
                    80 - Apache httpd 2.4.29
  
- Name of VM 2 : Kali Machine
  - **Operating System**: Linux
  - **Purpose**: The virtual machine which all attacks will originate from.
  - **IP Address**: 192.168.1.8
  -  **Open Ports**: 22 - OpenSSH 7.8pl Debian 1 (protocol 2.0)

- Name of VM 3 : ELK
  - **Operating System**: Linux
  - **Purpose**: The machine which collects metric, packet, and file beat information, accessed via web browser from the Host Machine
  - **IP Address**: 192.168.1.100
  -  **Open Ports**: 22 - OpenSSH 7.6pl Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0) 9200 - rtsp

- Name of VM 4 : Capstone
  - **Operating System**: Linux
  - **Purpose**: The machine which Kibana alerts would be tested on to confirm proper configuration.
  - **IP Address**: 192.168.1.105
  -  **Open Ports**:  22 - OpenSSH 7.61pl Ubuntu 4 (Ubuntu Linux; protocol 2.0) 80 - Apache httpd 2.4.29

- Name of VM 5 : Target 1
  - **Operating System**: Linux
  - **Purpose**: The primary machine to attempt to compromise from the Kali machine.
  - **IP Address**: 192.168.1.110
  -  **Open Ports**:  22 - OpenSSH 6.7pl Debian 5+deb8u4 (protocol 2.0) 80 - Apache httpd 2.4.10 ((Debian)) 111 - 2-4 (RPC #100000) 139 - netbios-ssn Samba smbd 3.X - 4. X (workgroup: WORKGROUP) 445 - netbios-ssn Samba smbd 3.X - 4. X (workgroup: WORKGROUP)

- Name of VM 6 : Target 2
  - **Operating System**: Linux
  - **Purpose**: The secondary machine to attempt to compromise from the Kali machine.
  - **IP Address**: 192.168.1.115
  -  **Open Ports**:  22 - OpenSSH 7.61pl Ubuntu 4 (Ubuntu Linux; protocol 2.0) 80 - Apache httpd 2.4.10 ((Debian)) 111 - 2-4 (RPC #100000)

### Description of Targets

The target of these attacks were: `Target 1` 192.168.1.110.

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. This target also has two vulnerable SAMBA ports, susceptible (based on SMB-VULN* scan) to dDOS (denial-of-service) attacks. 

'Target 2' 192.168.1.115.

Target 2 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Excessive HTTP Errors

"Excessive HTTP Errors" is implemented as follows:
  - **Metric**: packetbeat-* - count of *http.response.status_code*
  - **Threshold**: 400 over 5 minutes
  - **Vulnerability Mitigated**: Brute Force Attacks (Error codes over 400 and below 500 indicate a client error response)
  - **Reliability**: http.response.status_code is returned whenever a request is sent to the site.  Normally these requests consist of GET or POST types.  Depending on the request, a status code is returned indicating success or failure of the request.  When a high number of status codes are returned in a short window of time, that may be an indication of a brute force / password attack. This is a highly reliable alert and should not return a high number of fasle-positives.

#### HTTP Request Size

"HTTP Request Size" is implemented as follows:
  - **Metric**: packetbeat-* - sum of *http.request.bytes*
  - **Threshold**: 3500 over 1 minute
  - **Vulnerability Mitigated**: Monitoring for uploading payloads, specifically .php, .sh, or .py scripts which would be used to initiate a reverse or bind shell.
  - **Reliability**: Per [this Google Whitepaper](https://dev.chromium.org/spdy/spdy-whitepaper) "...the typical http request header ranges from 200 bytes to 2KB. However, typical header size of 700-800 bytes is common." If your website was not secure (HTTP vs HTTPS) a malicious actor could use POST or PUT requests to pass on harmful code / commands to a vulnerable server. Given that the normal request size is much lower than the threshold, this is a moderately reliable alert which should produce a low number of false-positives. Like other alerts in this report, establishing a threshold based on examination of actual traffic would greatly improve the reliability.

#### CPU Usage Monitor

"CPU Usage Monitor" is implemented as follows:
  - **Metric**: metricbeat-* - max of *system.process.cpu.total.pct*
  - **Threshold**: 0.5 over 5 minutes
  - **Vulnerability Mitigated**: Having high CPU utilization can be an indication of malicious activity over the network, for example, meterpreter shell sessions run untirely off of memory.
  - **Reliability**: While high CPU usage may be an indicator of malicious activity, it can just indicate normal processes occuring. This alert should be considered low reliability as a high number of false-positives would be expected unless a very finite threshold based on server activity was established ahead of time.

#### Port Scan Monitor

"Port Scan Monitor" is implemented as follows:
  - **Metric**: metricbeat-* sum of *destination.port*
  - **Threshold**: 1000 over 1 minute
  - **Vulnerability Mitigated**: Identifying a port scan against your server
  - **Reliability**: Port scans may be conducted for non-malicious purposes therefore this alert may be responsible for false-positives. This is a medium reliability alert.


