# 🦠 Malicious Macro Execution Investigation (LetsDefend SOC Lab)

## 📌 Incident Overview

* **Alert Name:** Malicious Macro has been executed
* **Severity:** Medium
* **Event ID:** 231
* **Date:** Feb 28, 2024
* **Attack Type:** Malware (Macro-based attack)

A security alert was triggered indicating execution of a malicious macro embedded in a Microsoft Word document.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/d1a64daf-74b6-41d0-a86d-d4f26e693690" />

---

## 🖥️ Affected Asset

* **Hostname:** Jayne
* **IP Address:** 172.16.17.198
* **Operating System:** Windows 10
* **User:** LetsDefend
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/8c0e34bd-ec97-4c0c-ada6-c1801068f04b" />

---

## 📄 Suspicious File Details

* **File Name:** edit1-invoice.docm
* **File Path:**
  `C:\Users\LetsDefend\Downloads\edit1-invoice.docm`
* **File Type:** Microsoft Word Document with Macros (.docm)
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/4541300b-50a4-4ea1-8047-07441cd8666d" />

---

## 🔎 Threat Intelligence Analysis

### 📊 VirusTotal Results

* **33/66 engines detected the file as malicious**

### 🧠 Detection Highlights:

* VBA Macro Malware
* Trojan Downloader
* PowerShell-based payload execution
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/719f1785-b14f-438d-9a07-86d9b9e733b6" />

---

## 🔬 Behavioral Analysis

### 📌 Key Findings:

* Document contains embedded VBA macro
* Macro executes automatically upon user interaction
* Uses hidden execution (no visible window)
* Executes system commands via shell

### 🚨 Indicators of Malicious Behavior:

* Macro triggers command execution
* Possible use of:

  * PowerShell
  * Command execution via shell
* Potential payload download functionality

---

## 🎯 Attack Scenario

The attack likely followed this chain:

1. User downloads a malicious document (invoice-themed)
2. Opens the file
3. Enables macros
4. Embedded VBA macro executes
5. System command is triggered silently
6. Potential payload is downloaded/executed

---

## ⚠️ Impact Assessment

* Malicious macro execution confirmed
* File classified as **Trojan Downloader**
* Potential for:

  * Remote code execution
  * Payload delivery
  * System compromise

➡️ Classified as:
**True Positive – Confirmed Malware Execution**

---

## 🛡️ Response Actions

* Identified malicious file via EDR alert
* Verified file hash using VirusTotal
* Analyzed macro behavior
* Isolated affected host (containment applied)
* Prevented further spread
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/e2369b5f-328c-4515-a5c9-8624bcf23816" />

---

## 🔐 Root Cause

The incident occurred due to:

* User interaction with a malicious document
* Macros being enabled in Microsoft Office
* Lack of macro execution restrictions

---

## ✅ Recommendations

* Disable macros by default in Office
* Implement email filtering for attachments
* Use sandboxing for suspicious files
* Educate users on phishing attacks
* Deploy EDR with behavioral detection

---

## 📚 Lessons Learned

* `.docm` files are high-risk
* Macro-based malware often uses social engineering
* Invoice-themed attacks are common
* Early containment is critical

---

## 🧠 SOC Analyst Takeaway

This case highlights:

* Importance of file analysis
* Value of threat intelligence tools
* Need for endpoint containment
* Understanding of macro-based attack chains

---

## 🏁 Conclusion

The investigation confirmed execution of a **malicious macro embedded in a Word document**, likely used as a **Trojan downloader**.

The threat was successfully contained before further escalation, demonstrating effective detection and response.

---

