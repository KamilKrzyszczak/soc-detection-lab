# 🔎 Splunk Detection Queries

Collection of SPL queries used to detect brute force activity based on Windows Security Event Logs.

These queries were used to build the detection logic behind the Splunk dashboard.

---

## 🎯 Objective

Provide practical detection queries for identifying brute force attacks and suspicious authentication behavior.

---

## 📊 Data Source

- Windows Event Logs (Security)
- Event ID 4625 – Failed Logon
- Event ID 4624 – Successful Logon (context)

---

## 🧪 Observations

During testing:

- Most failed login attempts came from a single source IP (Kali Linux)
- One account (`labuser`) was targeted significantly more than others
- A successful login after multiple failures clearly indicated potential account compromise

These observations helped validate the detection logic used in the dashboard.

---

## 📁 Queries

### 1. Top Attacking IPs

Shows which IP addresses generate the most failed login attempts.

    index=windows EventCode=4625
    | stats count by Source_Network_Address
    | sort -count

---

### 2. Failed Logins Over Time

Shows spikes in failed authentication attempts over time.

    index=windows EventCode=4625
    | timechart span=5m count

---

### 3. Top Targeted Accounts

Identifies accounts that are most frequently targeted.

    index=windows EventCode=4625
    | stats count by Account_Name
    | sort -count

---

### 4. Brute Force Correlation (IP + User)

Correlates source IP with targeted accounts and assigns severity based on number of attempts.

    index=windows EventCode=4625
    | eval src=coalesce(Source_Network_Address, IpAddress, WorkstationName)
    | eval user=coalesce(TargetUserName, Account_Name)
    | search NOT src IN ("-","::1")
    | stats count as attempts by src, user
    | eval severity=case(
        attempts>=10,"HIGH",
        attempts>=5,"MEDIUM",
        true(),"LOW"
    )
    | sort -attempts

**Note:**  
Thresholds (5 / 10 attempts) are example values and should be adjusted based on normal environment behavior.

---

### 5. MITRE ATT&CK Mapping

Maps authentication activity to MITRE ATT&CK based on behavior patterns.

    index=windows (EventCode=4625 OR EventCode=4624)
    | eval MITRE_Technique=case(
        EventCode=4625, "T1110 - Brute Force",
        EventCode=4624, "T1078 - Valid Accounts"
    )
    | eval MITRE_Tactic=case(
        EventCode=4625, "Credential Access",
        EventCode=4624, "Initial Access"
    )
    | stats count as events by MITRE_Tactic, MITRE_Technique
    | sort -events

**Note:**  
This mapping is based on correlated behavior, not single events.

---

## 🚀 Outcome

These queries demonstrate:

- Log analysis using Windows Security Logs  
- Event correlation (IP + user + time)  
- Detection of brute force patterns  
- Basic threat classification  

They form the foundation of a SOC-style detection workflow in Splunk.
