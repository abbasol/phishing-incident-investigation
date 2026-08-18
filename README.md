# Phishing Incident Investigation

## Overview

This project documents a simulated SOC investigation of a suspected Microsoft 365 phishing incident.

The investigation focuses on identifying phishing indicators, analyzing email authentication results, performing domain and DNS analysis, extracting IOCs, correlating evidence, and mapping the observed behavior to the MITRE ATT&CK framework.

## Scenario

An employee reported an email claiming to be from Microsoft 365 Security.

The email requested immediate account verification and contained a suspicious verification URL.

The objective of the investigation was to determine whether the message represented a phishing attempt and whether evidence of successful compromise could be identified.

## Investigation Workflow

The investigation followed a structured SOC workflow:

1. Case Setup
2. URL and Domain Analysis
3. WHOIS Analysis
4. DNS Analysis
5. Threat Intelligence Analysis
6. Email Authentication Analysis
7. IOC Extraction
8. IOC Correlation
9. MITRE ATT&CK Mapping
10. Incident Assessment
11. Incident Reporting

## Tools Used

- Kali Linux
- WHOIS
- dig
- grep
- VirusTotal
- MITRE ATT&CK
- Nano
- Linux command-line utilities

## Key Findings

- Microsoft lookalike sender domain identified.
- Suspicious account-verification URL identified.
- SPF authentication failed.
- DKIM signature was not present.
- DMARC authentication failed.
- Investigated domain returned NXDOMAIN.
- WHOIS returned no matching registration.
- No malicious detections were reported by VirusTotal for the queried domain.
- No evidence confirmed successful credential theft or endpoint compromise.

## MITRE ATT&CK

### T1566.002 — Phishing: Spearphishing Link

The observed behavior was mapped to T1566.002 based on the suspicious email containing a link intended to direct the recipient toward a fraudulent account-verification workflow.

## Final Assessment

**Classification:** Phishing Attempt

**Confidence:** High

**Severity:** Medium

**Compromise Status:** Not Confirmed

## Project Structure

```text
phishing-investigation/
├── Evidence/
├── Screenshots/
├── IOCs/
│   └── iocs.txt
├── Investigation/
│   ├── mitre-mapping.txt
│   ├── timeline.txt
│   └── verdict.txt
├── Report/
│   └── incident-report.md
├── case-notes.txt
├── phishing-email.txt
└── README.md
