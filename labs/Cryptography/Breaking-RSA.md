<img width="127" height="150" alt="image" src="https://github.com/user-attachments/assets/e44522fa-1692-4e56-b703-0d4934a6ac05" />

# [Breaking RSA — Weak Key Generation & Cryptographic Security Analysis](https://tryhackme.com/room/breakrsa)

---

## Project Overview

This TryHackMe lab focused on analyzing a poorly implemented RSA configuration and understanding how weak key generation can undermine the security of an otherwise strong cryptographic algorithm.

The environment involved identifying exposed services, discovering a hidden web directory, locating an RSA public key, and analyzing the RSA modulus to determine whether the underlying prime factors could be recovered.

This project belongs to the **Cryptography, Security Assessment, and Threat Analysis** domain, with emphasis on identifying implementation weaknesses rather than attacking RSA itself.

---

## Objectives

* Identify services exposed by the target environment
* Discover hidden web resources
* Analyze the discovered RSA public key
* Determine the RSA key length
* Examine the modulus `n`
* Understand Fermat's factorization algorithm
* Recover the prime factors `p` and `q`
* Understand how weak RSA key generation affects security
* Reconstruct the private key from the recovered parameters
* Connect cryptographic weaknesses to defensive security

---

## Methodology

The investigation was performed in the following phases:

1. **Service Enumeration**

   * Identify services running on the target
   * Determine potential attack surfaces

2. **Web Enumeration**

   * Investigate the web service
   * Identify hidden directories and exposed resources

3. **RSA Key Discovery**

   * Locate the exposed RSA public key
   * Examine the key structure and modulus

4. **Cryptographic Analysis**

   * Determine the RSA key length
   * Extract the modulus `n`
   * Analyze the relationship between the RSA prime factors

5. **Fermat Factorization**

   * Apply Fermat's factorization method
   * Recover the prime factors `p` and `q`

6. **Private-Key Reconstruction**

   * Use the recovered parameters and `e = 65537`
   * Calculate the private exponent `d`
   * Validate the reconstructed private key

7. **Security Analysis**

   * Determine why the RSA implementation was vulnerable
   * Identify defensive lessons from the weakness

---

## Investigation Walkthrough

### 1. Service Enumeration

**Observation**

The target environment exposed multiple network services.

**Interpretation**

Service enumeration provides the initial visibility required to understand the attack surface and identify services that may expose useful information.

**Risk**

Unnecessary or poorly secured services can provide attackers with additional opportunities for reconnaissance and information disclosure.

**SOC View**

Defenders can monitor unusual service discovery activity, repeated connection attempts, and reconnaissance patterns originating from unexpected sources.

**GRC View**

This relates to **asset inventory, attack-surface management, and secure service configuration**.

#### Evidence

<img width="637" height="198" alt="image" src="https://github.com/user-attachments/assets/43c3da92-60a7-499d-b0f8-7d0d57674aa9" />

---

### 2. Web Enumeration

**Observation**

Further investigation of the web service revealed a hidden directory containing information relevant to the investigation.

**Interpretation**

Hidden web resources can unintentionally expose sensitive configuration information, credentials, keys, documentation, or application functionality.

**Risk**

Information disclosure can significantly reduce the effort required for subsequent security analysis.

**SOC View**

Web-server access logs can be reviewed for directory enumeration, unusual request patterns, and repeated requests for non-public resources.

**GRC View**

This connects to **secure configuration, access control, vulnerability management, and information protection**.

#### Evidence
<img width="1911" height="762" alt="2 Screenshot 2026-08-25 205446" src="https://github.com/user-attachments/assets/01ab1b45-cdc9-45d3-a675-5eb546d49a0a" />

<img width="1917" height="667" alt="3_first Screenshot 2026-08-25 205521" src="https://github.com/user-attachments/assets/6ff2b4ea-bae9-4dd7-9d2a-5ca26aadfe01" />

---

### 3. RSA Key Analysis

An RSA public key was discovered containing the public exponent `e` and modulus `n`.

RSA security depends on the computational difficulty of factoring the modulus:

```text
n = p × q
```
The public key contains: `(e, n)`

where:

`e` = public exponent

`n` = RSA modulus

`p` and `q` = prime factors of `n`

**Observation**

The RSA implementation generated prime factors that were sufficiently close to make Fermat factorization practical.

**Interpretation**

RSA is designed around the difficulty of factoring a large modulus. However, insecure parameter generation can undermine this security assumption.

**Risk**

If `p` and `q` can be recovered, the private key can subsequently be reconstructed.

---

## Fermat Factorization

Fermat factorization represents an odd integer as the difference between two squares:

```text
n = a² − b²
```

which can be rewritten as:

```text
n = (a + b)(a − b)
```

When the two prime factors are sufficiently close, Fermat's method can efficiently search for suitable values of `a` and `b`.

### Script Used

The following Python script was used during the lab to:

* Read the RSA public key
* Determine the key size
* Extract the modulus
* Factorize the modulus using Fermat's method
* Recover `p` and `q`
* Calculate the private exponent
* Reconstruct the RSA private key

