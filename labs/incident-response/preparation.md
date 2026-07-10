# Preparation — Security Readiness Assessment

**Platform:** TryHackMe

**Module:** Incident Response

**Lab Type:** Guided Learning & Practical Assessment

**Framework:** NIST SP 800-61r2

**Scenario:** Nexus Financial

**My Role:** Security Learner / SOC Analyst

**Focus Areas:** Incident Response · Security Governance · Organizational Readiness · People · Processes · Technology · Logging · Detection

---

# Project Overview

This project documents my study of the **Preparation** phase within the Incident Response lifecycle using the Nexus Financial scenario provided by `TryHackMe`.

Rather than responding to an active incident, this room focused on evaluating whether an organization possesses the necessary capabilities to respond effectively when an incident occurs. The assessment explored how preparation extends beyond security tools and depends equally on governance, documented procedures, trained personnel, and operational visibility.

The practical component involved reviewing `organizational documentation, security policies, and existing controls to identify strengths, weaknesses, and preparation gaps` that could influence future incident response activities.

**This documentation reflects my understanding of incident response readiness, organizational security posture, and the relationship between preparation and effective security operations.**

---

# Learning Objectives

This project focused on understanding how organizations prepare for security incidents before they occur.

Primary learning objectives included:

* Understanding the Preparation phase of the `Incident Response lifecycle`.
* Differentiating between `events`, `alerts`, and confirmed `incidents`.
* Understanding how `Incident Response frameworks` structure organizational processes.
* Evaluating the importance of `people`, `processes`, and `technology`.
* Understanding why `logging`, `monitoring`, and `detection` are essential for effective investigations.
* Identifying `organizational weaknesses` that increase incident response risk.

---

# Assessment Objectives

The assessment aimed to evaluate the organization's preparedness by answering questions such as:

* Is an Incident Response capability established?
* Are roles and responsibilities clearly defined?
* Are documented procedures available?
* Does the organization have sufficient visibility into its environment?
* Are detection capabilities capable of identifying malicious activity?
* Which preparation gaps may increase organizational risk?

---

# Incident Response Lifecycle


The Incident Response lifecycle provides a structured approach for managing security incidents. This project follows the NIST SP 800-61r2 framework, which organizes incident handling into four continuous phases. Each phase builds on the previous one, and lessons learned from an incident are fed back into the Preparation phase to improve future response capabilities.

<img width="937" height="635" alt="image" src="https://github.com/user-attachments/assets/99fe9c1a-faf9-4576-8ec4-38bc298c093b" />


The four phases include:

- **Preparation** – Establish people, processes, technology, policies, and logging capabilities before an incident occurs.
- **Detection & Analysis** – Validate alerts, investigate suspicious activity, and determine the scope of an incident.
- **Containment, Eradication & Recovery** – Limit the impact, remove the threat, and restore affected systems.
- **Post-Incident Activity** – Review the incident, document findings, identify root causes, and implement improvements.

---

# Assessment Methodology

The assessment followed a structured review process.

```text
Incident Response Fundamentals
            │
            ▼
Framework Review
            │
            ▼
People Assessment
            │
            ▼
Process Assessment
            │
            ▼
Technology Assessment
            │
            ▼
Logging & Detection Review
            │
            ▼
Preparation Gap Analysis
            │
            ▼
Security Readiness Assessment
```

Each stage focused on evaluating one aspect of organizational preparedness before moving to the next.

---

# Environment

| Component       | Details                                    |
| --------------- | ------------------------------------------ |
| Platform        | TryHackMe                                  |
| Scenario        | Nexus Financial                            |
| Framework       | NIST SP 800-61r2                           |
| Assessment Type | Incident Response Readiness                |
| Practical Focus | Documentation Review & Security Assessment |

---

# Core Concepts Explored

## Incident Response Fundamentals

The room introduced Incident Response as a structured process designed to minimize business impact through preparation, detection, containment, recovery, and continuous improvement.

Special attention was given to distinguishing between:

* Events
* Alerts
* Confirmed Incidents

Understanding these differences is fundamental because not every alert represents a security incident.

<img width="840" height="371" alt="image" src="https://github.com/user-attachments/assets/8d264971-992c-4428-9d9e-099b179ab4d5" />

---

## Incident Response Frameworks

The assessment explored how standardized frameworks provide consistency during security incidents.

Frameworks discussed included:

* NIST SP 800-61r2
* SANS PICERL

Rather than memorizing phases, the emphasis was on understanding why structured processes improve decision-making during high-pressure situations.

---

## People, Processes & Technology

The room demonstrated that effective incident response depends on balancing three core pillars.

### People

Assessment areas included:

* CSIRT responsibilities
* Security awareness
* Analyst training
* Role assignments
* Principle of Least Privilege

### Processes

Documentation reviewed included:

* Incident Response Plans
* Communication Plans
* Playbooks
* Chain of Custody
* Ticketing Procedures
* Asset Inventory

### Technology

Technologies supporting preparedness included:

* SIEM
* EDR
* IDPS
* DLP
* Threat Intelligence Platforms
* Digital Forensics Tools

---

## Visibility, Logging & Detection

A significant portion of the assessment focused on organizational visibility.

Topics explored included:

* Log collection
* Centralized monitoring
* Audit logging
* Detection engineering
* Detection gaps
* Investigation readiness

This reinforced the idea that collecting logs alone does not improve security unless meaningful detection logic exists.

---

