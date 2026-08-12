# Day 12 – After Hours

> A Windows forensics challenge focused on WMI repository analysis, hidden configuration data, layered payload extraction, and .NET reverse engineering.

<img width="1228" height="727" alt="image" src="https://github.com/user-attachments/assets/7d7cd036-34f9-44d2-aadf-ebec7de97a4e" />

---

## Challenge Information

| **Property**   | **Value**                                                         |
| -------------- | ----------------------------------------------------------------- |
| **Platform**   | TryHackMe                                                         |
| **Event**      | Hacker Holidays                                                   |
| **Challenge**  | After Hours                                                       |
| **Category**   | Forensics / Windows Persistence / Reverse Engineering             |
| **Difficulty** | Medium                                                            |
| **Focus**      | WMI Repository, Windows Forensics, Base64, DEFLATE, .NET Analysis |
| **Link**       | [After Hours](https://tryhackme.com/room/hh-afterhours-b090d1f0) |
---

# Challenge Overview

Day 12, **After Hours**, was quite different from the earlier web-focused rooms.

Instead of interacting with a traditional web application, the challenge provided a collection of Windows forensic artifacts. The objective was to investigate a persistence mechanism that was intentionally hidden from common locations such as Startup folders, Scheduled Tasks, and Registry Run keys.

The key clue was that the persistence mechanism was hiding somewhere quieter in the Windows environment.

The investigation eventually led to the **Windows Management Instrumentation (WMI) repository**, where a custom WMI class contained an encoded payload.

The challenge then became a layered analysis process:

**`WMI Repository → Encoded PowerShell → Custom WMI Class → Base64 → Raw DEFLATE → .NET Assembly → Base64 → Flag`**

---

# Objectives

* Parse the supplied Windows system artifacts.
* Identify suspicious custom WMI configuration data.
* Locate the malicious WMI class.
* Extract the embedded payload.
* Decode and decompress the payload.
* Analyze the resulting .NET executable.
* Recover the final flag.

---

# Methodology

The investigation followed a structured forensic workflow:

1. Identify the supplied artifacts.
2. Determine the type of Windows data provided.
3. Extract useful strings from the repository.
4. Search for suspicious encoded content.
5. Decode the embedded PowerShell.
6. Identify the custom WMI class and property.
7. Extract the `ConfigData` payload.
8. Decode the Base64 layer.
9. Decompress the raw DEFLATE stream.
10. Analyze the resulting .NET assembly.
11. Follow the final Base64 clue.
12. Recover the flag.

---

# Core Commands & Tools

## Windows Forensics

* WMI Repository artifacts
* `OBJECTS.DATA`
* ASCII / UTF-16LE string extraction
* `grep`

## Encoding & Extraction

* Base64
* Python `zlib`
* Raw DEFLATE

## Reverse Engineering

* ILSpy
* .NET assembly analysis

---

# Investigation Walkthrough

## 1. Identifying the Evidence

The supplied evidence consisted of several files:

```bash
INDEX.BTR
MAPPING1.MAP
MAPPING2.MAP
MAPPING3.MAP
OBJECTS.DATA
```

These files were identified as components of a Windows **WMI repository**.

This was an important discovery because the challenge specifically hinted that the persistence mechanism was not located in the usual Windows persistence locations.

The WMI repository therefore became the primary focus of the investigation.

<img width="451" height="312" alt="image" src="https://github.com/user-attachments/assets/d9b153aa-f66d-4528-a81a-d213a6e4b205" />

---

## 2. Extracting Strings from the Repository

The `OBJECTS.DATA` file appeared to be the most useful artifact because it contains serialized WMI object data.

I extracted both standard ASCII strings and UTF-16LE strings:

```bash
strings -a -n 6 OBJECTS.DATA > ascii.txt
strings -a -el -n 6 OBJECTS.DATA > utf16.txt
```

I initially searched for common persistence and execution-related terms:

```bash
grep -Ein 'powershell|cmd\.exe|wscript|cscript|payload|encoded|base64|FromBase64|IEX|Invoke-|script|CommandLine|EventConsumer|EventFilter|FilterToConsumer|ActiveScript|root\\' ascii.txt utf16.txt
```

The initial search produced a large amount of information, so I changed the approach and looked for long Base64-like strings.

---

## 3. Finding the Encoded PowerShell

I searched the extracted strings for long Base64-style values:

```bash
grep -EIn '[A-Za-z0-9+/]{80,}={0,2}' ascii.txt utf16.txt
```

This produced several interesting results, including a long encoded PowerShell command.

I decoded the Base64 content and interpreted the resulting data as UTF-16LE.

The decoded PowerShell revealed the next stage of the investigation.

The important part was that it accessed a custom WMI class:

```text
ROOT\cimv2:Win32_HardwareTelemetry
```

and retrieved a property called:

```text
ConfigData
```

The script then performed the following operations:

1. Retrieved `ConfigData`.
2. Base64-decoded it.
3. Decompressed it using `DeflateStream`.
4. Loaded the resulting data as a .NET assembly.
5. Invoked the assembly's entry point directly from memory.

This was the key breakthrough.

---

## 4. Understanding the WMI Class

The suspicious class was:

```text
Win32_HardwareTelemetry
```

with the property:

```text
ConfigData
```

The `Win32_` naming convention made the class look similar to legitimate Windows classes.

However, its presence alongside the encoded payload made it suspicious.

Instead of storing a payload as an obvious executable on disk, the challenge used the WMI repository as a place to store the encoded configuration and payload.

This explained why conventional persistence tools could fail to identify it.

---

## 5. Extracting ConfigData

After identifying the class and property, I searched the extracted repository strings for the corresponding encoded data.

A long Base64 value beginning with:

```text
7VZPbFRFGP/
```

stood out as the likely `ConfigData` value.

I extracted the matching value:

```bash
grep '^7VZPbFRFGP/' ascii.txt | head -1 > payload.b64
```

I then decoded the Base64 layer:

```bash
base64 -d payload.b64 > payload.deflate
```

At this point, the output was not a normal executable yet.

The PowerShell loader had already given us an important clue: the data was compressed using **DeflateStream**.

---

## 6. Decompressing the Raw DEFLATE Stream

I used Python's `zlib` module to decompress the raw DEFLATE stream.

The important detail was using `-15`, which tells `zlib` to process the data as a raw DEFLATE stream.

```bash
python3 - <<'PY'
import zlib

data = open("payload.deflate", "rb").read()
out = zlib.decompress(data, -15)

open("payload.exe", "wb").write(out)

print("Written payload.exe:", len(out), "bytes")
PY
```

I then checked the resulting file:

```bash
file payload.exe
```

The output identified it as a Windows PE executable and .NET/Mono assembly:

```text
payload.exe: PE32 executable for MS Windows 4.00 (GUI), Intel i386 Mono/.Net assembly, 3 sections
```

This confirmed that the extraction and decompression process had worked.

---

# 7. Analyzing the .NET Payload

The extracted executable was a .NET assembly, so I opened it in **ILSpy** for static analysis.

The decompiled code revealed that the executable checked the machine name.

The expected hostname was:

```text
bytelotusdc
```

If the hostname matched, the program launched `cmd.exe` and attempted to create a local account named:

```text
patch
```

The command contained another Base64-encoded value.

This was the final layer of the challenge.

<img width="1658" height="886" alt="image" src="https://github.com/user-attachments/assets/de2d429c-9ce6-4664-a19e-18c46d9fd09c" />

---

# 8. Decoding the Final Clue

The value embedded in the `net user` command was Base64 encoded.

I copied the value into a decoder and decoded it.

The resulting plaintext contained the final TryHackMe flag.

The important part of this stage was recognizing that the payload wasn't necessarily the final answer itself. The .NET assembly contained another clue that had to be followed.

---

# Attack Chain

The complete chain can be represented as:

```text
WMI Repository
      ↓
OBJECTS.DATA
      ↓
Encoded PowerShell
      ↓
Win32_HardwareTelemetry
      ↓
ConfigData
      ↓
Base64
      ↓
Raw DEFLATE
      ↓
.NET PE Assembly
      ↓
ILSpy Analysis
      ↓
Base64 value in command
      ↓
TryHackMe Flag
```

---

# Security Analysis

## Finding — Hidden Payload Storage in WMI

The challenge demonstrated how WMI can be abused as a location for storing malicious configuration data and payloads.

### Risk

* Persistence mechanisms may evade basic autorun checks.
* Payloads may not exist as obvious standalone files.
* WMI repository data can contain suspicious custom classes.
* Encoded or compressed content can make forensic identification more difficult.
* In-memory assembly loading can reduce traditional file-based indicators.

### Defensive Considerations

Security teams should:

* Monitor unusual WMI activity.
* Investigate suspicious custom WMI classes.
* Monitor WMI persistence mechanisms.
* Collect and preserve WMI repository artifacts during investigations.
* Look for encoded or compressed payloads within suspicious WMI data.
* Correlate WMI activity with process creation and PowerShell logging.
* Enable appropriate PowerShell and Windows security logging.

---

# What I Learned

**After Hours** was one of the more unfamiliar challenges for me because I had not previously worked deeply with Windows WMI repository artifacts.

The biggest learning point was understanding that Windows persistence doesn't always live in the obvious places.

The challenge made me look beyond:

* Startup locations
* Scheduled Tasks
* Registry Run keys

and investigate the **WMI repository** instead.

I also learned how different layers of encoding and compression can be followed by working backwards from the loader:

**Base64 → DEFLATE → .NET Assembly → Static Analysis → Base64**

That was probably the most valuable part of the room for me because it showed how forensic analysis can turn a seemingly random collection of bytes and strings into a clear execution chain.

---

# Skills Demonstrated

* Windows Forensics
* WMI Repository Analysis
* Persistence Analysis
* String Extraction
* ASCII / UTF-16LE Analysis
* Base64 Analysis
* DEFLATE Decompression
* Python Scripting
* .NET Reverse Engineering
* ILSpy
* Malware Analysis Concepts
* Technical Documentation
* Security Investigation

---

# MITRE ATT&CK Mapping

| **Technique**                                           | **Relevance**                                                                                   |
| ------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| **T1047 – Windows Management Instrumentation**          | WMI was used as the underlying mechanism for storing and accessing the malicious configuration. |
| **T1059.001 – PowerShell**                              | PowerShell was used to retrieve, decode, decompress, and load the embedded payload.             |
| **T1027 – Obfuscated/Compressed Files and Information** | Base64 encoding and DEFLATE compression were used to conceal the payload.                       |
| **T1055 / In-Memory Execution Concepts**                | The .NET assembly was loaded directly into memory rather than executed as a conventional file.  |

> The exact ATT&CK mapping can vary depending on which stage of the challenge is being mapped.

---

# Key Takeaway

**After Hours** demonstrated that effective Windows forensics requires looking beyond the locations that are normally checked first.

The most important lesson for me was:

> **If the obvious persistence locations are clean, don't assume the system is clean — understand where Windows can store and execute configuration in less obvious ways.**

The room also gave me practical experience following a layered payload from raw forensic artifacts all the way to the final decoded value.

---

# References

* TryHackMe – Hacker Holidays
* TryHackMe – [After Hours](https://tryhackme.com/room/hh-afterhours-b090d1f0)
* Microsoft Windows Management Instrumentation Documentation
* MITRE ATT&CK Framework
* ILSpy
* Python `zlib` Documentation

---

# Disclaimer

This documentation is intended for **educational and portfolio purposes**.

The investigation was conducted within an authorized TryHackMe training environment.

To preserve the learning experience of other participants, this repository intentionally omits:

* The final challenge flag
* Sensitive challenge-specific values
* Complete persistence details
* Any credentials recovered during the challenge

The purpose of this write-up is to demonstrate **my investigation methodology, technical understanding, forensic analysis, and security learning rather than provide a complete solution to the challenge.**
