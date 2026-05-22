# 🔐 Checkpoint 2: Web Application Protection, Encryption & Steganography

> **CompTIA Security+ Lab** | Kali Linux VM + Windows Host (XAMPP)  
> **Student:** Nour Henriahi

---

## 📋 Table of Contents

1. [Securing a Web Application – Access Control Strategies](#1-securing-a-web-application--access-control-strategies)
2. [Security Principles – CIA Triad and AAA](#2-security-principles--cia-triad-and-aaa)
3. [Data Confidentiality – Encryption Types](#3-data-confidentiality--encryption-types)
4. [Practical Encryption with CyberChef (AES)](#4-practical-encryption-with-cyberchef-aes)
5. [What is Steganography?](#5-what-is-steganography)
6. [Hide a Secret Using Steghide (Kali Linux)](#6-hide-a-secret-using-steghide-kali-linux)
7. [Serve the Steganographic Image via XAMPP](#7-serve-the-steganographic-image-via-xampp)

---

## 1. Securing a Web Application – Access Control Strategies

Imagine your web application is hosted in an on-premises server room. The following security controls are applied across four categories:

| Control Type | Example | Classification | Purpose |
|---|---|---|---|
| **Managerial** | Security Policy Documentation | Preventive | Defines acceptable use, password standards, and incident response |
| **Managerial** | Risk Assessment Reviews | Detective | Periodic evaluation of threats and vulnerabilities |
| **Operational** | Security Awareness Training | Preventive | Staff training on phishing and social engineering |
| **Operational** | Log Review & Monitoring | Detective | Daily review of system logs for anomalies |
| **Operational** | Incident Response Plan | Corrective | Procedures for containing and recovering from incidents |
| **Technical** | Firewall & WAF Rules | Preventive | Blocks unauthorized traffic and common web attacks |
| **Technical** | Intrusion Detection System (IDS) | Detective | Monitors traffic and alerts on suspicious patterns |
| **Technical** | Patch Management | Corrective | Automated patching to restore a secure state |
| **Physical** | Keycard / Biometric Door Access | Preventive | Only authorized personnel can enter the server room |
| **Physical** | CCTV Surveillance Cameras | Detective | Continuous recording to identify unauthorized access |

---

## 2. Security Principles – CIA Triad and AAA

### 🔺 CIA Triad

| Principle | Definition | Real-World Example |
|---|---|---|
| **Confidentiality** | Ensures data is accessible only to authorized parties | A hospital uses AES-256 encryption on patient records with role-based access control |
| **Integrity** | Guarantees data accuracy and prevents unauthorized modification | A bank uses SHA-256 hashing to verify wire transfer amounts have not been altered in transit |
| **Availability** | Ensures systems are accessible to authorized users when needed | An e-commerce platform uses load balancers and redundant servers to stay online during high traffic |

### 🔑 AAA Framework

- **Authentication** – Verifies the identity of a user or device.  
  _Example: Logging in with username + password + OTP (MFA) to access a VPN._

- **Authorization** – Determines what an authenticated user is allowed to do.  
  _Example: An employee can read files in their department folder but cannot access HR or Finance directories._

- **Accounting** – Tracks and logs all actions performed by authenticated users.  
  _Example: A SIEM records every login, file access, and configuration change with timestamps._

---

## 3. Data Confidentiality – Encryption Types

| Aspect | Symmetric Encryption | Asymmetric Encryption |
|---|---|---|
| **Key Model** | Single shared secret key for both encryption and decryption | Key pair: Public key encrypts, Private key decrypts |
| **Key Management** | Key must be securely shared in advance (key exchange problem) | Public key is freely distributed; private key is never shared |
| **Algorithm Example** | **AES** (Advanced Encryption Standard) | **RSA** (Rivest-Shamir-Adleman) |
| **Speed** | Much faster — suited for large data volumes | Slower — computationally expensive |
| **Typical Use** | File encryption, VPN tunnels, full-disk encryption (BitLocker) | SSL/TLS handshake, email (PGP), digital signatures |
| **Security Risk** | Compromise of the single key exposes all data | Only the private key must remain secret |

> 💡 In practice, modern protocols combine both: asymmetric encryption exchanges a symmetric session key, which then encrypts actual data. This is the foundation of **TLS/HTTPS**.

---

## 4. Practical Encryption with CyberChef (AES)

### Parameters Used

| Parameter | Value |
|---|---|
| Tool | CyberChef |
| Operation | AES Encrypt |
| Mode | CBC (Cipher Block Chaining) |
| Key | `gomycodegomycode` (16-byte UTF-8) |
| Input Message | `Security is not a product, but a process.` |
| Output Format | Base64 encoded ciphertext |

### Steps Performed

1. Opened CyberChef and selected the **AES Encrypt** operation
2. Entered the key `gomycodegomycode` and selected CBC mode
3. Input the message and copied the Base64-encoded encrypted output
4. Opened `index.html` in the XAMPP `htdocs` directory using VS Code
5. Inserted my full name followed by the AES encrypted ciphertext
6. Started Apache via the XAMPP Control Panel
7. Navigated to `http://localhost` to verify the output

### Screenshots

| Step | Screenshot |
|---|---|
| AES encryption in CyberChef | ![AES Encrypt](screenshots/AESEncrypt.png) |
| Editing index.html in VS Code | ![VS Code Edit](screenshots/vscodeedit.png) |
| Inserting encrypted text into HTML | ![Edit HTML](screenshots/edithtmlputencryptedtext.png) |
| Saving the HTML file | ![Save HTML](screenshots/edithtmlsaveimage.png) |
| Testing on localhost | ![Test Localhost](screenshots/testlocalhost.png) |
| Final page after edit | ![Final Page](screenshots/testlocalhostafteredit.png) |

---

## 5. What is Steganography?

**Steganography** is the practice of concealing secret information within an ordinary, non-secret file so that the very existence of the hidden data is not apparent to observers.

> From Greek: *steganos* (covered) + *graphein* (writing)

### Steganography vs. Encryption

| Aspect | Steganography | Encryption |
|---|---|---|
| **Goal** | Hide the *existence* of the message | Protect the *content* of the message |
| **Visibility** | The carrier file appears completely normal | Ciphertext is visible but unreadable |
| **Detection** | Hard to detect without forensic tools (steganalysis) | Anyone can see that data is encrypted |
| **Key Required?** | Optional passphrase to extract data | Always requires a key |
| **Common Media** | Images (JPEG, PNG), Audio (MP3), Video | Any data type |

### Use Cases

- **🔴 Attacker:** A malware author embeds a C2 server IP address inside an innocent JPEG posted on a public website. The malware downloads the image and extracts the hidden IP to receive commands — bypassing network-based detection.

- **🟢 Defender:** A forensics investigator uses steganalysis tools to find hidden data in seized images. Organizations embed invisible watermarks in proprietary documents to track leaks and identify the source.

---

## 6. Hide a Secret Using Steghide (Kali Linux)

### Installation

```bash
steghide --version        # Check if installed
sudo apt install steghide # Install if not present
```

### Embedding Process

```bash
# Create the secret file
echo "gomycode" > my.txt

# Embed it inside the image
steghide embed -ef my.txt -cf index.jpeg
# Enter passphrase when prompted
```

### Extraction (Reverse Operation)

```bash
steghide extract -sf index.jpeg
# Enter the same passphrase to retrieve hidden data
```

### Screenshots

| Step | Screenshot |
|---|---|
| Installing Steghide | ![Install Steghide](screenshots/instalteghide.png) |
| Hiding word in image | ![Hide Word](screenshots/hidewordinphoto.png) |
| Extracting hidden data | ![Extract](screenshots/extractpass.png) |

---

## 7. Serve the Steganographic Image via XAMPP

### Steps

| Step | Action | Screenshot |
|---|---|---|
| 1 | Start XAMPP and activate Apache | ![XAMPP Running](screenshots/Runningxampp.png) |
| 2 | Open the htdocs directory | ![Open htdocs](screenshots/openhtdocs.png) |
| 3 | Copy the steganographic image to htdocs | ![Save Image](screenshots/saveimageashtdocs.png) |
| 4 | Get Kali Linux IP for file transfer | ![Kali IP](screenshots/openkaliip.png) |
| 5 | Edit index.html: name + encrypted text + image tag | ![Edit HTML](screenshots/edithtmlputencryptedtext.png) |
| 6 | Verify server is running | ![Open Server](screenshots/openserver.png) |
| 7 | Open localhost in browser | ![Open Localhost](screenshots/openlocalhostafterajoutimage.png) |
| 8 | Confirm final rendered page | ![Final Page](screenshots/testlocalhostafteredit.png) |
| 9 | Secure file confirmation | ![Secure File](screenshots/securitefile.png) |

### Final index.html Structure

```html
<!DOCTYPE html>
<html>
<head>
  <title>Checkpoint 2 - Nour Henriahi</title>
</head>
<body>
  <h1>Nour Henriahi</h1>

  <!-- AES Encrypted Message (CyberChef - CBC mode, key: gomycodegomycode) -->
  <p><strong>Encrypted Message:</strong> U2FsdGVkX1+... (Base64 AES ciphertext)</p>

  <!-- Steganographic Image (contains hidden word: gomycode) -->
  <img src="index.jpeg" alt="Steganographic Image" />
</body>
</html>
```

### What the Page Demonstrates

The final page at `http://localhost` contains three elements that combine all lab concepts:

- ✅ **Name** — Student identification
- ✅ **AES Encrypted Text** — Data confidentiality via symmetric encryption
- ✅ **Steganographic Image** — Hidden data inside a normal-looking image

> An observer visiting the page sees a name, some text, and an image — with no indication that the image hides a secret or that the visible text is encrypted ciphertext.

---

## 🧠 Conclusion

This checkpoint demonstrated practical application of core cybersecurity principles:

| Concept | Tool / Method |
|---|---|
| Access Control | Managerial, Operational, Technical, Physical controls |
| CIA Triad | Confidentiality, Integrity, Availability with real examples |
| Encryption Theory | Symmetric (AES) vs Asymmetric (RSA) |
| Practical Encryption | CyberChef — AES CBC encryption |
| Steganography | Steghide on Kali Linux — embed & extract |
| Web Deployment | XAMPP / Apache — serving the final page |

---

*CompTIA Security+ Labs — Checkpoint 2 | Nour Henriahi*
