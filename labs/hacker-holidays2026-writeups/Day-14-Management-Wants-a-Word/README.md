# Day 14 – The Management Wants a Word

> A Windows digital forensics and credential-recovery challenge involving registry hives, NTLM password cracking, DPAPI, Chrome credentials, and a VeraCrypt container.

<img width="1300" height="706" alt="image" src="https://github.com/user-attachments/assets/9fe30fde-f293-4c4a-b3bb-1ac180423baf" />

---

## Challenge Information

| Property      | Value                                                                       |
| ------------- | --------------------------------------------------------------------------- |
| **Platform**  | TryHackMe                                                                   |
| **Event**     | Hacker Holidays                                                             |
| **Challenge** | The Management Wants a Word                                                 |
| **Category**  | Windows / Digital Forensics                                                 |
| **Focus**     | Windows Triage, Registry Hives, NTLM, DPAPI, Browser Credentials, VeraCrypt |
| **Link**      | [Management Wants a Word](https://tryhackme.com/room/hh-managementwantsaword-6bf3cc41) |

---

# Challenge Overview

Day 14 was quite different from the previous web-focused rooms.

Instead of attacking a live web application, the challenge provided a Windows triage image containing artifacts from a user's system.

The objective was essentially to follow the trail from the available Windows artifacts until reaching the final flag.

The investigation involved several layers:

**Windows Registry → NTLM Hash → Password → DPAPI Master Key → Chrome Credentials → VeraCrypt Container → Invoice → Flag**

This was probably one of the more challenging rooms for me because it required understanding several different areas of Windows security and digital forensics rather than relying on a single vulnerability.

<img width="1400" height="2000" alt="5dbea226085ab6182a2ee0f7-1785253542845" src="https://github.com/user-attachments/assets/14188fd0-5781-4d23-8fd2-42de4427d34c" />

---

# Investigation Methodology

The overall process was:

1. Inventory the supplied Windows triage image.
2. Identify the relevant Windows artifacts.
3. Extract Vera's NTLM password hash.
4. Crack the password offline.
5. Use the recovered password to decrypt Vera's DPAPI master key.
6. Recover the Chrome encryption key.
7. Decrypt the saved Chrome credential.
8. Use the recovered credential to access the VeraCrypt container.
9. Mount the container read-only.
10. Locate the relevant invoice.
11. Recover the final flag.

---

<img width="437" height="368" alt="image" src="https://github.com/user-attachments/assets/50fd6509-d3a5-4a4b-a3fd-c4f391fdbf09" />

# 1. Triage

The supplied evidence appeared to be a Windows triage image extracted with KAPE.

The first step was simply to inventory the available files and confirm that the expected Windows artifacts were present.

Important artifacts included:

* `Users`
* `Windows`
* `SAM`
* `SYSTEM`
* Vera's user profile

The registry hive directory also contained a file named:

```text
nt.john
```

This suggested that the Windows credential artifacts could be used for offline password recovery.

<img width="1044" height="484" alt="image" src="https://github.com/user-attachments/assets/12388432-b73a-4937-a12f-fa26960e4f98" />

---

# 2. Extracting Vera's NTLM Hash

The `SAM` and `SYSTEM` registry hives were available, allowing the local Windows account hashes to be extracted offline.

I used Impacket's `secretsdump`:

```bash
impacket-secretsdump -sam SAM -system SYSTEM LOCAL
```

The extracted entry for Vera was:

```text
vera:1000:aad3b435b51404eead3b435b51404ee:1241186a4aac4f34f4bf7ace71b396a8:::
```

The important value here was Vera's NTLM hash:

```text
1241186a4aac4f34f4bf7ace71b396a8
```

<img width="857" height="293" alt="image" src="https://github.com/user-attachments/assets/4bfe1434-e8b0-4f64-9dd3-dead90a5bfb7" />

---

# 3. Cracking the Password

I then prepared the hash for John the Ripper and used the RockYou wordlist:

```bash
echo 'vera:$NT$1241186a4aac4f34f4bf7ace71b396a8' > nt.john

john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt nt.john
```

The password recovered was:

```text
minivera
```

This password became important for the next stage of the investigation because Vera's Windows credentials could be used to recover her DPAPI-protected secrets.

---

# 4. Decrypting the DPAPI Master Key

The next important artifact was Vera's DPAPI master key located under her Windows profile:

```text
C/Users/vera/AppData/Roaming/Microsoft/Protect/S-1-5-21-2529683458-431225740-1723070931-1000/c90719ef-5b98-474e-b934-136d606a702a
```

Using the recovered password and Vera's SID, I used Impacket's DPAPI functionality:

```bash
impacket-dpapi masterkey \
  -file "C/Users/vera/AppData/Roaming/Microsoft/Protect/S-1-5-21-2529683458-431225740-1723070931-1000/c90719ef-5b98-474e-b934-136d606a702a" \
  -sid "S-1-5-21-2529683458-431225740-1723070931-1000" \
  -password minivera
```

This successfully decrypted the DPAPI master key.

The recovered key was then used to access secrets protected through Windows DPAPI.

<img width="1200" height="261" alt="image" src="https://github.com/user-attachments/assets/50c50445-eaa1-4aa8-92fe-4fa60cbd41fd" />

---

# 5. Recovering the Chrome Credential

The next clue pointed toward Chrome.

Chrome stores its browser encryption information in the user's `Local State` file, while saved credentials are stored in the `Login Data` database.

The important part of the investigation was understanding that the Chrome encryption key itself was protected using Windows DPAPI.

After recovering the DPAPI master key, I was able to decrypt the relevant Chrome data offline.

The recovered saved credential was:

```text
URL:      http://bytelotus.thm:8080/
Username: VeraSecretVault
Password: Wh4t1sV3raD0inG0nTh1sH0st
```

This credential provided the next clue in the chain.

---

# 6. Opening the VeraCrypt Container

The recovered password pointed toward a VeraCrypt container stored in Vera's Documents directory.

The container was opened using `cryptsetup` with VeraCrypt support:

```bash
sudo cryptsetup open \
  --type tcrypt \
  --veracrypt \
  "C/Users/vera/Documents/backup" \
  veracontainer
```

I then created a mount point:

```bash
sudo mkdir -p /mnt/veradata
```

And mounted the volume read-only:

```bash
sudo mount -o ro /dev/mapper/veracontainer /mnt/vera
```

Using a read-only mount was useful because it allowed the contents to be examined without unnecessarily modifying the evidence.

<img width="493" height="228" alt="image" src="https://github.com/user-attachments/assets/1171fed0-04bf-4ec0-a6c3-043c08c39ef1" />

---

# 7. Investigating the Mounted Volume

The mounted VeraCrypt volume contained several directories and files.

Among them were files that appeared to contain financial and private information.

The two particularly interesting files were:

```text
important_invoice_byte_lotus.pdf
```

and a CSV transaction export.

The invoice became the final point of investigation.

<img width="434" height="238" alt="image" src="https://github.com/user-attachments/assets/447c6694-4a86-4301-a45a-ab240ded91ee" />

---

# 8. Finding the Flag

Opening:

```text
important_invoice_byte_lotus.pdf
```

revealed the final flag embedded within the invoice.

This completed the challenge.

<img width="868" height="646" alt="image" src="https://github.com/user-attachments/assets/90d087fa-b704-4885-b794-fb5bb2828425" />

---

# Attack / Investigation Chain

The entire investigation can be summarized as:

```text
Windows Triage Image
        ↓
SAM + SYSTEM
        ↓
NTLM Hash
        ↓
Password Cracking
        ↓
Vera's Password
        ↓
DPAPI Master Key
        ↓
Chrome Encryption Key
        ↓
Saved Chrome Credential
        ↓
VeraCrypt Password
        ↓
VeraCrypt Container
        ↓
Invoice
        ↓
Flag
```

---

# MITRE ATT&CK Mapping

This challenge is primarily a **credential-access and discovery** exercise rather than a traditional exploitation challenge.

Relevant ATT&CK concepts include:

| Technique                                    | Relevance                                                                                      |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| **T1003 – OS Credential Dumping**            | Windows SAM/SYSTEM artifacts were used to recover local account credential material.           |
| **T1110 – Brute Force**                      | Offline password guessing/cracking was used against the recovered NTLM hash.                   |
| **T1555 – Credentials from Password Stores** | Saved browser credentials were recovered from Chrome.                                          |
| **T1552 – Unsecured Credentials**            | Credentials stored within accessible system artifacts were recovered during the investigation. |
| **T1005 – Data from Local System**           | Local files and artifacts were examined to locate sensitive information and the final flag.    |

These mappings are intended to connect the CTF investigation to corresponding real-world attacker techniques.

---

# What I Learned

This challenge taught me that sometimes a security investigation isn't about immediately finding an obvious vulnerability.

The important part was following the evidence.

A single recovered password led to DPAPI, which led to Chrome credentials, which led to the encrypted container, and finally to the invoice containing the flag.

The biggest takeaway for me was understanding how different Windows security mechanisms connect together:

**NTLM → Windows credentials → DPAPI → Browser secrets → Encrypted storage**

It also gave me more exposure to digital forensics and Windows internals, which is an area I want to understand better alongside web security.

---

# Skills Demonstrated

* Windows Digital Forensics
* Windows Registry Analysis
* KAPE Triage Analysis
* NTLM Hash Extraction
* Offline Password Cracking
* Impacket
* DPAPI Analysis
* Chrome Credential Analysis
* VeraCrypt
* Linux Command Line
* Evidence-Based Investigation
* Security Documentation

---

# Conclusion

Day 14 was one of the more interesting challenges because it required connecting multiple pieces of evidence instead of relying on a single exploit.

The investigation started with a Windows triage image and ended with a flag hidden inside an invoice.

The most useful lesson was the importance of understanding **how credentials and secrets move between different layers of a system**. Once one layer was understood, it provided the information needed to investigate the next.

---

## Disclaimer

This write-up is intended for educational and portfolio purposes.

The investigation was performed in an authorized TryHackMe training environment.

Challenge flags and sensitive challenge-specific values may be omitted from the public repository to preserve the learning experience for other participants.
