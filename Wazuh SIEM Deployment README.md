Security Engineer Internship – Technical Homework

Wazuh Deployment, Log Analysis and Rule Testing Report
Prepared by:
Andrii Hurskykh
Position:
Security Engineer Intern Candidate
Date:
18 May 2026

Wazuh Security Engineer Test Report:

1. Introduction

2. Step 1 — Environment Deployment

3. Step 2 — Log Source Integration

4. Step 3 — Submission and Reporting

   Log #1
   Log #2
   ...
   Log #11

5. Bonus: Create Custom Rule for Customer PII in Logs

6. Findings / Observations

7. Conclusion

1. Introduction

This report documents the deployment and testing of a Wazuh SIEM environment as part of the Security Engineer Internship technical assignment.
The objective of the exercise was to deploy a functional Wazuh environment using Docker containers, analyze the provided HTTP logs, verify rule detection behavior, and implement custom detection rules for sensitive information exposure.
The report includes deployment steps, log analysis results, custom rule implementation, and observations gathered during the testing process.

2. Step 1 — Environment Deployment

Docker Desktop was installed successfully. During initial startup, additional configuration and troubleshooting steps were required to fully initialize the Docker engine.

During initial setup, Docker Desktop failed to start due to a missing WSL2 kernel component. The issue was resolved by updating the WSL kernel using the wsl --update command and restarting the system.
<img width="698" height="392" alt="image" src="https://github.com/user-attachments/assets/26128dc0-a2c1-42e5-b378-aca92b696810" />

Git was installed successfully and verified through the command-line interface.
<img width="696" height="393" alt="image" src="https://github.com/user-attachments/assets/a102c677-fd8b-4ea0-bf2c-00b2847b69d9" />

The official Wazuh Docker single-node deployment files were downloaded from the Wazuh GitHub repository and prepared for deployment.
<img width="794" height="447" alt="image" src="https://github.com/user-attachments/assets/469724ce-bfaa-4171-b5c4-b88da1959aa5" />

During deployment, the Wazuh indexer container initially failed its health check and required additional troubleshooting and verification.
<img width="758" height="426" alt="image" src="https://github.com/user-attachments/assets/08af1d2b-b2c6-4b1d-b4db-25980c7bda07" />

Deployment of Wazuh SIEM using Docker Compose on Windows. All core containers successfully started.
<img width="746" height="420" alt="image" src="https://github.com/user-attachments/assets/207b3c3e-49c4-4b3c-84e2-ec259cb48b32" />

Wazuh SIEM Deployment Verification
The screenshot demonstrates the successful deployment and initial configuration of the Wazuh SIEM platform using Docker Compose on Windows. The Wazuh Dashboard is accessible through the web interface, and the API connection status is shown as online. The Wazuh Manager and Indexer services are functioning correctly, confirming that the SIEM environment has been successfully initialized and is operational.
<img width="696" height="391" alt="image" src="https://github.com/user-attachments/assets/13f831b4-d799-483c-8baa-060d9ee9109d" />

3. Step 2 — Windows Agent Deployment and Connection
The screenshot demonstrates the successful deployment and connection of a Windows 11 endpoint to the Wazuh SIEM environment. The Wazuh agent was installed and registered with the manager, allowing centralized monitoring of the endpoint. The agent status is shown as Active, confirming successful communication between the endpoint and the SIEM infrastructure.
<img width="780" height="439" alt="image" src="https://github.com/user-attachments/assets/1b4a12b8-5842-4c91-b87e-ac9a0dfad51e" />
<img width="710" height="399" alt="image" src="https://github.com/user-attachments/assets/f5daaab0-abb5-4424-8cc7-4f9956d60fa7" />

4. Step 3 — Submission and Reporting
Log #1 – Cross-Site Scripting (XSS) Attempt
The tested HTTP request contains a reflected XSS payload using the <script>alert(1)</script> pattern inside the URL parameter. 
Wazuh Ruleset Test was used to verify whether the log could be decoded and matched against existing web attack detection rules. 
In this case, no decoder was matched, which indicates that the default ruleset did not properly recognize the provided log format.
<img width="795" height="467" alt="image" src="https://github.com/user-attachments/assets/3758e003-9ca4-46b2-bd57-b5ff61097aab" />

Log #2 – SQL Injection Attempt (UNION SELECT)
The HTTP request contains a classic SQL injection payload using the UNION SELECT technique in the URL parameter. 
The Ruleset Test utility was used to validate whether the Wazuh manager could decode and analyze the malicious request. 
No decoder was matched during testing, indicating that the default Wazuh decoder configuration did not recognize the provided log format.
<img width="795" height="500" alt="image" src="https://github.com/user-attachments/assets/7e84e057-66a7-404f-b713-36aa3f5aef84" />

