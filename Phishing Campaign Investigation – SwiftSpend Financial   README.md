Phishing Campaign Investigation – SwiftSpend Financial
Alert Information
Field	Value
Incident Type	Phishing Campaign
Analyst Role	SOC Analyst L1
Affected Organization	SwiftSpend Financial
Detection Method	User Report
Severity	High
Scenario
Several employees at SwiftSpend Financial reported receiving suspicious emails requesting credential verification. Some users had already entered their login credentials, resulting in unauthorized account access.
The SOC team initiated an investigation to identify the phishing infrastructure, attacker techniques, and affected users.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/d2f593cf-1bc1-489e-8f4c-ebebab30cf7d" />

Email Analysis
Analysis of the provided email samples revealed that one of the employees, William McClean, received a phishing email containing a malicious PDF attachment.
The email impersonated a financial department communication to trick the user into interacting with the content.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/9984cbf9-68c1-4b3b-972e-7e1bdb318e86" />

Attacker Email Address
Further investigation identified the sender address used by the attacker:
Accounts.Payable@groupmarketingonline.icu
This domain was used to distribute phishing emails across the organization.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/7be50d34-5760-4360-82f0-809564f670e1" />

Phishing URL
The email contained a malicious redirection link designed to harvest user credentials.
hxxp://kennaroads[.]buzz/data/Update365/office365/40e7baa2f826a57fcf04e5202526f8bd/?email=zoe[.]duncan@swiftspend[.]finance&er
This URL redirected victims to a fake Office365 login page.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/2bdf6f22-98ad-4fe4-af30-683c2515182c" />
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/c5bd56cd-c01b-4cfc-a36f-76858d134839" />

Phishing Kit Discovery
During the investigation, analysts identified a phishing kit hosted on the attacker infrastructure.
hxxp://kennaroads[.]buzz/data/Update365[.]zip
This archive contained the phishing website files used to collect victim credentials.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/6bf9ac12-82ab-4c08-8aa5-1fb78620ff6e" />

File Hash Identification
The phishing kit archive was analysed and its SHA256 hash was calculated.
ba3c1519b419eb08c7b2652b8b6b39b406ef300ae8a18fee4d16b19ac9686
This hash can be used to detect the phishing kit in threat intelligence databases.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/51d67d80-69b6-4224-95b0-c6451227d97b" />

Threat Intelligence Findings
Threat intelligence analysis revealed that the phishing kit archive was first submitted on:
2020-04-08 21:55:50 UTC
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/65c01f02-ebdc-4dc2-836e-94c2d6e82483" />

SSL Certificate Analysis
The SSL certificate associated with the phishing infrastructure was first logged on:
2020-06-25
This suggests that the attacker infrastructure had been active for some time before the phishing campaign.

Compromised User
Investigation of the phishing kit logs revealed that the user:
michael.ascot@swiftspend.finance
submitted their credentials twice, confirming a successful credential harvesting event.
<img width="974" height="548" alt="image" src="https://github.com/user-attachments/assets/8c0d2780-9fa8-4238-9805-42f3741f3baa" />

Credential Collection Email
The phishing kit was configured to send stolen credentials to the attacker email address:
m3npat@yandex.com
Additional Attacker Email
An additional attacker email found in the phishing kit configuration was:
jamestanner2299@gmail.com
Indicators of Compromise (IOCs)
Type	Indicator
Email	Accounts.Payable@groupmarketingonline.icu
Domain	kennaroads.buzz
URL	hxxp://kennaroads[.]buzz/data/Update365
Hash	ba3c1519b419eb08c7b2652b8b6b39b406ef300ae8a18fee4d16b19ac9686
Email	m3npat@yandex.com
Email	jamestanner2299@gmail.com

Conclusion
The investigation confirmed a targeted phishing campaign against SwiftSpend Financial employees. The attacker used a malicious domain to host a phishing page that mimicked a legitimate Office365 login portal.
At least one user submitted their credentials, which were exfiltrated to attacker-controlled email accounts.
Immediate mitigation actions should include password resets for affected users, domain blocking, and security awareness training.

Recommended Actions
•	Reset passwords for affected users
•	Block malicious domains and URLs
•	Block attacker email addresses
•	Monitor for additional phishing attempts
•	Implement phishing awareness training











