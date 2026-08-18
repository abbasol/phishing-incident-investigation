# Phishing Incident Investigation Report

## 1. Executive Summary

A suspected Microsoft 365 phishing incident was investigated after an employee reported a suspicious account verification email.

The investigation identified multiple indicators consistent with a phishing attempt, including a Microsoft lookalike sender domain, a suspicious verification URL, SPF authentication failure, absence of a valid DKIM signature, and DMARC authentication failure.

WHOIS and DNS analysis also indicated that the investigated domains were not registered or resolving at the time of analysis. VirusTotal did not report malicious detections for the queried domain.

Based on the available evidence, the incident was classified as a high-confidence phishing attempt with a Medium severity rating. However, there was no evidence confirming successful credential theft or endpoint compromise.

## 2. Incident Overview

### Incident Type
Phishing / Credential Theft Attempt

### Detection Source
Employee-reported suspicious email

### Date and Time
17 August 2026, 09:14:22 +0300

### Affected User
employee@nexacorp.local

### Impersonated Brand
Microsoft 365

### Initial Sender
Microsoft 365 Security <security@micros0ft365-security.com>

### Suspicious URL
https://login-microsoft365-security.com/verify

### Initial Trigger
The employee received an email claiming that their Microsoft 365 account required immediate verification. The message instructed the recipient to access a verification link to avoid account suspension.

### Investigation Objective
The objective of this investigation was to determine whether the reported email represented a phishing attempt, identify relevant indicators of compromise, assess the credibility of the sender and URL, and determine whether evidence of successful compromise was present.


## 3. Investigation Methodology

The investigation followed a structured SOC workflow designed to analyze the reported phishing email without directly accessing the suspicious destination.

### 3.1 Evidence Collection
The reported email, sender information, URL, authentication results, and relevant network indicators were collected as the initial evidence set.

### 3.2 URL and Domain Analysis
The suspicious URL was parsed to identify its scheme, domain, and path. The associated domains were investigated using WHOIS and DNS queries to determine registration and resolution status.

### 3.3 Threat Intelligence Analysis
The primary suspicious domain was queried through VirusTotal to identify available reputation and detection information.

### 3.4 Email Authentication Analysis
The email headers and authentication results were reviewed, including:
- SPF
- DKIM
- DMARC
- Sender domain
- Reply-To address
- Source IP

### 3.5 IOC Extraction and Correlation
Potential Indicators of Compromise (IOCs) were extracted from the email and correlated across the available evidence.

### 3.6 MITRE ATT&CK Mapping
The observed phishing behavior was mapped to the relevant MITRE ATT&CK technique, T1566.002 — Phishing: Spearphishing Link.

### 3.7 Assessment
The collected evidence was evaluated collectively to determine the incident classification, severity, and whether successful compromise could be confirmed.


## 4. Evidence & Findings

### Finding 01 — WHOIS Analysis

**Indicator:** `login-microsoft365-security.com`

**Tool:** WHOIS

**Result:**  
No match was found for the investigated domain in the `.com` WHOIS registry at the time of analysis.

**Assessment:**  
The domain did not appear to have an active registration in the queried registry.

---

### Finding 02 — DNS Analysis

**Indicator:** `login-microsoft365-security.com`

**Tool:** `dig`

**Result:**  
The DNS query returned `NXDOMAIN`.

**Assessment:**  
The domain did not resolve to an IPv4 address through the DNS infrastructure used during the investigation.

---

### Finding 03 — Threat Intelligence Analysis

**Indicator:** `login-microsoft365-security.com`

**Source:** VirusTotal

**Result:**  
No malicious detections were reported for the queried domain at the time of analysis.

**Assessment:**  
The absence of detections does not establish that the domain is legitimate. The result was evaluated alongside the other investigation findings.

---

### Finding 04 — URL Structure Analysis

**URL:** `https://login-microsoft365-security.com/verify`

**Components:**
- Scheme: HTTPS
- Domain: `login-microsoft365-security.com`
- Path: `/verify`

**Assessment:**  
The domain does not belong to the official `microsoft.com` domain. The `/verify` path is consistent with an account-verification theme commonly used in phishing scenarios.

---

### Finding 05 — Email Authentication Analysis

**Sender:** `security@micros0ft365-security.com`

**Authentication Results:**
- SPF: FAIL
- DKIM: NONE
- DMARC: FAIL

**Assessment:**  
The message failed SPF and DMARC authentication checks and did not contain a valid DKIM signature. The sender domain also uses a lookalike representation of Microsoft by replacing the letter `o` with the digit `0`.

These indicators collectively increase confidence that the message represents a phishing attempt.

---

### Finding 06 — IOC DNS Correlation

**Indicators Tested:**
- `micros0ft365-security.com`
- `mail.micros0ft365-security.com`
- `login-microsoft365-security.com`

**Result:**  
No A records were returned for the tested domains through the DNS resolver used during the investigation.

**Assessment:**  
No active IPv4 resolution was observed for the tested indicators at the time of analysis. The absence of DNS resolution alone does not establish whether an indicator is benign or malicious.

---

### Finding 07 — Source IP

**Source IP:** `203.0.113.45`

**Assessment:**  
The IP address was observed in the simulated email header as the originating mail server.

**Note:**  
`203.0.113.0/24` is reserved for documentation and example purposes. It is therefore treated as simulated evidence and not as a real attacker infrastructure indicator.

## 5. Indicators of Compromise (IOCs)

