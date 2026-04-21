# 🌩️ CloudTech Case 002 — *“The Server That Whispered Secrets”*

**TL;DR:** Internal SMB misconfiguration exposed deployment files and a service account (`svc_deploy`) with local admin rights; **immediate action**: restrict share access and rotate service account credentials.  


<p align="center">
<img width="700"  alt="case002upgrade" src="https://github.com/user-attachments/assets/6d2f5292-03a9-44e0-a05d-0d5e4c358992" />

</p>

---

## 🔎 Jump to Section

### ⚡ Overview

[⚡ Quick Wins](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-quick-wins)

[🧾 Executive Summary](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-executive-summary)

[🧩 Case Overview](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-case-overview)

[📊 Quantified Impact](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-quantified-impact)

---

### 🏢 Environment & Misconfiguration
  
[📸 Active Directory Environment](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-active-directory-environment)

[📸 Server Misconfiguration](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-server-misconfiguration)

[📸 Workstation Evidence](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-workstation-evidence)

---

### 💥 Attack Path

[💥 Example Attack Scenario](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-example-attack-scenario)

[🧑‍💻 Attacker Foothold](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-attacker-foothold)

[🌐 Network Discovery](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-network-discovery)

[🛰️ Service Enumeration](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-service-enumeration)

[🧠 DNS Discovery](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-dns-discovery)

[📂 SMB Enumeration & Share Access](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-smb-enumeration--share-access)

---

### 🛡️ Response & Lessons

[🔎 Investigation & Remediation](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-investigation--remediation)

[📚 Lessons Learned](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-lessons-learned)

[🔥 Final Takeaway](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-final-takeaway)


---


### 🧪 Appendix

