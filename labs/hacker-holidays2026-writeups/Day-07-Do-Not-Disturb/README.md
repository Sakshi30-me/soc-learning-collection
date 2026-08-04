# Day 7 – Do Not Disturb

> A medium-difficulty Boot2Root challenge focused on chaining multiple web application vulnerabilities with Linux enumeration and privilege escalation.

<img width="1400" height="2000" alt="image" src="https://github.com/user-attachments/assets/8722a160-bc51-4bdb-832c-cd273d913d56" />

---

# Challenge Information

| Property | Value |
|----------|-------|
| **Platform** | TryHackMe |
| **Event** | Hacker Holidays |
| **Challenge** | Do Not Disturb |
| **Category** | Boot2Root |
| **Difficulty** | Medium |
| **Focus** | NoSQL Injection, SSTI, Node.js, Linux Enumeration, Privilege Escalation |
| **Link** | [Do Not Disturb](https://tryhackme.com/room/hh-donotdisturb-84a45644) |


---

# Challenge Overview

Day 7 was one of the most challenging rooms in the Hacker Holidays event because it required understanding an entire attack chain rather than exploiting a single vulnerability.

The challenge began with bypassing authentication in a Node.js web application, followed by identifying a **Server-Side Template Injection (SSTI)** vulnerability that allowed server-side code execution. After obtaining an initial shell, the investigation shifted toward Linux enumeration, discovering an internal debugging service, and ultimately leveraging system permissions to retrieve the root flag.

More than simply exploiting vulnerabilities, this room highlighted the importance of understanding how seemingly unrelated weaknesses can be combined into a complete compromise.

<img width="1205" height="673" alt="image" src="https://github.com/user-attachments/assets/e9e9c56f-5e92-40b6-9881-03bcad317b06" />

---

# Objectives

- Analyze the web application.
- Obtain an authenticated session.
- Identify and validate a Server-Side Template Injection (SSTI) vulnerability.
- Achieve initial code execution.
- Enumerate the compromised Linux system.
- Identify internal services.
- Investigate the Node.js debugging interface.
- Escalate privileges.
- Retrieve both user and root flags.

---

# Methodology

The investigation followed a structured workflow:

1. Enumerate the web application.
2. Analyze the authentication mechanism.
3. Obtain an authenticated session.
4. Investigate server-side template functionality.
5. Confirm server-side code execution.
6. Obtain an interactive shell.
7. Enumerate the Linux system.
8. Identify localhost-only services.
9. Investigate the Node.js debugging interface.
10. Escalate privileges.
11. Retrieve the root flag.

---

# Core Tools & Technologies

### Web

- Burp Suite
- Gobuster
- Express.js
- Node.js
- EJS Templates

### Linux

- Bash
- Netcat
- Linux Enumeration
- Node.js Inspector
- DebugFS

### Concepts

- NoSQL Injection
- Authentication Bypass
- Server-Side Template Injection (SSTI)
- Remote Code Execution
- Localhost Services
- Privilege Escalation

---

# Investigation Walkthrough

## 1. Web Enumeration

The investigation began by enumerating the exposed web application to identify accessible resources and restricted functionality.

Directory enumeration revealed endpoints that suggested authenticated functionality existed within the application.

<img width="1100" height="690" alt="image" src="https://github.com/user-attachments/assets/c9706023-2347-4a2c-93a0-9b097ccf52dd" />

---

## 2. Authentication Analysis

The login mechanism was analyzed by intercepting authentication requests.

The application was found to be vulnerable to a NoSQL authentication bypass, allowing access without valid credentials.

After successful authentication, an active application session was established.

---

## 3. Server-Side Template Injection

Once authenticated, functionality allowing users to customize templates was identified.

Simple arithmetic expressions confirmed that user input was being evaluated on the server rather than displayed as plain text.

This verified the presence of a Server-Side Template Injection (SSTI) vulnerability.

Because the application was built with Express.js and EJS, server-side JavaScript execution became possible.

---

## 4. Initial Shell

After confirming server-side code execution, an interactive shell was obtained.

This provided access as a low-privileged user and allowed retrieval of the user flag.

<img width="1100" height="781" alt="image" src="https://github.com/user-attachments/assets/3291ce41-289f-49d5-88eb-8970c6a07669" />

<img width="646" height="572" alt="image" src="https://github.com/user-attachments/assets/7e23f9be-d8c4-4776-9291-170332e76c05" />

---

## 5. Linux Enumeration

Following initial access, attention shifted toward system enumeration.

The investigation focused on:

- Running processes
- Active users
- Listening services
- Network interfaces
- Local applications

Enumeration identified an interesting service bound exclusively to the localhost interface.

---

## 6. Internal Service Investigation

A Node.js debugging service was discovered listening on the default debugger port.

Because the service accepted only local connections, it was inaccessible during external reconnaissance but became reachable after obtaining shell access.

Investigating the debugger revealed that it was attached to a different process with higher privileges.

---

## 7. Privilege Escalation

Further investigation showed that the target process belonged to a privileged Linux group capable of accessing raw filesystem devices.

Instead of reading files through standard filesystem permissions, filesystem debugging utilities could directly access the underlying storage device.

This ultimately allowed retrieval of the root flag.

---

# Attack Chain

```text
NoSQL Authentication Bypass
        ↓
Authenticated Session
        ↓
Server-Side Template Injection
        ↓
Remote Code Execution
        ↓
Reverse Shell
        ↓
Linux Enumeration
        ↓
Node.js Debug Service
        ↓
Higher-Privilege Process
        ↓
Filesystem Access
        ↓
Root Flag
```

---

# Security Analysis

## Findings

The challenge demonstrated how multiple seemingly independent weaknesses can be chained together to achieve complete system compromise.

The compromise relied on:

- Weak authentication validation
- Unsafe server-side template evaluation
- Exposed debugging functionality
- Excessive local permissions

Although each issue individually presented risk, their combination significantly increased the overall impact.

---

## Potential Risks

- Authentication bypass
- Remote code execution
- Unauthorized server access
- Internal service exposure
- Privilege escalation
- Full system compromise

---

## Defensive Considerations

Organizations should:

- Properly validate authentication requests.
- Use parameterized database queries.
- Avoid evaluating user-controlled template input.
- Disable debugging interfaces in production.
- Restrict localhost debugging services.
- Apply the Principle of Least Privilege.
- Regularly audit group memberships and privileged services.

---

# MITRE ATT&CK Mapping

| Technique | Description |
|-----------|-------------|
| **T1190 – Exploit Public-Facing Application** | Authentication bypass and template injection allowed compromise of the web application. |
| **T1059 – Command and Scripting Interpreter** | Server-side JavaScript execution enabled operating system command execution. |
| **T1106 – Native API** | Native operating system functionality was leveraged through the Node.js runtime. |
| **T1046 – Network Service Discovery** | Enumeration identified internal services listening on localhost. |
| **T1082 – System Information Discovery** | System and process information were gathered during enumeration. |
| **T1068 – Exploitation for Privilege Escalation** | Weak local permissions were leveraged to obtain higher privileges. |

> **Note:** These mappings are included for educational purposes to illustrate attacker techniques demonstrated during the challenge.

---

# What I Learned

This was my first room that combined several different attack techniques into a single exploitation chain.

Some of my key takeaways were:

- Security vulnerabilities are often exploited together rather than individually.
- Enumeration is one of the most important phases after obtaining initial access.
- Localhost-only services should not automatically be considered secure.
- Development features such as debugging interfaces should never remain enabled in production environments.
- Understanding the purpose behind each command is far more valuable than memorizing exploitation steps.

This room also reinforced the importance of revisiting completed challenges to fully understand every concept involved rather than focusing solely on obtaining the flags.

---

# Skills Demonstrated

- Web Application Enumeration
- NoSQL Injection Analysis
- Burp Suite Usage
- Server-Side Template Injection Analysis
- Node.js Security
- Linux Enumeration
- Process Investigation
- Local Service Analysis
- Privilege Escalation
- Security Documentation
- Threat Analysis

---

# References

- TryHackMe – [Do Not Disturb](https://tryhackme.com/room/hh-donotdisturb-84a45644)
- MITRE ATT&CK Framework
- OWASP Server-Side Template Injection
- MongoDB Query Operators
- Node.js Documentation
- GTFOBins

---

# Disclaimer

This write-up is intended for **educational and portfolio purposes.**

The challenge was completed within an authorized TryHackMe training environment.

To preserve the learning experience of other participants, this repository intentionally omits:

- Challenge flags
- Exact payloads
- Copy-paste exploitation commands
- Sensitive challenge-specific information

The objective of this documentation is to demonstrate **`my investigation methodology, technical understanding, and defensive security perspective rather than provide a complete walkthrough.`**
