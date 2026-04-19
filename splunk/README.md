# 📊 Splunk Detection Dashboard

Detection of brute force attacks using Windows Event Logs and correlation logic in Splunk.

---

## 🎯 Objective

Build a centralized detection dashboard to identify brute force attacks based on Windows authentication logs and behavioral patterns.

Zbudowanie dashboardu detekcyjnego do wykrywania brute force na podstawie logów uwierzytelniania Windows oraz analizy zachowań.

---

## 🧱 Data Sources

* Windows Event Logs (Security)
* Event ID 4625 – Failed Logon
* Event ID 4624 – Successful Logon (context)

---

## ⚙️ Detection Scope

### 🔹 Brute Force Detection

* Multiple failed login attempts (Event ID 4625)
* Repeated attempts from same source
* Targeting specific accounts

### 🔹 Correlation Logic

* Source IP ↔ Target User
* Failed logons ↔ Successful logon
* Time-based aggregation (spikes)

---

## 📊 Dashboard Panels

### 🔹 Top Attacking IPs

* Identifies most active sources generating failed logins

### 🔹 Failed Logins Over Time

* Visualizes spikes in authentication failures

### 🔹 Top Targeted Accounts

* Highlights most frequently attacked users

### 🔹 Brute Force Correlation (IP + User)

* Core detection panel including:

  * source IP
  * target account
  * number of attempts
  * severity level

### 🔹 MITRE ATT&CK Mapping

* **T1110** – Brute Force
* **T1078** – Valid Accounts

---

## 🧠 Detection Logic

### 🔹 Field Normalization

* `Source_Network_Address`
* `IpAddress`
* `WorkstationName`

```spl
| eval src=coalesce(Source_Network_Address, IpAddress, WorkstationName)
```

---

### 🔹 User Normalization

* `TargetUserName`
* `Account_Name`

```spl
| eval user=coalesce(TargetUserName, Account_Name)
```

---

### 🔹 Noise Filtering

* Excluding localhost traffic:

```spl
| search NOT src IN ("127.0.0.1","::1")
```

---

### 🔹 Severity Scoring

```spl
| eval severity=case(
    attempts>=10,"HIGH",
    attempts>=5,"MEDIUM",
    true(),"LOW"
)
```

---

## 🔍 Detection Approach

* Log analysis (Windows Security Logs)
* SIEM correlation (Splunk)
* Behavioral detection (pattern-based)
* Aggregation and thresholding
* Reduction of false positives

---

## 📸 Visualization

See `dashboard.png` for full dashboard view.

---

## 🚀 Outcome

The dashboard enables detection of brute force activity through:

* correlation of authentication events
* identification of attack patterns
* time-based anomaly detection
* severity classification

Represents a practical SOC detection workflow using SIEM.

---

## 🧩 Skills Demonstrated

* SIEM (Splunk)
* Log analysis & event correlation
* Detection engineering mindset
* MITRE ATT&CK mapping
* Threat detection design

---

## 📌 Notes

Most activity in the lab environment originates from local simulation (PowerShell), with additional remote attempts (Kali Linux) to emulate real-world attack behavior.

This reflects realistic limitations such as system throttling and connection constraints.
