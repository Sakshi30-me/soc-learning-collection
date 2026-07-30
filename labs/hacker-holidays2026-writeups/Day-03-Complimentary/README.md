# Day 3 – Complimentary

<img width="1332" height="711" alt="image" src="https://github.com/user-attachments/assets/bf8cff2e-23c3-4d3b-89f0-ed2c045a57d0" />

## Challenge Information

| Property | Value |
|----------|-------|
| **Platform** | TryHackMe |
| **Event** | Hacker Holidays |
| **Challenge** | Complimentary |
| **Focus** | Cloud Security, AWS IAM, Identity & Access Management, Authorization |
| **Link** | https://tryhackme.com/room/hh-complimentary-05e0b604 |

---

## Overview

The third challenge in the **Hacker Holidays** event shifts the focus to cloud security. The scenario revolves around a complimentary mobile application that grants guests temporary cloud credentials. While convenient, the application demonstrates how improperly configured cloud permissions can unintentionally expose sensitive information.

The objective is to investigate how guest credentials interact with cloud resources and understand the security risks associated with excessive permissions and weak authorization controls.

---

## Objectives

- Understand how cloud credentials are assigned to guest users.
- Investigate the permissions granted through the application.
- Analyze the risks of overly permissive IAM policies.
- Explore the importance of secure authorization in cloud environments.
- Learn defensive strategies for protecting cloud resources.

---

<img width="1585" height="802" alt="image" src="https://github.com/user-attachments/assets/d830fad5-4469-4f99-9d26-473f7b5cc919" />

## My Approach

### 1. Initial Analysis

I began by reviewing the challenge description and interacting with the application to understand how guest users were authenticated and granted access to cloud resources.

Rather than immediately searching for sensitive information, I focused on understanding how the application communicated with its backend services.

<img width="1160" height="850" alt="image" src="https://github.com/user-attachments/assets/81f150b8-723b-4b19-9059-7309e7b4b033" />

---

### 2. Cloud Permission Analysis

After identifying the cloud components involved, I examined how the provided credentials were intended to interact with backend resources.

This helped me understand the role of cloud identity management and why permissions should always be restricted to the minimum required level.

<img width="1891" height="552" alt="image" src="https://github.com/user-attachments/assets/71dd0f81-a7e6-4268-bcb1-c41c9dca44c6" />

---

### 3. Authorization Assessment

I analyzed how access to backend resources was controlled and considered the security implications of granting identical permissions to every guest user.

This highlighted how excessive permissions can lead to unintended information disclosure if proper authorization controls are not enforced.

<img width="1905" height="681" alt="image" src="https://github.com/user-attachments/assets/b5387d99-adca-4002-8118-77e5024a53f2" />

---

### 4. Defensive Perspective

The room emphasized that cloud security extends beyond authentication. Even temporary or guest credentials must follow secure authorization practices and the Principle of Least Privilege (PoLP) to reduce unnecessary exposure.

---

## Security Concepts

- Cloud Security
- AWS Identity and Access Management (IAM)
- Temporary Cloud Credentials
- Authorization
- Principle of Least Privilege (PoLP)
- Cloud Misconfiguration
- Information Disclosure

---

## What I Learned

This challenge reinforced that:

- Authentication alone does not guarantee secure access.
- Cloud credentials should always follow the Principle of Least Privilege.
- Authorization must be enforced for every request.
- Misconfigured IAM policies can expose sensitive cloud resources.
- Cloud security depends on careful permission management rather than simply protecting login mechanisms.

---

## Skills Demonstrated

- Cloud Security Fundamentals
- IAM Permission Analysis
- Authorization Assessment
- Security Analysis
- Critical Thinking
- Technical Documentation

---

## References

- TryHackMe – Hacker Holidays: Complimentary
- AWS IAM Documentation
- AWS Security Best Practices
- OWASP Cloud Security Guidance

---

## Disclaimer

This write-up is intended for educational purposes and portfolio documentation.

To respect the learning experience of others and the `TryHackMe platform`, this repository intentionally `omits challenge flags, exact solution steps, and sensitive challenge details.` The focus is on **methodology, security concepts, and defensive learning.**