# Practical Assessment

The practical component of this room focused on assessing Nexus Financial's preparedness for responding to security incidents. Rather than investigating an active attack, the assessment involved reviewing organizational documentation and local security configurations to identify strengths, weaknesses, and potential gaps in the organization's incident response capability.

The review covered organizational documentation, security policies, historical incidents, penetration testing results, asset management, and workstation security settings. The objective was not simply to answer the lab questions but to evaluate how preparation influences an organization's ability to effectively detect, respond to, and recover from security incidents.

---

## Hardware Asset Inventory

<img width="991" height="510" alt="Screenshot 2026-07-06 194314" src="https://github.com/user-attachments/assets/eb39680f-2769-4c0d-a5cf-d800b672eb30" />

The hardware asset inventory was reviewed to understand the organization's infrastructure, including system ownership, operating systems, and network addressing. Maintaining an accurate inventory is fundamental to incident response, as it enables responders to quickly identify affected assets, determine system ownership, and assess the scope of an incident.

---

## Penetration Test Findings

<img width="1002" height="580" alt="Screenshot 2026-07-06 194513" src="https://github.com/user-attachments/assets/1b805c08-9575-4f0a-8e47-7c9299e1a5b2" />

The penetration test report was examined to identify previously discovered security weaknesses and determine whether critical findings had been remediated. Reviewing historical assessment results helps prioritize remediation efforts and supports continuous improvement of the organization's security posture.

---

## Identity Security Assessment

<img width="917" height="515" alt="3Screenshot 2026-07-06 194559" src="https://github.com/user-attachments/assets/1b93e9d7-5876-4df4-b7d2-c802adc9cf21" />

A detailed review of the penetration test findings highlighted weaknesses in identity protection controls, including Multi-Factor Authentication (MFA). Identity security controls play a critical role during the Preparation phase by reducing the likelihood of unauthorized account compromise before an incident occurs.

---

## Historical Incident Review

<img width="991" height="535" alt="4Screenshot 2026-07-06 194723" src="https://github.com/user-attachments/assets/f1794f36-9287-417a-8632-cdf101aebbff" />

Historical incident records were reviewed to understand previously reported security events, their impact, and how they were handled. Studying past incidents provides valuable context for improving future response procedures and identifying recurring security trends.

---

## Local Security Policy Review

<img width="1035" height="795" alt="5Screenshot 2026-07-06 194916" src="https://github.com/user-attachments/assets/688c27a3-50a8-47f3-9ad3-5cbc8a6b0384" />

The Local Security Policy console was accessed to review security configurations implemented on the workstation. This assessment focused on understanding how local security settings contribute to authentication, authorization, and overall system hardening.

---

## Password Policy Assessment

<img width="1167" height="812" alt="6Screenshot 2026-07-06 195105" src="https://github.com/user-attachments/assets/28204dbb-898c-4ecb-b424-f4c9ae7990c5" />

Password policy settings, including password age and minimum password length, were examined to evaluate authentication controls. Reviewing these configurations helps determine whether password policies align with security best practices and organizational requirements for protecting user accounts.

# Key Observations

Throughout the assessment several recurring themes became apparent.

* Security frameworks provide structure but require effective implementation.
* Documentation is only valuable when maintained and regularly reviewed.
* Preparation extends beyond technology and includes governance and communication.
* Logging without detection creates investigative blind spots.
* Incident response maturity depends on continuous improvement rather than one-time implementation.

---

# Security Perspective

### SOC Perspective

* Improve monitoring coverage.
* Validate detection rules.
* Ensure visibility across critical systems.
* Maintain incident documentation.

### GRC Perspective

* Regularly review security policies.
* Maintain accurate asset inventories.
* Improve governance processes.
* Conduct periodic readiness assessments.

---

# MITRE ATT&CK Alignment

| Assessment Area            | Security Context      |
| -------------------------- | --------------------- |
| Asset Visibility           | Discovery             |
| Logging Strategy           | Detection             |
| Detection Rules            | Detection Engineering |
| Identity Controls          | Credential Protection |
| Incident Response Planning | Defensive Operations  |

---

# My Learning Reflection

Before completing this room, I viewed incident response primarily as the activities performed after an attack had already occurred.

Working through the Preparation phase changed that perspective. I realized that successful incident response begins long before the first alert appears. Policies, communication plans, trained personnel, visibility, and detection capabilities all influence how effectively an organization responds under pressure.

One of the most valuable lessons was understanding that technology alone cannot compensate for weak governance or poorly defined processes. Even sophisticated security tools become far less effective when they lack quality logging, appropriate detection logic, or clearly documented response procedures.

This assessment reinforced the importance of preparation as a continuous process rather than a one-time activity and highlighted how organizational readiness directly impacts every subsequent phase of the Incident Response lifecycle.

---

# Skills Demonstrated

* Incident Response Fundamentals
* Security Governance Assessment
* Security Documentation Review
* Risk Identification
* Logging & Detection Analysis
* Organizational Readiness Assessment
* Security Operations Understanding
* Analytical Documentation

---

# References

* NIST SP 800-61 Revision 2
* SANS PICERL Framework
* TryHackMe — Preparation Room

---

# Attribution

This assessment was completed using the **Preparation** room provided by **TryHackMe**.

The environment, organizational scenario, and practical exercises were supplied by the platform. The assessment methodology, documentation, observations, analysis, and reflections presented in this document represent my own understanding and learning throughout the exercise.
