

<div align="center">

# 🌩️ CloudTech Case 001 — *“The Domain That Couldn’t Find Itself”*

### Diagnosing and Stabilizing a Tier‑0 Identity Failure in Active Directory
</div>

<div align="center">

## ⚡ TL;DR
A new AD environment looked healthy but behaved unpredictably. Clients intermittently failed to join the domain, DNS returned inconsistent answers, and SRV records were missing. The root cause was a layered misconfiguration: incorrect DNS settings, multi‑homed NIC instability, missing SRV records, and stale AD objects. By validating fundamentals, correcting DNS, simplifying NICs, and restoring SRV registration, I stabilized the entire identity infrastructure.

</div>



<p align="center"> <img width="700" alt="Case001Upgrade" src="https://github.com/user-attachments/assets/6a0ee9ca-6fac-406c-a686-299b916f69c2" />
  </p>

---

## ⭐ Executive Summary
This case simulates a real Tier‑0 identity failure where nothing was “broken enough” to raise alarms — yet everything was unstable. The domain controller appeared healthy, but authentication, domain joins, and service discovery were unreliable.

My structured troubleshooting approach focused on fundamentals first:

- Validate NIC configuration and routing  
- Correct DNS alignment  
- Restore SRV record registration  
- Clean up stale AD objects  
- Rebuild predictable identity behavior  

This case demonstrates Tier‑0 troubleshooting discipline, identity engineering fundamentals, and the ability to stabilize a fragile AD environment.

---


## 🏗️ Environment Snapshot
- **Windows Server 2019/2022** (AD DS, DNS)  
- **Windows 10/11 Client**  
- **Kali Linux** (attacker recon simulation)  
- Tools: `ipconfig`, `nslookup`, `ping`, DNS Manager, ADUC, `nmap`


---


## ❌ What Broke (Root Cause)
The instability was caused by the *interaction* of several misconfigurations:

- **Incorrect DNS settings** on clients and the DC  
- **Missing SRV records** preventing domain controller discovery  
- **Multiple active NICs** causing routing ambiguity and DNS registration conflicts  
- **Stale AD computer objects** causing trust failures  

These combined to create an identity environment that was unpredictable and unstable.


---


## 🔍 How I Investigated (Layered Troubleshooting)

### **1. NIC & Routing Validation**
- Multiple active adapters  
- APIPA addresses present  
- Conflicting routes  
- Multi‑homed DC registering incorrect DNS data  

### **2. DNS Alignment Check**
- Clients pointed to the wrong DNS  
- DC not registering its own records  
- External DNS configured on the DC (breaking SRV registration)

### **3. SRV Record Verification**
Queries such as:

nslookup -type=SRV _ldap._tcp.dc._msdcs.cloudtech.local

returned inconsistent or empty results.

### **4. AD Object Review**
- Stale computer accounts  
- Conflicting join attempts  
- Trust relationship failures  

This confirmed the issue was foundational — not replication, not firewalls, not advanced networking.

---

## 🛠️ How I Fixed It (Remediation Steps)

### **1. Simplified NICs**
- Disabled unused adapters  
- Ensured a single static IP  
- Removed conflicting routes  

### **2. Corrected DNS Configuration**
- Clients → Domain Controller IP  
- DC → Itself (no external DNS)  

### **3. Forced DNS + SRV Re‑Registration**
net stop netlogon

net start netlogon

ipconfig /registerdns

### **4. Repaired DNS Zone Structure**
- Removed incorrect folders  
- Verified `_msdcs` hierarchy  
- Confirmed SRV records repopulated correctly  

### **5. Cleaned Active Directory**
- Deleted stale computer objects  
- Cleared trust conflicts  

### **6. Validated Success**
- Domain join succeeded  
- SRV queries returned correct results  
- DNS resolution stabilized  
- Kerberos, LDAP, and GPO operations normalized

---
## ⚡ Attacker POV 
A passive attacker performing low‑impact reconnaissance would observe:

- Inconsistent DNS responses  
- Missing SRV records  
- Filtered or unpredictable ports  
- Multi‑homed hosts revealing segmentation gaps  

These signals indicate misconfiguration — not strong security.  
Unpredictability = opportunity.

---

### 🔍 Network Scan (Attacker View)
Ports appear filtered or inconsistent — a classic sign of routing confusion or DNS instability rather than intentional firewalling.

