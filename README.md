# SOC Phishing Alert Investigation
## Project Overview
This project documents a simulated Security Operations Center investigation completed in the TryHackMe SOC environment. The investigation involved triaging five phishing and firewall alerts, analyzing email content, searching SIEM logs, correlating network activity, checking threat intelligence, classifying incidents, and preparing an escalation report.
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
| 8816 | Access to a blacklisted shortened URL blocked by the firewall | True Positive |
| 8817 | Fake Microsoft account-security email | True Positive |
| 8818 | Legitimate HR onboarding email | False Positive |
### 2. SIEM Searches
The following searches were used to investigate email delivery and network activity:
```spl
datasource="firewall" SourceIP="10.20.2.17"
datasource="email" recipient="h.harris@thetrydaily.thm"
datasource="firewall" URL="*mlcrosoftsupport.co*"
datasource="firewall" URL="*hrconnex.thm*"
```
### 3. Email and Firewall Correlation
Alert 8815 contained a fake Amazon package-delivery message with an urgent call to action and a shortened Bitly URL. Alert 8816 showed that endpoint `10.20.2.17` attempted to access the same URL. The firewall blocked the outbound connection to `67.199.248.11` over destination port `80`.
This correlation confirmed that the phishing email reached a user and the link was clicked, but the firewall prevented the connection from completing.
### 4. Additional Phishing Activity
Alert 8817 contained a fake Microsoft account-security message from a lookalike domain. A firewall search returned no matching connection attempts, indicating that the message was malicious but no click was detected.
### 5. False-Positive Validation
- Alert 8814 was traced to legitimate HubSpot activity involving `crm.hubspot.com` and was classified as a false positive.
- Alert 8818 was a legitimate HR onboarding email from `hrconnex.thm`. Threat-intelligence review returned a clean result, and no suspicious firewall activity was identified.
## Incident Timeline
| Time | Event |
|---|---|
| 21:00:55 | Fake Amazon package-delivery email received |
| 21:02:09 | User endpoint attempted to access the shortened URL |
| 21:02:09 | Firewall blocked the outbound connection |
| 21:03:13 | Fake Microsoft account-security email received |
| 21:03:41 | Legitimate HR onboarding email received |
## Indicators of Compromise
| Type | Indicator |
|---|---|
| Sender | `urgent@amazon.biz` |
| Malicious URL | `http://bit.ly/3sHkX3da12340` |
| Destination IP | `67.199.248.11` |
| Source endpoint | `10.20.2.17` |
| Lookalike domain | `mlcrosoftsupport.co` |
| Phishing URL | `https://mlcrosoftsupport.co/login` |
> These indicators belong to a simulated training environment and should not be treated as current real-world threat intelligence.
## Affected Entities
- `h.harris@thetrydaily.thm` — received the Amazon-themed phishing email
- `10.20.2.17` — attempted to access the malicious shortened URL
- `c.allen@thetrydaily.thm` — received the Microsoft-themed phishing email
## MITRE ATT&CK Mapping
- **T1566.002 — Phishing: Spearphishing Link**
- **T1204.001 — User Execution: Malicious Link**
## Findings
- Three alerts were confirmed as true positives.
- Two alerts were classified as false positives.
- A user clicked the link in the Amazon-themed phishing email.
- The firewall successfully blocked the outbound connection.
- No connection to the fake Microsoft domain was detected.
- The overall incident required escalation because confirmed phishing activity and user interaction occurred.
## Recommended Remediation
- Block the identified malicious domains, URL, sender, and destination IP.
- Remove related phishing emails from all user mailboxes.
- Review endpoint `10.20.2.17` for suspicious activity or compromise.
- Reset the affected user’s credentials if compromise is suspected.
- Require or verify multifactor authentication for affected accounts.
- Provide targeted phishing-awareness training.
- Monitor for additional connections involving the identified indicators.
## Investigation Evidence
The following screenshots document key stages of the simulated SOC investigation. Select a heading to expand its evidence image.
<details>
<summary><strong>1. Alert Queue and Initial Triage</strong></summary>
Reviewed and prioritized five related phishing and firewall alerts.
<img src="screenshots/01-alert-queue-overview.jpeg" alt="SOC alert queue overview" width="750">
</details>
<details>
<summary><strong>2. Firewall-Blocked Malicious URL</strong></summary>
Correlated firewall activity with the phishing email and confirmed that the shortened URL request was blocked.
<img src="screenshots/02-firewall-blocked-url.jpeg" alt="Firewall event showing blocked malicious URL" width="750">
</details>
<details>
<summary><strong>3. Amazon-Themed Phishing Email</strong></summary>
Analyzed a fake package-delivery message containing an urgent call to action and a suspicious shortened link.
<img src="screenshots/03-amazon-phishing-email.jpeg" alt="Amazon-themed phishing email evidence" width="750">
</details>
<details>
<summary><strong>4. Microsoft-Themed Phishing Email</strong></summary>
Identified a lookalike sender domain and fraudulent Microsoft account-security link.
<img src="screenshots/04-microsoft-phishing-email.jpeg" alt="Microsoft-themed phishing email evidence" width="750">
</details>
<details>
<summary><strong>5. Incident Classification and Reporting</strong></summary>
Documented the findings, affected entities, true-positive classification, remediation recommendations, and escalation decision.
<img src="screenshots/05-incident-report.jpeg" alt="SOC incident report and escalation decision" width="750">
</details>
## Skills Demonstrated
- SOC alert triage
- Phishing-email analysis
- Splunk search and event analysis
- Email and firewall-log correlation
- Indicator-of-compromise identification
- True-positive and false-positive classification
- MITRE ATT&CK mapping
- Incident reporting and escalation
- Remediation planning
## Disclaimer
This project was completed in a controlled TryHackMe training environment. All users, domains, IP addresses, messages, and events shown are simulated and are documented solely for educational and portfolio purposes.
