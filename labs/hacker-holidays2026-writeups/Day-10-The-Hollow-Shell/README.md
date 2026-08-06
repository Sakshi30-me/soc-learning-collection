# Day 10 – The Hollow Shell

> A web exploitation challenge focused on assessing an insecure file upload mechanism and understanding how improper ZIP archive extraction can lead to Remote Code Execution (RCE).

---

# Challenge Information

| Property       | Value                                                                       |
| -------------- | --------------------------------------------------------------------------- |
| **Platform**   | TryHackMe                                                                   |
| **Event**      | Hacker Holidays                                                             |
| **Challenge**  | The Hollow Shell                                                            |
| **Category**   | Web Exploitation                                                            |
| **Difficulty** | Medium                                                                      |
| **Focus**      | File Upload Security, ZIP Archive Analysis, Zip Slip, Remote Code Execution |
| **Link**       | [The Hollow Shell](https://tryhackme.com/room/hh-thehollowshell-ddb582ac)   | 
---

# Challenge Overview

<img width="1262" height="707" alt="image" src="https://github.com/user-attachments/assets/eb85f9d4-ddae-461f-9fa1-b467f3981877" />

Day 10 focused on assessing a web application's ZIP upload functionality. After authenticating to the application, users could upload ZIP archives containing a manifest file used by the system.

The challenge involved understanding how uploaded archives were extracted, identifying weaknesses in the extraction process, and analyzing how those weaknesses could be chained into server-side code execution.

Rather than being about complex exploitation, this room emphasized careful enumeration, understanding application behavior, and recognizing insecure archive extraction.

---

# Objectives

* Enumerate the target application.
* Analyze the authentication mechanism.
* Investigate the ZIP upload feature.
* Understand how uploaded archives are processed.
* Identify insecure archive extraction.
* Demonstrate the impact of a Zip Slip vulnerability.
* Retrieve the challenge flag.

---

# Methodology

The investigation followed a structured workflow:

1. Enumerated the exposed service.
2. Inspected the web application.
3. Reviewed the application's source code.
4. Authenticated using the discovered credentials.
5. Explored the ZIP upload functionality.
6. Created test archives to understand processing behavior.
7. Identified insecure archive extraction.
8. Analyzed how the vulnerability could lead to Remote Code Execution.
9. Retrieved the challenge flag.

---

# Investigation Walkthrough

## 1. Initial Enumeration

The assessment began with basic reconnaissance.

The scan identified an HTTP service hosting the target application.

This became the primary focus for further investigation.

<img width="787" height="547" alt="image" src="https://github.com/user-attachments/assets/df11a4ca-3324-4aed-ada9-07f5f64ff2e0" />

---

## 2. Source Code Review

Instead of attempting credential guessing, I reviewed the application's HTML source.

During this review, I discovered hardcoded credentials left within the page, allowing successful authentication into the application.

This reinforced the importance of checking publicly accessible client-side code during web assessments.

<img width="1100" height="376" alt="image" src="https://github.com/user-attachments/assets/dcc881b7-b050-4394-844a-6a7e2bc32314" />

---

## 3. Exploring the Upload Functionality

After logging in, the application presented a ZIP upload feature.

To understand its expected format, I created a simple archive containing only a valid manifest.

The upload completed successfully, confirming that the server automatically extracted and processed uploaded archives.

<img width="1182" height="755" alt="image" src="https://github.com/user-attachments/assets/6b22baa6-4310-4505-b067-7028b30539ee" />

---

## 4. Payload Generation

To better understand how the upload feature worked, I created a small Python script that generated a ZIP archive containing a valid manifest together with an additional file placed using a crafted archive path.

```python
import json
import zipfile

manifest = {
    "name": "reverse",
    "assets": []
}

# Placeholder hook used for demonstration purposes.
callback = """
# Example hook file
print("Hook executed")
"""

with zipfile.ZipFile("test-upload.zip", "w") as archive:
    archive.writestr("shell.json", json.dumps(manifest))
    archive.writestr("../../hooks/callback.py", callback)
```

This script demonstrates the archive structure used during testing without including the working exploit used in the challenge.

---

## 5. Archive Processing Analysis

Further testing revealed that the extraction routine trusted the internal file paths stored inside uploaded ZIP archives.

Because these paths were not properly validated, files containing directory traversal sequences (`../`) could be written outside the intended upload directory.

This behavior exposed a classic **Zip Slip** vulnerability.

<img width="1088" height="300" alt="image" src="https://github.com/user-attachments/assets/ce1b2809-3685-41bc-a3c1-9eb3f5b139f2" />

---

## 6. Impact Analysis

The application automatically loaded files placed inside its **hooks** directory.

By combining this behavior with the insecure archive extraction process, it became possible to place files where the application would later execute them.

This transformed what initially appeared to be a simple file upload issue into a path leading toward **Remote Code Execution (RCE)**.

Once server-side code execution was achieved, further system enumeration allowed retrieval of the challenge flag.

<img width="1037" height="526" alt="image" src="https://github.com/user-attachments/assets/d670504d-9f5e-47bf-88c7-bbe792701727" />

<img width="883" height="428" alt="image" src="https://github.com/user-attachments/assets/4a6c1c31-8323-4fb7-bd08-19856f754dfa" />

---

# Security Analysis

## Finding

The application was vulnerable to **Zip Slip**, an insecure archive extraction vulnerability caused by insufficient validation of file paths inside uploaded ZIP archives.

Combined with automatic processing of uploaded content, this weakness could ultimately result in Remote Code Execution.

---

## Potential Risks

* Arbitrary file write
* Directory Traversal
* Remote Code Execution (RCE)
* Server compromise
* Unauthorized file modification
* Deployment of malicious server-side code
* Loss of application integrity

---

# Defensive Considerations

To mitigate these risks, applications should:

* Validate all archive paths before extraction.
* Normalize extraction paths to prevent directory traversal.
* Reject ZIP archives containing `../` sequences.
* Store uploaded files outside executable directories.
* Never automatically execute user-controlled files.
* Restrict permissions on upload directories.
* Perform server-side validation of uploaded content.
* Regularly review file upload functionality during security assessments.

---

# MITRE ATT&CK Mapping

| Technique                                     | Description                                                                             |
| --------------------------------------------- | --------------------------------------------------------------------------------------- |
| **T1190 – Exploit Public-Facing Application** | Exploiting a vulnerability in an internet-facing web application.                       |
| **T1105 – Ingress Tool Transfer**             | Uploading files to the compromised system during exploitation.                          |
| **T1505.003 – Web Shell**                     | Executing attacker-controlled server-side code through uploaded application components. |

> These mappings are provided for educational purposes to relate the challenge to real-world attacker techniques.

---

# What I Learned

This room reinforced several important lessons:

* Always inspect HTML source before attempting more complex attacks.
* File upload functionality requires careful security validation.
* ZIP extraction routines should never trust user-controlled file paths.
* Small implementation mistakes can lead to severe security consequences.
* Understanding application behavior is often more valuable than immediately searching for exploits.
* Careful experimentation with simple test files can reveal critical vulnerabilities.

---

# Skills Demonstrated

* Web Application Enumeration
* Source Code Analysis
* File Upload Security Assessment
* ZIP Archive Analysis
* Directory Traversal Identification
* Zip Slip Analysis
* Remote Code Execution Concepts
* Security Investigation
* Technical Documentation
* Defensive Security Thinking

---

# References

* TryHackMe – [The Hollow Shell](https://tryhackme.com/room/hh-thehollowshell-ddb582ac)
* OWASP File Upload Cheat Sheet
* OWASP Top 10
* MITRE ATT&CK Framework
* Snyk – Zip Slip Vulnerability Research

---

# Disclaimer

This documentation is intended for educational and portfolio purposes.

The challenge was completed within an authorized TryHackMe training environment.

To preserve the learning experience of other participants, this repository intentionally excludes:

* Challenge flags
* Working reverse shell payloads
* Complete exploitation payloads
* Sensitive challenge-specific information

The purpose of this write-up is to demonstrate my investigation methodology, technical understanding, and defensive security perspective rather than provide a complete walkthrough.
