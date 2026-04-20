<h1 align="center">⚔️ Labs — Cloud Tech vs Thunderbyte</h1>

<p align="center">
  <em>A cinematic simulation of attackers and defenders in modern enterprise infrastructure</em>
</p>

<p align="center">
  <strong>Red Team vs Blue Team • Full‑Spectrum Security Engineering</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Blue%20Team-Active%20Directory-blue" />
  <img src="https://img.shields.io/badge/Red%20Team-Offensive%20Security-red" />
  <img src="https://img.shields.io/badge/PowerShell-Automation-5391FE" />
  <img src="https://img.shields.io/badge/Virtualization-Lab%20Environment-green" />
</p>

---

<div align="center">

<img width="750" alt="CloudTechVSThunderbyte" src="https://github.com/user-attachments/assets/d228d718-cb3e-43ff-9893-059fcf3a57fa" />

  
<strong>Welcome to the battlefield.</strong><br>
This is not a simple lab collection — it’s a cinematic cyber war between two engineered forces.
</div>

---

# 🔍 Recruiter Summary

This project demonstrates the ability to **design, secure, attack, and analyze** enterprise‑grade environments.  
It showcases hands‑on experience across **identity security, network segmentation, system hardening, and adversarial operations** — all mapped to real organizational challenges.

You’ll see:
- How segmented architectures are built and defended  
- How attackers exploit misconfigurations and weak IAM  
- How defenders detect, mitigate, and remediate threats  
- How findings are documented with clarity and precision  

This is a **portfolio‑grade security engineering simulation**, not a classroom exercise.

---

# 🧠 Architecture Overview

The lab environment mirrors a real enterprise network — segmented, logged, and fortified.

## Core Components

| **Machine** | **Role** | **Function in the Environment** |
|------------|----------|---------------------------------|
| **CT‑FW01 (pfSense)** | Firewall / Segmentation Gateway | Enforces network boundaries, routes traffic between all subnets, collects logs, and acts as the choke point for both attacker ingress and defender monitoring. |
| **CT‑DC01** | Primary Domain Controller | Hosts Active Directory, authentication, Group Policy, and identity‑first security controls. Central to Blue Team defense. |
| **CT‑DC02** | Secondary Domain Controller | Provides AD redundancy, load balancing, and resilience. Ensures authentication continuity during attacks or outages. |
| **CT‑CL01** | Domain‑Joined Client | Represents a real enterprise workstation. Used for user‑level testing, GPO validation, and simulating lateral movement paths. |
| **CT‑FS01** | File Server | Stores shared resources, SMB shares, and sensitive data. A common attacker target for credential harvesting and privilege escalation. |
| **CT‑WEB01** | Web Server | Hosts internal or external web applications. Used to simulate web‑layer attack surfaces and server‑side misconfigurations. |
| **CT‑SVR01** | Application Server | Runs application services and infrastructure components. Supports scenarios like service abuse, privilege escalation, and lateral movement. |
| **CT‑DB01** | Database Server | Stores structured data and backend information. Represents high‑value targets for attackers and critical assets for defenders. |
| **CT‑SIEM01** | SIEM / Log Aggregation Node | Collects logs from DCs, servers, and the firewall. Enables detection engineering, correlation, and Blue Team visibility. |
| **TB‑KALI01** | Attacker Node (Thunderbyte) | Performs reconnaissance, exploitation, lateral movement, and adversarial testing against the CloudTech environment. |


Each segment connects through the firewall, creating a realistic **defense‑in‑depth topology** where CloudTech defends and Thunderbyte attacks.

---

# 🛠️ Skills Mapping

## ☁️ Cloud Tech — Defender Skills
- Active Directory configuration and hardening  
- DNS setup and secure resolution  
- System baseline enforcement  
- Vulnerability identification and remediation  
- Log analysis and detection engineering  
- PowerShell automation for security tasks  
- Network architecture and segmentation design  

## ⚡ Thunderbyte — Attacker Skills
- Network enumeration and reconnaissance  
- Exploiting misconfigurations and weak IAM  
- Lateral movement and privilege escalation  
- Credential harvesting and attack path discovery  
- Adversarial methodology and offensive mindset  

## 🧩 Cross‑Discipline Skills
- Threat modeling and documentation  
- Diagramming and environment visualization  
- Translating technical findings into actionable insights  
- Understanding attacker–defender interplay  

---

# 🧰 Technologies Used

- Windows Server 2022 (AD, DNS)
- Windows 11 Enterprise (Client)
- Kali Linux (Offensive Security)
- pfSense Firewall (CT‑FW01)
- PowerShell (Automation)
- Wireshark, Nmap, BloodHound
- VirtualBox / VMware / Hyper‑V
- draw.io / Excalidraw (Network Diagramming)
- SIEM Integration (CT‑SIEM01)

---

## ⚔️ The Theme — Cloud Tech vs Thunderbyte
A cinematic simulation of modern security engineering —  
where CloudTech defends the enterprise and Thunderbyte attacks its weaknesses.


---

> ⚡ Every misconfiguration is an opportunity.  
> 🛡️ Defense begins with understanding the attack.  
> 🎯 This lab is where both sides collide.

---

# 🎯 Purpose

To simulate real‑world enterprise environments and explore them from both perspectives:

### ☁️ Cloud Tech (Blue Team)
- Secure Active Directory  
- Configure DNS correctly  
- Harden systems  
- Detect vulnerabilities  

### ⚡ Thunderbyte (Red Team)
- Enumerate networks  
- Exploit misconfigurations  
- Move laterally  
- Identify weak points  

---

# 🧩 What This Demonstrates About Me

- I can design and deploy enterprise‑style environments  
- I understand identity‑first security and misconfiguration risks  
- I think like both an attacker and a defender  
- I document clearly, visually, and professionally  
- I translate complex systems into actionable insights  

---

# 🏆 Key Outcomes
- Built a segmented enterprise domain environment from scratch  
- Identified and exploited real misconfigurations  
- Hardened systems using best practices  
- Documented attack paths and defensive mitigations  
- Demonstrated attacker‑minded defensive engineering  

---

# ⚡ Final Note

This isn’t just a lab.  
It’s a **cinematic cyber war** — a clash of light and storm, logic and chaos.  
Every packet tells a story.  
Every misconfiguration hides an opportunity.  
Every defense is forged in the aftermath of an attack.  

**This is where engineering meets art.**  
**This is Cloud Tech vs Thunderbyte.**

---

# 📬 Connect With Me

- **LinkedIn:** [linkedin.com/in/bessie-mullins-1ba343183](https://www.linkedin.com/in/bessie-mullins-1ba343183)  
- **GitHub:** [github.com/CloudByBessie](https://github.com/CloudByBessie)
