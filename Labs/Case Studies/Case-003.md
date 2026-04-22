<div align="center">

# 🌩️ CloudTech Case 003  
## *“When DNS Becomes a Backdoor”*  
### Covert Data Movement Hidden Inside Trusted DNS Traffic  

---

## ⚡ TL;DR  
An attacker on a segmented network generated **anomalous DNS queries** that bypassed firewall restrictions because DNS was allowed for operational needs. These queries contained **encoded subdomains**, enabling covert data movement through a trusted protocol. Detection was achieved using Windows DNS debug logs and Splunk, which flagged unusual query lengths, patterns, and domain structures.

<img width="700"  alt="case003" src="https://github.com/user-attachments/assets/f25532c8-d887-43e7-91e5-f79c31f9cbec" />


</div>


---

<details>
  <summary><strong>📚 Quick Navigation</strong></summary>

- [Executive Summary](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-003.md#-executive-summary)
- [Environment Snapshot](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-003.md#%EF%B8%8F-environment-snapshot)
- [Tools & Technologies Used](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-003.md#%EF%B8%8F-tools--technologies-used)
- [What Broke (Root Cause)](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-003.md#-what-broke-root-cause)
- [How I Investigated (Layered Troubleshooting)](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-003.md#-how-i-investigated-layered-troubleshooting)
- [How I Fixed It (Remediation Steps)](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-003.md#%EF%B8%8F-how-i-fixed-it-remediation-steps)
- [Attacker POV](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-003.md#-attacker-pov)
- [Defender POV](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-003.md#%EF%B8%8F-defender-pov)
- [Impact](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-003.md#-impact)
- [Skills Demonstrated](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-003.md#-skills-demonstrated)
- [Why This Matters](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-003.md#-why-this-matters)
- [Final Takeaway](https://github.com/CloudByBessie/Server-and-Cloud/blob/main/Labs/Case%20Studies/Case-003.md#-final-takeaway)

</details>


---

## ⭐ Executive Summary
CloudTech’s network segmentation relied heavily on pfSense firewall rules that blocked most outbound traffic — except DNS. ThunderByte (attacker) leveraged this gap by generating DNS queries with **encoded subdomains**, simulating covert data movement through a protocol the firewall inherently trusted.

On the defensive side, DNS debug logging was enabled on CT‑DC01 and ingested into Splunk. Through log analysis, I identified:

- Unusually long DNS labels  
- Repetitive query patterns  
- Queries targeting a custom domain (`tunnel.lab`)  
- Encoded subdomain structures  

This case demonstrates how attackers can exploit trusted infrastructure protocols and how defenders can detect subtle anomalies hidden inside legitimate traffic.

---

## 🏗️ Environment Snapshot
- **Attacker:** TB‑KALI01  
- **Firewall:** CT‑FW01 (pfSense)  
- **DNS Server:** CT‑DC01  
- **SIEM:** Splunk  
- **Network:** Segmented, DNS allowed outbound  

### Network Configuration  
<img width="700"  alt="01-kali-ip-routing(1)" src="https://github.com/user-attachments/assets/f83525f8-82ec-4fee-9bc6-a62369040171" />


### Firewall Rules  
<img width="500"  alt="02-pfsense-dns-rule" src="https://github.com/user-attachments/assets/cc257467-0bda-4aea-ae32-e359bc2afb95" />


### DNS Zone Setup  
<img width="500"  alt="05-dc01-zone-created png" src="https://github.com/user-attachments/assets/c7574c8b-c160-4785-a68c-2555bf4e05e5" />


### DNS Logging Enabled  
<img width="395"  alt="06-dc01-dns-logging-enabled png" src="https://github.com/user-attachments/assets/cdfb1875-1528-40ff-8a71-e04db7add8d3" />


---

## 🛠️ Tools & Technologies Used
- **Kali Linux** — attacker simulation  
- **pfSense Firewall** — segmentation and packet capture  
- **Windows Server (DNS Role)** — DNS logging and analysis  
- **Splunk SIEM** — detection and correlation  
- **PowerShell** — log handling and validation  
- **DNS utilities** — generating DNS queries for testing  

---

## ❌ What Broke (Root Cause)
The covert channel succeeded because of three systemic weaknesses:

- **DNS was allowed through the firewall without inspection**  
- **No monitoring existed for DNS query structure or behavior**  
- **No anomaly‑based detection for DNS traffic**  

This created a blind spot where encoded DNS queries could move freely.

---

## 🔍 How I Investigated (Layered Troubleshooting)

### **1. Validated Attacker Network Path**  
<img width="700" alt="01-kali-ip-routing(1)" src="https://github.com/user-attachments/assets/29564bc4-da4a-428d-b10a-1d349b241e5b" />

Confirmed the attacker could only reach the DNS server — all other traffic was blocked.

### **2. Reviewed Firewall Rules**  
<img width="500"  alt="03-pfsense-block-rule" src="https://github.com/user-attachments/assets/009afc93-d136-4059-a9a6-a426806fe046" />
 
pfSense allowed DNS (UDP/53) outbound, blocking everything else.

### **3. Verified DNS Zone and Logging**  
<img width="700" alt="05-dc01-zone-created png" src="https://github.com/user-attachments/assets/01d2286f-37c2-4dda-bf13-2560e851fc99" />
 
<img width="395" alt="06-dc01-dns-logging-enabled png" src="https://github.com/user-attachments/assets/1828f7fd-5017-4afa-91a3-e9b40daaf707" />
 
A custom zone (`tunnel.lab`) was created for controlled testing, and DNS debug logging was enabled.

### **4. Observed DNS Query Behavior**  
<img width="585" height="395" alt="07-kali-dns-test-scuess" src="https://github.com/user-attachments/assets/64382637-6865-427f-bb01-2ba1c1ee2edb" />

  
Initial DNS queries confirmed connectivity.

### **5. Detected Anomalous DNS Patterns**  
<img width="512" height="134" alt="08-kali-dns-exfil-running(1)" src="https://github.com/user-attachments/assets/ff2b00ee-d507-4f76-b3a0-6db6c3ab7680" />

 
The attacker generated DNS queries containing **encoded subdomains**, producing long, repetitive, and unusual label structures.

### **6. Analyzed DNS Logs**  
<img width="500" alt="09-dc01-dns-log-anomaly(1)" src="https://github.com/user-attachments/assets/31e6cf24-e626-4898-9051-a2469d8742a3" />
  
Logs showed patterns such as:  
- Long subdomain labels  
- Repetitive encoded sequences  
- Queries targeting `tunnel.lab`  

### **7. Correlated in Splunk**  
<img width="500"  alt="10-splunk-dns-anomaly-search" src="https://github.com/user-attachments/assets/b41ecea3-4f7b-4f48-b2c7-3c0a356052a4" />
  
Splunk detections flagged:  
- Abnormal query lengths  
- Suspicious domain usage  
- Repetitive encoded patterns  

### **8. Validated Firewall Bypass**  
<img width="500"  alt="11-pfsense-packet-capture" src="https://github.com/user-attachments/assets/4891d361-448c-4949-b255-9396a527b04f" />

Packet capture confirmed DNS traffic crossing the firewall as expected.

---

## 🛠️ How I Fixed It (Remediation Steps)

### **1. Enable DNS Logging Everywhere**
- DNS debug logging enabled on all DNS servers  
- Logs forwarded to Splunk  

### **2. Build DNS Anomaly Detections**
- Alerts for long DNS labels  
- Alerts for repetitive encoded patterns  
- Alerts for unusual domain usage  

### **3. Restrict DNS Traffic**
- Only approved resolvers allowed  
- Block outbound DNS to unauthorized servers  

### **4. Implement DNS Filtering**
- Domain reputation checks  
- Block newly registered or suspicious domains  

### **5. Deploy DNS Anomaly Detection Tools**
- Behavioral analytics  
- Threat intelligence integration  

---

## ⚡ Attacker POV 
From the attacker’s perspective:

- DNS was the only protocol allowed outbound  
- Generating DNS queries with **encoded subdomains** allowed covert movement of information  
- The traffic blended with legitimate DNS activity  
- The firewall trusted DNS and did not inspect query content  

The attacker relied on the assumption that **DNS is rarely monitored**.

---

## 🛡️ Defender POV 
From the defender’s perspective:

- DNS logs revealed **unusually long and repetitive query patterns**  
- Queries targeted a domain not used by any legitimate service  
- Encoded subdomains stood out from normal DNS behavior  
- Splunk correlation tied together DNS logs, firewall logs, and packet captures  

The defender’s visibility turned a trusted protocol into a detectable signal.

---

## 📊 Impact
- **Firewall Bypass:** 100% success via DNS  
- **Detection Latency:** Near real‑time in Splunk  
- **Query Volume:** ~20–50 anomalous DNS queries per session  
- **Detection Fidelity:** High — based on structure, not content  

---

## 🧠 Skills Demonstrated
- DNS protocol analysis  
- Firewall segmentation validation  
- SIEM detection engineering  
- Log correlation across multiple systems  
- Threat hunting for covert channels  
- Defensive monitoring strategy  

---

## 🎯 Why This Matters
DNS is one of the most trusted and least monitored protocols in enterprise environments. This case demonstrates the ability to:

- Identify covert channels hidden inside legitimate traffic  
- Build high‑fidelity detections based on behavior  
- Strengthen segmentation by monitoring allowed protocols  
- Correlate attacker activity across DNS, firewall, and SIEM  

This is core to modern detection engineering and blue team operations.

---

## 🚀 Final Takeaway
> **If DNS is trusted but not monitored, it becomes an invisible tunnel for attackers.**  
Visibility turns a blind spot into a defensive advantage.
