# Web Enumeration & Privilege Escalation Analysis — Vulnversity

> **Platform:** TryHackMe
>
> **Lab Type:** Guided Hands-on Lab
>
> **My Role:** Security Learner / Analyst
>
> **Focus Areas:** Reconnaissance · Web Enumeration · Upload Validation · Privilege Escalation · SOC Visibility · Security Controls · MITRE Mapping

---

## Project Overview

This project documents my investigation and analysis of the Vulnversity lab completed on **TryHackMe**.

The **environment and objectives** were provided by the platform, while the `observations, documentation, interpretation, and security reflections` presented here represent my own learning and analytical process.

The project explored how seemingly separate **weaknesses — exposed services, discoverable application paths, weak upload validation, and privilege configuration decisions** — can combine into a larger security risk.

Rather than focusing only on obtaining elevated access, this investigation focused on understanding how visibility gaps and trust assumptions influence attack progression.

---

## Objectives

* Perform structured reconnaissance
* Discover exposed application functionality
* Understand upload validation weaknesses
* Analyze privilege escalation conditions
* Connect findings to defensive security thinking
* Map observations to MITRE ATT&CK concepts

---

## Methodology

The investigation followed this workflow:

1. Reconnaissance
2. Web Enumeration
3. Validation Analysis
4. Access Evaluation
5. Privilege Escalation Investigation
6. Security Reflection & Mapping

---

## Core Commands Used

| Phase                   | Representative Commands | Purpose                           |
| ----------------------- | ----------------------- | --------------------------------- |
| Recon                   | `nmap`                  | Identify exposed services         |
| Web Enumeration         | `gobuster`              | Discover hidden application paths |
| Request Analysis        | Burp Suite              | Observe application behavior      |
| Access Validation       | `nc`                    | Validate connectivity             |
| Privilege Investigation | `find`                  | Review elevated execution paths   |

---

## Investigation Walkthrough

### Phase 1 — Reconnaissance

#### Observation

Enumerated exposed services and application entry points.

#### Interpretation

Attack surface starts with visibility. Services reveal potential business functions and operational exposure.

#### Security Perspective

* SOC → Monitor unusual service interaction patterns.
* GRC → Maintain asset inventory and exposure management.

---

### Phase 2 — Web Enumeration

#### Observation

Application enumeration revealed functionality that was not immediately visible.

#### Interpretation

Hidden functionality is not access control. `Discoverability` alone may increase risk.

#### Security Perspective

* SOC → Monitor unusual path discovery behavior.
* GRC → Review application exposure controls.

---

### Phase 3 — Upload Validation Analysis

#### Observation

The upload mechanism relied on limited validation assumptions.

#### Interpretation

Validation controls should not rely on simple extension checks alone.

This highlighted the importance of layered validation approaches and secure design decisions.

#### Security Perspective

* SOC → Monitor unusual upload behavior.
* GRC → Strengthen application control requirements.

---

### Phase 4 — Access & Execution Analysis

#### Observation

Application functionality created opportunities for execution under inherited trust conditions.

#### Interpretation

Initial access is rarely one event — it is often a sequence of accepted assumptions.

#### Security Perspective

* SOC → `Correlate upload → execution → privilege events`
* GRC → Define stronger application governance.

---

### Phase 5 — Privilege Escalation Investigation

#### Observation

Privilege inheritance and execution permissions significantly affected system risk.

#### Interpretation

Privilege escalation is often created by configuration decisions rather than software defects.

#### Security Perspective

* SOC → Detect abnormal process execution chains.
* GRC → Enforce least privilege and configuration review.

---

## MITRE ATT&CK Mapping

| Investigation Activity     | MITRE Context        |
| -------------------------- | -------------------- |
| Service Enumeration        | Discovery            |
| Application Enumeration    | Discovery            |
| Upload Validation Analysis | Initial Access       |
| Access Evaluation          | Execution            |
| Privilege Investigation    | Privilege Escalation |

---

## My Learning Reflection

This project changed how I think about exploitation. Initially, I viewed compromise as a sequence of tools and commands.

After completing this `investigation`, I realized **successful attacks are usually built through small trust assumptions accumulating across multiple layers**.

**Reconnaissance** created visibility.

**Enumeration** created context.

**Application behavior** revealed assumptions.

**Privilege analysis** revealed control weaknesses.

`What stood out most was that exploitation itself was only a small part of the process.`

The larger lesson was understanding how security decisions interact across infrastructure, applications, and access controls.

Instead of asking:

> “How can access be achieved?”

I started asking:

> “What assumptions allowed this path to exist, and how could those assumptions be reduced or monitored?”

This investigation strengthened:

1. `Structured investigation`
2. `Security analysis`
3. `Risk-oriented thinking`
4. `Documentation and reflection`

---

## Evidence

<img width="746" height="297" alt="image" src="https://github.com/user-attachments/assets/9fedd2e0-b43b-423a-ae4c-50c243c06747" />

<img width="916" height="299" alt="image" src="https://github.com/user-attachments/assets/63c1b11f-aa5a-470d-bf91-75cfe29cc9e8" />

<img width="1002" height="321" alt="image" src="https://github.com/user-attachments/assets/46e53615-0747-4668-bc0d-45a1a7831377" />

---

## Skills Demonstrated

* Reconnaissance
* Web Enumeration
* Linux Fundamentals
* Security Analysis
* Threat Thinking
* Documentation
* SOC Visibility Mindset
* Risk-Based Thinking

---

## Attribution

This investigation was completed using a lab environment provided by TryHackMe.

All observations, documentation, reflections, analysis, and security interpretations are my own.
