<div align="center">

# 🖧 Network Diagram — Cloud Tech vs Thunderbyte (v2.0)

**A segmented enterprise lab built for identity security, adversarial simulation, and real‑world detection engineering.**

</div>

---

## 📘 Overview

This document provides a visual and structural breakdown of the **Cloud Tech vs Thunderbyte** cybersecurity lab — a fully segmented, enterprise‑style Active Directory environment designed for both offensive and defensive security workflows.

The architecture mirrors real organizations by incorporating:

- Multi‑tier network segmentation  
- Identity‑centric infrastructure  
- Web, application, and data layers  
- A dedicated attacker network  
- A pfSense firewall enforcing strict boundaries  

---

## 🧰 Tech Stack

This lab demonstrates hands‑on experience with a modern, enterprise‑aligned security and infrastructure stack:

**Identity & Windows Infrastructure**
- Windows Server (Active Directory, DNS, Kerberos, GPO)
- Multi‑DC replication and identity redundancy

**Linux Infrastructure**
- Ubuntu Server (Web, Application, and Database tiers)
- Systemd services, hardening, and tier‑based access control

**Network & Segmentation**
- pfSense Firewall (VLAN segmentation, NAT, rule enforcement)
- Multi‑tier network design (LAN → Server → App → Data)

**Security & Detection**
- Splunk Enterprise (log aggregation, detection engineering)
- Sysmon + Windows Event Logging

**Adversarial Tooling**
- Nmap (network scanning)
- BloodHound / SharpHound (AD enumeration)
  

---

## 🖼️ Segmented Network Topology (v2.0)

<p align="center">

<img width="750" alt="LabDiagram42026" src="https://github.com/user-attachments/assets/17100add-e543-479d-827c-6208f4464de6" />


</p>

<em>Figure: Segmented enterprise lab topology showing systems distributed across LAN_NET, SERVER_NET, APP_TIER, and DATA_TIER within an isolated internal network.</em>

---

## 🧩 Network Segments

| Segment | Subnet | Purpose |
|--------|--------|---------|
| **LAN_NET** | 172.16.10.0/24 | Identity, clients, SIEM |
| **SERVER_NET** | 172.16.20.0/24 | Web and service hosting |
| **APP_TIER** | 172.16.30.0/24 | Application logic layer |
| **DATA_TIER** | 172.16.40.0/24 | Database and backend storage |
| **ATTACK_NET** | 172.16.50.0/24 | Thunderbyte attacker network |
| **Firewall Gateway** | 172.16.x.1 | pfSense segmentation and routing |

---
## 🧩 Environment Summary

| Component        | Hostname   | Role                               | IP Address      | Operating System |
|-----------------|------------|------------------------------------|-----------------|------------------|
| Primary DC      | CT‑DC01    | Active Directory + DNS             | 172.16.10.10    | Windows Server   |
| Secondary DC    | CT‑DC02    | AD Redundancy / Failover           | 172.16.10.11    | Windows Server   |
| Client          | CT‑CL01    | Domain‑Joined Workstation          | 172.16.10.25    | Windows 10/11    |
| SIEM Node       | CT‑SIEM01  | Log Aggregation / Detection        | 172.16.10.50    | Ubuntu (Splunk)  |
| Web Server      | CT‑WEB01   | Web / Service Hosting              | 172.16.20.30    | **Ubuntu Server** |
| App Server      | CT‑SVR01   | Application Logic Tier             | 172.16.30.20    | **Ubuntu Server** |
| Database Server | CT‑DB01    | Data Storage / Backend Services    | 172.16.40.10    | **Ubuntu Server** |
| Attacker        | TB‑KALI01  | Red Team / Offensive Operations    | 172.16.50.10    | Kali Linux       |
| Firewall        | CT‑FW01    | pfSense Segmentation Gateway       | 172.16.x.1      | pfSense          |


---

## 🛡️ Identity Tier — Domain Controllers (LAN_NET)

### **CT‑DC01 / CT‑DC02 — Active Directory Core**

The identity tier anchors the entire environment. These systems provide:

- Kerberos authentication  
- DNS service discovery  
- Group Policy enforcement  
- Directory object management  
- Multi‑tier trust relationships  

---

## 💻 User Tier — CT‑CL01 (LAN_NET)

### **CT‑CL01 — Domain Client**

A realistic enterprise workstation used to model:

- Credential harvesting  
- User‑driven attack paths  
- Lateral movement into server tiers  

**Why it matters:**  
Endpoints are the #1 initial compromise vector in real organizations.

---

## 🌐 CT‑WEB01 — Web Server (SERVER_NET)

