# Day 11 – Infinity Pool

> A **Boot2Root** challenge focused on network enumeration, discovering hidden services, and understanding how exposed internal systems can lead to further compromise.

<img width="1326" height="767" alt="image" src="https://github.com/user-attachments/assets/fb655a3f-c843-4730-828b-57b31c0bf89c" />

---

## Challenge Information

| **Property**   | **Value**                                                    |
| -------------- | ------------------------------------------------------------ |
| **Platform**   | TryHackMe                                                    |
| **Event**      | Hacker Holidays                                              |
| **Challenge**  | Infinity Pool                                                |
| **Category**   | Web / Boot2Root                                              |
| **Difficulty** | Medium                                                       |
| **Focus**      | Network Enumeration, Service Discovery, Privilege Escalation |
| **Link**       |[Infinity Pool](https://tryhackme.com/room/hh-infinitypool-5b3548af)|
---

# Challenge Overview

Day 11, **Infinity Pool**, shifted the focus from a single web application to the underlying network.

The main idea of the room was simple: the services visible at first were not the whole story. By enumerating the target more thoroughly, additional systems and services could be discovered that were not immediately obvious.

The challenge reinforced an important lesson in penetration testing:

> **What you can see initially is not necessarily everything that is exposed.**

<img width="1400" height="2000" alt="5dbea226085ab6182a2ee0f7-1784276504042" src="https://github.com/user-attachments/assets/0b541a9e-2326-4e68-a865-2f085fb9dd73" />

---

# Objectives

* Enumerate the target machine.
* Identify exposed network services.
* Discover hidden or previously unknown systems.
* Investigate the discovered services.
* Obtain the user flag.
* Continue enumeration from the compromised system.
* Escalate privileges.
* Obtain the root flag.

---

# Methodology

The investigation followed a structured Boot2Root methodology:

1. Initial reconnaissance
2. Port and service enumeration
3. Web/application enumeration
4. Investigation of discovered services
5. Initial access
6. Local enumeration
7. Privilege escalation
8. Flag retrieval

---

# Core Commands & Tools

## Network Enumeration

* Nmap
* Port scanning
* Service/version detection
* Network discovery

## Web Investigation

* Browser
* HTTP enumeration
* Service enumeration

## Linux Enumeration

* `whoami`
* `id`
* `pwd`
* `ls`
* `find`
* `ps`
* `ss`

## Privilege Escalation

* Permission enumeration
* Process/service investigation
* Sudo and configuration checks

---

<img width="1418" height="648" alt="image" src="https://github.com/user-attachments/assets/6bfba375-3b5e-449f-90cb-9127393c8aa7" />

# Investigation Walkthrough

## 1. Initial Enumeration

I started by enumerating the target to identify the network services that were exposed.

The important takeaway from the initial scan was that the target had more functionality than what was immediately visible through the main web interface.

This made deeper enumeration important rather than stopping after finding the first available service.

---

## 2. Discovering Additional Systems

The room's description provides an important hint:

> *"You trace the network to the horizon and find three systems nobody told you about on the other side."*

This points toward **network and service discovery** being an important part of the challenge.

Instead of focusing only on the main application, I investigated the network environment and looked for additional hosts and services.

This is an important real-world penetration-testing habit because internal services can expose functionality that isn't accessible through the public-facing application.

<img width="1167" height="553" alt="image" src="https://github.com/user-attachments/assets/b96a50bc-3754-4fca-83b1-17b0137bd8db" />

---

## 3. Investigating the Discovered Services

After identifying additional systems, I investigated the services running on them.

For each discovered service, I considered:

* What software is running?
* Which ports are exposed?
* Is authentication required?
* Is there a web interface?
* Are there unusual services or versions?
* Does the service reveal information about the underlying system?

The goal was to understand how the systems were connected rather than treating each service independently.

---

## 4. Initial Access

Further investigation of the exposed services eventually provided a path to initial access.

Once access was obtained, the focus shifted from:

**External Enumeration → Local Enumeration**

This transition is an important part of the Boot2Root methodology:

> **External access → User access → Local enumeration → Privilege escalation → Root**

---

## 5. Finding the User Flag

After obtaining user-level access, I investigated the accessible user directories and located the user flag.

The user flag confirmed that the initial-access portion of the challenge had been completed successfully.

<img width="1071" height="204" alt="image" src="https://github.com/user-attachments/assets/5a5eee6b-c591-423f-862a-9258cd8693aa" />

---

## 6. Local Enumeration

With user-level access established, I continued enumerating the system.

I investigated areas such as:

* Running processes
* Listening services
* User permissions
* Interesting files
* Configuration files
* Scheduled tasks
* Sudo permissions
* Services running with elevated privileges

This stage was important because the next objective was to move from the compromised user to root.

---

## 7. Privilege Escalation

The final stage involved identifying a weakness in the system that allowed the current user's privileges to be increased.

The key lesson was that privilege escalation is often less about finding a single "magic command" and more about connecting information discovered during enumeration.

After identifying the escalation path, I successfully obtained elevated privileges.

---

## 8. Finding the Root Flag

With root-level access achieved, I located the final flag.

This completed both objectives of **Infinity Pool**:

* ✅ User flag
* ✅ Root flag

---

# Security Analysis

## Finding — Exposed Internal Services

The challenge demonstrated how services that are not immediately visible from the primary application can still become relevant once the environment is properly enumerated.

### Risk

* Increased attack surface
* Information disclosure
* Unauthorized access to internal services
* Additional attack paths
* Potential privilege escalation

### Mitigation

Organizations should:

* Minimize unnecessary exposed services.
* Restrict internal services through network segmentation.
* Apply firewall rules and access controls.
* Monitor unexpected network services.
* Regularly perform internal network enumeration.
* Keep services updated and securely configured.

---

# MITRE ATT&CK Mapping

| **Technique**                                     | **Relevance**                                                                |
| ------------------------------------------------- | ---------------------------------------------------------------------------- |
| **T1046 – Network Service Scanning**              | Discovering exposed services during reconnaissance.                          |
| **T1018 – Remote System Discovery**               | Identifying additional systems within the environment.                       |
| **T1190 – Exploit Public-Facing Application**     | Relevant if initial access involved an exposed application.                  |
| **T1068 – Exploitation for Privilege Escalation** | Relevant to the privilege-escalation stage if a vulnerability was exploited. |

> The exact MITRE ATT&CK mapping depends on the specific exploitation path used in the room.

---

# What I Learned

Infinity Pool reinforced the importance of **thorough enumeration**.

The biggest takeaway for me was that finding one working service doesn't mean the investigation is over. The challenge encouraged looking beyond the obvious entry point and understanding the wider network.

I also became more comfortable with the transition between:

**Network Enumeration → Initial Access → Local Enumeration → Privilege Escalation**

This workflow is becoming much more familiar as I progress through the Hacker Holidays rooms.

---

# Skills Demonstrated

* Network Enumeration
* Port Scanning
* Service Discovery
* Web Enumeration
* Internal Network Discovery
* Linux Enumeration
* Privilege Escalation
* Boot2Root Methodology
* Security Investigation
* Threat Analysis
* Technical Documentation
* Defensive Security Thinking

---

# Key Takeaway

**Infinity Pool** was a great reminder that **enumeration is often the difference between being stuck and finding the next step**.

The room wasn't simply about compromising one machine. It was about looking beyond the obvious, discovering what else was present, understanding how those systems related to each other, and using that information to progress from initial access to root.

---

# References

* TryHackMe – Hacker Holidays
* MITRE ATT&CK Framework
* Nmap Documentation
* Linux Manual Pages

---

# Disclaimer

This write-up is intended for **educational and portfolio purposes**.

The investigation was conducted within an authorized TryHackMe training environment.

To preserve the learning experience of other participants, this repository intentionally omits:

* Challenge flags
* Exact exploit payloads
* Sensitive challenge-specific values
* Complete command sequences leading directly to the solution

The purpose of this write-up is to demonstrate **my investigation methodology, technical understanding, security analysis, and defensive takeaways rather than provide a complete step-by-step solution.**
