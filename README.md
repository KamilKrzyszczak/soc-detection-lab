# 🔐 SOC Detection Lab

Hands-on cybersecurity project focused on detecting and analyzing real-world attack scenarios using Windows Event Logs, Sysmon, and SIEM tools (Splunk).

---

## 💡 Why This Lab Matters

This lab simulates real-world attack scenarios and demonstrates how a SOC analyst detects, analyzes, and responds to threats using log correlation and behavioral analysis.

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

**Detection using:**

* Event ID 4625 (failed logon)
* Event ID 4624 (successful logon)

**Analysis:**

* Correlation of failed and successful authentication events
* Identification of potential account compromise

---

### 2. Suspicious PowerShell Activity

* Execution of potentially malicious PowerShell commands

**Detection using:**

* Script Block Logging (Event ID 4104)

**Indicators:**

* Use of `IEX (Invoke-Expression)`
* Use of `DownloadString`
* External URL references
* Obfuscation flags (e.g. `-enc`, `-nop`)

**Analysis:**

* Visibility into command execution and script content

---

### 3. Reverse Shell Attack

* Outbound connection from victim to attacker

**Detection using:**

* Sysmon Event ID 3 (network connection)
* Sysmon Event ID 1 (process creation)

**Indicators:**

* PowerShell spawning outbound connection
* Unusual destination IP and port
* Suspicious command-line execution

**Analysis:**

* Correlation between process activity and network behavior

**Technique:**

* Command and Control (C2)

---

### 4. Malware Analysis – Masquerading Process (in progress)

* Detection of a suspicious process mimicking a legitimate system binary

**Detection using:**

* Sysmon Event ID 1 (process creation)

**Indicators:**

* Process name mimicking a legitimate system process (e.g. `svchost.exe`)
* Execution from unusual paths (`AppData`, `Temp`)
* Mismatch between process name and file location

**Analysis:**

* Identification of suspicious process execution based on abnormal path and behavior

**Technique:**

* Masquerading (MITRE ATT&CK T1036)

---

## 📊 Splunk Detection Dashboard

A custom SIEM dashboard was built in Splunk to detect brute force attacks using Windows Event Logs.

**Features:**

* Aggregation of failed login attempts (Event ID 4625)
* Correlation between source IP and targeted accounts
* Time-based visualization of attack spikes
* Severity scoring (LOW / MEDIUM / HIGH)
* MITRE ATT&CK mapping (T1110, T1078)

The dashboard provides a centralized view of attack activity and simulates a real-world SOC monitoring workflow.

📸 See `splunk/dashboards/dashboard.png` for the full dashboard view.

---

## 🔎 Detection Rules (Examples)

### Brute Force (Splunk)

```spl
index=windows EventCode=4625
| stats count by Account_Name, src_ip
| where count > 5
```

### Reverse Shell (Sysmon)

```text
EventCode=3 AND DestinationPort=4444
```

### Suspicious PowerShell

```text
EventCode=4104 AND ("IEX" OR "DownloadString")
```

---

## 🧠 Detection Approach

* Log analysis (Windows Event Logs, Sysmon)
* SIEM correlation (Splunk)
* Behavioral detection (beyond signature-based)
* Correlation of multiple events across different sources
* Identification of anomalies and attacker techniques
* Focus on reducing false positives and identifying high-confidence alerts

---

## 📊 Skills Demonstrated

* Log analysis & threat detection
* Incident investigation & triage
* Endpoint telemetry analysis (Sysmon)
* SIEM usage (Splunk)
* Understanding attacker behavior (TTPs)
* Detection engineering fundamentals
* Basic threat hunting mindset
* Alert triage & prioritization

---

## 🚀 Future Improvements

### 🛡️ Detection Engineering

* Develop custom detection rules (Sigma / Splunk queries)
* Improve alert logic to reduce false positives
* Correlate multi-stage attacks (e.g. PowerShell → reverse shell → persistence)

### 🔄 Persistence Techniques

* Simulate attacker persistence:

  * Scheduled Tasks
  * Registry Run Keys
  * Startup folder

* Detect persistence using:

  * Sysmon Event ID 1 (process creation)
  * Sysmon Event ID 13 (registry changes)

### 🧠 Advanced PowerShell Detection

* Analyze encoded and obfuscated commands (`-EncodedCommand`)
* Detect fileless attacks using Event ID 4104
* Identify suspicious parent-child relationships

### 🌐 SIEM & Automation

* Build dashboards in Splunk

* Create alerts for:

  * Brute force attempts
  * Reverse shell connections
  * Suspicious PowerShell activity

* Automate log correlation and alerting workflows

### 🧪 Additional Attack Scenarios

* Persistence & privilege escalation
* Basic lateral movement simulation
* Data exfiltration scenarios

### 📄 Incident Response

* Create full incident reports (PDF format)
* Document attack timeline and analysis
* Map techniques to MITRE ATT&CK

### 📈 Lab Expansion

* Integrate Wazuh
* Add more endpoints (multi-host environment)
* Simulate enterprise-like network

---

## 🛠️ Status

🛠️ In progress — continuously expanding detection scenarios and improving analysis based on real-world attack techniques.
