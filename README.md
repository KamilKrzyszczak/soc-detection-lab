# 🔐 SOC Detection Lab

Hands-on cybersecurity project focused on detecting and analyzing real-world attack scenarios using Windows Event Logs, Sysmon, and SIEM tools (Splunk).

---

## 🎯 Objective

Build a practical SOC environment to simulate real-world attacks and develop detection, investigation, and response capabilities similar to a Security Operations Center (SOC) analyst.

---

## 🧱 Lab Environment

* Windows (victim machine with Sysmon)
* Kali Linux (attacker machine)
* Sysmon (enhanced endpoint visibility)
* Splunk (SIEM for log analysis and correlation)
* VirtualBox (lab environment)

---

## 💣 Attack Scenarios

### 1. Brute Force Attack

* RDP login attempts from Kali Linux
* Multiple failed login attempts (brute force pattern)
* Detection using:
  * Event ID 4625 (failed logon)
  * Event ID 4624 (successful logon)
* Correlation of failed and successful authentication events
* Identification of potential account compromise

---

### 2. Suspicious PowerShell Activity

* Execution of potentially malicious PowerShell commands
* Detection using:
  * Script Block Logging (Event ID 4104)
* Indicators:
  * Use of `IEX` (Invoke-Expression)
  * Use of `DownloadString`
  * External URL references
  * Obfuscation flags (e.g. `-enc`, `-nop`)
* Visibility into command execution and script content

---

### 3. Reverse Shell

* Outbound connection from victim to attacker
* Detection using:
  * Sysmon Event ID 3 (network connection)
  * Sysmon Event ID 1 (process creation)
* Indicators:
  * PowerShell spawning outbound connection
  * Unusual destination IP and port
  * Suspicious command line execution
* Correlation between process activity and network behavior

---

### 4. Malware Analysis (Planned)

* Detection of suspicious or masquerading processes
* Analysis of ransomware-like behavior patterns
* Extraction of Indicators of Compromise (IOCs):
  * Suspicious file paths (e.g. AppData)
  * Unusual process names
  * Network indicators (IP / URL)

---

## 🧠 Detection Approach

* Log analysis (Windows Event Logs, Sysmon)
* SIEM correlation (Splunk)
* Behavioral detection (beyond signature-based)
* Correlation of multiple events across different sources
* Identification of anomalies and attacker techniques

---

## 📊 Skills Demonstrated

* Log analysis & threat detection
* Incident investigation & triage
* Endpoint telemetry analysis (Sysmon)
* SIEM usage (Splunk)
* Understanding attacker behavior (TTPs)
* Detection engineering fundamentals

---

## 🚀 Status

🛠️ In progress — continuously expanding detection scenarios and improving analysis based on real-world attack techniques.
