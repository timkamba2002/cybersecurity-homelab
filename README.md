# 🔐 Cybersecurity Home Lab

Personal cybersecurity home lab built with VirtualBox to practice offensive security, defensive monitoring, and network security operations in a controlled, isolated environment.

## 📋 Overview

This repository documents my hands-on cybersecurity lab designed to simulate real-world attack and defense scenarios. The lab demonstrates both red team (offensive) and blue team (defensive) capabilities using industry-standard tools and techniques.

**Repository:** [github.com/timkamba2002/cybersecurity-homelab](https://github.com/timkamba2002/cybersecurity-homelab)

---

## 🏗️ Lab Architecture

### **Environment Specifications**
- **Host OS:** Windows 11 (Physical Machine)
- **Hypervisor:** Oracle VirtualBox
- **Network Configuration:** Isolated internal network (VulnLabs - 172.30.1.0/24)

### **Virtual Machines**

| Machine | OS | IP Address | Role |
|---------|----|-----------| -----|
| Kali Linux | Kali Linux 2025.4 | 172.30.1.23 | Attack Platform |
| Metasploitable 2 | Ubuntu Linux | 172.30.1.24 | Vulnerable Target |
| Metasploitable 3 | Windows Server 2008 | 172.30.1.25 | Vulnerable Target |
| Snort IDS | Ubuntu 24.04 LTS | 172.30.1.21 | Intrusion Detection |

### **Network Topology**
```
┌─────────────────────────────────────────────────────────────────┐
│                        Host Machine                              │
│                     Windows 11 (Physical)                        │
│                                                                  │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │              Oracle VirtualBox Hypervisor                   │ │
│  │                                                             │ │
│  │  ┌──────────────────────────────────────────────────────┐  │ │
│  │  │           Internal Network: VulnLabs                  │  │ │
│  │  │              (172.30.1.0/24)                          │  │ │
│  │  │                                                        │  │ │
│  │  │   ┌─────────────┐      ┌──────────────┐              │  │ │
│  │  │   │ Kali Linux  │      │ Snort IDS    │              │  │ │
│  │  │   │ (Attacker)  │      │ (Monitoring) │              │  │ │
│  │  │   │ 172.30.1.23 │      │ 172.30.1.21  │              │  │ │
│  │  │   └──────┬──────┘      └──────┬───────┘              │  │ │
│  │  │          │                    │                       │  │ │
│  │  │          └────────┬───────────┘                       │  │ │
│  │  │                   │                                   │  │ │
│  │  │          ┌────────┴─────────┐                         │  │ │
│  │  │          │                  │                         │  │ │
│  │  │   ┌──────▼──────┐    ┌──────▼──────┐                 │  │ │
│  │  │   │Metasploitable│    │Metasploitable│                │  │ │
│  │  │   │      2       │    │      3       │                │  │ │
│  │  │   │   (Linux)    │    │  (Win 2008)  │                │  │ │
│  │  │   │ 172.30.1.24  │    │ 172.30.1.25  │                │  │ │
│  │  │   └──────────────┘    └──────────────┘                │  │ │
│  │  │                                                        │  │ │
│  │  └────────────────────────────────────────────────────────┘  │ │
│  │                                                               │ │
│  └───────────────────────────────────────────────────────────────┘ │
│                                                                    │
└────────────────────────────────────────────────────────────────────┘
```

---

## 🎯 Lab Objectives

### **Offensive Security (Red Team)**
- Network reconnaissance and service enumeration
- Web application vulnerability assessment
- Exploitation of known vulnerabilities
- Remote code execution and privilege escalation
- Post-exploitation analysis

### **Defensive Security (Blue Team)**
- Network intrusion detection and monitoring
- Custom rule development for attack signatures
- Alert generation and log analysis
- Attack pattern recognition
- Incident response simulation

---

## 🛠️ Tools & Technologies

### **Offensive Tools**
- **Nmap** - Network scanning and service enumeration
- **Metasploit Framework** - Exploitation and payload delivery
- **Gobuster** - Web directory enumeration
- **WhatWeb** - Web application fingerprinting
- **Netcat** - Network communication and reverse shells
- **Burp Suite** - Web application security testing

### **Defensive Tools**
- **Snort IDS** - Network intrusion detection system
- **Wireshark** - Packet capture and analysis
- **tcpdump** - Command-line packet analyzer

### **Operating Systems**
- Kali Linux 2025.4
- Ubuntu 24.04 LTS
- Windows Server 2008 (Metasploitable 3)

---

## 📚 Completed Labs

### **1. Metasploitable 2 - Enumeration & Exploitation**
**Status:** ✅ Complete

**Activities:**
- Full network reconnaissance with Nmap
- Web application enumeration (Gobuster, WhatWeb)
- WebDAV misconfiguration exploitation
- Remote code execution as www-data user
- Documented attack methodology with evidence

**Documentation:** [`labs/metasploitable2/`](./labs/metasploitable2/)

---

### **2. Snort IDS - Network Intrusion Detection**
**Status:** ✅ Complete

**Activities:**
- Deployed Snort IDS on Ubuntu 24.04
- Configured custom detection rules (ICMP, large packets, port scans)
- Simulated attacks from Kali Linux
- Validated real-time alert generation
- Analyzed logs and packet captures

**Documentation:** [`labs/snort-ids/`](./labs/snort-ids/)

---

## 📈 Skills Demonstrated

**Technical Skills:**
- Network scanning & enumeration
- Vulnerability assessment
- Exploitation techniques
- Intrusion detection systems (IDS)
- Custom security rule development
- Log analysis & interpretation
- Network traffic analysis
- Linux system administration
- Technical documentation

**Security Concepts:**
- Attack lifecycle (reconnaissance → exploitation → post-exploitation)
- Defense-in-depth
- Network segmentation
- Signature-based detection
- Blue team vs. Red team operations
- SOC analyst workflows

---

## 📂 Repository Structure
```
cybersecurity-homelab/
├── README.md
├── LICENSE
├── labs/
│   ├── metasploitable2/
│   │   ├── notes/
│   │   │   ├── metasploitable2-nmap-enumeration.md
│   │   │   ├── metasploitable2-web-enumeration.md
│   │   │   └── metasploitable2-webdav-enumeration.md
│   │   └── screenshots/
│   └── snort-ids/
│       ├── notes/
│       │   └── snort-installation-and-detection.md
│       └── screenshots/
└── assets/
    └── network-diagram.png
```

---

## ⚠️ Disclaimer

**Legal Notice:** All activities documented in this repository are performed in a completely isolated, virtualized lab environment using intentionally vulnerable machines designed for educational purposes. This lab is for **authorized security research and training only**.

**Do not** attempt these techniques against systems you do not own or have explicit written permission to test.

---

## 📞 Contact

**Timothy Kamba**  
Aspiring SOC Analyst | Cybersecurity Student  
📧 timothykumba2002@gmail.com  
🔗 [LinkedIn](https://linkedin.com/in/timothy-kamba-it202502)  
💻 [GitHub](https://github.com/timkamba2002)

---

**Last Updated:** January 30, 2026
