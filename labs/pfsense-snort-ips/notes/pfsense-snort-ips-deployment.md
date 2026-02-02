# pfSense Firewall with Snort IPS - Enterprise Network Security Implementation

## 🎯 Project Overview

Deployed pfSense as a virtual firewall/router with integrated Snort IPS to provide enterprise-grade network security. Transitioned from passive intrusion detection (IDS) to active intrusion prevention (IPS) with real-time threat blocking capabilities. Successfully demonstrated inline blocking, custom rule development, and security policy management.

---

## 🔧 Lab Environment

### Infrastructure

**pfSense Router:**
- **Platform:** pfSense 2.8.1-RELEASE (VirtualBox VM)
- **Hostname:** pfSense.timcyber.com
- **Resources:** 4GB RAM, 2 vCPUs, 16GB disk

**Network Configuration:**
- **WAN Interface (em0):** 192.168.1.197/24 (Bridged to home network)
- **LAN Interface (em1):** 172.30.2.1/24 (Internal lab network)

**Virtual Machines:**

| Machine | OS | IP Address | Role |
|---------|----|-----------| -----|
| Kali Linux | Kali Linux 2025.4 | 172.30.2.100 | Attack Platform |
| Metasploitable 2 | Ubuntu Linux | 172.30.2.101 | Vulnerable Target |
| Metasploitable 3 | Windows Server 2008 | 172.30.2.102 | Vulnerable Target |

---

### Network Architecture
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

## 🚀 Implementation

### Phase 1: pfSense Deployment

**Objectives:**
- Replace VirtualBox default networking with centralized firewall
- Implement enterprise-grade network segmentation
- Enable advanced security monitoring and logging

**Network Configuration:**
- WAN interface obtained DHCP from home router (192.168.1.197)
- LAN configured as 172.30.2.1/24 with DHCP range 100-150
- Migrated all VMs to pfSense-managed network (PFVulnLabs)
- Verified full connectivity between all lab systems

**Firewall Rules Implemented:**
- Blocked internet access from vulnerable VMs (security isolation)
- Allowed internal VM-to-VM communication (penetration testing requirements)
- Created NAT port forward to expose Metasploitable 2 for external testing

**Validation:**
- All internal connectivity verified (Kali ↔ Metasploitable 2/3)
- Internet blocking confirmed (Metasploitable unable to reach google.com)
- Port forwarding operational (Windows host can SSH to 192.168.1.197 → Metasploitable 2)

---

### Phase 2: Snort IPS Integration

**Deployment:**
- Installed Snort v4.1.6 via pfSense package manager
- Configured Snort on LAN interface for internal traffic monitoring
- Enabled inline IPS mode for active threat blocking

**Custom Detection Rules:**
```
Rule 1 - ICMP Detection:
alert icmp any any -> $HOME_NET any (msg:"Ping detected";sid:1000001;rev:1;)

Rule 2 - SSH Detection:
alert tcp any any -> $HOME_NET 22 (msg:"SSH Detected";sid:1000002;rev:1;)
```

**Testing Results:**

| Test Scenario | Source | Destination | Result |
|--------------|--------|-------------|---------|
| Internal ping | Kali (172.30.2.100) | Metasploitable 2 | Detected & Logged ✓ |
| External ping | Windows Host (192.168.1.191) | pfSense WAN | Detected & Logged ✓ |
| SSH connection | Windows Host | Metasploitable 2 | Detected & Logged ✓ |
| Outbound ping | Metasploitable 2 | google.com | Detected return traffic ✓ |

---

### Phase 3: Active Blocking (IPS Mode)

**Configuration Change:**
- Enabled "Block Offenders" on Snort LAN interface
- Transitioned from passive detection to active prevention

**Blocking Validation:**

**Test 1: ICMP Blocking**
```
Windows Host → ping 192.168.1.197

Result:
- First packet: Success (pattern recognition delay)
- Subsequent packets: Blocked
- Statistics: 75% packet loss (1 allowed, 3 blocked)
- Host IP added to Snort block table
```

**Test 2: SSH Blocking**
```
Windows Host → PuTTY SSH to 192.168.1.197

Result:
- Connection initiated successfully
- Username entered: "msfadmin"
- Connection terminated before password prompt
- Error: "Network error: Software caused connection abort"
- Blocked in under 1 second
```

**Key Findings:**
- First packet always succeeds (Snort requires traffic pattern to identify threat)
- Blocking occurs within milliseconds after signature match
- Once blocked, ALL traffic from source IP is dropped (stateful blocking)

---

### Phase 4: Pass List Configuration

**Challenge:**
After enabling blocking, legitimate management access from Windows host was also blocked, preventing authorized testing.

**Solution: Implemented Pass List (Whitelist)**

**Initial Configuration (Failed):**
- Added only Windows host IP (192.168.1.191) to pass list
- Result: External access restored, but internal VMs also blocked
- Root cause: Missing internal lab network (172.30.2.0/24)

**Corrected Configuration:**
```
Pass List Contents:
- 192.168.1.191 (Windows management host)
- 172.30.2.0/24 (Internal lab network - auto-generated)
- pfSense infrastructure IPs (gateways, DNS)
```

**Critical Step:** Restarted Snort service (configuration loaded at startup)

**Validation Results:**
- Windows host → pfSense: 0% packet loss, SSH works ✓
- Kali → Metasploitable: Full connectivity restored ✓
- Metasploitable → google.com: External IPs still blocked ✓
- All traffic still logged for audit trail ✓

---

## 📊 Final Security Architecture

### Defense-in-Depth Implementation

