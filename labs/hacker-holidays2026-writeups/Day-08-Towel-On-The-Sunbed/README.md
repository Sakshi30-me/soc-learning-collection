# Day 8 – Towel on the Sunbed

> A medium-difficulty web application challenge focused on identifying and exploiting a business logic flaw within a reward-based application.

---

# Challenge Information

| Property | Value |
|----------|-------|
| **Platform** | TryHackMe |
| **Event** | Hacker Holidays |
| **Challenge** | Towel on the Sunbed |
| **Category** | Web Exploitation |
| **Difficulty** | Medium |
| **Focus** | Business Logic, API Analysis, Burp Suite, Web Security |
| **Link**  | [Towel on the Sunbed](https://tryhackme.com/room/hh-towelonthesunbed-61271709)  |
---

# Challenge Overview

Day 8 focused on analyzing the business logic behind a web-based rewards application rather than exploiting a traditional software vulnerability.

The application implemented a daily reward mechanism that was intended to limit reward claims to once every 24 hours. Instead of searching for injection flaws or authentication weaknesses, the investigation required understanding how the application validated reward requests and identifying inconsistencies between the client and server.

This challenge demonstrated that even when an application is technically secure, flawed business logic can still allow unintended actions.

<img width="1228" height="717" alt="image" src="https://github.com/user-attachments/assets/f4ea8aa6-6935-4c91-9877-c64478a16f04" />

---

# Objectives

- Register a guest account.
- Explore the reward system.
- Analyze the application's daily reward mechanism.
- Intercept and inspect HTTP requests.
- Understand how the application validates reward claims.
- Identify the business logic weakness.
- Access the Whale Vault.
- Retrieve the challenge flag.

---

# Methodology

The investigation followed a structured workflow:

1. Register a new account.
2. Explore available application features.
3. Observe the daily reward functionality.
4. Intercept HTTP requests using Burp Suite.
5. Analyze request and response behavior.
6. Identify weaknesses in the reward validation process.
7. Manipulate application logic.
8. Access the protected reward area.
9. Retrieve the flag.

---

# Core Tools & Technologies

### Tools

- Burp Suite
- Web Browser

### Concepts

- HTTP Requests
- HTTP Responses
- API Analysis
- Business Logic Vulnerabilities
- Client-Server Communication

---

# Investigation Walkthrough

## 1. Application Exploration

The investigation began by creating a guest account and exploring the available functionality within the rewards application.

The dashboard displayed a daily reward feature that appeared to enforce a 24-hour waiting period between reward claims.

<img width="500" height="465" alt="image" src="https://github.com/user-attachments/assets/95328097-e5eb-4807-923e-46cea825547b" />

---

## 2. Request Analysis

Burp Suite was used to intercept and inspect the communication between the browser and the application.

Rather than focusing on traditional attack vectors such as SQL injection or Cross-Site Scripting (XSS), the investigation centered on understanding how reward requests were processed.

<img width="1362" height="647" alt="image" src="https://github.com/user-attachments/assets/c2bc9556-f008-43d3-83bf-cb181ed11d68" />

<img width="1001" height="594" alt="image" src="https://github.com/user-attachments/assets/ea84987f-3f90-4f90-b6b0-0553d56969af" />

---

## 3. Business Logic Analysis

The application's behavior suggested that reward eligibility relied on application logic rather than cryptographic or authentication controls.

By carefully analyzing how requests were validated, it became possible to identify a weakness in the reward mechanism.

The issue was not caused by insecure code execution but by incorrect implementation of the application's intended workflow.

---

## 4. Accessing the Whale Vault

After understanding the reward validation process, the application logic could be manipulated to satisfy the conditions required for accessing the Whale Vault.

The protected area became accessible, allowing retrieval of the challenge flag.

<img width="537" height="285" alt="image" src="https://github.com/user-attachments/assets/64fdafab-ad25-47d0-a626-7c6044739665" />

---

# Security Analysis

## Finding

The application contained a business logic vulnerability within its reward mechanism.

Although the application correctly implemented authentication and normal request handling, flaws in the reward validation process allowed unintended application behavior.

---

## Potential Risks

- Abuse of promotional systems
- Unauthorized reward accumulation
- Financial loss
- Application integrity issues
- Loss of trust in reward systems

---

## Defensive Considerations

Organizations should:

- Perform server-side validation for reward eligibility.
- Avoid relying solely on client-controlled values.
- Validate business rules independently of user input.
- Monitor abnormal reward claim patterns.
- Perform security reviews focused on business workflows in addition to technical vulnerabilities.

---

# MITRE ATT&CK Mapping

| Technique | Description |
|-----------|-------------|
| **T1190 – Exploit Public-Facing Application** | The challenge demonstrated how weaknesses in application logic can be abused through normal application functionality. |

> **Note:** Business logic vulnerabilities are not always represented directly within the MITRE ATT&CK framework because they depend on application design rather than specific exploitation techniques.

---

# What I Learned

This challenge reinforced that not every security issue is caused by programming mistakes such as SQL Injection or Remote Code Execution.

Some of my key takeaways were:

- Understanding application workflows is just as important as identifying technical vulnerabilities.
- Burp Suite is valuable for analyzing application behavior, not just modifying requests.
- Business logic vulnerabilities often arise from incorrect assumptions about how users will interact with an application.
- Security testing should include both technical vulnerabilities and application workflow analysis.

---

# Skills Demonstrated

- Web Application Analysis
- Burp Suite
- HTTP Analysis
- API Investigation
- Business Logic Testing
- Security Analysis
- Critical Thinking
- Technical Documentation

---

# References

- TryHackMe – [Towel on the Sunbed](https://tryhackme.com/room/hh-towelonthesunbed-61271709)
- OWASP Business Logic Vulnerabilities
- Burp Suite Documentation
- MITRE ATT&CK Framework

---

# Disclaimer

This write-up is intended for `educational and portfolio purposes.`

The challenge was completed within an authorized TryHackMe training environment.

To preserve the learning experience for others, this repository intentionally omits:

- Challenge flags
- Exact request modifications
- Challenge-specific exploitation steps
- Sensitive challenge-specific information

The purpose of this documentation is to demonstrate **`my investigation methodology, technical understanding, and defensive security perspective rather than provide a complete walkthrough.`**
