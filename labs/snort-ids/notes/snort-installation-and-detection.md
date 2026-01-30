# Snort IDS - Installation, Configuration & Detection Testing

## 🎯 Lab Overview

Successfully deployed Snort IDS on Ubuntu 24.04 LTS to detect malicious network traffic from a Kali Linux attacker machine. Configured custom detection rules for ICMP, large packets, and common network services.

---

## 🔧 Environment Setup

**Target System (IDS):**
- OS: Ubuntu 24.04.3 LTS
- Snort Version: 2.9.20 GRE (Build B2)
- Network Interface: enp0s8
- IP Address: 172.30.1.21

**Attacker System:**
- OS: Kali Linux 2025.4
- IP Address: 172.30.1.23

**Network Configuration:**
- Internal Lab Network: 172.30.1.0/24
- Isolated VirtualBox host-only network

---

## 📦 Installation Process

### Step 1: System Preparation
```bash
# Update package repositories
sudo apt-get update
```

### Step 2: Install Snort IDS
```bash
# Install Snort with automatic dependency resolution
sudo apt-get install snort -y
```

### Step 3: Configure Network Variables
```bash
# Edit Snort configuration file
sudo nano /etc/snort/snort.conf

# Modified HOME_NET variable
# Changed from: ipvar HOME_NET any
# Changed to:   ipvar HOME_NET 172.30.1.0/24
```

**Rationale:** Defining HOME_NET ensures Snort only monitors traffic destined for our lab network, reducing false positives.

### Step 4: Validate Configuration
```bash
# Test configuration for syntax errors
sudo snort -T -i enp0s8 -c /etc/snort/snort.conf
```

**Result:** Configuration validated successfully with no errors.

---

## 🚨 Custom Detection Rules

### Rule 1: Basic ICMP Detection

**Rule Definition:**
```bash
# Edit local rules file
sudo nano /etc/snort/rules/local.rules

# Add ICMP detection rule
alert icmp any any -> $HOME_NET any (msg:"ICMP Detected!!!";sid:1000001;rev:1;)
```

**Rule Breakdown:**
- `alert icmp` - Generate alert for ICMP protocol
- `any any` - From any source IP and port
- `-> $HOME_NET any` - To our defined home network, any port
- `msg:"ICMP Detected!!!"` - Alert message
- `sid:1000001` - Unique signature ID (custom rules start at 1000000)
- `rev:1` - Revision number for tracking rule updates

**Testing:**
```bash
# Start Snort in console alert mode
sudo snort -q -l /var/log/snort -i enp0s8 -A console -c /etc/snort/snort.conf

# From Kali Linux
ping 172.30.1.21
```

**Result:** Successfully detected ICMP echo requests with real-time console alerts.

---

### Rule 2: Large ICMP Packet Detection

**Rule Definition:**
```bash
alert icmp any any -> $HOME_NET any (msg:"LARGE ICMP DETECTED!!!!";dsize:>500;sid:1000002;rev:1;)
```

**Rule Breakdown:**
- `dsize:>500` - Detect ICMP packets larger than 500 bytes
- Useful for detecting ICMP tunneling or exfiltration attempts

**Testing:**
```bash
# From Kali Linux - send oversized ICMP packet
ping -s 501 172.30.1.21
```

**Result:** Rule successfully triggered on packets exceeding 500 bytes.

---

### Rule 3: Network Service Port Detection

**Rule Definitions:**
```bash
# FTP Detection
alert tcp any any -> $HOME_NET 21 (msg:"FTP Connection Detected";sid:1000003;rev:1;)

# SSH Detection
alert tcp any any -> $HOME_NET 22 (msg:"SSH Connection Detected";sid:1000004;rev:1;)

# HTTP Detection
alert tcp any any -> $HOME_NET 80 (msg:"HTTP Connection Detected";sid:1000005;rev:1;)
```

**Testing:**
```bash
# From Kali Linux

# TCP SYN Scan (stealth scan)
nmap -sS 172.30.1.21

# TCP Xmas Scan
nmap -sX 172.30.1.21

# TCP FIN Scan
nmap -sF 172.30.1.21

# HTTP request to admin page
curl http://172.30.1.21/adminphp
```

**Results:** All port-specific alerts triggered correctly during Nmap scanning and HTTP requests.
---

## 📊 Log Analysis

### Viewing Real-Time Alerts
```bash
# Console output mode (real-time)
sudo snort -q -l /var/log/snort -i enp0s8 -A console -c /etc/snort/snort.conf
```

### Analyzing Stored Logs
```bash
# List log files
sudo ls -l /var/log/snort

# Read binary log with verbose output
sudo snort -v -r /var/log/snort/snort.log.1769682734
```

### Alert File Contents
```bash
# View triggered alerts
sudo cat /var/log/snort/snort.alert
```

**Log Structure:**
- Timestamp
- Rule ID and priority
- Protocol and packet details
- Source → Destination IPs
- Ports and flags

---

## 🔍 Key Observations

### Detection Capabilities Demonstrated
1. ✅ **ICMP Traffic Monitoring** - Basic ping detection
2. ✅ **Packet Size Analysis** - Anomalous packet detection
3. ✅ **Port-Based Detection** - Service enumeration alerts
4. ✅ **Scan Detection** - Multiple Nmap scan techniques identified
5. ✅ **HTTP Request Monitoring** - Web traffic analysis

### Attack Patterns Detected
- **256 ICMP packets** detected during continuous ping
- **Nmap stealth scans** (SYN, Xmas, FIN) all triggered alerts
- **Service probes** on ports 21, 22, 80 logged successfully

---

## 🛡️ Security Insights

**What This Demonstrates:**
- Snort can detect reconnaissance activities in real-time
- Custom rules allow tailored detection for specific threats
- IDS placement in the network provides visibility into attack patterns
- Log retention enables post-incident analysis

**Real-World Applications:**
- Early warning system for network intrusions
- Compliance requirement for monitoring (PCI-DSS, HIPAA)
- Security operations center (SOC) alert generation

---

## 📈 Skills Demonstrated

- **IDS Deployment** - Installation and configuration on Linux
- **Rule Creation** - Custom Snort signature development
- **Network Monitoring** - Traffic analysis and alerting
- **Attack Simulation** - Generating test traffic from Kali Linux
- **Log Analysis** - Interpreting IDS alerts and packet captures
- **Security Operations** - Detecting common attack patterns

---


## 🚀 Next Steps & Improvements

**Potential Enhancements:**
- [ ] Integrate Snort with Splunk or ELK for centralized logging
- [ ] Create rules for SQL injection and XSS detection
- [ ] Implement rule tuning to reduce false positives
- [ ] Test against more advanced evasion techniques
- [ ] Configure Snort in IPS (inline) mode for active blocking
- [ ] Add automated alert notifications (email/Slack)

---

## 📸 Evidence & Screenshots

See `screenshots/` folder for visual documentation of:
- Snort installation and validation
- Custom rule configuration
- Real-time alert detection
- Kali Linux attack generation
- Log file analysis

---

**Lab Completed:** January 29, 2026  
**Time Investment:** ~3 hours
