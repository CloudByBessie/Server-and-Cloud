# 🌐 Network Architecture — Cloud Tech vs Thunderbyte Lab

---

## 📘 Overview

This document provides a detailed breakdown of the network architecture used in the **Cloud Tech vs Thunderbyte cybersecurity lab**.

The environment simulates a **real-world enterprise Active Directory network** under attack conditions, allowing for both offensive (Red Team) and defensive (Blue Team) operations.

---

## 🎯 Threat Model

This lab simulates a scenario where an attacker has gained network access and is attempting to compromise the domain.

### Assumed Attacker Capabilities:
- Network-level access to internal systems
- Ability to scan and enumerate services
- No initial domain credentials (pre-compromise phase)

### Target Assets:
- Domain Controller (CT-DC01)
- Member Server (CT-SVR01)
- Domain user credentials
- Client workstation (CT-CL01)

### Attack Goals:
- Gain valid credentials
- Escalate privileges to domain admin
- Maintain persistence within the environment

---

## 🏗️ Architecture Design Decisions

This lab environment was intentionally designed to reflect a simplified enterprise network while maintaining clarity for learning and demonstration.

### 🔹 Single Domain Model
A single Active Directory domain was used to:
- Simplify identity management
- Focus on core AD attack paths
- Reduce unnecessary complexity for initial testing

### 🔹 Flat Network (192.168.1.0/24)
A flat subnet was chosen to:
- Allow unrestricted communication between systems
- Simulate common small/medium business environments
- Enable easier enumeration from an attacker perspective

### 🔹 Dedicated Attacker Machine
The Kali Linux system is separated logically to:
- Represent an external or compromised internal host
- Perform realistic offensive security operations

### 🔹 Domain-Joined Client
The client machine exists to:
- Simulate a real user endpoint
- Serve as an entry point for lateral movement scenarios

### 🔹 Member Server (CT-SVR01)
A dedicated member server was added to:
- Simulate real enterprise infrastructure (file server, application host, etc.)
- Introduce additional attack paths beyond the client
- Enable lateral movement and service-based exploitation scenarios

---

## 🖼️ Network Diagram

<p align="center">
  
<img width="600"  alt="Cybersecurity lab network diagram" src="https://github.com/user-attachments/assets/b0c1d6d5-8911-4e0c-8fd5-88c4dbf384c5" />


</p>

<em>Figure: Four-node lab environment including domain controller, member server, client workstation, and attacker system.</em>

---

## 🧩 Environment Summary

| Component     | Hostname   | Role                      | IP Address     | Operating System        |
|--------------|-----------|---------------------------|----------------|------------------------|
| Domain Ctrl  | CT-DC01   | Active Directory + DNS    | 192.168.1.10   | Windows Server         |
| Member Server| CT-SVR01  | Domain-Joined Server      | 192.168.1.15   | Windows Server         |
| Client       | CT-CL01   | Domain-Joined Workstation | 192.168.1.20   | Windows 10/11          |
| Attacker     | TB-Kali01 | Red Team / Offensive Ops  | 192.168.1.30   | Kali Linux             |
---

## 🛡️ CT-DC01 — Domain Controller (Blue Team Core)

**Role:** Central identity and authentication authority

### 🔧 Services Running
- Active Directory Domain Services (AD DS)
- DNS Server (Port 53)
- Kerberos Authentication (Port 88 — Ticket Granting System)
- LDAP Directory Services (Port 389)

## 🖥️ CT-SVR01 — Member Server

**Role:** Domain-joined infrastructure server

### 🔧 Typical Use Cases
- File server (SMB shares)
- Application hosting
- Service account usage

### 🎯 Purpose in Lab
- Provides an additional target beyond the client
- Enables more realistic lateral movement scenarios
- Simulates common enterprise server roles

### ⚠️ Security Considerations
Member servers are often:
- Trusted by the domain but less hardened than DCs
- Running services with elevated privileges
- A key pivot point for attackers moving toward the Domain Controller

### 🔍 Protocol Breakdown

- **DNS (Port 53)**  
  Resolves domain names (e.g., `company.local`) to IP addresses.  
  In an Active Directory environment, DNS is critical for locating domain controllers and services.

- **Kerberos (Port 88)**  
  Primary authentication protocol used by Active Directory.  
  It uses ticket-based authentication to securely verify user identities without transmitting passwords over the network.

- **LDAP (Port 389)**  
  Used to query and interact with the Active Directory database.  
  Enables systems and users to retrieve information such as user accounts, groups, and domain structure.

- **AD DS (Active Directory Domain Services)**  
  Core service that stores identity and access management data.  
  Works in conjunction with DNS, Kerberos, and LDAP to provide centralized authentication and authorization.

### 🎯 Responsibilities
- Authenticate all domain users and machines
- Manage Group Policy Objects (GPOs)
- Provide DNS name resolution
- Store and manage directory objects

### 🔐 Security Importance
The Domain Controller is the **most critical asset in the network**.

If compromised:
- Full domain takeover is possible
- Credential dumping can expose all users
- Attackers can create persistence mechanisms

---

## 💻 CT-CL01 — Domain Client

**Role:** Standard enterprise workstation

### 🔧 Configuration
- Joined to the Active Directory domain
- Authenticates via Kerberos to CT-DC01
- Uses DNS to locate domain services

