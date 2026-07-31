# Day 4 – Packed Light

> A network traffic analysis challenge focused on identifying suspicious HTTP activity and understanding how data can be concealed within seemingly normal web traffic.

## Challenge Information

| Property | Value |
|---|---|
| Platform | TryHackMe |
| Event | Hacker Holidays |
| Challenge | Packed Light |
| Category | Network Security / Traffic Analysis |
| Focus | PCAP Analysis, HTTP, Cookies, Encoding, Traffic Analysis |
| Link | [TryHackMe – Packed Light](https://tryhackme.com/room/hh-packedlight-02e5330c) |

<img width="1533" height="715" alt="image" src="https://github.com/user-attachments/assets/69a5c7fc-0214-4a56-8183-3e20acded39e" />

---

## Challenge Overview

Day 4 of Hacker Holidays focused on analyzing a provided `traffic.pcapng` file to understand network communication and identify interesting HTTP traffic.

The investigation involved reviewing HTTP requests and responses, identifying an unusual resource, and analyzing the returned source code to understand how the client communicated with the server.

The challenge also provided an opportunity to understand how repetitive packet-analysis tasks can be automated with Python.

---

## Objectives

- Analyze the provided PCAPNG file.
- Identify relevant network traffic.
- Investigate HTTP requests and responses.
- Identify unusual or interesting resources.
- Analyze exposed application source code.
- Understand the communication mechanism used by the client.
- Identify how transmitted data was encoded and transformed.
- Understand where automation can improve the investigation process.

---

## Methodology

The investigation followed a structured approach:

1. Review the challenge description and identify potential clues.
2. Inspect the provided PCAPNG file.
3. Identify protocols and interesting network conversations.
4. Focus on HTTP traffic because it provides readable application-layer information.
5. Investigate unusual HTTP requests.
6. Analyze the `/temp/updates.py` resource.
7. Review the returned Python source code.
8. Identify the cookie and data-transformation mechanism.
9. Analyze the relevant packet data.
10. Use Python to automate repetitive processing.
11. Interpret the findings from a defensive security perspective.

---

## Investigation Walkthrough

### 1. PCAP Analysis

The first step was to inspect the provided PCAPNG file and determine what types of network traffic were present.

For the initial analysis, I used the [A-Packets online PCAP](https://apackets.com) analysis tool to inspect the capture and review the available network traffic.

The analysis focused on HTTP traffic because it provided readable application-layer information such as:

- HTTP requests
- HTTP responses
- Cookies
- Headers
- Requested resources
- Server information

This allowed me to identify interesting HTTP requests without relying on a locally installed packet-analysis application.

<img width="1146" height="822" alt="image" src="https://github.com/user-attachments/assets/89bf5cad-e8c2-4289-af5f-f2d00c0ab47b" />

---

### 2. Identifying Interesting HTTP Traffic

During the HTTP investigation, an interesting request was identified:

    GET /temp/updates.py HTTP/1.1

The server responded successfully:

    HTTP/1.0 200 OK
    Content-Type: text/x-python

This indicated that the server was providing Python source code through HTTP.

This was an important finding because source code can reveal how an application communicates with external systems.

<img width="1304" height="680" alt="image" src="https://github.com/user-attachments/assets/8fa3fd5e-9cec-40a3-9422-0970e52046dd" />

---

### 3. Source Code Analysis

The returned Python code revealed important details about the communication mechanism.

The investigation identified:

- The server endpoint used by the client.
- The cookie used for transmitting data.
- The construction of the key.
- The XOR transformation.
- Base64 encoding.
- The method used to send data to the server.

The relevant cookie was:

    hotel_sess_state

The source code showed that information was transformed before being placed inside the cookie and transmitted through HTTP requests.

<img width="1007" height="827" alt="image" src="https://github.com/user-attachments/assets/2d4035d1-f8c0-48f2-8841-907f8ac03368" />

---

### 4. Understanding the Data Transformation

The observed process could be represented as:

    Original Data
         ↓
    XOR Transformation
         ↓
    Base64 Encoding
         ↓
    HTTP Cookie
         ↓
    HTTP Request

An important observation was that `Base64` is an encoding mechanism, not encryption.

The data transformation involved XOR, after which the resulting bytes were Base64 encoded for transmission.

---

### 5. Automation

After understanding the communication mechanism, Python automation was used to process the relevant packet data.

The general workflow was:

    Read PCAP
         ↓
    Inspect packets
         ↓
    Filter relevant traffic
         ↓
    Locate the relevant cookie
         ↓
    Extract cookie value
         ↓
    Base64 decode
         ↓
    Apply XOR transformation
         ↓
    Reconstruct transmitted data

The purpose of automation was to reduce repetitive manual work and make processing more efficient and consistent.

For a small number of packets, the process could theoretically be performed manually. However, automation becomes increasingly useful when dealing with a larger number of packets or multiple captures.

---

## Security Analysis

### Finding

The investigation demonstrated how application behavior and sensitive communication details can potentially be exposed through network traffic and accessible application resources.

### Security Concerns

Potential concerns include:

- **Sensitive information** being transmitted through HTTP.
- Application source code being accessible through an HTTP resource.
- Client-side communication logic revealing security-sensitive information.
- Sensitive data being placed inside HTTP cookies.
- Custom data-protection mechanisms being used.
- Network traffic providing enough information to reconstruct application behavior.

### Defensive Considerations

Applications should:

- Use HTTPS for sensitive communication.
- Restrict access to development and internal resources.
- Avoid exposing application source code unnecessarily.
- Implement appropriate access controls.
- Avoid custom cryptographic mechanisms when established cryptographic solutions are available.
- Protect sensitive session information.
- Monitor unusual outbound network traffic.
- Review server and application configurations before deployment.

---

## MITRE ATT&CK Mapping

The activity can be considered from a threat-analysis perspective using the following ATT&CK techniques:

### T1071.001 – Web Protocols: HTTP

HTTP was used as the communication mechanism between the client and server.

### T1041 – Exfiltration Over C2 Channel

The captured traffic demonstrated information being transmitted through a communication channel controlled by the application.

> These mappings are provided for educational context and do not imply that the challenge represents a complete real-world implementation of these techniques.

---

## What I Learned

This challenge reinforced several important concepts.

### PCAP Analysis

Packet captures can provide valuable evidence about application behavior and network communications.

### HTTP Investigation

HTTP requests and responses can reveal:

- Requested resources
- Cookies
- Headers
- Server information
- Application behavior

### Source Code Exposure

An accessible source-code resource can reveal `implementation details` that may help an analyst understand an application's network behavior.

### Encoding vs Encryption

Base64 is encoding, not encryption. It is commonly used to represent binary data as text.

### Automation

Automation is useful when the same investigation process needs to be repeated across many packets.

The key workflow I took from this challenge was:

> Understand the traffic → identify the pattern → understand the protocol → automate repetitive analysis.

---

## Skills Demonstrated

- PCAP Analysis
- Online PCAP Analysis
- Network Traffic Analysis
- HTTP Analysis
- Source Code Analysis
- Cookie Analysis
- Base64 Analysis
- XOR Analysis
- Python Automation
- Security Investigation
- Threat Analysis
- Technical Documentation
- Defensive Security Thinking

---

## References

- [TryHackMe – Packed Light](https://tryhackme.com/room/hh-packedlight-02e5330c)
- [A-Packets – PCAP Analysis](https://apackets.com)
- [MITRE ATT&CK](https://attack.mitre.org/)
- [Scapy Documentation](https://scapy.readthedocs.io/)

---

## Disclaimer

This documentation is intended for **educational and portfolio purposes**.

The challenge was performed within an authorized **TryHackMe** training environment.

To respect the learning experience of other participants and the TryHackMe platform, this repository intentionally omits:

- Challenge flags
- Sensitive challenge secrets
- Full solution outputs
- Copy-paste exploitation steps
- Unnecessary reproduction of protected challenge material

The purpose of this documentation is to demonstrate `my investigation methodology, technical understanding, learning process, and defensive security perspective rather than provide a complete walkthrough.`