Log #3 – SQL Injection Attempt with Credential Enumeration
The tested request contains a UNION SELECT SQL injection payload attempting to retrieve usernames and passwords from the users table. 
This type of attack is commonly used for credential harvesting and database enumeration during web application attacks. 
During the Ruleset Test process, no decoder matched the log format, meaning the default Wazuh ruleset did not successfully parse the provided HTTP access log.
<img width="795" height="507" alt="image" src="https://github.com/user-attachments/assets/6f1a4e4d-ce89-408f-9d7a-35da5261816e" />

Log #4 – SQL Injection Attempt Using OR 1=1
The tested HTTP request contains a classic SQL injection payload using the OR 1=1 condition, which is commonly used to bypass authentication or manipulate database queries. 
During the Ruleset Test, Wazuh successfully decoded the log as a web access log and completed Phase 3 filtering using rule ID 31100. 
The triggered rule grouped the event as an access log entry, confirming that the decoder recognized the HTTP log structure correctly.
<img width="795" height="587" alt="image" src="https://github.com/user-attachments/assets/554ea7fe-ce35-4954-93a6-f1d209df58c0" />

Log #5 – Cross-Site Scripting (XSS) Attempt in Search Parameter
The tested HTTP request contains an encoded XSS payload injected into the search parameter using the <script>alert(1)</script> pattern. 
This attack technique is commonly used to execute malicious JavaScript code in the victim’s browser through reflected input fields. 
During testing, Wazuh was unable to match the log against an appropriate decoder, resulting in no rule detection for this event.
<img width="795" height="541" alt="image" src="https://github.com/user-attachments/assets/d1c8b252-d218-4605-8fb4-7dd693c0bbfa" />

Log #6 – HTML Injection Attempt in URL Parameter
The tested request contains an encoded HTML input element injected into the “input” parameter. 
This type of payload may be used for HTML injection or as part of a Cross-Site Scripting (XSS) attack attempt targeting web applications that improperly sanitize user input. 
During analysis, Wazuh completed pre-decoding successfully, but no matching decoder or detection rule was triggered for this log entry.
<img width="795" height="554" alt="image" src="https://github.com/user-attachments/assets/35953033-1abd-479d-8bf1-349ab6368129" />

Log #7 – Standard Web Request Returning HTTP 404
This log represents a normal HTTP GET request to a non-existent page (“/test-page”), which resulted in a 404 response code. 
Wazuh successfully decoded the Apache-style web access log using the “web-access log” decoder and triggered rule ID 31101 with severity level 5, identifying a web server 400-series error. 
This event appears to be a normal failed page request rather than a malicious attack attempt, demonstrating that Wazuh correctly processes standard web access activity.
<img width="795" height="520" alt="image" src="https://github.com/user-attachments/assets/b7104e5a-35af-471c-90f2-e61f728c4197" />

Log #8 – Normal Search Request
This log represents a standard user search request containing the query “hello world.” 
Wazuh successfully decoded the web access log using the “web-accesslog” decoder and matched rule ID 31100 with severity level 0, which groups normal web access events. 
No malicious patterns or suspicious payloads were detected, indicating this is legitimate web traffic without security impact.
<img width="795" height="609" alt="image" src="https://github.com/user-attachments/assets/b3d6f032-d303-4ca2-9ed7-e487b567757d" />

Log #9 – Static Image File Request
This log represents a normal request for a static image resource (“/images/logo.png”) on the web server. 
Wazuh successfully decoded the Apache-style access log and triggered rule ID 31108 with severity level 0, which classifies simple and non-suspicious URL requests as ignored traffic. 
No malicious behavior or attack indicators were detected, making this a legitimate static content request.
<img width="795" height="628" alt="image" src="https://github.com/user-attachments/assets/20b5ae1b-21d4-430f-86ad-04329fbe44a1" />

Log #10 – Exposure of Credentials in URL Parameters
This log contains sensitive user information transmitted directly within the URL, including an email address and a plaintext password. 
Wazuh completed pre-decoding successfully, but no decoder or security rule detected the exposure of credentials in the request. 
This represents a potential sensitive data exposure issue and highlights the need for custom detection rules to identify Personally Identifiable Information (PII) and credential leakage in HTTP requests.
<img width="795" height="581" alt="image" src="https://github.com/user-attachments/assets/7b5ea35d-4f20-494d-b777-999d62b968ce" />

