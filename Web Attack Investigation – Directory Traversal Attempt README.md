Web Attack Investigation – Directory Traversal Attempt (CVE-2024-24919)
Scenario

A security alert was triggered in the SOC monitoring environment indicating a possible exploitation attempt targeting a Check Point Security Gateway.
The alert suggested an Arbitrary File Read attack associated with CVE-2024-24919.

The goal of the investigation was to determine:

whether the attack was successful

the origin of the activity

potential impact on the system

required response actions

The investigation was performed using the LetsDefend SOC simulation environment.

Alert Information
Field	Value
Rule Name	SOC287 – Arbitrary File Read on Checkpoint Security Gateway
CVE	CVE-2024-24919
Severity	High
Attack Type	Web Attack
Source IP	203.160.68.12
Destination IP	172.16.20.146
HTTP Method	POST
The alert indicates a possible directory traversal exploit attempt targeting a Check Point gateway.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/4b759836-fe41-436a-8b5f-e81c238a9a7d" />

Attack Analysis

The HTTP request contained the following payload:

../../../../../../etc/passwd

Another request attempted to access:

../../../../../../etc/shadow

These payloads are commonly used in Directory Traversal attacks, where attackers attempt to escape the web root directory and access sensitive system files.

Targeted Files
File	Description
/etc/passwd	Contains user account information
/etc/shadow	Contains password hashes

If successful, attackers could obtain sensitive authentication data.

Attack Analysis

The HTTP request contained the following payload:

../../../../../../etc/passwd

Another request attempted to access:

../../../../../../etc/shadow

These payloads are commonly used in Directory Traversal attacks, where attackers attempt to escape the web root directory and access sensitive system files.

Targeted Files
File	Description
/etc/passwd	Contains user account information
/etc/shadow	Contains password hashes

If successful, attackers could obtain sensitive authentication data.

Log Analysis

Log entries show the following activity:

203.160.68.12 POST /clients/MyCRL
../../../../../../etc/passwd

Another request attempted:

../../../../../../etc/shadow

Server response:

403 Forbidden
Interpretation

The 403 response code indicates that the server blocked the request, meaning the exploit attempt was unsuccessful.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/38177632-b760-4ac3-9b86-d77baa6b1368" />

Threat Intelligence Analysis

The source IP was analyzed using VirusTotal.

Results
Indicator	Result
IP Address	203.160.68.12
Detection	2 / 94 vendors
ASN	China Unicom Global

This suggests the IP may be part of automated internet scanning infrastructure rather than a targeted attack.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/655e1005-9f89-4aff-a7d0-37458fec2364" />

Affected System
Field	Value
Hostname	CP-Spark-Gateway-01
Operating System	Check Point R80.20 Gaia
IP Address	172.16.20.146
Role	Security Gateway

No signs of:

malware execution

system compromise

unauthorized processes

were observed on the endpoint.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/7e51dcd9-efa5-48fa-a317-6f3e256475f2" />

Indicators of Compromise (IOC)
Type	Value
Source IP	203.160.68.12
Exploit Technique	Directory Traversal
Target Files	/etc/passwd, /etc/shadow
CVE	CVE-2024-24919

MITRE ATT&CK Mapping
Technique	Description
T1190	Exploit Public-Facing Application
T1006	Path Traversal
T1046	Network Service Discovery

Conclusion

The investigation confirmed that an external IP address attempted to exploit CVE-2024-24919 on a Check Point Security Gateway using directory traversal payloads.
The attacker attempted to retrieve sensitive system files such as /etc/passwd and /etc/shadow.
Log analysis showed the server returned HTTP 403 responses, indicating that the exploit attempt was blocked and the system was not compromised.
The activity appears consistent with automated vulnerability scanning across internet-facing infrastructure.

Recommended Security Actions:

Patch systems vulnerable to CVE-2024-24919
Monitor repeated requests from the source IP
Implement WAF rules for directory traversal patterns
Block malicious IP addresses if scanning persists


Skills Demonstrated:

This investigation demonstrates the following SOC analyst skills:
SIEM log analysis
Web attack investigation
Threat intelligence analysis
Vulnerability exploitation analysis
Incident response workflow


Tools Used:
LetsDefend
VirusTotal
