# 🔐 Checkpoint 2: Web Application Protection, Encryption, and Steganography

**CompTIA Security+ Checkpoint 2 - Complete Lab Documentation**

**Student:** Nourhen Riahi  
**Date:** May 19, 2026  
**Status:** ✅ Complete  
**Difficulty:** Intermediate

---

## 📌 Objective

Apply fundamental cybersecurity concepts (CIA triad, access control, encryption, and steganography) in a practical environment using Kali Linux VM and Windows XAMPP web server.

---

## 📚 Table of Contents

1. [Security Controls Framework](#1-security-controls-framework)
2. [CIA Triad & AAA Framework](#2-cia-triad--aaa-framework)
3. [Encryption Types](#3-encryption-types-symmetric-vs-asymmetric)
4. [Practical Implementation](#4-practical-implementation-with-screenshots)
5. [Screenshots & Evidence](#5-screenshots--evidence)
6. [Learning Outcomes](#6-learning-outcomes)

---

# 1. SECURITY CONTROLS FRAMEWORK

## Objective
Outline security controls for protecting a web application hosted in an on-premises server room.

---

## Security Controls Matrix

### **A. MANAGERIAL CONTROLS**

#### Preventive Controls
```
Policy: "All web applications must use HTTPS encryption"
├─ Ensures confidentiality of data in transit
├─ Prevents man-in-the-middle attacks
└─ Compliance requirement for data protection
```

#### Detective Controls
```
Access Review: "Quarterly review of admin access permissions"
├─ Identifies unauthorized access patterns
├─ Detects privilege creep
└─ Maintains audit trail
```

#### Corrective Controls
```
Incident Response Plan: "5-step breach response procedure"
├─ Containment within 1 hour
├─ Investigation within 24 hours
└─ Notification within 72 hours
```

---

### **B. OPERATIONAL CONTROLS**

#### Preventive Controls
```
User Training: "Annual security awareness program"
├─ Educates staff on phishing and social engineering
├─ Reduces human error incidents by 40%
└─ Creates security culture
```

#### Detective Controls
```
Configuration Audits: "Monthly review of server configurations"
├─ Identifies misconfigurations
├─ Detects unauthorized changes
└─ Ensures compliance with security baselines
```

#### Corrective Controls
```
Patch Management: "Monthly security patch deployment"
├─ Fixes known vulnerabilities
├─ Tested before production deployment
└─ Minimizes exploitation window
```

---

### **C. TECHNICAL CONTROLS**

#### Preventive Controls
```
Firewall Rules: "Allow only ports 80 (HTTP) and 443 (HTTPS)"
├─ Blocks unauthorized network access
├─ Prevents reconnaissance attacks
└─ Reduces attack surface

Web Application Firewall (WAF): "Block SQL injection attempts"
├─ Inspects HTTP traffic for malicious patterns
├─ Prevents application-layer attacks
└─ Protects database from unauthorized queries

Encryption: "AES-256 for data at rest, TLS 1.3 for data in transit"
├─ Protects confidentiality
├─ Prevents eavesdropping
└─ Meets regulatory requirements

Access Control: "Role-based access control (RBAC)"
├─ Admin: Full system access
├─ Editor: Create/modify content
├─ Viewer: Read-only access
└─ Guest: Limited access
```

#### Detective Controls
```
Logging & Monitoring: "All access attempts logged with timestamp, user, action"
├─ Enables forensic analysis
├─ Detects suspicious patterns
└─ Provides accountability

Intrusion Detection System (IDS): "Alert on port scans, failed login attempts"
├─ Real-time threat detection
├─ Automatic blocking of brute-force attacks
└─ Incident response triggering
```

#### Corrective Controls
```
Vulnerability Patching: "Deploy patches within 48 hours of release"
├─ Fixes known exploits
├─ Reduces vulnerability window
└─ Prevents widespread compromise

Malware Removal: "Antivirus engine scans on upload and execution"
├─ Detects malicious files
├─ Quarantines infected content
└─ Prevents malware spread
```

---

### **D. PHYSICAL CONTROLS**

#### Preventive Controls
```
Server Room Lock: "Secure door with badge access"
├─ Restricts unauthorized entry
├─ Prevents theft of physical servers
└─ Protects power/cooling infrastructure

Environmental Controls: "Temperature/humidity monitoring"
├─ Prevents hardware failure
├─ Maintains optimal operating conditions
└─ Avoids downtime

Badging System: "RFID badge + PIN for multi-factor access"
├─ Dual authentication required
├─ Non-transferable credential
└─ Audit trail of entries
```

#### Detective Controls
```
CCTV Cameras: "24/7 video surveillance in server room"
├─ Monitors physical access
├─ Records suspicious activity
├─ Enables forensic investigation

Access Logs: "Automatic log of badge scans, timestamps, duration"
├─ Identifies unauthorized access attempts
├─ Detects after-hours access
└─ Creates accountability
```

#### Corrective Controls
```
Disaster Recovery: "Off-site backup facility with failover capability"
├─ Restores operations after physical damage
├─ Maintains business continuity
└─ Recovers data from backups
```

---

## Summary Table

| Category | Preventive | Detective | Corrective |
|----------|-----------|-----------|-----------|
| **Managerial** | Security policy | Access review | Incident response |
| **Operational** | Staff training | Configuration audit | Patch management |
| **Technical** | Firewall rules | Logging/IDS | Vulnerability patching |
| **Physical** | Server room lock | CCTV cameras | Disaster recovery |

---

# 2. CIA TRIAD & AAA FRAMEWORK

## CIA Triad - Confidentiality, Integrity, Availability

### **1. Confidentiality**

**Definition:** Only authorized users can access/read data. Unauthorized people cannot view sensitive information.

**Real-World Example: Bank Account Password**
```
Scenario: Your bank account balance is sensitive data

Confidentiality Broken:
❌ Employee reads customer account details
❌ Hacker intercepts password in plain text
❌ Data breach exposes customer SSN

Confidentiality Protected:
✅ Password encrypted with SHA-256
✅ Account details visible only to account owner
✅ HTTPS encrypts data in transit
✅ Only authenticated users see balances

Protection Methods:
├─ Encryption (AES-256, TLS)
├─ Access controls (RBAC, password protection)
├─ Data classification (public, confidential, secret)
└─ NDAs (non-disclosure agreements)
```

---

### **2. Integrity**

**Definition:** Data cannot be modified by unauthorized users. Data remains accurate and unchanged.

**Real-World Example: Bank Transfer Amount**
```
Scenario: You transfer $100 to a friend

Integrity Broken:
❌ Attacker intercepts transfer, changes $100 → $1,000
❌ Database hacked, balance modified
❌ Receipt forged to show wrong amount
❌ Transfer amount changed mid-transmission

Integrity Protected:
✅ Transfer amount verified with digital signature
✅ Database uses checksums to detect changes
✅ Receipt cryptographically signed
✅ TLS ensures no man-in-the-middle modification

Protection Methods:
├─ Hashing (SHA-256, MD5)
├─ Digital signatures (RSA signature)
├─ Message authentication codes (HMAC)
├─ Access controls (only authorized users modify)
└─ Audit logs (record all changes)
```

---

### **3. Availability**

**Definition:** Data/services accessible when needed. Not blocked, not slow, not offline.

**Real-World Example: ATM Machine**
```
Scenario: You need to withdraw cash at 3 AM

Availability Broken:
❌ ATM offline for maintenance
❌ Network connection down
❌ Server overloaded, taking 10 minutes per request
❌ DDoS attack floods server with requests

Availability Protected:
✅ ATM operational 24/7/365
✅ Redundant network connections (primary + backup)
✅ Load balanced servers (distribute traffic)
✅ DDoS protection (rate limiting, filtering)

Protection Methods:
├─ Redundancy (backup systems)
├─ Backups (data recovery)
├─ Load balancing (distribute load)
├─ Disaster recovery (rapid restoration)
└─ DDoS protection (block attack traffic)
```

---

## AAA Framework - Authentication, Authorization, Accounting

### **1. Authentication**

**Definition:** Prove you are who you claim to be. Verify identity.

**Methods:**
```
Something You KNOW:
├─ Password: "MyPassword123"
├─ PIN: "1234"
└─ Security question: "What is your pet's name?"

Something You HAVE:
├─ Security token: Hardware or software token
├─ Badge: Physical identification card
├─ Phone: SMS code or authenticator app
└─ Certificate: Digital certificate

Something You ARE:
├─ Fingerprint: Biometric scan
├─ Face: Facial recognition
├─ Iris: Eye scan
└─ Voice: Voice recognition

Multi-Factor Authentication (MFA):
✅ Password (something you know)
  + SMS code (something you have)
  = Secure authentication
```

---

### **2. Authorization**

**Definition:** Determine what authenticated user can do. What resources can they access?

---

### **3. Accounting**

**Definition:** Track and record who did what and when. Create audit trail.

---

# 3. ENCRYPTION TYPES: SYMMETRIC VS ASYMMETRIC

## Symmetric Encryption (AES)

### How It Works
```
Alice and Bob share same key: "MySecurePassword"

Alice encrypts message:
    Plain text: "Attack at dawn"
    + Key: "MySecurePassword"
    = Encrypted: "LKJH&*@#$%^&*()"

Alice sends: "LKJH&*@#$%^&*()"

Bob decrypts message:
    Encrypted: "LKJH&*@#$%^&*()"
    + Key: "MySecurePassword"
    = Plain text: "Attack at dawn"
```

---

## Asymmetric Encryption (RSA)

### How It Works
```
Bob has TWO keys:
    Public Key: Available to EVERYONE
    Private Key: Only Bob has

Alice encrypts with Bob's PUBLIC key:
    Plain text: "Secret message"
    + Bob's PUBLIC key
    = Encrypted: "LKJH&*@#$%^&*()"

Only Bob can decrypt with his PRIVATE key:
    Encrypted: "LKJH&*@#$%^&*()"
    + Bob's PRIVATE key
    = Plain text: "Secret message"
```

---

## Symmetric vs Asymmetric Comparison

| Feature | Symmetric (AES) | Asymmetric (RSA) |
|---------|-----------------|------------------|
| **Keys** | One shared key | Public + Private |
| **Key Size** | 128-256 bits | 2048-4096 bits |
| **Speed** | ⚡ Fast | 🐢 Slow |
| **Key Distribution** | ❌ Problem | ✅ No problem |
| **Use Cases** | File encryption, disk | Email, HTTPS, signatures |
| **Example** | AES | RSA, ECC |

---

# 4. PRACTICAL IMPLEMENTATION WITH SCREENSHOTS

## Step 1: CyberChef AES Encryption

### Screenshot 1: CyberChef Interface
![CyberChef AES Encryption](./screenshots/AES_Encrypt.png)

**What's Shown:**
- Operations panel (left) with AES options
- Recipe area (middle) with AES Encrypt selected
- Input area (top right) with message: "cybersecurity test"
- Key field: "MySecurePassword" (UTF8)
- IV field: "000000000000..." (HEX)
- Mode: CBC
- Output area (bottom right) showing encrypted result

**Configuration:**
```
Input Message: cybersecurity test
Key: MySecurePassword
Key Format: UTF8
IV: HEX format
Mode: CBC
Input Format: Hex
Output Format: Hex

Result: d27fc4544aebf5363126203991f97f16
```

---

## Step 2: Directory Structure on Kali Linux

### Screenshot 2: Files on Desktop
![Desktop Directory with Files](./screenshots/Directoryimage.png)

**What's Shown:**
- Directory listing showing:
  - `my.txt` - Contains the secret message "nourhen"
  - `photo.jpeg` - Image file for steganography

**Purpose:**
These files will be used for steganographic embedding

---

## Step 3: Steghide Installation on Kali

### Screenshot 3: Steghide Installation Process
![Steghide Installation](./screenshots/instalteghide.png)

**Command Executed:**
```bash
steghide --version
# Output: Command not found

sudo apt install steghide -y
# Installing steghide with dependencies...
# libmcrypt4, libmhash2
```

**Status:** ✅ Installation successful

---

## Step 4: Steghide Embedding Secret Message

### Screenshot 4: Embedding "nourhen" in Image
![Steghide Embed Command](./screenshots/hidewordinphoto.png)

**Command:**
```bash
steghide embed -ef my.txt -cf photo.jpeg
# Enter passphrase: (optional)
# Re-enter passphrase:
# embedding "my.txt" in "photo.jpeg" ... done
```

**Result:**
- Secret message "nourhen" is now hidden in photo.jpeg
- Image appears unchanged to the naked eye
- Only extractable with steghide + correct passphrase

---

## Step 5: Steghide Extraction & Verification

### Screenshot 5: Extracting Hidden Message
![Steghide Extract](./screenshots/extractpass.png)

**Command:**
```bash
steghide extract -sf photo.jpeg -xf extracted.txt
# Enter passphrase:
# wrote extracted data to "extracted.txt".

cat extracted.txt
# Output: nourhen ✅
```

**Verification:**
- Hidden message successfully extracted
- Proves steganography working correctly
- Only possible with correct passphrase

---

## Step 6: XAMPP Web Server Setup

### Screenshot 6: XAMPP Control Panel - Apache Running
![XAMPP Running](./screenshots/Runningxampp.png)

**Server Status:**
```
MySQL Database:    Stopped (red)
ProFTPD:           Running (green)
Apache Web Server: Running (green) ✅
```

**Configuration:**
- Apache Web Server: RUNNING
- Port: 80 (HTTP)
- Document Root: /Applications/XAMPP/xamppfiles/htdocs/

---

## Step 7: XAMPP htdocs Directory Contents

### Screenshot 7: Files in Web Server Directory
![htdocs Folder Contents](./screenshots/openhtdocs.png)

**Files Present:**
```
applications.html    - XAMPP landing page
bitnami.css         - Styling
dashboard/          - Admin dashboard folder
favicon.ico         - Website icon
img/                - Image folder
index.html          - ✅ Our main page
index.php           - PHP script
webalizer/          - Web stats folder
photo.jpeg          - ✅ Steganographic image
```

---

## Step 8: HTML File with Encrypted Message

### Screenshot 8: Editing index.html in VS Code
![HTML Editing](./screenshots/edithtmlputencryptedtext.png)

**Code Visible:**
```html
<h1>Nourhen Riahi</h1>
<h2>AES Encrypted Message</h2>
<p>d27fc4544aebf5363126203991f97f16</p>
```

**What's Being Done:**
- Adding student name
- Embedding encrypted message
- Ready for web display

---

## Step 9: Initial Localhost Test

### Screenshot 9: Localhost Page - Before Image
![Initial Localhost](./screenshots/testlocalhostafteredit.png)

**Display:**
```
Nourhen Riahi

AES Encrypted Message
d27fc4544aebf5363126203991f97f16
```

**Status:**
- ✅ Apache serving page
- ✅ Encrypted message displays
- ✅ HTML rendering correctly

---

## Step 10: Final Localhost with Steganographic Image

### Screenshot 11 (FINAL): Complete Page with Hidden Image
![Final Localhost with Image](./screenshots/openlocalhostafterajoutimage.png)

**Complete Display:**
```
Nourhen Riahi

AES Encrypted Message
d27fc4544aebf5363126203991f97f16

Steganography Image
[Photo with security lock icon displayed]
```

**What's Demonstrated:**
- ✅ Student name displayed
- ✅ AES encrypted message visible (unreadable without key)
- ✅ Steganographic image served from XAMPP
- ✅ Image contains hidden "nourhen" message
- ✅ All practical security concepts working together

---

# 5. SCREENSHOTS & EVIDENCE

## Complete Lab Screenshot Inventory

| # | File Name | Description | Demonstrates |
|---|-----------|-------------|---------------|
| 1 | AES_Encrypt.png | CyberChef encryption interface | Symmetric encryption (AES) |
| 2 | Directoryimage.png | my.txt and photo.jpeg files | Steganography preparation |
| 3 | instalteghide.png | Steghide installation process | Tool setup |
| 4 | hidewordinphoto.png | Steghide embed command | Steganography embedding |
| 5 | extractpass.png | Steghide extract verification | Steganography extraction |
| 6 | Runningxampp.png | XAMPP Apache running | Web server operational |
| 7 | openhtdocs.png | htdocs folder with files | Web files organized |
| 8 | edithtmlputencryptedtext.png | HTML editing in VS Code | Integration of encryption |
| 9 | testlocalhostafteredit.png | Localhost with encrypted msg | Web display verification |
| 10 | testlocalhost.png | Initial page test | Server connectivity |
| 11 | openlocalhostafterajoutimage.png | Final page with image | Complete integration |
| 12 | securitfile.png | Documentation | Lab completion |

---

## Visual Evidence Summary

### Security Implementation Layers

```
Layer 1: Data Encryption (Screenshot 1)
└─ CyberChef AES-256
   Input: "cybersecurity test"
   Output: "d27fc4544aebf5363126203991f97f16"

Layer 2: Data Hiding (Screenshots 3-5)
└─ Steghide Steganography
   Hidden message: "nourhen"
   Container: photo.jpeg
   Status: ✅ Verified via extraction

Layer 3: Web Delivery (Screenshots 6-11)
└─ XAMPP Apache Server
   Port: 80 (HTTP)
   Files: index.html, photo.jpeg
   Display: Both encrypted message and image

Result: Complete security chain demonstrated
```

---

# 6. LEARNING OUTCOMES

## Technical Skills Demonstrated

✅ **Security Control Implementation**
- Understand 4 control categories (Managerial, Operational, Technical, Physical)
- Classify controls as Preventive, Detective, or Corrective
- Apply defense-in-depth principle

✅ **CIA Triad Application**
- Confidentiality: Encryption protects data visibility
- Integrity: Checksums/hashes detect unauthorized changes
- Availability: Redundancy and backups ensure accessibility

✅ **AAA Framework Implementation**
- Authentication: Verify user identity with credentials
- Authorization: Control what users can access based on roles
- Accounting: Log all actions for audit trail

✅ **Encryption Techniques**
- Symmetric encryption (AES): Fast, shared key
- Asymmetric encryption (RSA): Secure key exchange, digital signatures
- Hybrid approach: Combine both for optimal security

✅ **Steganography**
- Hide data in images using Steghide
- Understand difference from encryption
- Practical application in security

✅ **Web Server Configuration**
- XAMPP setup and management
- HTML integration of security concepts
- Local testing and verification

---

## Key Concepts Mastered

```
┌─ Security Controls
│  ├─ Preventive (stop before happening)
│  ├─ Detective (discover after happening)
│  └─ Corrective (fix after discovery)
│
├─ CIA Triad
│  ├─ Confidentiality (encryption)
│  ├─ Integrity (hashing, signatures)
│  └─ Availability (redundancy, backups)
│
├─ AAA Framework
│  ├─ Authentication (prove who you are)
│  ├─ Authorization (what you can do)
│  └─ Accounting (what you did, when, where)
│
├─ Encryption
│  ├─ Symmetric (AES - fast, shared key)
│  ├─ Asymmetric (RSA - secure, key exchange)
│  └─ Hybrid (combine for best security)
│
└─ Steganography
   ├─ Hide message in innocent-looking files
   ├─ Difference from encryption
   └─ Defender and attacker use cases
```

---

## Exam Preparation

### Topics Covered (CompTIA Security+)
```
✅ Domain 1.0: General Security Concepts
   ├─ CIA Triad
   ├─ AAA Framework
   ├─ Security Controls
   └─ Defense in Depth

✅ Domain 2.0: Threats, Vulnerabilities, Mitigations
   ├─ Encryption as mitigation
   ├─ Access controls
   └─ Data protection strategies

✅ Domain 3.0: Security Architecture
   ├─ Firewalls and network controls
   ├─ Access control lists (ACLs)
   └─ Cryptographic systems
```

---

## Real-World Applications

```
Banking:
├─ AES encryption for customer data
├─ TLS for online banking
├─ RBAC for employee access
└─ Audit logs for compliance

Healthcare (HIPAA):
├─ Symmetric encryption for patient records
├─ Access controls (doctors, nurses, administrators)
├─ Audit trails for all access
└─ Regular backups (availability)

E-commerce:
├─ HTTPS with RSA + AES hybrid
├─ PCI compliance (encryption, access control)
├─ Steganography for digital watermarks
└─ WAF to block attacks

Government:
├─ Military-grade AES-256 encryption
├─ Strict RBAC with clearance levels
├─ Complete audit trails
└─ Air-gapped networks for top secret
```

---

## Completion Checklist

- [x] Security controls framework documented
- [x] CIA triad explained with real-world examples
- [x] AAA framework implemented
- [x] Symmetric encryption (AES) demonstrated with screenshot
- [x] Asymmetric encryption concepts explained
- [x] Steganography with Steghide completed with screenshots
- [x] Steghide installation documented
- [x] Steghide extraction verified
- [x] XAMPP web server running
- [x] HTML file created with encrypted message
- [x] Steganographic image served via HTTP
- [x] Final page screenshot showing complete integration
- [x] All 12 evidence screenshots included
- [x] README documentation complete

---

## Summary

**Checkpoint 2: Complete ✅**

**Date Completed:** May 19, 2026  
**Total Time:** 4-5 hours  
**Total Screenshots:** 12  
**Lab Status:** Production-ready  

**Key Deliverables:**
- ✅ CyberChef AES encryption (d27fc4544aebf5363126203991f97f16)
- ✅ Steghide steganography (hidden "nourhen" in photo.jpeg)
- ✅ XAMPP web integration (localhost page with both elements)
- ✅ Complete documentation with visual evidence (this README)
- ✅ All screenshots embedded for reference

**Next Checkpoint:** Threats, Vulnerabilities, and Mitigations (Domain 3.0)

---

<div align="center">

### 🎓 Building Security Skills One Checkpoint at a Time 🔐

**CompTIA Security+ Progress: 2/6 Checkpoints Complete**

**Certification Goal:** Achieve CompTIA Security+ (SY0-701)  
**Career Path:** DevSecOps Engineer → Germany 🇩🇪

</div>

---

**Created By:** Nourhen Riahi  
**Last Updated:** May 19, 2026  
**Status:** ✅ Complete with Visual Evidence and Ready for GitHub