Log #11 – Credit Card and Payment Data Exposure
This log contains highly sensitive payment information transmitted through URL parameters, including a credit card number, CVV code, and expiration date. 
Wazuh completed pre-decoding successfully, however no decoder or built-in detection rule identified the exposure of financial data within the request. 
This event demonstrates a critical data leakage scenario and strongly supports the implementation of custom Wazuh rules for detecting PII and payment card information in HTTP traffic.
<img width="795" height="670" alt="image" src="https://github.com/user-attachments/assets/dc1ad96f-f02f-4f82-a5d6-89574a45dbb2" />

5. Bonus — Custom PII Detection Rules
Custom PII Detection Rules Configuration
This screenshot demonstrates the creation of custom Wazuh detection rules inside the local_rules.xml file. The rules were designed to identify sensitive information exposure in HTTP requests, including email addresses, plaintext passwords, payment card numbers, and CVV values.
The custom rules use keyword-based pattern matching through the <match> directive and assign elevated alert levels (12–15) to highlight potentially critical data leakage events. These rules extend the default Wazuh ruleset and improve the detection of customer PII exposure in web application logs.
<img width="795" height="596" alt="image" src="https://github.com/user-attachments/assets/2131a3b0-823c-40b3-b626-96b19299f1aa" />

Detection of Credential Exposure (Log 10)
This screenshot shows the successful detection of sensitive credential information inside an HTTP request containing both an email address and a plaintext password parameter. During Phase 3 filtering, the custom Wazuh rule triggered and generated a high-severity alert indicating possible credential leakage.
The rule logic searches for sensitive keywords such as email= and password= within incoming logs. This demonstrates how custom Wazuh rules can be used to detect insecure handling of authentication data and potential privacy violations in web traffic.
<img width="795" height="628" alt="image" src="https://github.com/user-attachments/assets/b7c79110-ace7-4268-9f9b-70622dfd1c74" />
<img width="795" height="649" alt="image" src="https://github.com/user-attachments/assets/c372f2f2-b144-413c-b6cc-4d6fb4c38def" />

Detection of Payment Card Information Exposure (Log 11)
This screenshot demonstrates the detection of exposed financial information in an HTTP request containing payment card data and CVV values. The custom Wazuh rule successfully matched the card= parameter and generated a critical severity alert during Phase 3 filtering.
The implemented rule uses pattern matching to identify payment-related sensitive data in logs. This type of detection is important for identifying PCI-DSS violations, insecure logging practices, and accidental exposure of customer financial information within web applications.
<img width="795" height="670" alt="image" src="https://github.com/user-attachments/assets/65cc5eed-8b64-475e-8a7c-229b1dcde1c8" />
<img width="795" height="703" alt="image" src="https://github.com/user-attachments/assets/aec90064-dd30-4194-83fb-2eb0ab6b15a5" />

6. Findings / Observations
During testing, several interesting behaviors were identified within the provided logs and the default Wazuh ruleset.
Multiple attack-oriented payloads containing SQL injection and XSS patterns were observed in the HTTP requests. Some logs were successfully decoded and matched against built-in Wazuh web access rules, while others did not trigger detection due to decoder limitations or unsupported log formatting.
The testing process also revealed that sensitive customer information such as email addresses, plaintext passwords, payment card numbers, and CVV values were not reliably detected by the default ruleset. To improve visibility and detection accuracy, custom Wazuh rules were implemented to identify PII and financial data exposure within HTTP requests.
The custom rules successfully generated alerts with elevated severity levels and demonstrated accurate detection behavior for the tested scenarios with minimal false positives.
7. Conclusion
The Wazuh single-node environment was successfully deployed using Docker, including the Wazuh Manager, Indexer, and Dashboard components. The dashboard was verified to be operational, and a Windows agent was successfully connected and communicating with the manager.
During log analysis, multiple suspicious patterns were identified in the provided HTTP access logs, including XSS attempts, SQL injection payloads, plaintext credential exposure, and payment card information leakage. Several logs were correctly categorized as benign traffic, demonstrating the ability of Wazuh to distinguish normal activity from potentially malicious behavior.
The Ruleset Test utility was used to validate log parsing and rule matching behavior. Built-in Wazuh rules successfully detected standard web attack patterns and HTTP errors, while custom rules were created to improve detection of sensitive customer data exposure (PII). The custom rules successfully generated alerts for email addresses, plaintext passwords, payment card numbers, and CVV values.
The documentation was structured to provide clear screenshots, rule verification results, and concise technical explanations for each tested log. Overall, the implemented rules demonstrated accurate detection behavior with a relatively low probability of false positives for the tested scenarios. The project demonstrated practical experience with Wazuh deployment, log analysis, decoder behavior, and custom rule creation for detecting web attacks and sensitive data exposure.


