| Type | Indicator | Source | Classification | Notes |
|---|---|---|---|---|
| Domain | `micros0ft365-security.com` | Email Header | Suspicious | Microsoft lookalike domain |
| Domain | `mail.micros0ft365-security.com` | Received Header | Suspicious | Subdomain associated with sender infrastructure |
| Domain | `login-microsoft365-security.com` | Email URL | Suspicious | Used in account-verification URL |
| URL | `https://login-microsoft365-security.com/verify` | Email Body | Suspicious | Credential-verification themed URL |
| IP | `203.0.113.45` | Received Header | Simulated | Documentation/example IP; not treated as real malicious infrastructure |

## 6. MITRE ATT&CK Mapping

### T1566.002 — Phishing: Spearphishing Link

**Tactic:** Initial Access

**Evidence:**
- The employee received a suspicious email impersonating Microsoft 365.
- The email contained a link directing the recipient to an account-verification page.
- The sender used a Microsoft lookalike domain.
- SPF and DMARC authentication failed.

**Assessment:**
The observed behavior is consistent with the MITRE ATT&CK technique T1566.002 — Phishing: Spearphishing Link.

**Confidence:** High

**Limitation:**
No evidence confirms that the recipient submitted credentials or that the endpoint was successfully compromised.


---

## 7. Incident Timeline

| Time | Event |
|---|---|
| 17 Aug 2026 09:14:22 +0300 | Suspicious email received by `employee@nexacorp.local` |
| 17 Aug 2026 09:14:22 +0300 | Email identified as impersonating Microsoft 365 |
| 17 Aug 2026 09:14:22 +0300 | Suspicious verification URL identified |
| Investigation | WHOIS query returned no matching registration |
| Investigation | DNS query returned `NXDOMAIN` |
| Investigation | VirusTotal returned no malicious detections |
| Investigation | SPF authentication returned `FAIL` |
| Investigation | DKIM authentication returned `NONE` |
| Investigation | DMARC authentication returned `FAIL` |
| Investigation | Indicators were extracted and correlated |
| Investigation | Incident classified as a high-confidence phishing attempt |


---

## 8. Impact Assessment

### Confirmed Impact

No successful credential theft or endpoint compromise was confirmed during the investigation.

### Potential Impact

If the recipient had submitted credentials to the fraudulent verification page, the attacker could potentially have obtained account credentials and attempted unauthorized access to the user's Microsoft 365 account.

Potential consequences could include:

- Account takeover
- Unauthorized access to corporate email
- Credential reuse against other services
- Business email compromise
- Further phishing activity using the compromised account

### Current Risk Assessment

**Severity:** Medium

**Compromise Status:** Not Confirmed

**Phishing Confidence:** High


---

## 9. Recommended Response Actions

The following actions are recommended for a real-world SOC environment:

### Immediate Actions

1. Block the identified suspicious domains and URL at the appropriate security controls.
2. Search the organization's email logs for other recipients of the same message.
3. Search DNS, proxy, firewall, and web gateway logs for connections to the identified domains.
4. Determine whether the affected user accessed the suspicious URL.
5. If credentials were entered, immediately initiate the organization's account-compromise response procedure.

### Credential Protection

If credential submission is confirmed:

1. Force a password reset.
2. Revoke active sessions and authentication tokens where appropriate.
3. Review recent authentication activity.
4. Check for suspicious MFA activity or authentication attempts.
5. Review mailbox rules for unauthorized forwarding or persistence.

### Detection and Monitoring

Create or update detections for:

- Microsoft lookalike domains
- Suspicious account-verification URLs
- SPF/DMARC authentication failures
- Similar phishing email patterns
- Repeated access to known suspicious indicators

### User Awareness

Notify the affected user and reinforce phishing awareness, particularly around:

- Urgent account-verification requests
- Lookalike domains
- Unexpected login links
- Requests for credentials


---

## 10. Final Verdict

**Classification:** Phishing Attempt

**Confidence:** High

**Severity:** Medium

**Compromise Status:** Not Confirmed

### Analyst Conclusion

The collected evidence strongly indicates that the reported email represents a phishing attempt designed to impersonate Microsoft 365 and direct the recipient toward a suspicious account-verification URL.

The investigation identified multiple supporting indicators, including a Microsoft lookalike sender domain, a suspicious verification URL, SPF failure, absence of a DKIM signature, and DMARC failure.

WHOIS and DNS analysis did not identify an active registration or DNS resolution for the investigated domains at the time of analysis. VirusTotal also returned no malicious detections for the queried domain.

Although the evidence strongly supports a phishing classification, there is currently insufficient evidence to confirm successful credential theft or endpoint compromise.

Further investigation of email, proxy, DNS, authentication, and endpoint logs would be required to determine whether any user interaction or compromise occurred.


---

## 11. Investigation Limitations

This investigation was performed using a controlled training scenario and simulated evidence.

The source IP `203.0.113.45` belongs to the documentation/example address space and does not represent real attacker infrastructure.

The investigated domains were not treated as real malicious infrastructure, and no suspicious URL was directly accessed during the investigation.

VirusTotal and DNS results represent the state of the available information at the time of analysis and should not be interpreted as definitive proof that an indicator is benign.

No endpoint telemetry, authentication logs, proxy logs, or actual mailbox data were available to confirm successful credential theft or system compromise.


---

## 12. Analyst Summary

The investigation demonstrated a structured SOC workflow for analyzing a suspected phishing incident:

**Alert → Evidence Collection → URL Analysis → Domain Analysis → DNS Investigation → Threat Intelligence → Email Authentication → IOC Extraction → Correlation → MITRE ATT&CK Mapping → Assessment → Incident Reporting**

The final assessment was a **high-confidence phishing attempt with no confirmed compromise**.


