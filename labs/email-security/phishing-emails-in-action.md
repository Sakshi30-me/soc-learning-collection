# Phishing Emails in Action — Email Threat Investigation & Security Analysis

> **Author:** Sakshi Maurya
> 
> **Platform:** TryHackMe
> 
> **Room:** Phishing Emails in Action
> 
> **Project Type:** Security Investigation & Documentation
> 
> **Security Domain:** Email Security • SOC Operations • Incident Response • Threat Analysis
> 
> **Focus Areas:** Phishing Analysis • Email Security • MITRE ATT&CK • Defensive Security

---

# Project Overview

## Overview

This project documents my investigation of the **TryHackMe "Phishing Emails in Action"** room, where multiple phishing email samples were analyzed to understand how attackers exploit social engineering and technical deception to compromise users.

The investigation examined phishing campaigns impersonating trusted organizations such as PayPal, DHL, Apple, Netflix, Microsoft, and shipping providers. Each email demonstrated different attack techniques, including spoofed sender identities, malicious hyperlinks, shortened URLs, tracking pixels, credential harvesting portals, and weaponized attachments.

Rather than relying solely on obvious grammar mistakes, this investigation focused on identifying technical indicators of compromise (IOCs), understanding attacker objectives, and connecting the findings to Security Operations Center (SOC) investigations and defensive security practices.

---

# Project Objectives

* Analyze phishing email headers and metadata.
* Identify spoofed sender identities and suspicious domains.
* Investigate hyperlinks and URL redirection chains.
* Examine malicious attachments used in phishing campaigns.
* Understand credential harvesting techniques.
* Recognize common phishing indicators and social engineering tactics.
* Map observed behaviors to the MITRE ATT&CK framework.
* Connect technical findings to defensive security and organizational controls.

---

# Investigation Methodology

The investigation followed a structured workflow commonly used by SOC analysts when triaging suspicious emails.

## Phase 1 — Initial Email Assessment

* Review subject lines.
* Identify urgency indicators.
* Examine sender and recipient information.
* Inspect branding consistency.

---

## Phase 2 — Email Header Analysis

* Compare display names with actual sender addresses.
* Identify spoofed domains.
* Review recipient information, including BCC usage.
* Identify header inconsistencies.

---

## Phase 3 — Hyperlink Investigation

* Analyze visible hyperlinks.
* Inspect raw HTML source.
* Identify shortened URLs.
* Investigate redirect chains.
* Validate destination domains safely.

---

## Phase 4 — Attachment Analysis

* Examine PDF, Word Template, and Excel attachments.
* Identify embedded hyperlinks.
* Observe potential payload execution.
* Determine possible malware delivery mechanisms.

---

## Phase 5 — Threat Mapping & Reflection

* Identify phishing indicators.
* Extract Indicators of Compromise (IOCs).
* Map attacker techniques to MITRE ATT&CK.
* Evaluate defensive security controls.
* Reflect on investigative findings.

---

# Investigation Walkthrough

## Investigation 1 — Fake PayPal Purchase Receipt

### Observation

The email impersonated PayPal by presenting a fake purchase receipt and encouraging the recipient to cancel an unauthorized transaction immediately. Although the displayed sender appeared legitimate, the actual sender domain was unrelated to PayPal. The "Cancel the Order" button redirected to a shortened URL.

> **Figure 1:** Phishing email sample provided in the **TryHackMe "Phishing Emails in Action"** room, showing the spoofed sender address and urgent purchase notification.
>
> *Image source: TryHackMe – Phishing Emails in Action.*

<img width="993" height="520" alt="image" src="https://github.com/user-attachments/assets/cd84d115-c2f7-436a-a488-2230b9893b13" />

### Interpretation

The attacker leveraged urgency and trust in a well-known brand to encourage immediate user interaction. The shortened URL concealed the final destination, making manual verification difficult.

> **Figure 2:** HTML source inspection revealing the shortened URL embedded within the "Cancel the Order" button, demonstrating how the attacker concealed the final destination behind a URL shortening service.
>
> *Image Source: TryHackMe – "Phishing Emails in Action" room.*

<img width="1062" height="237" alt="image" src="https://github.com/user-attachments/assets/92101de1-11b2-466a-bea6-cd33c7b51a4b" />

### Risk

* Credential theft
* Malware delivery
* Financial fraud
* Account compromise

---

## Investigation 2 — Shipping Notification

### Observation

The email claimed a shipment required immediate attention. The display name suggested a legitimate distribution center, while the underlying sender domain was unrelated. Raw HTML inspection revealed embedded tracking pixels.
> **Figure 3:** Shipping notification phishing email demonstrating sender spoofing, fraudulent shipment tracking information, and social engineering designed to prompt user interaction.
>
> *Image Source: TryHackMe – "Phishing Emails in Action" room.*

<img width="1630" height="358" alt="image" src="https://github.com/user-attachments/assets/157600b7-2096-4594-bce4-a9af1d81553d" />


### Interpretation

