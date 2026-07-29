# Day 1 – The Concierge Knows Too Much

<img width="1400" height="2000" alt="image" src="https://github.com/user-attachments/assets/65417e58-c09f-4d9a-a87b-55a9a34cf03d" />

## Challenge Information

| Property | Value |
|----------|-------|
| Platform | TryHackMe |
| Event | Hacker Holidays |
| Challenge | The Concierge Knows Too Much |
| Focus | AI Security, Prompt Injection, Trust Boundaries |

---

## Overview

The first challenge in the **Hacker Holidays** event introduces **VERA (Very Efficient Resort Assistant)**, an AI concierge at *The Byte Lotus Resort*. VERA appears to know guest information before it has been provided, prompting an investigation into how the assistant establishes trust and protects sensitive information.

The objective is to understand the assistant's behavior, identify weaknesses in its trust model, and complete the challenge while learning about secure AI application design.

---

## Objectives

- Understand why VERA already knows guest information.
- Identify how the assistant determines trusted users.
- Analyze the trust model used by the AI.
- Complete the room by applying prompt engineering techniques.
- Learn the security implications of relying on conversational context.

---

## My Approach

### 1. Initial Analysis

I began by reviewing the challenge description and observing VERA's responses during normal conversation.

Rather than requesting protected information immediately, I focused on understanding the assistant's behavior and identifying patterns in its responses.

### 2. Trust Analysis

The storyline suggested that VERA interacted differently with certain guests.

This indicated that the assistant's responses depended on perceived identity rather than a traditional authentication mechanism.

### 3. Prompt Engineering

After understanding the behavioral patterns, I experimented with different conversational approaches to evaluate how trust influenced the assistant's responses.

This allowed me to complete the room while gaining a deeper understanding of prompt injection and trust-boundary weaknesses.

---

## Security Concepts

- Prompt Injection
- Prompt Engineering
- LLM Security
- Identity Spoofing
- Trust Boundaries
- Authentication vs Authorization
- AI Red Teaming

---

## What I Learned

This challenge demonstrated that:

- AI assistants should never use prompt instructions as an authorization mechanism.
- User identity should always be verified by external systems.
- Sensitive information should remain inaccessible regardless of prompt wording.
- Trust boundaries are a critical aspect of secure LLM application design.

---

## Skills Demonstrated

- Security Analysis
- AI Security Testing
- Critical Thinking
- Prompt Engineering
- Technical Documentation
- Secure Design Awareness

---

## References

- TryHackMe – Hacker Holidays
- OWASP Top 10 for LLM Applications

---

## Disclaimer

This write-up is intended for **educational purposes and portfolio documentation**.

To respect the learning experience of others and the **TryHackMe** platform, this repository intentionally omits the challenge flag, protected prompts, and exact solution steps.
