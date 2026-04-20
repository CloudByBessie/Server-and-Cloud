<div align="center">

# 🧪 Enterprise Lab Setup (v2.0)

</div>


**Last Updated:** April 2026  
**Version:** 2.0

## 📘 Overview

**Environment Type:** Fully isolated, multi‑segment virtual enterprise network  
**Purpose:** Build the foundation for a cinematic attacker‑vs‑defender security engineering universe.

This environment builds on the original virtual lab and evolves into a segmented, enterprise‑style architecture. It includes domain controllers, Ubuntu servers, a SIEM, a firewall, and a dedicated attacker network — reflecting the complexity of real organizations and enabling identity security, misconfiguration analysis, and adversarial operations.

---
## 🧠 Why This Setup Matters

This environment is engineered to demonstrate **real‑world security engineering**, including:

- Active Directory deployment, hardening, and identity‑first security  
- DNS configuration, troubleshooting, and secure resolution  
- Network segmentation and traffic control using pfSense  
- Log aggregation and detection fundamentals  
- Offensive security testing in a safe, isolated environment  
- Lateral movement, privilege escalation, and attack path mapping  
- Enterprise‑style architecture design and documentation  

This is the backbone of the **CloudTech vs Thunderbyte** storyline — a defender‑built infrastructure under pressure from an attacker‑driven adversary.

---
## 🎯 Who This Setup Is For

This environment is ideal for:

- Security engineers practicing enterprise‑grade configurations  
- Blue Team learners building detection and hardening skills  
- Red Team learners exploring attack paths safely  
- Students preparing for security certifications  
- Anyone wanting hands‑on experience with Windows, Linux, Ubuntu, and real network segmentation  
---

## 🏗️ Lab Diagram

<img width="800"  alt="LabNetworkTopology" src="https://github.com/user-attachments/assets/fcc42f24-6675-44eb-b8ea-c6d4c057d55f" />


<em>Figure: Segmented enterprise lab topology showing multiple systems distributed across LAN_NET, SERVER_NET, APP_TIER, and DATA_TIER zones within an isolated internal network.</em>

## 🖥️ Lab Components (v2.0)

| **Machine Name** | **Role** | **Operating System** |
|------------------|----------|----------------------|
| **CT‑FW01** | Firewall / Segmentation Gateway | pfSense |
| **CT‑DC01** | Primary Domain Controller / DNS | Windows Server 2019/2022 |
| **CT‑DC02** | Secondary Domain Controller | Windows Server 2019/2022 |
| **CT‑CL01** | Domain Workstation | Windows 10/11 |
| **CT‑FS01** | File Server | Windows Server 2019/2022 |
| **CT‑WEB01** | Web Server | **Ubuntu Server** |
| **CT‑SVR01** | Application Server | **Ubuntu Server** |
| **CT‑DB01** | Database Server | **Ubuntu Server** |
| **CT‑SIEM01** | SIEM / Log Aggregation Node | **Ubuntu Server** |
| **TB‑KALI01** | Attacker System | Kali Linux |

---



## 🧩 Service‑Tier Mapping (Web → App → DB)

| **Tier** | **Machine** | **Purpose** | **Example IP (Safe / Non‑Real)** |
|----------|-------------|-------------|----------------------------------|
| **Web Tier** | CT‑WEB01 (Ubuntu) | Hosts web services, APIs, or front‑end applications. Entry point for user traffic and attacker probing. | 10.50.10.20 |
| **App Tier** | CT‑SVR01 (Ubuntu) | Processes logic, API calls, and middleware functions. Communicates with both Web and DB tiers. | 10.50.20.30 |
| **Database Tier** | CT‑DB01 (Ubuntu) | Stores structured data, credentials, and sensitive information. Highly restricted access. | 10.50.30.40 |

### 🔐 Tiered Access Model
- **Web → App:** Allowed  
- **App → DB:** Allowed  
- **Web → DB:** Blocked  
- **Attacker → Any Tier:** Controlled by pfSense segmentation  

This mirrors real enterprise Zero‑Trust segmentation.



---

## ⚙️ System Specifications

