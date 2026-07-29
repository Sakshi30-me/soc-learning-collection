# Room 404

<img width="1462" height="725" alt="image" src="https://github.com/user-attachments/assets/18271cab-4fcf-494d-883d-99003c8144b2" />


> **Platform:** TryHackMe  
> **Event:** Hacker Holidays  
> **Room:** Room 404  
> **Category:** Web  
> **Topic:** Directory Enumeration  
> **Difficulty:** Very Easy  
> **Room Link:** https://tryhackme.com/room/hh-room404-804573bf

---

# Overview

The second challenge in the **Hacker Holidays** event focuses on web application reconnaissance and directory enumeration. The scenario suggests that more than just the public website has been deployed, encouraging a methodical investigation of the application's exposed attack surface.

The objective was to identify publicly accessible resources, understand the risks of exposed development artifacts, and recognize how deployment mistakes can lead to information disclosure.

---

# Learning Objectives

Throughout this challenge, I explored the following concepts:

- Web Reconnaissance
- Directory Enumeration
- Information Disclosure
- Source Code Exposure
- Secure Deployment Practices

---

# Methodology

## 1. Understanding the Challenge

I began by reviewing the room description and interacting with the target web application to understand its intended functionality and identify areas for further investigation.

**Screenshot – Target Application**

<img width="1482" height="975" alt="image" src="https://github.com/user-attachments/assets/ac20b80d-1735-4645-903f-100115208bfe" />


---

## 2. Reconnaissance & Enumeration

Instead of assuming the homepage represented the complete application, I performed directory enumeration to discover additional resources that were not directly accessible through the user interface.

Using **Gobuster**, I identified hidden content exposed by the web server. This reinforced the importance of reconnaissance as the first phase of a web application security assessment.

**Tool Used**

- Gobuster

**Screenshot – Directory Enumeration**

<img width="987" height="605" alt="image" src="https://github.com/user-attachments/assets/b614ff29-f040-4662-89e9-a08570fca845" />


---

## 3. Investigation

After identifying the exposed resource, I manually examined the available content to understand the application's structure and determine whether sensitive information had been unintentionally disclosed.

This stage demonstrated how development artifacts or misconfigured deployments can reveal internal application details that were never intended to be publicly accessible.

---

## 4. Challenge Completion

By following a structured reconnaissance and analysis process, I successfully completed the challenge while gaining a deeper understanding of information disclosure vulnerabilities and secure deployment practices.

---

# Key Takeaways

This room reinforced several important cybersecurity concepts:

- Reconnaissance should always be the starting point of a web security assessment.
- Directory enumeration can uncover hidden resources that expand an application's attack surface.
- Development artifacts should never be exposed in production environments.
- Information disclosure vulnerabilities are often caused by deployment or configuration mistakes rather than programming flaws.
- Small operational oversights can create significant security risks.

---

# Skills Demonstrated

- Web Application Reconnaissance
- Directory Enumeration
- Information Gathering
- Information Disclosure Analysis
- Security Assessment
- Technical Documentation

---

# Defensive Perspective

To reduce the likelihood of similar issues, organizations should:

- Remove development artifacts before deployment.
- Restrict access to internal resources.
- Perform deployment validation before publishing applications.
- Integrate automated security checks into CI/CD pipelines.
- Conduct regular web application security assessments.

---

# Reflection

This challenge demonstrated that effective security assessments begin with careful reconnaissance and systematic investigation.

Rather than immediately searching for a vulnerability, I learned the importance of understanding the application's attack surface and following a structured methodology. Even a simple deployment oversight can expose valuable information that significantly increases the overall security risk of an application.

---

# Tools Used

- Gobuster
- Linux Terminal
- Web Browser
- Git Commands

---

# References

- **TryHackMe – Room 404:** https://tryhackme.com/room/hh-room404-804573bf
- **OWASP Web Security Testing Guide (WSTG):** https://owasp.org/www-project-web-security-testing-guide/
- **OWASP Top 10 (A05:2021 – Security Misconfiguration):** https://owasp.org/Top10/

---

# Disclaimer

This write-up is part of my **SOC Learning** portfolio and is intended for educational purposes.

To respect the learning experience of other `TryHackMe participants`, this documentation intentionally `omits the challenge flag, exact vulnerable endpoint, exploitation steps, and complete solution path`. The focus is on the methodology, security concepts, and lessons learned.