**Role:** Ubuntu‑based web and service hosting platform

### 🔧 Configuration
- Runs on **Ubuntu Server**
- Hosts internal web applications or APIs
- Communicates with CT‑SVR01 in the APP_TIER
- Positioned in the SERVER_NET segment

### 🎯 Purpose in Lab
- Demonstrates web‑to‑app‑tier pivoting
- Models real‑world Linux web infrastructure
- Enables testing of misconfigurations, weak services, and exposed ports

### ⚠️ Security Considerations
- Web servers are high‑exposure assets
- Often targeted for initial footholds
- Misconfigured services can leak credentials or internal paths


---

## 🖥️ CT‑SVR01 — Application Server (APP_TIER)

**Role:** Ubuntu‑based application logic server**

### 🔧 Configuration
- Runs on **Ubuntu Server**
- Hosts internal application logic or middleware
- Acts as the bridge between web and data tiers

### 🎯 Purpose in Lab
- Demonstrates multi‑tier lateral movement
- Enables service account abuse scenarios
- Models real enterprise app‑tier segmentation

### ⚠️ Security Considerations
- App servers often run elevated services
- Misconfigurations can expose sensitive backend connections
- Critical pivot point toward the DATA_TIER


---

## 🗄️ CT‑DB01 — Database Server (DATA_TIER)

**Role:** Ubuntu‑based backend data storage**

### 🔧 Configuration
- Runs on **Ubuntu Server**
- Hosts PostgreSQL, MySQL, or another enterprise DB engine
- Accessible only from the APP_TIER (CT‑SVR01)

### 🎯 Purpose in Lab
- Represents the “crown jewels” of the environment
- Enables realistic data exfiltration and privilege escalation scenarios
- Demonstrates secure tier‑to‑tier communication

### ⚠️ Security Considerations
- Databases are high‑value targets
- Weak service accounts or open ports can lead to full compromise
- Segmentation is critical to protect sensitive data


---

## 🔥 Adversary Tier — TB‑KALI01 (ATTACK_NET)

### **TB‑KALI01 — Thunderbyte Attacker System**

A fully isolated attacker network used for:

- Enumeration  
- Credential attacks  
- Lateral movement  
- Privilege escalation  
- Multi‑tier exploitation
  
---


## 🛡️ Security Controls Implemented

This environment includes multiple defensive controls to mirror real enterprise security posture:

- **Network Segmentation:** pfSense enforces strict boundaries between LAN, Server, App, Data, and Attacker tiers.
- **Least Privilege Access:** Tier‑based access rules restrict east‑west movement.
- **Identity Hardening:** Kerberos authentication, DNS‑based service discovery, and multi‑DC redundancy.
- **Logging & Visibility:** Splunk Enterprise collects logs from domain controllers, clients, and servers.
- **Endpoint Telemetry:** Sysmon and Windows Event Logging provide detailed process, authentication, and network visibility.
- **Service Isolation:** Web, application, and database services run on separate Ubuntu hosts to reduce blast radius.


---

## 🛠️ What I Built Myself

This lab was designed, deployed, and configured entirely by me, including:

- **Network Architecture Design:** Planned and implemented a fully segmented enterprise topology with five distinct network tiers.
- **pfSense Firewall Configuration:** Created VLANs, routing rules, NAT policies, and inter‑tier access controls.
- **Active Directory Deployment:** Installed and configured two domain controllers, DNS, Kerberos, GPOs, and domain replication.
- **Linux Server Build‑Out:** Deployed Ubuntu‑based web, application, and database servers with custom service configurations.
- **SIEM Integration:** Installed Splunk, configured forwarders, and built detection‑focused dashboards.
- **Adversarial Simulation Setup:** Configured a dedicated attacker network with Kali Linux and integrated offensive tooling.
- **Documentation & Diagramming:** Authored all architecture documentation, diagrams, and environment summaries for clarity and recruiter readability.

---

## 🧱 Segmentation & Traffic Flow

The pfSense firewall (CT‑FW01) enforces strict boundaries between all tiers:

- LAN → Server → App → Data  
- Attacker → LAN (controlled exposure)  
- App → Data (restricted)  

This models real enterprise segmentation strategies and forces attackers to navigate controlled pathways.

---

## 📝 Key Takeaways

- This lab is **not** a flat home lab — it’s a **tiered enterprise simulation**.  
- It demonstrates my understanding of **identity security**, **network segmentation**, and **adversarial operations**.  


---

<div align="center">

**Cloud Tech vs Thunderbyte — where enterprise security meets hands‑on adversarial engineering.**

</div>
