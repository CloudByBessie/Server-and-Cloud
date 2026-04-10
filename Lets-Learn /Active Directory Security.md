<h1 align="center">✈️ Active Directory Security — Defending the Airport</h1>

<p align="center">
<em>Understanding real-world identity attacks and how they impact enterprise environments</em>
</p>

---

## 🧭 Visual Attack Flow — How Attackers Move Through the Airport

<p align="center">
<img width="800" height="1024" alt="Active Directory Security" src="https://github.com/user-attachments/assets/7a198a59-8a19-4b3e-8505-0229aa808653" />
</p>

💡 This diagram visualizes how attackers move from initial access to full domain compromise by chaining together privilege escalation, credential theft, and lateral movement techniques.

---
## 📘 Executive Summary

This lesson explores **Active Directory (AD) security fundamentals** through both a technical and conceptual lens.

Using an airport analogy, it explains how attackers:
- Escalate privileges
- Abuse authentication systems
- Exploit misconfigurations

And how these actions translate into **real-world enterprise security risks**.

💡 **Outcome:**  
Built a strong understanding of how identity systems are attacked—and why securing them is critical.

---

## 🌍 Why This Matters in the Real World

Active Directory is still the identity backbone for most enterprises.  
When AD is compromised:

- Attackers gain access to sensitive systems  
- Ransomware spreads quickly  
- Business operations can be disrupted  
- Cloud identities can be indirectly affected  

Understanding these attacks helps engineers recognize identity as the core of security.

---

## 🏢 Airport Model (Conceptual Mapping)

| Airport System        | Active Directory Component              |
|----------------------|------------------------------------------|
| Employees            | User Accounts                            |
| Security Badges      | Credentials (Passwords / Hashes / Tickets) |
| Restricted Areas     | Systems, Servers, Resources              |
| Security Checkpoints | Authentication Protocols                 |

---

# 🔐 Privilege Escalation  
## “From Passenger to Control Tower Access”

### 📖 Definition

Privilege escalation is the process where a user gains higher permissions than originally assigned, eventually obtaining administrative-level control over systems or the domain.

---

### ✈️ Airport Analogy

A normal passenger:
- Sneaks into a staff hallway  
- Gains access to baggage operations  
- Eventually reaches the **control tower**

Each step increases their level of access.

---

### 💻 In Active Directory

Occurs when:
- Permissions are misconfigured  
- Credentials are exposed  
- Systems trust users too broadly  

---

### 🌍 Real-World Example

An employee:
- Has access to a shared folder  
- Finds a script containing admin credentials  
- Uses those credentials to access a server  
- Extracts more credentials  
- Gains full domain control  

---

### 🎯 Impact

- Full network compromise  
- Unauthorized data access  
- Long-term persistence  

---

# 🎟️ Kerberos Authentication  
## “Boarding Pass System”

### 📖 Definition

Kerberos is an authentication protocol that uses encrypted tickets to verify identity and securely grant access to systems without repeatedly transmitting passwords.

---

### ✈️ Airport Analogy

At an airport:
- You verify your identity once  
- Receive a boarding pass 🎟️  
- Use it to move through different checkpoints  

You don’t show your ID repeatedly—just the pass.

---

### 💻 In Active Directory

- **TGT (Ticket Granting Ticket):** proves identity  
- **Service Ticket:** grants access to specific systems  

---

### 🌍 Real-World Example

An employee logs into their computer:
- Gets authenticated once  
- Accesses email, file shares, and apps without re-entering a password  

---

### ⚠️ Security Risk

If attackers steal tickets:
- They can impersonate users  
- Access systems without passwords  
- Move undetected across the network  

---

# 🧨 Attack Techniques

---

## 🕵️ Pass-the-Hash (PtH)

### 📖 Definition

Pass-the-Hash is an attack where an attacker uses a stolen password hash to authenticate to systems without needing to know the actual plaintext password.

---

### ✈️ Airport Analogy

An attacker:
- Steals a security badge  
- Doesn’t know the PIN  
- But the system accepts the badge anyway  

---

### 💻 How It Works

