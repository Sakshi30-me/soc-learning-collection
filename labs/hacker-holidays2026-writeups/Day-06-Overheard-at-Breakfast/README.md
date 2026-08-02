# Day 6 – Overheard at Breakfast

> *Sometimes the biggest clues aren't hidden in systems—they're hidden in conversations.*

<img width="1236" height="665" alt="image" src="https://github.com/user-attachments/assets/f8d74ac4-6b92-42ee-86c3-1dc58d9fd61a" />


## Challenge Information

| Property | Value |
|----------|-------|
| **Platform** | TryHackMe |
| **Event** | Hacker Holidays |
| **Challenge** | Overheard at Breakfast |
| **Category** | OSINT |
| **Difficulty** | Easy |
| **Focus** | Open Source Intelligence (OSINT), Hashing, Social Media Investigation |
| **Link** | [Overheard at Breakfast](https://tryhackme.com/room/hh-overheardatbreakfast-6f01793c) |

---
## Challenge Overview

Day 6 focused on an Open Source Intelligence (OSINT) investigation rather than exploiting a vulnerable system.

A conversation between two individuals, **Lambo** and **Ponzi**, was provided as the primary source of information. At first glance, it appeared to be an ordinary discussion, but careful analysis revealed several subtle clues that could be correlated to identify an online profile.

By piecing together the available information and following the digital trail, the investigation ultimately led to a **Gravatar** profile, where the hidden flag was discovered.

The room emphasized that successful investigations often depend on observation, correlation, and patience rather than technical exploitation.

---

## Methodology

The investigation followed a structured approach:

1. Carefully read the provided conversation.
2. Identify names, references, and contextual clues.
3. Correlate multiple pieces of information rather than relying on a single detail.
4. Investigate the collected information using publicly available sources.
5. Validate findings before drawing conclusions.
6. Follow the digital footprint to the associated Gravatar profile.
7. Retrieve the hidden flag.
8. Document the investigation and security takeaways.

---

## Investigation Walkthrough

### 1. Conversation Analysis

The investigation began by carefully reading the conversation between **Lambo** and **Ponzi**.

Instead of searching random keywords, I focused on understanding the context and identifying details that appeared intentional or unique.

<img width="1175" height="781" alt="conversation" src="https://github.com/user-attachments/assets/3e596e24-5788-48fc-a688-37855c4fb2bf" />

---

### 2. Identifying Useful Clues

Several pieces of information within the conversation stood out.

Individually, they seemed insignificant, but together they helped build a profile that could be investigated further using OSINT techniques.

This reinforced the importance of analyzing context rather than looking for obvious indicators.

---

### 3. Information Correlation

After extracting the relevant clues, I correlated them to narrow down the possible online identity associated with the conversation.

Using publicly available information, the investigation eventually led to a **Gravatar** profile connected to the identified individual.

---

### 4. Verification

The Gravatar profile contained the information required to successfully complete the challenge.

This final step demonstrated how publicly available digital footprints can reveal more information than users may expect when multiple clues are combined.

<img width="1123" height="742" alt="Day6 2026-08-02 171443" src="https://github.com/user-attachments/assets/345c9222-69b6-4a56-a3ec-c37c83d078c7" />

---

## Security Analysis

### Finding

The challenge demonstrated how small pieces of publicly available information can be correlated to identify an individual's online presence.

Although each clue appeared harmless on its own, combining them allowed an investigator to trace a digital footprint and locate a public profile containing additional information.

### Potential Risks

- Digital footprint exposure
- Identity profiling
- Information disclosure
- Social engineering opportunities
- Privacy risks through publicly accessible profiles

### Defensive Considerations

Individuals should:

- Be aware of the information they share across different platforms.
- Regularly review public profile settings.
- Consider how separate pieces of information may be correlated by others.
- Minimize unnecessary personal details in public-facing accounts.

Organizations should:

- Promote OSINT awareness.
- Train employees on operational security (OPSEC).
- Encourage responsible use of online identities and public profiles.

---

# MITRE ATT&CK Mapping

The investigation aligns with the **Reconnaissance** tactic within the MITRE ATT&CK framework. The challenge demonstrated how publicly available information can be collected and correlated to identify an individual's online presence.

| Technique | Description |
|-----------|-------------|
| **T1593 – Search Open Websites/Domains** | Publicly available resources, including a Gravatar profile, were used to gather information and identify the target. |
| **T1589.001 – Gather Victim Identity Information** | Details disclosed during the conversation were correlated to identify an individual's online profile. |

> **Note:** These mappings are provided for educational purposes to demonstrate how attackers may use publicly available information during the reconnaissance phase of an attack.

---

## What I Learned

This challenge reinforced that OSINT investigations are often less about advanced tools and more about careful observation and logical reasoning.

Some key takeaways were:

- Small details become valuable when correlated.
- Publicly available information can unintentionally expose a larger digital footprint.
- Verification is essential before drawing conclusions.
- OSINT investigations require patience, curiosity, and attention to detail.
- A seemingly ordinary conversation can provide enough information to identify an online profile.

---

# Skills Demonstrated

- Open Source Intelligence (OSINT)
- Information Correlation
- Pattern Recognition
- Critical Thinking
- Analytical Reasoning
- Attention to Detail
- Security Analysis
- Technical Documentation

---

# References

- TryHackMe – [Overheard at Breakfast](https://tryhackme.com/room/hh-overheardatbreakfast-6f01793c)
- MITRE ATT&CK Framework
- OSINT Framework

---

# Disclaimer

This write-up is intended for `educational and portfolio purposes.`

The investigation was conducted within an authorized TryHackMe training environment.

To preserve the learning experience for others, this repository intentionally omits:

- Challenge flags
- Exact search terms
- Hidden identifiers
- Step-by-step solution details

The purpose of this documentation is to demonstrate **`my investigation methodology, analytical thinking, and defensive security perspective rather than provide a complete walkthrough.`**
