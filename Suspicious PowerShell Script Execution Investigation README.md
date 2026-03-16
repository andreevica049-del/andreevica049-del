Suspicious PowerShell Script Execution Investigation (LetsDefend SOC Lab)
Scenario

A security alert was triggered in the SOC environment indicating the execution of a suspicious PowerShell script on a host within the network. The SOC analyst was responsible for investigating the alert, identifying whether the activity was malicious, and taking containment actions if necessary.

The investigation was conducted using the LetsDefend environment.

Alert Information

Alert details

Field	Value
Event ID	238
Rule Name	SOC153 – Suspicious PowerShell Script Executed
Severity	Medium
Host	Tony
Host IP	172.16.17.206
File Name	payload_1.ps1
File Path	C:\Users\LetsDefend\Downloads\payload_1.ps1

The alert indicates that a PowerShell script was executed on a host, which can often be associated with malware or attacker activity.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/14fdeb7f-2c14-45b2-83f8-ed5b90767c0c" />

Threat Intelligence Analysis

The file hash was analyzed using VirusTotal.

Result
Detection	Result
AV Detections	33 / 62
Malware Type	Trojan Downloader
Threat Labels	PowerShell / Boxter / Azorult

This confirms that the script is malicious.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/2848e3c2-fc8f-49b5-9a53-082d37284bbb" />

Endpoint Investigation

The affected endpoint was investigated in the Endpoint Security section.

Host Information
Field	Value
Hostname	Tony
Operating System	Windows 10
IP Address	172.16.17.206
Domain	LetsDefend
User	LetsDefend

After confirming malicious activity, the host was contained to prevent further communication with potential attacker infrastructure.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/411ce09b-f1f7-4bea-a08b-cd8ecfbf1035" />

Log Analysis

Log analysis revealed the following process execution:

EventID: 1 (Process Create)

Image:
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe

CommandLine:
powershell.exe
Set-ExecutionPolicy -Scope Process Bypass
C:\Users\LetsDefend\Downloads\payload_1.ps1
Key indicators

Suspicious indicators observed:

PowerShell execution

ExecutionPolicy Bypass

Script executed from Downloads directory

Malicious file hash

These indicators strongly suggest malicious PowerShell execution.
Indicators of Compromise (IOC)
Type	Value
File Name	payload_1.ps1
SHA256	db8be06ba6d2d3595dd0c86654a48cfc4c0c5408fdd3f4e1eaf342ac7a2479d0
Host	Tony
IP Address	172.16.17.206

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/3594d0fc-deea-4604-ab4d-0024dcf76d40" />

MITRE ATT&CK Mapping
Technique	Description
T1059.001	Command and Scripting Interpreter – PowerShell
T1105	Ingress Tool Transfer
T1204	User Execution
Containment

To prevent further compromise:

The infected host Tony was isolated.

Malicious activity was contained.

Additional monitoring was recommended.

Conclusion

The investigation confirmed that a malicious PowerShell script (payload_1.ps1) was executed on host Tony.

Threat intelligence analysis identified the file as a Trojan downloader, which could potentially download additional malware from attacker-controlled infrastructure.

The host was successfully contained to mitigate further risk.

Skills Demonstrated

This investigation demonstrates the following SOC skills:

SIEM log analysis

Threat intelligence analysis

Malware investigation

Endpoint security investigation

Incident response

IOC identification

Tools Used

LetsDefend

VirusTotal

