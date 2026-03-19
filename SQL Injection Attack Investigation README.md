# 🚨 SQL Injection Attack Investigation (LetsDefend SOC Lab)

## 📌 Incident Overview

* **Alert Name:** SQL Injection Detected
* **Severity:** High
* **Event ID:** 235
* **Date:** Mar 07, 2024
* **Attack Type:** Web Application Attack (SQL Injection)

A security alert was triggered indicating a potential SQL Injection attempt targeting a web server.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/804f8a36-416f-4ecd-a025-96d7625f7fa5" />

---

## 🖥️ Affected Asset

* **Hostname:** Atlanta-Server
* **IP Address:** 172.16.20.121
* **Operating System:** Ubuntu 20.04
* **Role:** Web Server
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/56bc3c51-4ff3-4ca0-a90e-f3976400b949" />

---

## 🌐 Source of Attack

* **IP Address:** 118.194.247.28
* **ASN:** AS4808 (China Unicom)
* **Country:** China
  <img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/d45df2b6-83f0-4b5f-a0c4-cfdc475fe6ff" />
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/b57adedd-62bf-42d4-8af0-d5f1ac33f017" />


### 🔎 Threat Intelligence Findings

* **VirusTotal:** 9 security vendors flagged the IP as malicious
* **AbuseIPDB:**

  * Reported **4304 times**
  * Associated with:

    * Brute-force attacks
    * Unauthorized access attempts
    * Web attacks

➡️ The IP is considered **highly suspicious and malicious**
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/cccff8f4-4d2f-44b8-9d9e-014e45e9a6be" />

---

## 🔍 Log Analysis

### 📄 Raw Log Evidence

```
GET /index.php?id=1%27 AND 1=1 ...
User-Agent: sqlmap/1.7.2
```

### 🧠 Key Findings

* `%27` → `'` (SQL Injection indicator)
* `AND 1=1` → classic injection test
* Presence of:

  * `SELECT`
  * `ASCII`
  * `SUBSTRING`
  * `information_schema`

➡️ Indicates:

* Database enumeration
* Data extraction attempt
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/7aab376f-6057-4310-a548-476fc7265c18" />


---

## 🤖 Attack Method

The attacker used:

* **Automated tool:** `sqlmap`
* **Technique:** Blind SQL Injection

### 🎯 Objectives:

* Extract database structure
* Retrieve sensitive data
* Possibly escalate to command execution

---

## ⚠️ Impact Assessment

* No confirmed evidence of successful exploitation
* However:

  * Multiple crafted payloads observed
  * Automated enumeration detected

➡️ Classified as:
**Confirmed attack attempt (True Positive)**

---

## 🛡️ Response Actions

* Investigated logs in SIEM (LetsDefend)
* Identified malicious payloads
* Correlated IP with threat intelligence sources
* Contained affected host (Atlanta-Server)
* Recommended blocking attacker IP
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/d6d0414c-0a66-4f48-8685-5a453d26e232" />

---

## 🔐 Root Cause

The web application is vulnerable to SQL Injection due to:

* Lack of input validation
* Absence of prepared statements
* Improper sanitization of user input

---

## ✅ Recommendations

* Implement **prepared statements / parameterized queries**
* Deploy **Web Application Firewall (WAF)**
* Sanitize all user inputs
* Monitor for repeated attack patterns
* Block malicious IP addresses

---

## 📚 Lessons Learned

* SQL Injection attacks are often automated (sqlmap)
* Presence of `UNION`, `SELECT`, `information_schema` is a strong indicator
* Threat intelligence correlation is critical
* Even unsuccessful attempts must be treated seriously

---

## 🧠 SOC Analyst Takeaway

This case demonstrates the importance of:

* Log analysis
* Understanding attack patterns
* Threat intelligence correlation
* Rapid response and containment

---

## 🏁 Conclusion

The investigation confirmed a **SQL Injection attack attempt using an automated tool (sqlmap)** targeting a web server.

Although no confirmed data exfiltration occurred, the presence of advanced payloads indicates a **serious vulnerability that requires remediation**.

---






