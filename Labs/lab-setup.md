# 🧪 Basic Lab Setup — Virtual Environment


## 📘 Overview

**Environment Type:** Isolated virtual lab environment (host-only network)

This lab demonstrates a simple multi-machine virtual environment. 
It is designed to show system setup, resource allocation, and network configuration in a controlled setting.

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

## ✅ Setup Checklist

- [ ] Virtual machines created  
- [ ] Resources allocated (CPU, RAM, Storage)  
- [ ] Network adapter configured  
- [ ] IP addresses assigned  
- [ ] All machines connected to the same network  

---

## 🎯 Purpose

This lab demonstrates foundational system administration and networking skills:

- Virtual machine deployment and configuration  
- Resource allocation and system planning  
- Basic network configuration and segmentation  
- Multi-system environment setup  
