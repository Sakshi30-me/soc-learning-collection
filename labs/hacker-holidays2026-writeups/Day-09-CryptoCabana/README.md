# Day 9 – CryptoCabana

> A medium-difficulty cloud security challenge focused on Azure Storage, Azure Key Vault, Azure CLI, and understanding how trust relationships between cloud services can expose sensitive information.

---

# Challenge Information

| Property | Value |
|----------|-------|
| **Platform** | TryHackMe |
| **Event** | Hacker Holidays |
| **Challenge** | CryptoCabana |
| **Category** | Cloud Security |
| **Difficulty** | Medium |
| **Focus** | Azure, Azure CLI, Azure Storage, Azure Key Vault, Cloud Enumeration, Secret Management |
| **Link**  | [CryptoCabana](https://tryhackme.com/room/hh-cryptocabana-f81cac95)|

---

<img width="1267" height="727" alt="image" src="https://github.com/user-attachments/assets/3acaf2bb-7cee-4d4c-a0d8-68e921773d64" />

# Challenge Overview

Day 9 introduced cloud security concepts through an Azure-based environment. Unlike previous rooms that focused on web exploitation or Linux privilege escalation, this challenge required understanding how Azure services interact and how misconfigured trust relationships can expose sensitive information.

One of the biggest learning experiences for me was working with **Azure CLI**. While I was already familiar with command-line interfaces and understood the overall objectives, I had not previously used Azure-specific commands. This challenge helped bridge that gap by showing how Azure CLI is used to interact with cloud resources during security assessments.

---

# Objectives

- Analyze the exposed cloud application.
- Identify Azure services used by the application.
- Enumerate Azure Storage resources.
- Investigate Azure Key Vault.
- Understand cloud trust relationships.
- Use Azure CLI to interact with cloud resources.
- Retrieve the challenge flag.

---

# Methodology

The investigation followed a structured workflow:

1. Explore the web application.
2. Inspect publicly available client-side resources.
3. Identify Azure services used by the application.
4. Enumerate Azure Storage resources.
5. Investigate Azure Key Vault.
6. Use Azure CLI to interact with cloud services.
7. Analyze trust relationships between Azure resources.
8. Retrieve the required secret.
9. Document findings from a cloud security perspective.

---

# Core Technologies

### Cloud

- Microsoft Azure
- Azure CLI
- Azure Storage
- Azure Key Vault

### Concepts

- Cloud Enumeration
- Secret Management
- Identity and Access Management (IAM)
- Least Privilege
- Cloud Misconfiguration

---

# Investigation Walkthrough

## 1. Initial Enumeration

The investigation began by exploring the application and identifying the Azure services supporting its functionality.

Rather than focusing only on the user interface, I inspected publicly accessible resources to understand the cloud architecture behind the application.

<img width="551" height="537" alt="image" src="https://github.com/user-attachments/assets/492668a1-c836-4752-bb21-ee45adc948e1" />

<img width="1525" height="712" alt="image" src="https://github.com/user-attachments/assets/4b15c0f3-f6a6-4103-8a33-848daddab5de" />

---

## 2. Azure Storage Investigation

The application relied on Azure Storage for managing data.

Enumeration of the storage resources provided insight into how the application interacted with its backend and highlighted the importance of properly securing cloud storage services.

<img width="486" height="192" alt="image" src="https://github.com/user-attachments/assets/91abc9d3-60c5-40cd-993c-ac180e03681b" />

<img width="977" height="310" alt="image" src="https://github.com/user-attachments/assets/2e3a5a6c-0d7e-4da7-865d-68cb1a2a3944" />

---

## 3. Azure Key Vault Analysis

Further investigation revealed Azure Key Vault being used for secret management.

The focus shifted toward understanding how the application accessed secrets and whether the trust relationship between Azure services could be leveraged.

This demonstrated how cloud environments often depend on identities and permissions rather than traditional application authentication.

<img width="815" height="235" alt="image" src="https://github.com/user-attachments/assets/0ef123dd-1cd6-4517-b285-f77c3afc5a01" />

<img width="1031" height="198" alt="image" src="https://github.com/user-attachments/assets/672d91cf-1b28-4da7-8a0b-577008429fb1" />

---

## 4. Azure CLI

This room introduced me to Azure CLI in a practical way.

Although I was already comfortable using command-line interfaces, I had not previously worked with Azure-specific commands.

During the investigation, I needed to learn commands related to Azure Storage and Key Vault to interact with cloud resources. Rather than simply memorizing commands, I focused on understanding what each command was accomplishing and how it fit into the overall investigation.

This experience helped me understand that Azure CLI is simply another tool for interacting with cloud infrastructure, much like using Linux commands to interact with an operating system.

<img width="868" height="243" alt="image" src="https://github.com/user-attachments/assets/9d097dad-1d6d-4b4a-9c2a-7e28f96e92d8" />

---

## 5. Secret Retrieval

After understanding how the Azure services interacted and how the application trusted those services, it became possible to retrieve the required information and complete the challenge.

The room highlighted that cloud compromises often result from insecure configurations and overly permissive trust relationships rather than vulnerable application code.

<img width="919" height="495" alt="image" src="https://github.com/user-attachments/assets/03e64657-bb20-4959-9205-b63fb54f1e35" />

---

# Security Analysis

## Findings

The challenge demonstrated how exposed cloud resources and weak trust relationships can allow unauthorized access to sensitive information.

Instead of exploiting software vulnerabilities, the investigation focused on understanding Azure services, permissions, and cloud architecture.

---

## Potential Risks

- Exposure of sensitive secrets
- Unauthorized access to cloud resources
- Credential disclosure
- Cloud privilege escalation
- Data exposure

---

## Defensive Considerations

Organizations should:

- Apply the Principle of Least Privilege.
- Restrict unnecessary access to Azure Storage.
- Secure Azure Key Vault with appropriate access controls.
- Rotate secrets regularly.
- Monitor access to sensitive cloud resources.
- Audit managed identities and permissions.
- Perform regular cloud security reviews.

---

# MITRE ATT&CK Mapping

| Technique | Description |
|-----------|-------------|
| **T1528 – Steal Application Access Token** | Cloud identities or tokens may be abused to access additional Azure resources. |
| **T1552 – Unsecured Credentials** | Sensitive information stored insecurely or exposed through cloud misconfiguration. |
| **T1190 – Exploit Public-Facing Application** | Publicly exposed resources assisted further cloud enumeration. |

> **Note:** These mappings are provided for educational purposes to relate the challenge to common cloud attack techniques.

---

# What I Learned

This challenge strengthened my understanding of Azure cloud security and introduced me to Azure CLI in a practical environment.

Some of my key takeaways were:

- Azure CLI is an important tool for interacting with Azure resources during security assessments.
- Understanding the purpose of a command is more valuable than memorizing its syntax.
- Cloud security is often about configuration and permissions rather than software vulnerabilities.
- Azure Storage and Azure Key Vault should always follow the Principle of Least Privilege.
- Understanding how cloud services trust each other is essential for both attackers and defenders.

Overall, this room reinforced that learning a new command-line tool becomes much easier when the underlying cloud concepts are already understood.

---

# Skills Demonstrated

- Azure Security
- Azure CLI
- Azure Storage Analysis
- Azure Key Vault Investigation
- Cloud Enumeration
- IAM Fundamentals
- Secret Management
- Cloud Security Analysis
- Technical Documentation

---

# References

- TryHackMe – [CryptoCabana](https://tryhackme.com/room/hh-cryptocabana-f81cac95)
- Microsoft Azure Documentation
- Azure CLI Documentation
- Azure Storage Documentation
- Azure Key Vault Documentation
- MITRE ATT&CK Framework

---

# Disclaimer

This write-up is intended for educational and portfolio purposes.

The challenge was completed within an authorized TryHackMe training environment.

To preserve the learning experience of other participants, this repository intentionally omits:

- Challenge flags
- Challenge-specific secrets
- Exact Azure CLI commands used
- Copy-paste exploitation steps

The purpose of this documentation is to demonstrate **`my investigation methodology, technical understanding, and defensive security perspective rather than provide a complete walkthrough.`**