```python
#!/usr/bin/env python3

import math
from Crypto.PublicKey import RSA


def factorize(n):
    # Since even numbers are divisible by 2, return immediately
    if (n & 1) == 0:
        return (n // 2, 2)

    # Calculate the integer square root
    a = math.isqrt(n)

    # If n is a perfect square
    if a * a == n:
        return a, a

    while True:
        a = a + 1
        bsq = a * a - n
        b = math.isqrt(bsq)

        if b * b == bsq:
            break

    return a + b, a - b


def get_private_key(e, p, q):
    # Calculate Carmichael's function λ(n)
    lambda_n = (
        (p - 1) * (q - 1)
        // math.gcd(p - 1, q - 1)
    )

    # Calculate modular inverse of e
    return pow(e, -1, lambda_n)


# Read public key
with open("id_rsa.pub", "r") as file:
    public_key = RSA.import_key(file.read())


# Extract RSA parameters
bit_size = public_key.size_in_bits()
n = public_key.n

x = str(n)[-10:]

# Public exponent
e = 65537


# Factorize the modulus
p, q = factorize(n)


# Calculate private exponent
d = get_private_key(e, p, q)


# Reconstruct private key
private_key = RSA.construct((n, e, int(d)))


# Write private key
with open("id_rsa", "wb") as file:
    file.write(private_key.export_key("PEM"))


print(f"The size in bits of the public key is: {bit_size}")
print(f"The last 10 digits of the public key are: {x}")
print(f"The difference between p and q is: {p - q}")
print(f"The value of the private key is {d}")
print("You can find your key in this directory.")
```
> **Note:** The generated private key is intentionally not included in this repository.

### What the Code Demonstrates

The implementation:

The script performs the following operations:

1. Checks whether `n` is even.
2. Starts from the integer square root of `n`.
3. Calculates `a² - n`.
4. Checks whether the result is a perfect square.
5. Uses the resulting values to derive `p` and `q`.
6. Calculates the RSA private exponent.
7. Reconstructs the private key from the recovered parameters.

The important security observation is that Fermat factorization becomes particularly effective when the RSA prime factors are close together.

---

## Private-Key Reconstruction

Once `p` and `q` have been recovered, the RSA private exponent can be reconstructed using the public exponent.

For this lab, the public exponent was:

```text
e = 65537
```

The RSA private exponent can be calculated using the appropriate totient-related value:

```text
d = inverse(e, lcm(p - 1, q - 1))
```

### Python Example

```python
from math import lcm
from Crypto.Util.number import inverse

e = 65537

# Replace these with the recovered factors
p = ...
q = ...

lambda_n = lcm(p - 1, q - 1)

d = inverse(e, lambda_n)

print("Private exponent d:", d)
```

This demonstrates how recovering the prime factors of the RSA modulus can lead to reconstruction of the private key.

#### Evidence

<img width="1917" height="431" alt="3 Screenshot 2026-08-25 205344" src="https://github.com/user-attachments/assets/de10bd74-b9e7-43b2-8498-74798a151450" />
<img width="615" height="152" alt="5 Screenshot 2026-08-25 205856" src="https://github.com/user-attachments/assets/7f7fe8bd-55b8-4893-b73f-6bf78dbf36ea" />

---

## Key Findings

### Finding 1 — Weak RSA Prime Generation

**Observation**

The RSA prime factors were sufficiently close for Fermat factorization to become practical.

**Impact**

The RSA modulus could be factored, allowing reconstruction of the private key.

**Security Domain**

Cryptographic Security / Key Management

---

### Finding 2 — Sensitive Information Exposure

**Observation**

Cryptographic information was accessible through the target environment.

**Impact**

Exposed information reduced the effort required to analyze the cryptographic implementation.

**Security Domain**

Information Disclosure / Secure Configuration

---

### Finding 3 — Cryptography Depends on Implementation

**Observation**

RSA itself was not fundamentally broken. The vulnerability resulted from weak generation of the RSA parameters.

**Impact**

An implementation weakness undermined the security normally provided by RSA.

**Security Lesson**

Strong cryptographic algorithms still require secure implementation, parameter generation, and key management.

---

## Defensive Perspective

A secure implementation should:

* Use trusted cryptographic libraries
* Use cryptographically secure random number generation
* Generate RSA primes appropriately and independently
* Protect private keys from unauthorized access
* Avoid exposing sensitive cryptographic material through web applications
* Maintain appropriate key rotation and lifecycle procedures
* Monitor access to sensitive cryptographic resources
* Regularly assess cryptographic implementations for weaknesses

---

## Analyst Reflection

This lab changed my understanding of cryptography from viewing RSA primarily as a mathematical algorithm to understanding the importance of **secure implementation and key generation**.

The main lesson was that a strong algorithm does not automatically guarantee a secure system. Small implementation decisions, such as how RSA prime numbers are generated, can introduce weaknesses that fundamentally affect the security of the resulting key pair.

It also reinforced the importance of approaching security investigations from multiple perspectives: identifying the technical weakness, understanding its impact, considering how defenders could detect related activity, and identifying the controls that should prevent the weakness in the first place.

---

## Skills Demonstrated

* Network Service Enumeration
* Web Enumeration
* RSA Fundamentals
* Cryptographic Analysis
* Fermat Factorization
* Python
* Public/Private Key Analysis
* Security Weakness Identification
* Threat Analysis
* SOC Perspective
* GRC Perspective
* Security Documentation

---

## Final Takeaway

This lab demonstrated **`how poor cryptographic implementation can undermine an otherwise secure algorithm.`** The RSA system became vulnerable because the prime factors used to construct the modulus were too close together, making Fermat factorization practical.

From a defensive perspective, the key lesson is that secure cryptography requires not only strong algorithms but also secure key generation, proper key management, controlled exposure of cryptographic material, and continuous security assessment.