<img width="1273" height="193" alt="nmapscanattackermachine" src="https://github.com/user-attachments/assets/b500f9cd-d45d-49a1-8cbb-4af8bca49896" />

---

### 🔍 Domain Discovery Attempt
Basic domain resolution (`nslookup cloudtech.local`) fails or returns inconsistent answers, revealing foundational DNS instability.

<img width="714" height="124" alt="nslookupattackermachine" src="https://github.com/user-attachments/assets/a1965043-9821-4b70-8331-f5eb250e0b72" />

---

### 🔍 SRV Record Enumeration
SRV queries for `_ldap._tcp.dc._msdcs.cloudtech.local` fail or return empty results — a clear indicator that the domain controller is not registering critical locator records.

<img width="600" height="147" alt="failedsrvlookupattacker" src="https://github.com/user-attachments/assets/945f4803-7702-46ee-97a8-b36c9d091e2d" />

---

### ⚡ Attacker Conclusion
The environment appears misconfigured and unstable — not hardened.  
To an attacker, this signals weak configuration baselines and inconsistent identity services.

---

## 🛡️ Defender POV 
From a defender’s perspective, the instability revealed foundational identity issues rather than security controls. Each symptom pointed to gaps in configuration hygiene, monitoring, and Tier‑0 reliability.

Defenders would immediately recognize:

- DNS instability = identity instability  
- Missing SRV records = AD health issues  
- Multi‑NIC systems require intentional design  
- Stale AD objects signal weak lifecycle management  
- Inconsistent domain joins indicate architectural drift  

Predictability is security.

---

### 🔑 DNS Misconfiguration
Clients were pointed to the wrong DNS server, and the domain controller was not properly registering its own records — breaking service discovery and authentication.

<img width="395" height="449" alt="IncorrectDNSSettingsDefender" src="https://github.com/user-attachments/assets/b6a44c64-7045-4af7-956a-62cf978c1192" />

---

### 🔑 Missing SRV Records
Critical SRV records required for domain controller discovery were missing or not resolving, confirming DNS registration failures.

<img width="670" height="257" alt="nslookupsrvquerydefender" src="https://github.com/user-attachments/assets/223da870-df9f-4484-a117-430a95c7174a" />

---

### 🔑 Multi‑Homed Domain Controller
Multiple active NICs, APIPA addresses, and conflicting routes caused unpredictable DNS registration and routing behavior.

**Key Principle:**  
Multiple NICs without intentional design create instability — not redundancy.

<img width="780"  alt="multipleadaptersdefender" src="https://github.com/user-attachments/assets/e2fa212c-3565-4f63-b125-19ec47b2a39b" />

---

### 🔑 Stale AD Objects
Old computer accounts remained in Active Directory, causing trust failures and join conflicts.

<img width="752"  alt="staleadobjectsdefender" src="https://github.com/user-attachments/assets/79380d50-8508-4549-8536-6f22bd4eb50d" />

---
## 📊 Impact
- Reduced domain join failures from **~50% → 0%**  
- Restored **100% SRV record coverage**  
- Stabilized Kerberos, LDAP, and GPO operations  
- Eliminated routing conflicts and DNS instability  
- Rebuilt a predictable Tier‑0 identity foundation

---

## 🧠 Skills Demonstrated
- Active Directory troubleshooting & recovery  
- DNS architecture & SRV record management  
- Identity infrastructure stabilization  
- Network configuration & routing analysis  
- Root cause analysis of multi-factor failures  
- Offensive security awareness (attacker recon mindset)  
- Clear technical documentation & incident reporting  

---

## 🎯 Why This Matters (Recruiter-Facing)
This case demonstrates capabilities directly relevant to:

- **Cloud Security Engineer**  
- **Cloud Support Engineer (Azure)**  
- **IAM Analyst**  
- **DoD contractor roles**  

It shows:

- Tier‑0 troubleshooting discipline  
- Identity dependency awareness  
- Ability to stabilize misconfigured environments  
- Strong documentation and communication skills  


---


## 🚀 Final Takeaway
Misconfiguration — not exploitation — is often the real threat.  
By validating fundamentals, restoring DNS integrity, and rebuilding SRV registration, I transformed a fragile AD environment into a stable, predictable identity system.





