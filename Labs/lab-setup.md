# 🧪 Basic Lab Setup — Virtual Environment

**Last Updated:** April 2026  
**Version:** 1.0

## 📘 Overview

**Environment Type:** Isolated virtual lab environment (host-only network)

This lab demonstrates a simple multi-machine virtual environment. 
It is designed to show system setup, resource allocation, and network configuration in a controlled setting.

---
## 🧠 Why This Setup Matters

This environment forms the foundation for demonstrating real-world security engineering skills, including:
- Active Directory deployment and hardening
- DNS configuration and troubleshooting
- Network segmentation and traffic analysis
- Offensive security testing in a safe, isolated environment

It shows the ability to design, build, and manage a multi-system enterprise-style lab — a core skill for security engineers.
CT-SVR01 adds a realistic member server to the lab, making the environment closer to a real enterprise network and supporting more advanced attack and defense scenarios.


---
## 🎯 Who This Setup Is For

This environment is designed for:

- Security engineers practicing enterprise‑style configurations  
- Blue Team learners building foundational defensive skills  
- Red Team learners needing a safe attack environment  
- Students preparing for security certifications  
- Anyone wanting hands‑on experience with Windows, Linux, and networking 

---

## 🏗️ Lab Diagram

<img width="519" height="370" alt="image" src="https://github.com/user-attachments/assets/17c0d74c-6140-409c-aab0-48e29b111864" />

<em>Figure: Basic virtual lab topology showing four systems connected within an isolated internal network.</em>

## 🖥️ Lab Components

| Machine Name | Role                         | Operating System         |
|--------------|------------------------------|--------------------------|
| CT-DC01      | Domain Controller / DNS      | Windows Server 2019/2022 |
| CT-SVR01     | Member Server                | Windows Server 2019/2022 |
| CT-CL01      | Workstation                  | Windows 10/11            |
| TB-Kali01    | Attacker System              | Kali Linux               |
---

## ⚙️ System Specifications

| Machine Name | CPU | RAM  | Storage |
|--------------|-----|------|---------|
| CT-DC01      | 2   | 4 GB | 50 GB   |
| CT-SVR01     | 2   | 4 GB | 50 GB   |
| CT-CL01      | 2   | 4 GB | 50 GB   |
| TB-Kali01    | 2   | 2 GB | 40 GB   |

---

## 🌐 Network Configuration

- Network Type: Internal / Host-Only  
- Subnet: 192.168.1.0/24  

### IP Address Assignment

| Machine Name | IP Address   |
|--------------|--------------|
| CT-DC01      | 192.168.1.10 |
| CT-SVR01     | 192.168.1.15 |
| CT-CL01      | 192.168.1.20 |
| TB-Kali01    | 192.168.1.30 |

---
> 🔒 **Security Note:**  
> This environment is intentionally isolated (host‑only) to ensure all testing is performed safely without impacting external networks.
---

## 🧰 Tools & Platform

- Hypervisor: VirtualBox or VMware Workstation
- Windows Server ISO  
- Windows 10/11 ISO  
- Kali Linux ISO  

---

## 📂 Repository Structure

```
.
├── Labs/
│   ├── assets/                       # Images and diagrams
│   └── cloudtech-vs-thunderbyte/     # Scenario & project storyline
└── lets-learn/                       # Notes and learning documentation
```
---
## 📦 Prerequisites

- VirtualBox or VMware installed  
- ISO images downloaded (Windows Server, Windows 10/11, Kali Linux)  
- Minimum 16–24 GB RAM on host machine    
- At least 200 GB free storage   


---
## ✅ Setup Checklist

- [ ] Virtual machines created  
- [ ] Resources allocated (CPU, RAM, Storage)  
- [ ] Network adapter configured  
- [ ] IP addresses assigned  
- [ ] All machines connected to the same isolated network  
- [ ] CT-SVR01 joined to the domain   

---

## 🎓 Learning Objectives

- Understand how to deploy and configure a multi-machine virtual environment  
- Practice assigning and managing static IP addresses  
- Build familiarity with Windows Server, Windows client, and Linux installation  
- Understand the role of a domain controller, member server, client, and attacker system  
- Strengthen foundational networking and system administration skills  

---
## 🛠️ Skills Demonstrated

- Virtualization and hypervisor management  
- System provisioning and resource planning  
- Network design and IP addressing  
- Windows Server and Linux installation  
- Host-only / internal network configuration  
- Multi-machine environment orchestration  
- Basic domain and member server architecture understanding   


---
## 🎯 Purpose

This lab demonstrates foundational system administration and networking skills:

- Virtual machine deployment and configuration  
- Resource allocation and system planning  
- Basic network configuration and segmentation  
- Multi-system environment setup  
- Enterprise-style architecture using a domain controller, member server, client, and attacker machine  

---

## 🚀 Next Steps

With the environment deployed, the next labs will cover:
- Active Directory installation and configuration  
- Domain joining CT-SVR01 and CT-CL01  
- DNS setup and security  
- Attack path enumeration using Kali Linux  
- System hardening and defensive controls  
- Member server misconfiguration and lateral movement scenarios  


