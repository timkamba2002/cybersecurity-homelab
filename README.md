# 🔐 Cybersecurity Home Lab

Personal cybersecurity home lab built with VirtualBox to practice offensive security, defensive monitoring, and network security operations in a controlled, isolated environment.

---

## 📋 Overview

This repository documents my hands-on cybersecurity lab designed to simulate real-world attack and defense scenarios. The lab demonstrates both red team (offensive) and blue team (defensive) capabilities using industry-standard tools and techniques.

**Repository:** [github.com/timkamba2002/cybersecurity-homelab](https://github.com/timkamba2002/cybersecurity-homelab)

---

## 🏗️ Current Lab Architecture

### pfSense Managed Network (Active)

**Environment:**
- **Host OS:** Windows 11 (Physical Machine)
- **Hypervisor:** Oracle VirtualBox
- **Firewall/Router:** pfSense 2.8.1-RELEASE with Snort IPS
- **Network:** 172.30.2.0/24 (pfSense LAN)

**Virtual Machines:**

| Machine | OS | IP Address | Role |
|---------|----|-----------| -----|
| pfSense | pfSense CE 2.8.1 | WAN: 192.168.1.197<br>LAN: 172.30.2.1 | Firewall/Router + IPS |
| Kali Linux | Kali Linux 2025.4 | 172.30.2.100 | Attack Platform |
| Metasploitable 2 | Ubuntu Linux | 172.30.2.101 | Vulnerable Target |
| Metasploitable 3 | Windows Server 2008 | 172.30.2.102 | Vulnerable Target |

**Network Topology:**
```
Home Network (192.168.1.0/24)
    |
    | WAN: 192.168.1.197
    |
┌───▼──────────────────────────────┐
│   pfSense Firewall/Router        │
│   with Snort IPS (Inline Mode)   │
└───┬──────────────────────────────┘
    | LAN: 172.30.2.1
    |
    | Internal Lab (172.30.2.0/24)
    |
    ├── Kali Linux (.100)
    ├── Metasploitable 2 (.101)
    └── Metasploitable 3 (.102)
```

---

### Legacy Network (Reference)

**Previous Configuration:**
- Internal Network: VulnLabs (172.30.1.0/24)
- Snort IDS: Ubuntu 24.04 (172.30.1.21) - Passive monitoring only
- Basic VirtualBox internal networking

**Migration:** Lab transitioned to pfSense-managed network for enterprise-grade security controls and inline IPS capabilities.

---

## 🎯 Lab Objectives

### Offensive Security (Red Team)
- Network reconnaissance and service enumeration
- Web application vulnerability assessment
- Exploitation of known vulnerabilities
- Remote code execution and privilege escalation
- Post-exploitation analysis

### Defensive Security (Blue Team)
- Network intrusion detection and prevention
- Firewall policy management
- Custom rule development for attack signatures
- Real-time threat blocking (IPS)
- Alert generation and log analysis
- Incident response simulation

---

## 🛠️ Tools & Technologies

### Offensive Tools
- **Nmap** - Network scanning and service enumeration
- **Metasploit Framework** - Exploitation and payload delivery
- **Gobuster** - Web directory enumeration
- **WhatWeb** - Web application fingerprinting
- **Netcat** - Network communication and reverse shells
- **Burp Suite** - Web application security testing

### Defensive Tools
- **pfSense** - Enterprise firewall/router platform
- **Snort IPS** - Intrusion prevention system (inline mode)
- **Snort IDS** - Intrusion detection system (passive mode)
- **Wireshark** - Packet capture and analysis
- **tcpdump** - Command-line packet analyzer

### Operating Systems
- pfSense CE 2.8.1
- Kali Linux 2025.4
- Ubuntu 24.04 LTS
- Windows Server 2008 (Metasploitable 3)

---

## 📚 Completed Labs

### 1. Metasploitable 2 - Enumeration & Exploitation
**Status:** ✅ Complete

**Activities:**
- Full network reconnaissance with Nmap
- Web application enumeration (Gobuster, WhatWeb)
- WebDAV misconfiguration exploitation
- Remote code execution as www-data user
- Documented attack methodology with evidence

**Documentation:** [`labs/metasploitable2/`](./labs/metasploitable2/)

---

### 2. Snort IDS - Network Intrusion Detection
**Status:** ✅ Complete

**Activities:**
- Deployed Snort IDS on Ubuntu 24.04
- Configured custom detection rules (ICMP, large packets, port scans)
- Simulated attacks from Kali Linux
- Validated real-time alert generation
- Analyzed logs and packet captures

**Documentation:** [`labs/snort-ids/`](./labs/snort-ids/)

---

### 3. pfSense Firewall with Snort IPS - Active Prevention
**Status:** ✅ Complete

**Activities:**
- Deployed pfSense as centralized firewall/router
- Migrated lab to enterprise network architecture
- Integrated Snort in inline IPS mode (active blocking)
- Created custom detection rules (ICMP, SSH)
- Configured NAT port forwarding for external testing
- Implemented pass lists for authorized access
- Validated sub-second threat blocking

**Key Achievements:**
- Transitioned from passive detection (IDS) to active prevention (IPS)
- Demonstrated 75% packet blocking after signature match
- Successfully tuned security policies to eliminate false positives
- Maintained complete audit trail while blocking threats

**Documentation:** [`labs/pfsense-snort-ips/`](./labs/pfsense-snort-ips/)

---

## 📈 Skills Demonstrated

**Technical Skills:**
- Network scanning & enumeration
- Vulnerability assessment
- Exploitation techniques
- Intrusion detection & prevention systems
- Firewall configuration & policy management
- Custom security rule development
- NAT and port forwarding
- Log analysis & interpretation
- Network traffic analysis
- Security policy tuning (false positive reduction)
- Linux/BSD system administration
- Technical documentation

**Security Concepts:**
- Attack lifecycle (reconnaissance → exploitation → post-exploitation)
- Defense-in-depth architecture
- Network segmentation & isolation
- Signature-based detection
- Real-time threat prevention
- Blue team vs. Red team operations
- SOC analyst workflows
- Security visibility vs. prevention tradeoffs

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
│   ├── snort-ids/
│   │   ├── notes/
│   │   │   └── snort-installation-and-detection.md
│   │   └── screenshots/
│   └── pfsense-snort-ips/
│       ├── notes/
│       │   └── pfsense-snort-ips-deployment.md
│       └── screenshots/
└── assets/
    └── network-diagrams/
```

---

## ⚠️ Disclaimer

**Legal Notice:** All activities documented in this repository are performed in a completely isolated, virtualized lab environment using intentionally vulnerable machines designed for educational purposes. This lab is for **authorized security research and training only**.

**Do not** attempt these techniques against systems you do not own or have explicit written permission to test.

---

## 📞 Contact

**Timothy Kamba**  
SOC Analyst | Cybersecurity Student  
📧 timothykamba2002@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/timothy-kamba-1b3034328/)  
💻 [GitHub](https://github.com/timkamba2002)

---

**Last Updated:** February 2, 2026
