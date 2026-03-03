1.Alert Overview
Alert Name: Brute Force Activity Detection
Date: 17/09/2025
Time: 09:00:21
Target Host: tryhackme-2404
Source IP: 10.10.242.248
Log Source: linux_secure
SIEM: Splunk
At 09:00 AM, an alert was triggered indicating possible brute force activity targeting a Linux server. The source IP address was identified as 10.10.242.248.
Initial observation showed that the IP address is internal, suggesting potential lateral movement or prior network compromise.

2.Initial Log Analysis
To verify whether brute force activity occurred, the following SPL query was executed:
index="linux-alert" sourcetype="linux_secure" 10.10.242.248
| search "Accepted password for" OR "Failed password for" OR "Invalid user"
| sort + _time
The results showed:
•	Multiple login attempts
•	Attempts against non-existent users
•	High frequency of failed logins
This behavior is commonly associated with credential enumeration and brute force attacks.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/ab043d48-cd7e-486b-9fb0-db4d4c532a58" />
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/d6e66f4b-51bb-4bee-9146-c7d92b87de13" />
3.User Target Analysis
To determine which account was targeted, the following query was used:
index="linux-alert" sourcetype="linux_secure" 10.10.242.248
| rex field=_raw "sshd\[\d+\]:\s*(?<action>Failed|Accepted)\s+\S+\s+for(?: invalid user)? (?<username>\S+)"
| stats count by username
| sort -count
Findings:
•	Four users targeted
•	john.smith received 503 failed login attempts
This strongly indicates automated brute force activity targeting a specific user.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/76531a7d-a9d4-4eb3-9771-81543e86cc0d" />
4.Attack Duration Analysis
To calculate attack duration:
index="linux-alert" sourcetype="linux_secure" 10.10.242.248
| search "Failed password for john.smith"
| stats min(_time) as first_attempt max(_time) as last_attempt
| eval duration_minutes = round((last_attempt - first_attempt)/60,2)
The attack lasted approximately X minutes, confirming automated behavior.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/68357444-0342-4c28-84c8-2e47601e4bba" />
5.Successful Compromise Confirmation
To determine whether the attack succeeded:
index="linux-alert" sourcetype="linux_secure" 10.10.242.248
| rex field=_raw "sshd\[\d+\]:\s*(?<action>Failed|Accepted)\s+\S+\s+for(?: invalid user)? (?<username>\S+)"
| search username="john.smith"
| sort + _time
| table _time username action

The logs confirm:
•	Multiple failed attempts
•	One successful login (Accepted password)
•	Account compromised: john.smith
This confirms a successful brute force attack.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/b2b41bbc-843a-4121-ad82-7d3cb4c14af8" />
6.Post-Compromise Activity
Shortly after privilege escalation, a new local account named system-utm was created on the compromised host.
The account was assigned a valid home directory and interactive shell (/bin/bash), indicating intentional persistence setup.
The activity originated from an interactive terminal session (/dev/pts/3), further confirming malicious operator involvement.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/a5e68560-a6f1-47f4-859f-5725a0d60f85" />
7.MITRE ATT&CK Mapping
•	T1110 — Brute Force
•	T1078 — Valid Accounts
•	T1068 / T1548 — Privilege Escalation
•	T1136 — Create Account
8.Incident Classification
Severity: High
Classification: True Positive
Impact: Unauthorized access to internal Linux server



9.Escalation Decision
Due to confirmed successful compromise and persistence activity, the incident was escalated to SOC Level 2 for:
•	Full forensic investigation
•	Host isolation
•	Credential reset
•	Lateral movement analysis







