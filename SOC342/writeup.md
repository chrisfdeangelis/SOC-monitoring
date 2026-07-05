# Incident Investigation - SOC342: CVE-2025-53770 SharePoint ToolShell Authentication Bypass & RCE
[https://app.letsdefend.io/case-management/casedetail/cfdeangelis/320]

> **Status:** Complete  
> **Date:** July 22, 2025  
> **Platform:** LetsDefend  
> **Severity:** Critical

---

# Objective

Investigate an alert indicating exploitation of the SharePoint ToolShell vulnerability (CVE-2025-53770). Determine whether the HTTP traffic is malicious, whether remote code execution was successful, and identify the appropriate containment and escalation actions.

---

# Alert Information

| Field | Value |
|-------|-------|
| Alert Name | SOC342 - CVE-2025-53770 SharePoint ToolShell Auth Bypass and RCE |
| Date | July 22, 2025 |
| Severity | Critical |
| Source IP | 107.191.58.76 |
| Destination IP | 172.16.20.17 |
| Hostname | SharePoint01 |
| Detection Rule | SharePoint ToolShell Authentication Bypass & Remote Code Execution |

---

# Initial Triage

## Questions

- Why was the alert generated?
- Is the source external?
- Does the source have a malicious reputation?
- Was the SharePoint server successfully exploited?
- Is containment and escalation required?

### Initial Assessment

The alert identified inbound HTTP traffic targeting an internal SharePoint server. Given the detection rule referenced a known SharePoint authentication bypass and remote code execution vulnerability, the investigation focused on determining whether exploitation was successful.

---

# Investigation

## 1. Asset Identification

### Affected Asset

SharePoint01

### Destination IP

172.16.20.17

### Source IP

107.191.58.76

### Hostname

107.191.58.76.vultrusercontent.com

### Last Logon

July 22, 2025 13:07

---

## 2. Network Investigation

### Source

107.191.58.76

### Destination

172.16.20.17 (Internal SharePoint Server)

### Reputation Checks

- AbuseIPDB
- VirusTotal
- Cisco Talos

### Findings

- Source infrastructure belongs to Vultr cloud hosting.
- AbuseIPDB identified the address as having a malicious reputation.
- Traffic originated from the Internet toward an internal SharePoint server.
- No legitimate business purpose for the connection was identified.

---

## 3. HTTP Investigation

### Attack Type

Command Injection

### Observations

Inspection of the HTTP request indicated an attempt to exploit the SharePoint ToolShell vulnerability through command injection, allowing remote code execution against the SharePoint server.

No evidence suggested this activity was consistent with legitimate SharePoint administration.

---

## 4. Endpoint Investigation

### Command History

The endpoint command history revealed execution of the following process:

```cmd
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\csc.exe /out:C:\Windows\Temp\payload.exe C:\Windows\Temp\payload.cs
```

### Observations

- `csc.exe` (Microsoft C# Compiler) was used to compile attacker-supplied source code.
- Output executable was written to the Windows Temp directory.
- This strongly indicates successful remote code execution following exploitation of the SharePoint vulnerability.

### Persistence

No persistence mechanisms were investigated as part of this case.

---

## 5. Timeline

| Time | Event |
|------|-------|
| Alert Generated | Suspicious HTTP traffic detected |
| Investigation | HTTP request reviewed |
| Endpoint Review | Command history examined |
| Discovery | `csc.exe` compiled payload.exe |
| Response | Endpoint contained |
| Escalation | Tier 2 notified |

---

## 6. Evidence Collected

### Indicators of Compromise

#### IP Addresses

- 107.191.58.76

#### Hostnames

- 107.191.58.76.vultrusercontent.com
- SharePoint01

#### Commands

```cmd
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\csc.exe /out:C:\Windows\Temp\payload.exe C:\Windows\Temp\payload.cs
```

#### Files

- payload.cs
- payload.exe

---

# MITRE ATT&CK Mapping

| Technique | ID |
|-----------|----|
| Exploit Public-Facing Application | T1190 |
| Command and Scripting Interpreter | T1059 |
| Native API / System Binary Execution | T1127 |
| Ingress Tool Transfer | T1105 |

---

# Root Cause

An Internet-based attacker exploited the SharePoint ToolShell authentication bypass and remote code execution vulnerability (CVE-2025-53770). Following successful exploitation, the attacker leveraged the Microsoft C# compiler (`csc.exe`) to compile a malicious executable from source code placed in the Windows Temp directory.

---

# Impact Assessment

| Question | Result |
|-----------|--------|
| External attacker | Yes |
| Malicious HTTP request | Yes |
| Command injection observed | Yes |
| Remote code execution | Yes |
| Payload compiled | Yes |
| Endpoint compromised | Yes |
| Containment required | Yes |
| Tier 2 escalation required | Yes |

The endpoint command history provided clear evidence that attacker-controlled code executed successfully. Compilation of a malicious executable confirmed compromise of the SharePoint server.

---

# Response Actions

- Contained affected SharePoint server
- Verified malicious external source
- Confirmed successful exploitation
- Collected command history evidence
- Escalated incident to Tier 2
- Documented indicators of compromise

---

# Final Verdict

**Classification**

**True Positive**

**Summary**

Investigation confirmed successful exploitation of the SharePoint ToolShell vulnerability (CVE-2025-53770) through malicious HTTP traffic originating from an external cloud-hosted system. Review of the endpoint command history showed execution of the Microsoft C# compiler (`csc.exe`) to compile a malicious executable (`payload.exe`) from attacker-supplied source code.

Because remote code execution was successfully achieved on an internal SharePoint server, the affected host was immediately contained and the incident was escalated to Tier 2 for further investigation, remediation, and forensic analysis.

---

# Lessons Learned

- Public-facing applications should be prioritized for rapid patch management when critical vulnerabilities are disclosed.
- HTTP traffic should always be correlated with endpoint telemetry to confirm exploitation.
- Legitimate Windows binaries such as `csc.exe` can be abused by attackers to compile malicious payloads.
- Successful exploitation of Internet-facing infrastructure requires immediate containment and escalation.

---

# Skills Demonstrated

- Incident Triage
- Web Application Security
- HTTP Traffic Analysis
- Threat Intelligence
- IOC Analysis
- Windows Process Analysis
- Remote Code Execution Investigation
- Endpoint Investigation
- Incident Containment
- Tier 2 Escalation
- MITRE ATT&CK Mapping
- Incident Documentation