[🧪 Reproducibility Appendix](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-002.md#-reproducibility-appendix--sanitized-commands)


---

## ⚡ Quick Wins

- Identified Active Directory infrastructure from an internal foothold  
- Enumerated SMB shares on CT-SVR01  
- Discovered exposed deployment share **IT-Deploy**  
- Extracted operational files revealing service account usage (**svc_deploy**)  
- Demonstrated how trust-based misconfiguration creates attack paths 


---


## 🧾 Executive Summary

CloudTech’s environment appeared stable after Case 001 remediation, but lacked internal security boundaries. From an internal foothold (TB-KALI01), ThunderByte performed reconnaissance and found an over‑permissive deployment share that exposed operational files and a service account with elevated privileges. No exploit or zero‑day was required — the environment revealed sensitive information through misconfiguration and excessive trust.

**Scope used for this report:** single AD domain with 1 domain controller, 1 file server (CT-SVR01), and ~25 endpoints. Quantified impact below uses this scope.




---
## 🔍 Key Insight

> **Attackers don’t break into environments — they observe what was never hidden.**

---

## 🎯 Outcome

- Domain infrastructure mapped  
- SMB share accessed  
- Service account identified (**svc_deploy**)  
- Sensitive operational data exposed  
- Path to lateral movement identified


---

## 🧠 What This Demonstrates

- Internal attacker methodology  
- Active Directory discovery via services  
- SMB share abuse  
- Misconfiguration-driven compromise 


---

## 🧩 Case Overview

Following Case 001, CloudTech’s systems were operational but not segmented. From **TB-KALI01**, the attacker used DNS and service enumeration to map the domain and enumerate SMB shares. A deployment share accessible to domain users contained operational files that revealed service account usage and local admin assignments.

---
## 📊 Quantified Impact

**Assumptions:** single AD domain; 1 DC, 1 file server, ~25 endpoints.

- **Affected hosts:** *Directly affected* — CT-SVR01 (file server), CT-CL01 (mapped workstation). *Potentially affected* — up to **~12 endpoints** that had the share mapped or used deployment artifacts.  
- **Privilege escalation potential:** **High** — `svc_deploy` is a service account used by scheduled tasks and is a member of local Administrators on CT-SVR01.  
- **Estimated business impact:** **High** — unauthorized lateral movement and privilege escalation could lead to data exfiltration and service disruption.  
- **Confidence level:** Medium-high (based on enumerated evidence and screenshots).

This attack path requires no exploit development and can be executed by any authenticated domain user with basic enumeration capability.
---


## 🚨 Risk Statement

If exploited in a real enterprise environment, this misconfiguration could allow an attacker to transition from a standard domain user to administrative control of critical infrastructure within a single attack path.

The combination of exposed deployment artifacts, service account privilege, and lack of segmentation significantly increases the likelihood of lateral movement, privilege escalation, and potential domain compromise.


---

## 🛠️ Tools & Technologies Used

- Kali Linux (TB-KALI01)  
- Nmap  
- SMBClient / smbclient  
- Windows Server (CT-SVR01)  
- Active Directory enumeration tools  
- DNS queries  

---

## 🎯 Objective

Simulate an internal attacker attempting to:

- Discover infrastructure  
- Identify services  
- Enumerate resources  
- Extract operational intelligence  

---

## ⚠️ Root Cause (At a Glance)

- Over-permissive SMB share  
- Weak NTFS permissions  
- Service account exposure and misuse  
- Lack of network segmentation  
- Excessive trust of internal network  
 
---

## 📸 Active Directory Environment

### AD OU Structure
<img width="260" height="417" alt="Active Directory OU Structure" src="https://github.com/user-attachments/assets/a05b64f3-a5e6-4708-84ae-71c241715e71" />

### Security Groups
<img width="716" height="364" alt="CloudTech Security Groups Created" src="https://github.com/user-attachments/assets/9b4426cb-aca4-4e9a-a2a7-67f799d46068" />

### Standard Users
<img width="722" height="356" alt="Standard Domain Users" src="https://github.com/user-attachments/assets/6ed21a90-4105-47fb-a24e-4637a2d6a027" />

### svc_deploy Account
<img width="718" height="353" alt="Service Account Created - svc_deploy" src="https://github.com/user-attachments/assets/9e27adb8-a286-428b-b07d-8ea1a1f253db" />

### svc_deploy Memberships
<img width="407" height="538" alt="svc_deploy Group Membership" src="https://github.com/user-attachments/assets/893e401e-bf46-4b14-9552-c60412217c82" />

---

## 📸 Server Misconfiguration

### IT-Deploy Folder
<img width="782" height="160" alt="IT-Deploy Folder Created on CT-SVR01" src="https://github.com/user-attachments/assets/67ccc8ef-a687-4483-ac29-6a394cca5ede" />

### Share Configuration
<img width="760" height="264" alt="IT-Deploy Share Configuration" src="https://github.com/user-attachments/assets/a0d41820-3228-4dce-b808-8e26ccf367b5" />

### Share Permissions
<img width="911" height="383" alt="Over-Permissive Share Permissions" src="https://github.com/user-attachments/assets/89076096-2ad7-4c47-ad03-e0905739b148" />

### NTFS Permissions
<img width="359" height="476" alt="NTFS Permissions on IT-Deploy" src="https://github.com/user-attachments/assets/77293767-f230-4814-82b8-f0dbbd910ebf" />

### Sensitive Files in Share
<img width="785" height="256" alt="Sensitive Operational Files in IT-Deploy Share" src="https://github.com/user-attachments/assets/d982f81d-4a8e-491c-aa3d-329feeed36ba" />

### WinRM Enabled
<img width="573" height="88" alt="WinRM Enabled on CT-SVR01" src="https://github.com/user-attachments/assets/dbf205ca-aa57-4498-ba75-c2b9a509828b" />

### Local Admin Group Membership
<img width="394" height="451" alt="svc_deploy Added to Local Administrators" src="https://github.com/user-attachments/assets/964d3ee6-f054-45df-be16-b45cf4a87dcd" />

### Scheduled Task Using svc_deploy
<img width="653" height="628" alt="Scheduled Task Running as svc_deploy 3" src="https://github.com/user-attachments/assets/25e940f6-4b6c-497a-a540-c8aaadbb54a7" />

---

## 📸 Workstation Evidence

### Mapped Drive on CT-CL01
<img width="787" height="594" alt="Mapped Deployment Share on CT-CL01 2" src="https://github.com/user-attachments/assets/7a0e60e9-c318-4c95-ba23-81b7fcc5f139" />

### Admin Note on CT-CL01
<img width="749" height="515" alt="Workstation Evidence of Server Trust Relationship" src="https://github.com/user-attachments/assets/728c32cc-78d2-4844-9736-5771ed35c868" />

---

## 💥 Example Attack Scenario

An attacker with internal access (compromised endpoint, rogue device, or malware) can:

1. Scan the network  
2. Identify domain services  
3. Enumerate SMB shares  
4. Access exposed resources  
5. Extract operational intelligence and move laterally  

**Finding:** No exploit required — misconfiguration and over-trust enabled the attack path.

---

## 🧑‍💻 Attacker POV

> “The network didn’t fight back. It introduced itself.”

- Domain controller identified via open ports  
- DNS revealed structure  
- SMB share permissions allowed any authenticated domain user to enumerate and read deployment artifacts, exposing operational logic and service account usage patterns.  
- Deployment share revealed sensitive information 

---

## 📸 Attacker Foothold

### Kali Foothold
<img width="811" height="510" alt="TB-Kali01 Internal Network Foothold" src="https://github.com/user-attachments/assets/ea4ed424-1510-4cf4-b57a-91c9b84b8b21" />


---

## 📸 Network Discovery

### Host Discovery
<img width="559" height="357" alt="Host DIscovery Across the CloudTech Subnet" src="https://github.com/user-attachments/assets/04cae5a9-6356-4f92-b933-f3d4ca48837a" />

---

## 📸 Service Enumeration

### CT-DC01 Scan
<img width="1024" height="384" alt="CT-DC01 Service Enumeration" src="https://github.com/user-attachments/assets/f8770cf4-329b-402e-9d34-8b9a8c153267" />

### CT-SVR01 Scan
<img width="770" height="271" alt="CT-SVR01 Service Enumeration" src="https://github.com/user-attachments/assets/60c31009-cf73-4091-b9f0-9798918028e7" />

---

## 📸 DNS Discovery

### DNS SRV Lookup
<img width="422" height="374" alt="DNS SRV Query Reveals Domain COntroller Services" src="https://github.com/user-attachments/assets/b481cc3a-6074-46c2-8749-e43131ecae59" />

---

## 📸 SMB Enumeration & Share Access

### Share Access
<img width="995" height="377" alt="Attacker Accesses IT-Deploy Share" src="https://github.com/user-attachments/assets/2b19096c-b6b6-44ba-8a0a-becbe400f886" />

### File Contents
<img width="544" height="550" alt="Operational Files Reveal Service Account Details" src="https://github.com/user-attachments/assets/a4d19db5-b32a-482f-b2ac-2072946e1348" />

---

## 🔐 Credential Validation Attempt

Based on the extracted operational files, the attacker identified references to the `svc_deploy` service account and its use in scheduled tasks and administrative operations.

At this stage, a real-world attacker would attempt to validate these credentials across accessible services such as:

- SMB authentication
- WinRM (remote management)
- Scheduled task execution context

Even without direct credential extraction, the exposure of service account usage significantly reduces the effort required for further compromise.

### 🔎 Analysis

This represents a critical transition point:

- From **information gathering → access validation**
- From **visibility → potential control**

The presence of a service account with local administrative privileges introduces a high-risk pathway for lateral movement and privilege escalation.

---

## 🧑‍💻 Attacker Conclusion

The attacker did not exploit a vulnerability. They:

- Observed  
- Connected  
- Collected  

**Finding:** The environment exposed itself.

---

## 🛡️ Defender Analysis

CloudTech prioritized functionality over security. Failures included:

- Overexposed resources  
- Weak access control  
- Lack of monitoring  
- Poor credential handling  

---

## 🚨 Core Issues Identified

- Open SMB share access  
- Sensitive operational files exposed  
- Service account misuse  
- Lack of segmentation  
- Excessive internal trust  

---

## 🔎 Investigation & Remediation

### Investigation Steps Taken

- Enumerated SMB shares  
- Accessed deployment files  
- Identified service account usage  
- Analyzed exposed data and local admin assignments 
 

### Prioritized Remediation Plan (30/60/90 Days)

**30 Days (Immediate, High Priority)**  
- **Action:** Restrict IT-Deploy share to specific service accounts and deployment servers.  
- **Owner:** IT Ops (Server Team)  
- **Effort:** 1–2 days  
- **Success metric:** Share ACLs updated; domain users no longer have read access.

- **Action:** Rotate `svc_deploy` credentials and disable account where possible.  
- **Owner:** Identity Team / IT Ops  
- **Effort:** 1 day  
- **Success metric:** Old credentials invalidated; new credentials stored in vault.

**60 Days (Medium Priority)**  
- **Action:** Apply least privilege to NTFS ACLs and remove `svc_deploy` from local Administrators where not required.  
- **Owner:** Server Team / Security Engineering  
- **Effort:** 3–5 days  
- **Success metric:** NTFS ACLs tightened; `svc_deploy` only has required rights.

- **Action:** Implement monitoring and alerting for SMB access and scheduled tasks.  
- **Owner:** Security Operations Center (SOC)  
- **Effort:** 2–4 weeks  
- **Success metric:** Alerts for unusual SMB reads and scheduled task changes.

**90 Days (Longer Term, Low-Medium Priority)**  
- **Action:** Network segmentation to separate deployment infrastructure from general user workstations.  
- **Owner:** Network Engineering / IT Ops  
- **Effort:** 4–8 weeks (phased)  
- **Success metric:** Deployment servers in restricted VLANs; limited inbound access.

- **Action:** Introduce managed service accounts or group-managed service accounts and store secrets in a vault.  
- **Owner:** Identity Team / Security Engineering  
- **Effort:** 2–4 weeks  
- **Success metric:** Service accounts managed centrally; secrets rotated automatically.

---

## 🧠 Troubleshooting Philosophy (Layered)

1. Network → connectivity  
2. Discovery → host identification  
3. Service → open ports  
4. Identity → domain discovery  
5. Access → SMB enumeration  
6. Data → file extraction 

---

## 📚 Lessons Learned

- Internal ≠ secure  
- SMB is high-risk and must be monitored  
- Service accounts must be protected and rotated  
- Misconfigurations create exposure  

---

## 🛡️ Defender Insights

- Restrict internal access to deployment resources  
- Audit shares and NTFS ACLs regularly  
- Monitor SMB activity and scheduled tasks 

---

## ⚔️ Attacker Insights

- Internal access is powerful  
- SMB reveals critical information quickly  
- Scripts and deployment artifacts often leak infrastructure details
  
---

## 🏁 Final Outcome

- SMB shares enumerated  
- Deployment share accessed  
- Files extracted  
- Service account identified  
- Security weaknesses documented

---

## 🔥 Final Takeaway

From a defensive standpoint, this case highlights how minor misconfigurations — when combined — create a complete attack path without the need for exploitation.

> **CloudTech wasn’t breached — it was understood.**  
> The system worked exactly as designed. And that was the vulnerability.


---
# 🧪 Reproducibility Appendix — Sanitized Commands

> Replace <TARGET_HOST>, <TARGET_SUBNET>, <SHARE>, and <DOMAIN> with your environment values. Do not include real credentials in public artifacts.

---

## Service Enumeration (DC and File Server)

    nmap -sV -p 88,135,139,389,445,5985 <TARGET_HOST>

---

## SMB Enumeration

    smbclient -L //<TARGET_HOST> -N
    smbclient //<TARGET_HOST>/<SHARE> -N -c 'ls'

---

## Enumerate AD via DNS and SRV

    dig _ldap._tcp.dc._msdcs.<DOMAIN> SRV

---

## List Scheduled Tasks (Sanitized Example)

    Get-ScheduledTask -TaskName "Deploy*" | Format-List

---

## Check Local Admin Membership (Sanitized)

    Get-LocalGroupMember -Group "Administrators"
