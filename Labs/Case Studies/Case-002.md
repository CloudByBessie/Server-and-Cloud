
# 🌩️ CloudTech Case 002 — *“The Server That Whispered Secrets”*

**TL;DR:** Internal SMB misconfiguration exposed deployment files and a service account (`svc_deploy`) with local admin rights; **immediate action**: restrict share access and rotate service account credentials.  


<p align="center">
<img width="700"  alt="case002upgrade" src="https://github.com/user-attachments/assets/6d2f5292-03a9-44e0-a05d-0d5e4c358992" />

</p>

---

<details>
  <summary><strong>📚 Quick Navigation</strong></summary>

- [Executive Summary](#-executive-summary)
- [Environment Snapshot](#-environment-snapshot)
- [Tools & Technologies Used](#️-tools--technologies-used)
- [What Broke (Root Cause)](#-what-broke-root-cause)
- [How I Investigated (Layered Troubleshooting)](#-how-i-investigated-layered-troubleshooting)
- [How I Fixed It (Remediation-Steps)](#️-how-i-fixed-it-remediation-steps)
- [Attacker POV](#-attacker-pov-condensed)
- [Defender POV](#️-defender-pov-condensed)
- [Impact](#-impact)
- [Skills Demonstrated](#-skills-demonstrated)
- [Why This Matters](#-why-this-matters)
- [Final Takeaway](#-final-takeaway)

</details>

---

## ⭐ Executive Summary
After stabilizing identity services in Case 001, CloudTech’s environment still lacked internal security boundaries. From an internal foothold (TB‑KALI01), I enumerated SMB shares on CT‑SVR01 and discovered an over‑permissive deployment share (**IT‑Deploy**) accessible to any authenticated domain user.

Inside the share were operational files referencing the **svc_deploy** service account — an account with **local administrator privileges** on CT‑SVR01 and used by scheduled tasks. This misconfiguration created a direct path for lateral movement and privilege escalation without requiring vulnerabilities or exploits.

This case demonstrates how internal trust, weak access control, and exposed service accounts can silently undermine an otherwise functional environment.

---

## 🏗️ Environment Snapshot
- **Active Directory Domain:** cloudtech.local  
- **Domain Controller:** CT‑DC01  
- **File Server:** CT‑SVR01  
- **Attacker Machine:** TB‑KALI01  
- **Endpoints:** ~25 Windows clients  
- **Key Services:** SMB, DNS, WinRM  

### AD OU Structure
<img width="260" src="https://github.com/user-attachments/assets/a05b64f3-a5e6-4708-84ae-71c241715e71" />

### Security Groups
<img width="500" src="https://github.com/user-attachments/assets/9b4426cb-aca4-4e9a-a2a7-67f799d46068" />

### svc_deploy Account
<img width="500" src="https://github.com/user-attachments/assets/9e27adb8-a286-428b-b07d-8ea1a1f253db" />

### svc_deploy Memberships
<img width="407" src="https://github.com/user-attachments/assets/893e401e-bf46-4b14-9552-c60412217c82" />

---

## 🛠️ Tools & Technologies Used
- **Kali Linux (TB‑KALI01)** — internal attacker simulation  
- **Nmap** — host and service enumeration  
- **SMBClient** — SMB share enumeration and file access  
- **Windows Server (CT‑SVR01)** — file server hosting exposed share  
- **Active Directory Users & Computers** — identity and group membership review  
- **DNS utilities** — domain discovery and service enumeration  

---

## ❌ What Broke (Root Cause)
The compromise path was created by the intersection of several misconfigurations:

- **Over‑permissive SMB share** (`IT‑Deploy`)  
- **Weak NTFS permissions** allowing broad read access  
- **Service account exposure** (`svc_deploy`) inside deployment files  
- **Local admin rights assigned to svc_deploy**  
- **Lack of segmentation** between deployment infrastructure and general endpoints  
- **Assumption that “internal = safe”**  

These issues combined to create a silent privilege‑escalation path.

---

## 🔍 How I Investigated (Layered Troubleshooting)

### 1. Internal Foothold (TB‑KALI01)
<img width="500" src="https://github.com/user-attachments/assets/ea4ed424-1510-4cf4-b57a-91c9b84b8b21" />

### 2. Host Discovery
<img width="559" src="https://github.com/user-attachments/assets/04cae5a9-6356-4f92-b933-f3d4ca48837a" />

### 3. Service Enumeration (CT‑SVR01)
<img width="500" src="https://github.com/user-attachments/assets/60c31009-cf73-4091-b9f0-9798918028e7" />

### 4. SMB Enumeration & Share Access
<img width="500" src="https://github.com/user-attachments/assets/2b19096c-b6b6-44ba-8a0a-becbe400f886" />

### 5. Sensitive File Exposure
<img width="544" src="https://github.com/user-attachments/assets/a4d19db5-b32a-482f-b2ac-2072946e1348" />

### 6. Server Misconfiguration Evidence

**IT‑Deploy Folder**  
<img width="500" src="https://github.com/user-attachments/assets/67ccc8ef-a687-4483-ac29-6a394cca5ede" />

**Share Permissions**  
<img width="500" src="https://github.com/user-attachments/assets/89076096-2ad7-4c47-ad03-e0905739b148" />

**NTFS Permissions**  
<img width="359" src="https://github.com/user-attachments/assets/77293767-f230-4814-82b8-f0dbbd910ebf" />

**svc_deploy in Local Admins**  
<img width="394" src="https://github.com/user-attachments/assets/964d3ee6-f054-45df-be16-b45cf4a87dcd" />

**Scheduled Task Using svc_deploy**  
<img width="500" src="https://github.com/user-attachments/assets/25e940f6-4b6c-497a-a540-c8aaadbb54a7" />

---

## 🛠️ How I Fixed It (Remediation Steps)

### 1. Restrict SMB Share Access
- Removed “Authenticated Users” from share permissions  
- Limited access to deployment admins only  

### 2. Tighten NTFS Permissions
- Applied least privilege  
- Removed broad read access  

### 3. Rotate svc_deploy Credentials
- Reset password  
- Stored new credentials in a vault  

### 4. Remove Unnecessary Local Admin Rights
- svc_deploy removed from Administrators group  
- Replaced with gMSA where possible  

### 5. Segment Deployment Infrastructure
- Moved CT‑SVR01 to a restricted VLAN  
- Limited inbound access  

### 6. Implement Monitoring
- Alerts for SMB access  
- Alerts for scheduled task modifications  

---

## ⚡ Attacker POV 
From an attacker’s perspective, the environment introduced itself:

- SMB enumeration revealed an exposed deployment share  
- Operational files disclosed service account usage  
- svc_deploy had local admin rights  
- No exploit required — only misconfiguration and trust  

The environment wasn’t hardened.  
It was **predictable, exposed, and overly trusting**.

---

## 🛡️ Defender POV 
A defender reviewing this incident would immediately recognize:

- **SMB shares must never be broadly accessible**  
- **Service accounts must not have local admin rights**  
- **Deployment artifacts often leak sensitive information**  
- **Internal networks require segmentation and monitoring**  
- **Misconfiguration is a more common threat than exploitation**  

This case highlights the importance of operational hygiene and identity governance.

---

## 📊 Impact
- **Privilege Escalation Potential:** High  
- **Affected Hosts:** CT‑SVR01 directly; ~12 endpoints indirectly  
- **Business Impact:** High — lateral movement and service disruption possible  
- **Exploit Required:** None  
- **Root Cause:** Excessive trust + weak access control  

---

## 🧠 Skills Demonstrated
- Internal attack path analysis  
- SMB enumeration and misconfiguration detection  
- Identity and service account governance  
- Least‑privilege access control  
- Network segmentation strategy  
- Clear technical communication and incident reporting  

---

## 🎯 Why This Matters
This case demonstrates the ability to identify and remediate silent privilege‑escalation paths created by misconfiguration — one of the most common and dangerous issues in modern enterprise environments. It reflects disciplined investigation, strong identity awareness, and the ability to restore secure operational boundaries.

---

## 🚀 Final Takeaway
Attackers don’t need zero‑days when environments expose themselves.  
By tightening access control, removing unnecessary privileges, and segmenting critical infrastructure, I transformed a fragile internal configuration into a secure, predictable system.
