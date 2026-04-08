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

<img width="308" height="143" alt="image" src="https://github.com/user-attachments/assets/0a949485-1efb-4c27-b973-e5e0a01e5578" />

<em>Figure: Basic virtual lab topology showing three systems connected within an isolated internal network.</em>

## 🖥️ Lab Components

| Machine Name | Role            | Operating System         |
|--------------|-----------------|--------------------------|
| CT-DC01      | Server          | Windows Server 2019/2022 |
| CT-CL01      | Workstation     | Windows 10/11            |
| TB-Kali01    | Linux System    | Kali Linux               |

---

## ⚙️ System Specifications

| Machine Name | CPU | RAM  | Storage |
|--------------|-----|------|----------|
| CT-DC01      | 2   | 4 GB | 50 GB    |
| CT-CL01      | 2   | 4 GB | 50 GB    |
| TB-Kali01    | 2   | 2 GB | 40 GB    |

---

## 🌐 Network Configuration

- Network Type: Internal / Host-Only  
- Subnet: 192.168.1.0/24  

### IP Address Assignment

| Machine Name | IP Address   |
|--------------|--------------|
| CT-DC01      | 192.168.1.10 |
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
- Minimum 16 GB RAM on host machine  
- At least 150 GB free storage  


---
## ✅ Setup Checklist

- [ ] Virtual machines created  
- [ ] Resources allocated (CPU, RAM, Storage)  
- [ ] Network adapter configured  
- [ ] IP addresses assigned  
- [ ] All machines connected to the same network  

---

## 🎓 Learning Objectives

- Understand how to deploy and configure a multi‑machine virtual environment  
- Practice assigning and managing static IP addresses  
- Build familiarity with Windows Server and Linux installation  
- Strengthen foundational networking and system administration skills  

---
## 🛠️ Skills Demonstrated

- Virtualization and hypervisor management  
- System provisioning and resource planning  
- Network design and IP addressing  
- Windows Server and Linux installation  
- Host-only / internal network configuration  
- Multi-machine environment orchestration  


---
## 🎯 Purpose

This lab demonstrates foundational system administration and networking skills:

- Virtual machine deployment and configuration  
- Resource allocation and system planning  
- Basic network configuration and segmentation  
- Multi-system environment setup

---

## 🚀 Next Steps

With the environment deployed, the next labs will cover:
- Active Directory installation and configuration  
- DNS setup and security  
- Attack path enumeration using Kali Linux  
- System hardening and defensive controls  


