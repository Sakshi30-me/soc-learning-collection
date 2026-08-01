# Day 5 – Beach Bar

A **Boot2Root** challenge exploring web enumeration, insecure YAML deserialization, remote code execution, and Linux privilege escalation through exposed credentials.

---

## Challenge Information

| Property | Value |
|----------|-------|
| Platform | TryHackMe |
| Event | Hacker Holidays |
| Challenge | Beach Bar |
| Category | Web Security / Boot2Root |
| Difficulty | Easy |
| Focus | Web Enumeration, YAML Deserialization, Remote Code Execution, Linux Privilege Escalation |
| Link | [Beach Bar ](https://tryhackme.com/room/hh-beachbar-d849f7f7)|

<img width="1547" height="712" alt="image" src="https://github.com/user-attachments/assets/75764279-8eda-4885-bb55-1255af7fb2ee" />

---

# Challenge Overview

**Day 5 of the Hacker Holidays 2026** event combines web application testing with Linux privilege escalation.

The investigation began with a web-based jukebox application that exposed authenticated functionality. Through systematic enumeration, the application's playlist import feature became the primary focus. Further analysis revealed an insecure YAML deserialization vulnerability, which allowed remote code execution as a low-privileged user. Local enumeration then uncovered a privilege escalation path caused by poor credential management.

This room demonstrates how multiple security weaknesses, while individually significant, become far more impactful when chained together.

---

# Objectives

- Enumerate the target web application.
- Obtain authenticated access.
- Investigate the playlist import/export functionality.
- Analyze the application's handling of YAML data.
- Identify the insecure deserialization vulnerability.
- Obtain the user flag.
- Perform Linux privilege escalation.
- Obtain the root flag.
- Understand secure development and system hardening practices.

---

# Methodology

The investigation followed a structured methodology:

1. Perform initial reconnaissance.
2. Inspect the web application's publicly available resources.
3. Authenticate to the application.
4. Explore available functionality.
5. Analyze exported playlist data.
6. Investigate YAML import behavior.
7. Identify insecure deserialization.
8. Achieve command execution.
9. Enumerate the Linux system.
10. Identify privilege escalation opportunities.
11. Obtain administrative access.
12. Document findings and defensive recommendations.

---

# Core Commands & Tools

## Enumeration

- Browser
- Page Source Inspection
- Nmap

## Web Investigation

- YAML Analysis
- HTTP Requests
- Import / Export Functionality

## Linux Enumeration

- `id`
- `whoami`
- `pwd`
- `ls`
- `cat`
- `find`
- `ps`

## Remote Access

- Netcat
- Bash

---

# Investigation Walkthrough

## 1. Initial Enumeration

The investigation began by identifying the services exposed by the target machine.

The primary service presented a web application for a beach bar jukebox requiring user authentication.

Before attempting advanced testing, publicly available resources such as the page source and application interface were examined to identify useful information.

<img width="992" height="722" alt="image" src="https://github.com/user-attachments/assets/db247975-39a4-4a30-875b-95a2347f88d0" />

Then examining the **page source code** which revealed the login information with `Username and Password` as **dj** in both fields.

<img width="980" height="722" alt="day 5 logged in 2026-08-01 152026" src="https://github.com/user-attachments/assets/49e8070b-cae7-46f6-bcc7-e3696efed40f" />

---

## 2. Authentication

After identifying valid credentials, authenticated access to the application was obtained.

The dashboard exposed two primary features:

- Playlist Export
- Playlist Import

These features became the primary focus of the investigation because they accepted and processed user-controlled data.

<img width="988" height="707" alt="day 5 2 02026-08-01 151918" src="https://github.com/user-attachments/assets/f36b6b66-4bf0-47fa-b60a-678e068ad3fc" />

---

## 3. YAML Analysis

Exporting a playlist revealed that the application stored playlist information in YAML format.

The import functionality accepted `user-supplied YAML content` and processed it server-side.

Rather than treating the uploaded content as plain text, the application interpreted the YAML structure, suggesting that **`server-side deserialization`** was occurring.

<img width="760" height="738" alt="day 5 import-execution" src="https://github.com/user-attachments/assets/78ad5c88-fd4f-4fc1-bd60-f22be5ee815c" />

---

## 4. Understanding the Vulnerability

The application's behavior indicated that user-controlled YAML was being deserialized using an unsafe parser.

In Python applications, unsafe deserialization occurs when untrusted YAML is processed with insecure parsing methods capable of reconstructing Python objects.

Instead of simply reading structured data, the parser may instantiate Python objects defined within the YAML document. If appropriate protections are not implemented, this behavior can lead to arbitrary command execution.

Understanding the distinction between **parsing data** and **executing objects** was one of the key learning outcomes of this challenge.

---

## 5. Remote Code Execution

After confirming the deserialization behavior, it became possible to execute controlled commands on the target system.

This resulted in access as a **`low-privileged user`**, allowing further `Linux enumeration.`

At this stage, the focus shifted from web application testing to operating system investigation.

<img width="988" height="728" alt="day 5 2nd 2026-08-01 151834" src="https://github.com/user-attachments/assets/4d2f64c1-82b3-4c82-a585-16caedc07411" />

---

<img width="990" height="820" alt="day5 in the 3rd 2026-08-01 151229" src="https://github.com/user-attachments/assets/1e2143b2-e70d-4a31-80b1-a37d77fc8b19" />

---

## 6. Linux Privilege Escalation

Local enumeration revealed a process exposing sensitive credentials through its command-line arguments.

Since command-line arguments are often visible to other local users, sensitive information stored in this manner can become accessible.

The exposed credentials ultimately allowed elevation to administrative privileges.

<img width="982" height="820" alt="day5 4th imge2026-08-01 151327" src="https://github.com/user-attachments/assets/c2985274-2d51-4de9-aec2-1adab44a87fe" />

---

<img width="993" height="422" alt="THE root level" src="https://github.com/user-attachments/assets/3e9d09e2-f192-45ed-bcbe-9c3719fbf967" />

---

# Security Analysis

## Finding 1 — Exposed Credentials

Development or demonstration credentials remained available within the application.

### Risk

- Unauthorized access
- Increased attack surface
- Weak authentication controls

### Mitigation

- Remove default accounts before deployment.
- Enforce strong authentication policies.
- Disable development credentials in production environments.

---

## Finding 2 — Unsafe YAML Deserialization

The application processed user-controlled YAML using an unsafe deserialization mechanism.

### Risk

- Remote Code Execution
- Server compromise
- Unauthorized system access
- Potential data exposure

### Mitigation

- Use `yaml.safe_load()` or equivalent secure parsing methods.
- Validate all imported content.
- Restrict accepted object types.
- Treat all user input as untrusted.

---

## Finding 3 — Sensitive Credentials Exposed in Process Arguments

Sensitive credentials were exposed through a running system process.

### Risk

- Credential disclosure
- Privilege escalation
- Full system compromise

### Mitigation

- Never store passwords or secrets as command-line arguments.
- Use secure secret management solutions.
- Restrict unnecessary process visibility.
- Rotate exposed credentials immediately.
  
---

# MITRE ATT&CK Mapping

| Technique | Description |
|-----------|-------------|
| T1190 | Exploit Public-Facing Application |
| T1059 | Command and Scripting Interpreter |
| T1059.004 | Unix Shell |
| T1552 | Unsecured Credentials |
| T1078 | Valid Accounts |
| T1068 | Exploitation for Privilege Escalation |

---

# What I Learned

This challenge reinforced several important security concepts.

## Web Enumeration

Careful enumeration of publicly accessible resources often reveals valuable information before any exploitation begins.

---

## YAML Security

**Serialization formats** should never be assumed to be safe.

Applications must securely `deserialize user-controlled input` using appropriate parsing methods.

---

## Insecure Deserialization

Understanding **why** unsafe deserialization occurs is more valuable than memorizing exploit payloads.

Proper input handling and secure parser configuration are essential defensive controls.

---

## Linux Enumeration

Privilege escalation frequently depends on careful system observation rather than complex exploitation.

Running processes, permissions, and exposed credentials can all become valuable sources of information.

---

## Defense in Depth

The room demonstrated how several independent weaknesses combined into a complete attack chain:

- Exposed credentials
- Unsafe YAML deserialization
- Credential disclosure through process arguments

Individually, each issue increased risk. Together, they resulted in complete system compromise.

---

# Skills Demonstrated

- Web Enumeration
- Authentication Analysis
- YAML Analysis
- Insecure Deserialization Analysis
- Remote Code Execution
- Linux Enumeration
- Privilege Escalation
- Process Inspection
- Security Analysis
- Threat Analysis
- Technical Documentation
- Defensive Security Thinking

---

# References

- TryHackMe – [Beach Bar ](https://tryhackme.com/room/hh-beachbar-d849f7f7)
- MITRE ATT&CK Framework
- PyYAML Documentation
- OWASP Deserialization Cheat Sheet
- Linux `ps` Manual

---

# Disclaimer

This documentation is intended for **educational and portfolio purposes.**

The investigation was conducted within an authorized TryHackMe training environment.

To preserve the learning experience of other participants, this repository intentionally omits:

- Challenge flags
- Exact exploit payloads
- Full reverse shell commands
- Complete command sequences leading directly to the solution

The focus of this write-up is to document **`my investigation methodology, technical understanding, security analysis, and defensive takeaways rather than provide a step-by-step solution.`**