`Tracking pixels` allowed the attacker to determine whether the email had been opened and whether the recipient represented an active target. The visible hyperlink also disguised its actual destination.
> **Figure 4:** HTML source highlighting the embedded tracking pixel and manipulated hyperlink used to monitor recipient activity and redirect users to a malicious destination.
>
> *Image Source: TryHackMe – "Phishing Emails in Action" room.*

<img width="520" height="200" alt="image" src="https://github.com/user-attachments/assets/6d31c24e-6d5b-4161-b700-f45ece7984aa" />

### Risk

* User tracking
* Credential theft
* Increased phishing targeting
* Confirmation of active email addresses

---

## Investigation 3 — Fake Document Download

### Observation

The email claimed an important fax document was available for download with an expiration deadline. Selecting the download button redirected through multiple websites before reaching a fake Microsoft login portal.
> **Figure 5:** Phishing email delivering a fake document notification, creating urgency by claiming the shared document is available for a limited time.
>
> *Image Source: TryHackMe – "Phishing Emails in Action" room.*

<img width="969" height="413" alt="image" src="https://github.com/user-attachments/assets/d3a2d805-cd53-43f9-b4a0-b06218fd97e1" />

### Interpretation

The attacker layered trusted brands such as Microsoft, OneDrive, and Adobe to establish credibility before presenting a credential harvesting page designed to collect usernames and passwords.
> **Figure 6:** Multi-stage phishing workflow leading from a fake document-sharing page to a credential harvesting portal impersonating trusted Microsoft services.
>
> *Image Source: TryHackMe – "Phishing Emails in Action" room.*

<img width="1408" height="768" alt="image" src="https://github.com/user-attachments/assets/0d8c0fa2-5e6f-4664-bebd-2ed397a9048e" />


### Risk

* Credential compromise
* Account takeover
* Business Email Compromise (BEC)
* Lateral movement

---

## Investigation 4 — Netflix Billing Scam

### Observation

The attacker impersonated Netflix billing and instructed the recipient to open an attached PDF document containing an embedded phishing hyperlink. Branding inconsistencies and spelling errors were also present.
> **Figure 7:** Netflix-themed phishing email impersonating a billing notification and instructing the recipient to review an attached document.
>
> *Image Source: TryHackMe – "Phishing Emails in Action" room.*

<img width="1495" height="431" alt="image" src="https://github.com/user-attachments/assets/6304b153-6e46-4895-a09e-6ef0192502bd" />

### Interpretation

Rather than placing a malicious hyperlink directly within the email, the attacker attempted to bypass security filtering by embedding the phishing URL inside the attachment.
> **Figure 8:** Malicious PDF attachment containing an embedded hyperlink designed to redirect the victim to a phishing website outside the legitimate Netflix domain.
>
> *Image Source: TryHackMe – "Phishing Emails in Action" room.*

<img width="652" height="567" alt="image" src="https://github.com/user-attachments/assets/402163d7-fb42-47b3-b486-fc43834f999f" />

### Risk

* Credential theft
* Malware infection
* Financial fraud

---

## Investigation 5 — Apple Purchase Notification

### Observation

The email body contained no message and relied entirely on a Microsoft Word Template (.dot) attachment. The recipient had been Blind Carbon Copied (BCC).
> **Figure 9:** Apple-themed phishing email leveraging a fake purchase notification while distributing the message using Blind Carbon Copy (BCC) to conceal recipients.
>
> *Image Source: TryHackMe – "Phishing Emails in Action" room.*

<img width="641" height="320" alt="image" src="https://github.com/user-attachments/assets/905bb5ea-64b1-4923-aca4-ed6e50267b0e" />

### Interpretation

Using BCC concealed the recipient list while distributing the phishing campaign to multiple users. The uncommon attachment format increased suspicion.

### Risk

* Phishing
* Malware delivery
* Credential harvesting

---

## Investigation 6 — DHL Shipment

### Observation

The email impersonated DHL and delivered an Excel spreadsheet containing a hyperlink that attempted to download an executable payload.
> **Figure 10:** DHL-themed phishing email impersonating a shipment notification and encouraging the recipient to open the attached spreadsheet.
>
> *Image Source: TryHackMe – "Phishing Emails in Action" room.*

<img width="1410" height="480" alt="image" src="https://github.com/user-attachments/assets/32c719e8-0f09-40da-ba47-b2467f9a9401" />

### Interpretation

The phishing campaign progressed from social engineering to malware execution by encouraging the user to download and execute a malicious executable.

### Risk

* Persistence
* Credential theft
* Data exfiltration
* Ransomware deployment

---

# Key Findings

## Finding 1 — Sender Display Names Cannot Be Trusted

The displayed sender name often appeared legitimate, while the underlying sender domain revealed impersonation attempts.

**Impact**

Users may trust familiar brands without verifying the actual sender domain.

**MITRE ATT&CK**

* T1036 – Masquerading

---

## Finding 2 — URL Shortening Obscures Malicious Destinations

Shortened URLs concealed the final landing page, making manual verification difficult.

**Impact**

Users are more likely to interact with malicious links.

**MITRE ATT&CK**

