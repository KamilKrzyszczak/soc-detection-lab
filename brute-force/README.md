# 🔐 Brute Force Attack Detection

Detection of brute force attack attempts against RDP using Windows Security Logs and log correlation.

---

## 🎯 Objective

Simulate a brute force attack on RDP and detect it using failed and successful authentication events.

---

## ⚔️ Attack Simulation

- Tool: Hydra
- Attacker: Kali Linux
- Victim: Windows (RDP enabled)

### Steps:

1. Attacker performs brute force attack using Hydra
2. Multiple failed login attempts are generated
3. Eventually a successful login occurs

---

## 📊 Logs Analysis

### Windows Security Event ID 4625 – Failed Logon

- Multiple failed authentication attempts
- Same source IP
- High frequency of events

**Why suspicious:**
Repeated login failures indicate password guessing attack

---

### Windows Security Event ID 4624 – Successful Logon

- Successful authentication after multiple failures
- Same source IP as failed attempts

**Why suspicious:**
Possible account compromise after brute force

---

## 🚨 Detection Logic

Brute force attack is identified when:

- High number of Event ID 4625 in short time
- Same source IP across attempts
- Followed by Event ID 4624 success

---

## 🔎 Investigation Findings

- Source IP: 192.168.56.101 (Kali)
- Target account: labuser
- Attack type: RDP brute force

**Indicators:**
- Multiple failed logins  
- Same username attempts  
- Successful login after failures  

**Outcome:**
Unauthorized access gained

---

## 🧬 MITRE ATT&CK Mapping

- **T1110** – Brute Force  

---

## 🔎 Detection Queries (Splunk)

### Failed Logons

EventCode=4625

### Successful Logon After Failures

EventCode=4624

---

## 📸 Evidence

### Brute Force Attack (Hydra)
![Brute Force](screenshots/hydra_attack.png)

### Failed Logons (Event ID 4625)
![4625 Failed](screenshots/event_4625_failed.png)

### Failed Logon Details
![4625 Details](screenshots/event_4625_details.png)

### Successful Logon (Event ID 4624)
![4624 Success](screenshots/event_4624_success.png)

### Successful Logon Details
![4624 Details](screenshots/event_4624_details.png)

---

## 🧠 Conclusion

Brute force attack successfully detected by correlating failed and successful authentication events.

---

## 🚀 Key Takeaways

- Repeated login failures are strong attack indicator  
- Correlation between events is critical  
- Authentication logs are key in detecting brute force attacks