| Machine Name | CPU | RAM | Storage |
|--------------|-----|-----|---------|
| CT‑DC01 | 2 | 4 GB | 50 GB |
| CT‑DC02 | 2 | 4 GB | 50 GB |
| CT‑CL01 | 2 | 4 GB | 50 GB |
| CT‑FS01 | 2 | 4 GB | 50 GB |
| **CT‑WEB01** | 2 | 4 GB | 40–50 GB |
| **CT‑SVR01** | 2 | 4 GB | 40–50 GB |
| **CT‑DB01** | 2 | 4–8 GB | 60 GB (DB recommended) |
| **CT‑SIEM01** | 2 | 4–8 GB | 60–80 GB |
| TB‑KALI01 | 2 | 2 GB | 40 GB |
| CT‑FW01 | 1 | 1–2 GB | 20 GB |


---

## 🌐 Network Configuration (v2.0)

### **Network Segments**

| Segment | Subnet | Purpose |
|---------|--------|---------|
| **LAN_NET** | 172.16.10.0/24 | Domain controllers, client, SIEM |
| **SERVER_NET** | 172.16.20.0/24 | Web, file, and application servers |
| **APP_TIER** | 172.16.30.0/24 | Application interface layer |
| **DATA_TIER** | 172.16.40.0/24 | Database and storage systems |
| **ATTACK_NET** | 172.16.50.0/24 | Thunderbyte attacker network |
| **Firewall Gateway** | 172.16.x.1 | pfSense routing and segmentation |

### **IP Address Assignment**

| Machine Name | Example IP |
|--------------|------------|
| CT‑DC01 | 172.16.10.10 |
| CT‑DC02 | 172.16.10.11 |
| CT‑CL01 | 172.16.10.25 |
| CT‑SIEM01 | 172.16.10.50 |
| CT‑FS01 | 172.16.20.15 |
| CT‑WEB01 | 172.16.20.30 |
| CT‑SVR01 | 172.16.30.20 |
| CT‑DB01 | 172.16.40.10 |
| TB‑KALI01 | 172.16.50.10 |
| CT‑FW01 | 172.16.x.1 (per segment) |

---
> 🔒 **Security Note:**  
> This environment is intentionally isolated (host‑only) to ensure all testing is performed safely without impacting external networks.
---

## 🧰 Tools & Platform

- VirtualBox, VMware Workstation, or Hyper‑V  
- Windows Server ISO  
- Windows 10/11 ISO  
- Ubuntu Server ISO  
- Kali Linux ISO  
- pfSense ISO  
- draw.io / Excalidraw for diagramming 
---

## 📦 Prerequisites

- Hypervisor installed  
- ISO images downloaded  
- Minimum 24–32 GB RAM on host machine  
- At least 250 GB free storage
  
---
## ✅ Setup Checklist

- [ ] All virtual machines created  
- [ ] CPU, RAM, and storage allocated  
- [ ] pfSense firewall deployed  
- [ ] Network segments created  
- [ ] Static IPs assigned  
- [ ] Domain controllers configured  
- [ ] Servers placed in correct segments  
- [ ] SIEM connected and receiving logs  
- [ ] Attacker machine isolated in ATTACK_NET  

---

## 🎓 Learning Objectives

- Deploy and configure a multi‑segment enterprise environment  
- Understand identity‑first security and domain architecture  
- Practice segmentation, routing, and firewall rule creation  
- Build familiarity with Windows Server, Linux, and pfSense  
- Strengthen foundational networking and system administration skills  
- Prepare for real‑world attack and defense scenarios  
---
## 🛠️ Skills Demonstrated

- Virtualization and hypervisor management  
- System provisioning and resource planning  
- Network segmentation and IP addressing  
- Windows Server and Linux installation  
- Firewall configuration and traffic control  
- Multi‑machine orchestration  
- Enterprise‑style architecture design 


---
## 🎯 Purpose

This lab establishes the **core battlefield** for the CloudTech vs Thunderbyte universe.  
It provides the infrastructure required for:

- Active Directory deployment  
- DNS configuration  
- System hardening  
- Attack path enumeration  
- Lateral movement scenarios  
- Log analysis and detection engineering  

This is where the story begins — the defenders build, the attackers adapt, and the environment evolves.