### 🎯 Purpose in Lab
- Simulates a real employee workstation
- Target for:
  - Credential harvesting
  - Privilege escalation
  - Lateral movement

### ⚠️ Security Considerations
Client machines are often:
- The **initial entry point** for attackers
- Misconfigured or under-monitored
- Used to pivot deeper into the network

---

## ⚡ TB-Kali01 — Attacker Machine (Thunderbyte)

**Role:** Offensive Security Platform (Red Team)

### 🔧 Tools & Capabilities
- Nmap (network scanning)
- BloodHound (AD enumeration)
- Mimikatz (credential dumping)
- Hydra / CrackMapExec (password attacks)

### 🎯 Objectives
- Discover domain infrastructure
- Enumerate users and services
- Exploit misconfigurations
- Escalate privileges
- Move laterally across systems

---

## 🌐 Network Configuration

| Setting        | Value              |
|----------------|-------------------|
| Network Type   | Internal / Private|
| Subnet         | 192.168.1.0/24    |
| Gateway        | N/A (isolated lab)|
| DNS Server     | 192.168.1.10      |

### 🔒 Isolation

This lab is intentionally isolated to:
- Prevent external exposure
- Allow safe attack simulation
- Maintain full control over traffic

---

## 🔁 Network Communication Flow

### 🧭 Authentication Flow
1. User logs into **CT-CL01**
2. Client sends authentication request to **CT-DC01 using Kerberos (TCP/UDP 88)**
3. Domain Controller validates credentials via Kerberos
4. Access is granted or denied

---

### 🌍 DNS Resolution Flow
1. Client queries DNS (CT-DC01) over **port 53 (DNS)**
2. DNS resolves domain resources (e.g., `company.local`)
3. Client connects to requested service

---

### ⚔️ Attack Flow (Simulated)

1. **TB-Kali01 scans network**
2. Identifies:
   - Domain Controller
   - Open ports/services (e.g., 88, 389, 445)
3. Performs enumeration using:
   - **LDAP (TCP/389)** for directory queries
   - **SMB (TCP/445)** for shares and lateral movement
   - Leveraging tools such as:
     - **Nmap** for network scanning
     - **BloodHound** for Active Directory enumeration
4. Executes attacks:
   - Password spraying
   - Credential dumping
5. Gains access to:
   - Client machine (CT-CL01)
   - Member server (CT-SVR01)
   - Potentially Domain Controller (CT-DC01)
---

## 🧪 Example Attack Scenario

1. Attacker performs Nmap scan to identify open ports (88, 389, 445)
2. Uses LDAP enumeration to identify domain users
3. Executes password spraying attack against discovered accounts
4. Gains access to CT-CL01
5. Dumps credentials using Mimikatz
6. Moves laterally to CT-SVR01 via SMB
7. Identifies service accounts or misconfigurations
8. Escalates privileges and targets CT-DC01
---

## 🔍 Detection Opportunities

This lab environment provides visibility into common attacker behaviors such as enumeration, credential access, and lateral movement within an Active Directory network.

By monitoring authentication activity, network communication, and system interactions, defenders can identify early indicators of compromise and respond before full domain takeover occurs.

The following sections highlight key behaviors, logs, and events that can be used to detect malicious activity.

### 🚨 Suspicious Activity to Monitor
- Excessive authentication attempts (password spraying)
- Unusual Kerberos ticket requests
- Lateral movement between hosts
- Enumeration of domain resources

### 📊 Key Windows Event IDs   
- Event ID 4625 — Failed logon attempts
- Event ID 4768 / 4769 — Kerberos activity

### 🛡️ Potential Logging Sources
- Windows Event Logs (Security Logs)
- Domain Controller authentication logs
- PowerShell logging (if enabled)

---

## 🎯 Lab Objectives

This environment is designed to practice:

### 🔴 Red Team Skills
- Active Directory enumeration
- Credential attacks
- Privilege escalation
- Lateral movement

### 🔵 Blue Team Skills
- Monitoring authentication activity
- Detecting suspicious behavior
- Hardening AD configurations
- Incident response

---

## ⚠️ Security Considerations

- This lab is for **educational use only**
- All attacks are performed in a **controlled environment**
- No connection to production or external networks

---

## 🚀 Future Improvements

To increase realism and complexity, future iterations of this lab may include:

- Additional domain-joined hosts
- Network segmentation (VLANs)
- SIEM integration for log analysis
- Endpoint detection and response (EDR) simulation
- Privileged access management scenarios
- Service account abuse scenarios (Kerberoasting)
- Member server misconfiguration exploitation

---

## 📁 File Structure Reference

    lab/
    ├── lab-setup.md
    ├── network-diagram.md
    ├── assets/
    │    └── network-diagram.jpg

---

## 🧠 Key Takeaway

This lab models a simplified enterprise Active Directory environment designed to replicate common attack paths and defensive challenges found in real-world networks, demonstrating how:

- Identity infrastructure (Active Directory) is central to security
- Misconfigurations can lead to full compromise
- Attackers move methodically through environments
- Defenders must monitor, detect, and respond effectively

---

🚀 *This is not just a lab — it’s a simulation of real-world cyber warfare.*
