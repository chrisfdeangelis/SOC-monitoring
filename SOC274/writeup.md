# Incident Investigation - [SOC274 - Palo Alto Networks PAN-OS Command Injection Vulnerability Exploitation (CVE-2024-3400)]

> **Status:** Complete  
> **Date:** 2024-APR-18  
> **Platform:** LetsDefend
> **Severity:** Critical

---

# Objective

Briefly describe why the alert was generated and what this investigation aims to determine.

Example:

> Investigate a web attack (CVE-2024-3400).

---

# Alert Information

| Field | Value |
|-------|-------|
| Alert Name | SOC274 - Palo Alto Networks PAN-OS Command Injection Vulnerability Exploitation (CVE-2024-3400) |
| Time Detected | 2024-APR-18 03:09 |
| Severity | Critical |
| Source IP | 144.172.79.92 |
| Destination IP | 172.16.17.139 |
| Source User | |
| Destination User | |
| Hostname | PA-Firewall-01 |
|Requested URL| 172.16.17.139/global-protect/login.esp|

---

# Initial Triage

## Questions

- What triggered the alert?
- Is the source internal or external?
- Who owns the affected asset?
- Is the activity expected?
- What systems are involved?

### Initial Assessment

Write a short paragraph summarizing your first impression before digging deeper.

---

# Investigation

## 1. Asset Identification

### Affected User

-

### Hostname

-

### IP Address

-

### Asset Owner

-

### Last Logon

-

---

## 2. Network Investigation

### Source

-

### Destination

-

### Reputation Checks

- VirusTotal
- AbuseIPDB
- Cisco Talos
- WHOIS / ASN

### Findings

---

## 3. Email Investigation (if applicable)

### Sender

-

### Recipient

-

### Subject

-

### Attachments

-

### Embedded URLs

-

### Observations

---

## 4. Endpoint Investigation

### Process Tree

```
Parent Process
└── Child Process
    └── Grandchild Process
```

### Suspicious Processes

-

### Command Line

```powershell

```

### File Activity

-

### Registry Activity

-

### Persistence

-

---

## 5. Timeline

| Time | Event |
|------|-------|
| | |
| | |
| | |

---

## 6. Evidence Collected

### Indicators of Compromise

#### IP Addresses

-

#### Domains

-

#### URLs

-

#### File Hashes

-

#### File Names

-

#### Registry Keys

-

---

# MITRE ATT&CK Mapping

| Technique | ID |
|-----------|----|
| | |
| | |
| | |

---

# Root Cause

Describe how the attack occurred.

Example:

> The user clicked a phishing email which launched an obfuscated PowerShell command that executed `mshta.exe` against a remote payload.

---

# Impact Assessment

- Was the attack successful?
- Was malware executed?
- Was persistence established?
- Was lateral movement observed?
- Was data exfiltration observed?

---

# Response Actions

- [ ] Block malicious IP/domain
- [ ] Isolate endpoint
- [ ] Reset credentials
- [ ] Remove malicious files
- [ ] Notify user
- [ ] Escalate to Tier 2

---

# Final Verdict

**Classification**

- True Positive / False Positive

**Summary**

Write 1–3 paragraphs explaining exactly what happened, why you reached your conclusion, and what actions were taken.

---

# Lessons Learned

- What indicators were most useful?
- What detection logic caught the attack?
- What would you investigate next in a production environment?

---

# Skills Demonstrated

- Incident Triage
- Log Analysis
- Threat Hunting
- IOC Analysis
- PowerShell Analysis
- Windows Event Analysis
- Email Security
- Network Analysis
- Threat Intelligence
- MITRE ATT&CK Mapping
- Malware Analysis