- Windows stores passwords as hashes  
- Attackers extract hashes from memory  
- Use them to log into other systems  

---

### 🌍 Real-World Example

An attacker:
- Compromises one machine  
- Dumps credentials from memory  
- Uses a manager’s hash to access a file server  
- Moves across the network  

---

### 🎯 Impact

- Rapid lateral movement  
- No password cracking needed  
- Hard to detect without proper monitoring

---

### 🛡️ Defender Countermeasures

- Enforce strong credential isolation between systems  
- Limit local administrator accounts across endpoints  
- Rotate local admin passwords regularly  
- Reduce NTLM usage where possible  
- Monitor for unusual authentication patterns  
- Segment high-value systems to reduce lateral movement

---

## 🔥 Kerberoasting

### 📖 Definition

Kerberoasting is an attack where attackers request service tickets associated with service accounts and crack them offline to recover plaintext passwords.

---

### ✈️ Airport Analogy

Certain airport systems:
- Require special service passes  

An attacker:
- Requests these passes  
- Takes them offline  
- Tries to break them open  

---

### 💻 How It Works

- Attacker requests a service ticket (SPN)  
- Extracts encrypted data  
- Uses brute-force tools to crack it  

---

### 🌍 Real-World Example

An attacker:
- Targets a SQL service account  
- Extracts its ticket  
- Cracks the password offline  
- Uses it to access sensitive databases  

---

### 🎯 Impact

- Compromise of service accounts  
- Access to critical systems  
- Potential privilege escalation  

---

### 🛡️ Defender Countermeasures

- Use long, complex passwords for service accounts  
- Convert service accounts to Managed Service Accounts (MSA/gMSA)  
- Limit where service accounts can log in  
- Audit and remove unnecessary SPNs  
- Monitor for abnormal ticket‑request activity  

---

## 🧊 AS-REP Roasting

### 📖 Definition

AS-REP Roasting is an attack that targets accounts without Kerberos preauthentication, allowing attackers to retrieve encrypted authentication data and crack it offline.

---

### ✈️ Airport Analogy

Some employees:
- Don’t need to show ID  
- Can request a boarding pass immediately  

An attacker:
- Simply asks—and gets access  

---

### 💻 How It Works

- Targets accounts with preauthentication disabled  
- Requests authentication response  
- Cracks it offline  

---

### 🌍 Real-World Example

A legacy account:
- Has preauthentication disabled for compatibility  
- Attacker queries the domain  
- Extracts encrypted data  
- Cracks the password offline  

---

### 🎯 Impact

- Easy credential exposure  
- No initial authentication required  
- Often overlooked vulnerability  

---

### 🛡️ Defender Countermeasures

- Ensure preauthentication is enabled for all accounts  
- Identify legacy accounts with outdated authentication settings  
- Enforce strong password policies for vulnerable accounts  
- Monitor for repeated AS‑REQ requests  
- Remove or modernize accounts that require compatibility exceptions  

---

# ⚠️ Misconfiguration Risks

---

## 🔓 Weak Permissions

### 📖 Definition

Weak permissions occur when users or groups are granted access rights beyond what is required for their role, increasing the risk of misuse or escalation.

---

### ✈️ Airport Analogy

- A janitor can enter the control tower  
- A baggage worker can modify flight schedules  

---

### 🌍 Real-World Example

A user:
- Has write access to a sensitive folder  
- Modifies scripts or configurations  
- Introduces malicious changes  

---

### 🎯 Impact

- Unauthorized access  
- Privilege escalation opportunities  

---

### 🛡️ Defender Countermeasures

- Apply least‑privilege access across all AD objects  
- Regularly audit ACLs on sensitive folders and systems  
- Remove unnecessary write or modify permissions  
- Separate duties between operational and administrative roles  
- Monitor for permission changes on critical objects  

---

## 👑 Over-Privileged Accounts

### 📖 Definition

Over-privileged accounts are accounts that have more access rights than necessary, often including administrative privileges that increase security risk if compromised.

---

### ✈️ Airport Analogy

Too many employees:
- Have master keys  
- Can access critical systems  

---

### 🌍 Real-World Example

