# Linux Privilege Escalation — Enumeration & Security Analysis

> **Platform:** TryHackMe

> **Lab Type:** Guided Hands-on Lab

> **My Role:** Security Learner / Analyst

> **Focus Areas:** Linux Enumeration · Privilege Escalation · SOC Visibility · GRC Thinking · MITRE Mapping

---

## Project Overview

This project documents my investigation and analysis of a Linux privilege escalation enumeration lab completed on TryHackMe.

While the lab environment and objectives were provided by the platform, the observations, interpretation, documentation, and security reflections presented here represent my own learning and analytical process.

The objective was not to exploit the environment immediately, but to understand how visibility, configuration decisions, permissions, and system trust relationships influence privilege escalation opportunities.

---

## Objectives

* Perform structured Linux system enumeration
* Understand privilege escalation prerequisites
* Identify indicators of operational exposure
* Connect technical findings with defensive security thinking
* Relate observations to SOC, GRC, and MITRE ATT&CK concepts

---

## Methodology

The investigation followed a phased enumeration approach:

1. Operating System Enumeration
2. User & Access Enumeration
3. Network Enumeration
4. File & Permission Enumeration
5. Security Reflection & Analysis

---

## Core Commands Used

The following commands were used during the investigation to collect evidence and understand system behavior.

| Phase               | Commands                                                                 | Purpose                                                                        |
| ------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| OS Enumeration      | `hostname`, `uname -a`, `cat /proc/version`, `cat /etc/issue`, `dpkg -l` | Identify system identity, kernel details, distribution, and installed packages |
| Process Enumeration | `ps aux`, `ps axjf`                                                      | Review running processes and parent-child relationships                        |
| Scheduled Tasks     | `cat /etc/crontab`                                                       | Identify automated jobs and execution context                                  |
| User Enumeration    | `id`, `env`, `history`, `sudo -l`, `cat /etc/passwd`                     | Understand users, permissions, environment variables, and access controls      |
| Network Enumeration | `ifconfig`, `ip addr`, `netstat -tpln`, `ss -tulpn`                      | Enumerate interfaces, listening ports, and network exposure                    |
| File Enumeration    | `ls -la`, `find / -name`, `find / -perm -u=s -type f 2>/dev/null`        | Discover sensitive files, permissions, and privilege escalation indicators     |

### Command Selection Rationale

Commands were selected to maximize environmental visibility and follow a structured enumeration methodology rather than randomly collecting outputs. Each command contributed evidence that informed later analysis and security observations.

---

## Investigation Walkthrough

### OS Enumeration

#### Observation

Collected system identity, kernel, and package information.

#### Interpretation

Infrastructure context affects attack surface and operational exposure.

#### Security Perspective

* SOC → Improves visibility and investigation context
* GRC → Supports asset awareness and patch governance

---

### User Enumeration

#### Observation

Reviewed users, permissions, environment variables, and sudo capabilities.

#### Interpretation

Privilege is influenced by relationships between identities and controls.

#### Security Perspective

* SOC → Detect unusual access behavior
* GRC → Evaluate least privilege enforcement

---

### Network Enumeration

#### Observation

Examined interfaces, services, and listening ports.

#### Interpretation

Connectivity represents exposure and trust boundaries.

#### Security Perspective

* SOC → Monitor service activity
* GRC → Reduce unnecessary exposure

---

### File Enumeration

#### Observation

Investigated permissions, writable paths, and SUID binaries.

#### Interpretation

Small configuration decisions can create escalation opportunities.

#### Security Perspective

* SOC → Detect abnormal execution behavior
* GRC → Strengthen configuration controls

---

## MITRE ATT&CK Mapping

| Activity            | MITRE Context            |
| ------------------- | ------------------------ |
| System Enumeration  | Discovery                |
| User Enumeration    | Account Discovery        |
| Process Enumeration | Process Discovery        |
| Network Enumeration | System Network Discovery |
| Permission Analysis | Privilege Escalation     |

---

## My Learning Reflection

This lab changed how I think about privilege escalation.

Before doing this, I thought privilege escalation was mostly about finding a vulnerability and executing an exploit. After completing the enumeration phase manually, I realized that escalation starts much earlier — with observation, context gathering, and connecting small details.

The biggest shift in my thinking was understanding that enumeration is not simply collecting commands or outputs. It is a structured investigation process.

**Every command answered a different question:**

* What environment am I operating in?
* Who has access?
* What services exist?
* Which processes create risk?
* What assumptions can be validated?

When I checked the hostname and OS information, I understood that system identity matters because infrastructure roles influence attack surface.

When reviewing kernel and package information, I started thinking less like “find exploit” and more like:

> Is this system maintained?

> Is patching happening?

> Are there operational weaknesses?

While enumerating users and permissions, I realized privilege is rarely isolated. Group memberships, environment variables, command history, and sudo rights collectively tell the story of access control.

**Cron jobs** introduced another mindset shift.

I stopped viewing scheduled tasks as automation and started viewing them as trust relationships:

* Who created this task?
* Under which account does it execute?
* Can an unintended user influence execution?

**Network enumeration** also changed my approach.

**Open ports and interfaces** are not just technical outputs — they represent exposure, communication paths, and operational dependencies.

**File enumeration** had the strongest impact on my thinking.

Searching for writable files and SUID binaries made me realize that systems become vulnerable through small configuration decisions rather than dramatic failures.

From a consulting mindset, this lab taught me that security assessment is not about proving compromise.

It is about systematically reducing uncertainty, validating assumptions, and identifying where controls, visibility, and trust boundaries begin to break.

From a **SOC perspective**, I connected enumeration to visibility and investigation. Before detecting threats or responding to alerts, analysts need context about processes, users, services, scheduled tasks, network activity, and system behavior to understand what normal operations look like.

From a **GRC perspective**, I realized that technical findings often reveal larger governance and control issues. Weak permissions, unmanaged services, outdated software, excessive access, and poor configuration decisions are not only technical observations — they can indicate risk management gaps and ineffective security controls.

When relating this lab to the **MITRE ATT&CK framework**, I understood that many enumeration activities align with adversary discovery behavior. Commands used to gather information about accounts, system configuration, processes, network settings, permissions, and services mirror how attackers build situational awareness before moving into escalation or lateral movement.

Instead of asking:

> “How do I get root?”

I started asking:

> “How does this system trust users, services, and processes — and where could that trust fail?”

And then expanding that further into:

> “What controls exist, how would a SOC detect misuse, and how would governance reduce the likelihood or impact of that risk?”

**This lab strengthened three skills I want to continue developing:**

1. **Structured investigation**
2. **Evidence-based analysis**
3. **Thinking in terms of risk, exposure, and operational impact**

### Final Takeaway

Enumeration is not reconnaissance before the real work.

Enumeration **is** the real work that makes informed security decisions possible.

---

## Skills Demonstrated

* Linux Enumeration
* Threat Thinking
* Documentation
* Security Analysis
* SOC Investigation Mindset
* Risk-Based Thinking

---

## Attribution

This investigation was completed using a lab environment provided by **TryHackMe**.

All analysis, observations, documentation structure, reflections, and security interpretations are my own.

