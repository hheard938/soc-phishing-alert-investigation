# SOC Phishing Alert Investigation
## Project Overview
This project documents a simulated Security Operations Center investigation completed in the TryHackMe SOC environment. The investigation involved triaging multiple phishing and firewall alerts, analyzing email content, searching SIEM logs, correlating network activity, checking domain reputation, and preparing an incident report.
## Objectives
- Investigate suspicious inbound emails
- Distinguish true positives from false positives
- Determine whether users accessed suspicious links
- Correlate email and firewall events
- Identify affected users and endpoints
- Document findings and recommend remediation
## Tools Used
- TryHackMe SOC Simulator
- Splunk SIEM
- Firewall logs
- Email logs
- TryDetectThis threat-intelligence tool
## Investigation Process
### 1. Alert Triage
Five related alerts were reviewed:
| Alert | Description | Classification |
|---|---|---|
| 8814 | Suspicious external link involving legitimate HubSpot activity | False Positive |
| 8815 | Fake Amazon package-delivery email | True Positive |
| 8816 | Access to a blacklisted shortened URL | True Positive |
| 8817 | Fake Microsoft account-security email | True Positive |
| 8818 | Legitimate HR onboarding email | False Positive |
### 2. SIEM Searches
The following searches were used to investigate email delivery and network activity:
```spl
datasource="firewall" SourceIP="10.20.2.17"
datasource="email" recipient="h.harris@thetrydaily.thm"
datasource="firewall" URL="*mlcrosoftsupport.co*"
datasource="firewall" URL="*hrconnex.thm*"