A company:
- Grants Domain Admin rights to multiple IT staff  
- One account gets compromised  
- Entire domain is exposed  

---

### 🎯 Impact

- Increased attack surface  
- High-value targets for attackers  

---

### 🛡️ Defender Countermeasures

- Reduce the number of administrative accounts  
- Use role‑based access control (RBAC) to assign only required privileges  
- Enforce just‑in‑time (JIT) access for elevated roles  
- Regularly review group memberships  
- Remove dormant or unused privileged accounts  

---

# 🧠 How It All Connects

### 🧪 Example Attack Flow

1. Attacker gains access as a normal user  
2. Identifies weak permissions  
3. Escalates privileges  
4. Extracts credentials or tickets  
5. Performs:
   - Pass-the-Hash  
   - Kerberoasting  
6. Moves laterally  
7. Gains Domain Admin access

---

## 🛠️ Tools & Techniques Used in Real Environments

Common tools attackers and defenders use in Active Directory environments:

- **Mimikatz** → Credential dumping (Pass-the-Hash)
- **Rubeus** → Kerberos ticket manipulation
- **BloodHound** → Mapping privilege escalation paths
- **PowerView** → AD enumeration
- **Event Viewer / SIEM** → Detection and monitoring

💡 Understanding these tools helps bridge the gap between theory and real-world execution.

---

### 🔍 Supporting Visualization — Privilege Escalation Path

<img width="500" height="700" alt="The attack chain from access to control" src="https://github.com/user-attachments/assets/8780c855-9cef-4c5f-b1a9-d63803c6071f" />


## 🧠 Attacker vs Defender Thinking

| Attacker Goal | Defender Strategy |
|--------------|-----------------|
| Steal credentials | Protect LSASS and credential storage |
| Move laterally | Network segmentation and monitoring |
| Escalate privileges | Enforce least privilege |
| Abuse Kerberos tickets | Monitor authentication and ticket activity |
| Maintain persistence | Audit accounts and remove unnecessary access |

💡 This perspective highlights how every attacker action has a corresponding defensive control.

---

## 👀 How to Recognize These Attacks in the Wild

Security teams often detect AD attacks through:

- Unusual authentication patterns  
- Repeated ticket requests  
- Service accounts behaving differently  
- Sudden privilege changes  
- Lateral movement between unrelated systems  
- Logons from unexpected devices or locations  

---

# 🛡️ Defender Mindset

To secure Active Directory:

- Enforce least privilege  
- Monitor authentication behavior  
- Protect credentials in memory  
- Audit account permissions regularly  
- Identify and remove risky configurations  

---

## ❗ Common Misconceptions

- “Kerberos is unbreakable.”  
  ➝ Tickets can be stolen or forged.

- “Only Domain Admins matter.”  
  ➝ Low-privilege accounts often lead to full compromise.

- “Service accounts are harmless.”  
  ➝ They often have powerful, overlooked permissions.

- “AD attacks require malware.”  
  ➝ Most attacks use built-in Windows features.

---
## 🎯 Final Takeaway

Active Directory is not just an IT system—it is the **identity backbone of the enterprise**.

If identity is compromised:
- Attackers don’t just access systems  
- They control the entire environment  

Modern attacks are not about breaking in…

They are about:
- Logging in  
- Blending in  
- Expanding access quietly  

Strong security requires:
- Understanding attacker behavior  
- Identifying weak points early  
- Breaking attack chains before escalation  

🛫 In cybersecurity, protecting identity means protecting everything.

---

# 🧾 Quick Reference Cheat Sheet

- **Privilege Escalation:** Passenger sneaks into restricted zones  
- **Kerberos:** Boarding pass system  
- **Pass-the-Hash:** Using a stolen badge without the PIN  
- **Kerberoasting:** Cracking special service passes  
- **AS-REP Roasting:** Getting a boarding pass without showing ID  
- **Weak Permissions:** Too many people with access  
- **Over-Privileged Accounts:** Too many master keys  

---

<p align="center">
<strong>🛫 In cybersecurity, protecting identity means protecting the entire airport.</strong>
</p>
