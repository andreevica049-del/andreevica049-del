Executive Summary
The SOC team at MiddleMayhem Inc. detected suspicious network activity targeting the administrative portal of their web application. Although no confirmed breach was initially identified, further investigation of SIEM logs revealed a structured attack sequence.
Analysis confirmed that the attacker bypassed authentication mechanisms by exploiting a vulnerable Next.js framework instance, achieved remote code execution via file upload functionality, and later performed lateral movement within the internal network using SSH brute force techniques.
The investigation aimed to identify the attack vector, scope of compromise, and overall impact on the infrastructure.

. 1. Technology Identification
During initial reconnaissance, the web application framework was identified by reviewing the website footer.
The application was running:
•	Next.js 15.0.0
This information is critical because outdated framework versions may contain publicly disclosed vulnerabilities that could be exploited by attackers.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/94c08872-384b-44b0-a7d8-5a8986c46f93" />