* T1566.002 – Spearphishing Link

---

## Finding 3 — Credential Harvesting Remains a Primary Objective

Several phishing campaigns redirected victims to fake authentication portals.

**Impact**

Compromised credentials may result in account takeover, privilege escalation, and unauthorized access.

**MITRE ATT&CK**

* T1056 – Input Capture

---

## Finding 4 — Attachments Continue to Deliver Malware

PDF, Word Template, and Excel attachments were used to bypass traditional email filtering.

**Impact**

Weaponized documents can initiate secondary malware delivery.

**MITRE ATT&CK**

* T1566.001 – Spearphishing Attachment

---

## Finding 5 — Tracking Pixels Provide Attacker Reconnaissance

Hidden tracking pixels enabled attackers to identify active recipients and monitor user engagement.

**Impact**

Successful email delivery and interaction can increase future phishing attempts.

---

# Indicators of Compromise (IOCs)

## Suspicious Domains

* beginpro[.]club
* sultanbogor[.]com

## Suspicious File Types

* PDF
* [.]dot
* [.]xlsx
* [.]exe

## Suspicious Executable

* regasms[.]exe

## Common Social Engineering Themes

* Unauthorized purchases
* Shipment tracking
* Billing failures
* Document downloads
* Account suspension
* Urgent action required

---

# MITRE ATT&CK Mapping

| Technique                             | ATT&CK ID |
| ------------------------------------- | --------- |
| Phishing                              | T1566     |
| Spearphishing Attachment              | T1566.001 |
| Spearphishing Link                    | T1566.002 |
| Masquerading                          | T1036     |
| User Execution                        | T1204     |
| Credential Harvesting / Input Capture | T1056     |

---

# SOC Perspective

## Detection Opportunities

* Sender domain mismatches
* Newly registered domains
* URL shortening services
* Office applications spawning child processes
* Unexpected executable downloads
* Suspicious authentication attempts
* Email attachment reputation
* URL reputation analysis

## Investigation Workflow

* Validate sender authenticity.
* Review SPF, DKIM, and DMARC results.
* Analyze hyperlinks safely using threat intelligence platforms.
* Sandbox suspicious attachments.
* Extract and enrich IOCs.
* Correlate endpoint and identity telemetry.
* Monitor for post-phishing account activity.

## Threat Hunting Opportunities

* Search for repeated sender domains.
* Hunt for identical phishing subjects.
* Monitor Office processes creating network connections.
* Identify multiple authentication failures following phishing campaigns.

---

# GRC Perspective

## Failed Controls

* Email authentication validation
* User verification of sender identity
* Attachment filtering
* Link inspection
* Security awareness

## Recommended Controls

### Technical Controls

* SPF
* DKIM
* DMARC
* Secure Email Gateway
* URL Filtering
* Attachment Sandboxing
* Endpoint Detection and Response (EDR)
* Multi-Factor Authentication (MFA)

### Administrative Controls

* Security Awareness Training
* Phishing Simulation Exercises
* Incident Response Procedures
* Email Usage Policies

### Security Framework Alignment

* NIST Cybersecurity Framework (CSF)
* NIST SP 800-61 Rev. 2
* ISO/IEC 27001
* CIS Controls v8

---

# Analyst Reflection

This investigation reinforced that modern phishing attacks rely on much more than poor grammar or suspicious wording. By examining email headers, hyperlinks, attachments, and attacker infrastructure, I gained a deeper understanding of how phishing campaigns are carefully designed to establish trust before exploiting user interaction.

The investigation also strengthened my ability to identify technical indicators of compromise, think from a defender's perspective, and connect individual observations into a complete attack chain. Mapping attacker behavior to MITRE ATT&CK and considering both operational detection and governance controls provided a broader understanding of how organizations can reduce phishing risk.

---

# Skills Demonstrated

* Email Header Analysis
* Phishing Investigation
* Threat Intelligence
* IOC Identification
* URL Investigation
* Attachment Analysis
* HTML Email Analysis
* Credential Harvesting Detection
* Malware Delivery Analysis
* Social Engineering Analysis
* MITRE ATT&CK Mapping
* SOC Investigation Methodology
* Risk Assessment
* Blue Team Documentation

---

# Final Takeaway

This investigation demonstrated that effective phishing analysis requires a structured approach that extends beyond identifying suspicious emails. By systematically examining sender authenticity, hyperlinks, attachments, and attacker infrastructure, it becomes possible to uncover the complete attack chain, identify actionable indicators of compromise, and support effective detection and response. The project strengthened practical skills in email security, phishing analysis, incident triage, threat investigation, and defensive security while reinforcing the importance of combining technical analysis with governance and risk management practices.

---

## Acknowledgements

This project is based on the **TryHackMe** room **"Phishing Emails in Action"**.

The screenshots included in this documentation originate from the TryHackMe learning environment and are used for educational and portfolio purposes with attribution.

All investigation methodology, technical analysis, documentation, security observations, SOC and GRC perspectives, MITRE ATT&CK mapping, and analyst reflections were independently developed and documented by **Sakshi Maurya**.
