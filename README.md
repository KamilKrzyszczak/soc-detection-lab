# 🔐 SOC Detection Lab

Hands-on cybersecurity project focused on detecting and analyzing real-world attacks using Windows Event Logs, Sysmon, and SIEM tools (Splunk).

---

## 🎯 Objective

Build a practical SOC environment and simulate real-world attacks to detect, analyze, investigate, and respond like a Security Operations Center (SOC) analyst.

---

## 🧱 Lab Environment

- Windows (victim machine with Sysmon)
- Kali Linux (attacker)
- Sysmon (advanced endpoint logging)
- Splunk (SIEM for log analysis and correlation)
- VirtualBox

---

## 💣 Attack Scenarios

### 1. Brute Force Attack
- RDP login attempts from Kali Linux
- Multiple failed login attempts (brute force pattern)
- Detection via Event ID 4625 (failed logon) and Event ID 4624 (successful logon)
- Correlation of failed and successful logins

---

### 2. Suspicious PowerShell Activity
- Execution of encoded or obfuscated PowerShell commands
- Detection via Script Block Logging (Event ID 4104)
- Identification of suspicious keywords (e.g. IEX, DownloadString, -enc, -nop)

---

### 3. Reverse Shell
- Outbound connection from victim machine to attacker
- Detection via unusual network connections and process behavior
- Identification of suspicious parent-child process relationships

---

### 4. Malware Analysis
- Detection of suspicious processes (e.g. masquerading)
- Analysis of ransomware-like behavior
- Extraction of Indicators of Compromise (IOCs) such as:
  - Suspicious file paths (e.g. AppData)
  - Unusual process names
  - Network indicators (IP / URL)

---

## 🧠 Detection Approach

- Log analysis (Windows Event Logs, Sysmon)
- SIEM correlation (Splunk)
- Behavioral detection (not only signature-based)
- Correlation of multiple events to identify attack patterns
- Identification of suspicious activity and anomalies

---

## 📊 Skills Demonstrated

- Log analysis & threat detection
- Incident investigation & triage
- Malware analysis fundamentals
- SIEM usage (Splunk)
- Understanding attacker behavior (TTPs)

---

## 🚀 Status

🛠️ In progress – actively building, testing, and improving detection scenarios based on real-world attack techniques.