**Layer 1 - Firewall Rules (Network Layer):**
- Block outbound internet from vulnerable VMs
- Allow internal communication for penetration testing
- NAT port forwarding for controlled external access

**Layer 2 - Snort IPS (Application Layer):**
- Real-time packet inspection
- Signature-based threat detection
- Automatic blocking after first malicious packet
- Comprehensive alert logging

**Traffic Flow Summary:**

| Source | Destination | Firewall | Snort IPS | Final Result |
|--------|-------------|----------|-----------|--------------|
| Windows Host (whitelisted) | Metasploitable | Allow | Pass List | ✓ Allowed, Logged |
| Kali ↔ Metasploitable | Internal | Allow | Pass List | ✓ Allowed, Logged |
| Metasploitable | Internet | Block | N/A | ✗ Blocked (Layer 1) |
| External IPs | Metasploitable | Allow (port forward) | Inspect | ✗ Blocked after 1st packet |

---

## 🔍 Key Findings & Analysis

### IPS Performance Metrics

**Blocking Speed:**
- Initial packet: 0ms (allowed for pattern recognition)
- Subsequent packets: <100ms (real-time blocking)
- Total response time: Sub-second prevention

**Detection Accuracy:**
- ICMP detection: 100% (all ping attempts logged)
- SSH detection: 100% (all connection attempts logged)
- False positives: 0% (after pass list tuning)

---

### Troubleshooting Insights

**Issue 1: SSH Protocol Mismatch**
- **Symptom:** SSH connections immediately dropped
- **Cause:** Snort SSH preprocessor flagged Metasploitable's legacy SSH version
- **Alert:** `(spp_ssh) Protocol mismatch` - gen_id: 128, sig_id: 4
- **Resolution:** Disabled SSH protocol mismatch detection in Snort preprocessors
- **Result:** SSH connections stable while maintaining attack detection

**Issue 2: Pass List Not Working**
- **Symptom:** Configuration applied but blocking continued
- **Cause:** Snort service not restarted (cached old configuration)
- **Resolution:** Manual service restart required after pass list changes
- **Lesson:** Configuration changes require service reload

**Issue 3: Internal VM Blocking**
- **Symptom:** Whitelisted external access, but internal VMs blocked each other
- **Cause:** Pass list missing internal network (172.30.2.0/24)
- **Resolution:** Re-enabled pfSense default auto-generated IP addresses
- **Impact:** Prevented self-inflicted denial of service

---

## 💼 Professional Applications

### SOC Analyst Relevance

**Demonstrated Capabilities:**
- IDS/IPS deployment and configuration
- Custom signature development
- Alert triage and analysis
- False positive reduction
- Block management and whitelisting
- Multi-layer security architecture

**Real-World Parallels:**
- pfSense → Enterprise firewalls (Palo Alto, Fortinet, Cisco ASA)
- Snort → NGFW IPS modules or standalone IPS appliances
- Pass lists → Firewall whitelist policies
- Custom rules → Threat hunting signatures

---

### Security Operations Workflow

**Incident Detection:**
1. Snort generates alert (e.g., "SSH Detected" from 192.168.1.191)
2. Review alert details (source IP, destination, timestamp)
3. Correlate with known activity (authorized management vs. unauthorized access)

**Incident Response:**
1. Identify malicious traffic (external IP not in pass list)
2. Automatic blocking by Snort IPS (source IP added to block table)
3. Manual review of blocked hosts tab
4. Remove false positives (add legitimate IPs to pass list)

**Continuous Improvement:**
1. Monitor alert frequency and patterns
2. Tune rules to reduce false positives
3. Update pass lists for new authorized systems
4. Document exceptions and justifications

---

## 📈 Skills Demonstrated

**Network Security:**
- Firewall rule configuration and policy management
- Network segmentation and isolation
- NAT and port forwarding
- Intrusion prevention system deployment

**Threat Detection & Response:**
- Signature-based detection
- Custom rule development
- Alert analysis and triage
- Real-time threat blocking

**Troubleshooting:**
- Service configuration debugging
- Network connectivity verification
- False positive identification
- Security policy tuning

**Tools & Technologies:**
- pfSense firewall platform
- Snort IDS/IPS engine
- VirtualBox network architecture
- Linux/Windows command line

---

## 📸 Evidence Documentation

Complete visual documentation available in `screenshots/`:
- pfSense system dashboard and interface status
- Network connectivity between all VMs
- NAT port forwarding configuration
- Firewall LAN rules
- Snort custom detection rules
- Real-time alert generation
- Active blocking demonstration (75% packet loss)
- Pass list implementation
- Snort blocked hosts table

---

## 🎯 Outcomes

**Security Posture Improvements:**
- ✅ Centralized network security management
- ✅ Real-time threat detection and prevention
- ✅ Defense-in-depth architecture (firewall + IPS)
- ✅ Complete audit trail (all traffic logged)
- ✅ Controlled exposure of vulnerable systems
- ✅ Authorized access maintained via whitelisting

**Technical Achievements:**
- ✅ Deployed enterprise-grade firewall in virtualized environment
- ✅ Implemented inline IPS with sub-second blocking
- ✅ Created custom detection signatures
- ✅ Successfully tuned security policies (eliminated false positives)
- ✅ Documented complete implementation with evidence

---

**Lab Completed:** February 2, 2026  
**Documentation:** 14 screenshots, complete configuration reference  

**Timothy Kamba**  
SOC Analyst | Cybersecurity Professional  
GitHub: [github.com/timkamba2002/cybersecurity-homelab](https://github.com/timkamba2002/cybersecurity-homelab)
