# cybersecurity-homelab
Personal cybersecurity home lab using VirtualBox, Kali Linux, and intentionally vulnerable machines to practice network configuration, scanning, enumeration, and exploitation.
# Cybersecurity Home Lab

## Overview
This repository documents my personal cybersecurity home lab built to practice offensive security and networking fundamentals in a controlled environment.

The lab is hosted on a Windows 11 machine using VirtualBox with segmented internal networks to simulate real-world attack scenarios.

## Lab Environment
- Host OS: Windows 11
- Hypervisor: VirtualBox
- Attack Machine: Kali Linux
- Vulnerable Machines:
  - Metasploitable 2 (Linux)
  - Metasploitable 3 (Windows Server 2008)

## Network Architecture
- Internal Attack Network
- Internal Vulnerable Network
- Kali Linux connected to both networks
- Vulnerable machines isolated on VulnLabs network

## Objectives
- Network scanning and service enumeration
- Identifying vulnerable services
- Exploiting known vulnerabilities in a safe environment
- Understanding network segmentation and attack paths

## Tools Used
- Nmap
- Metasploit Framework
- Netcat
- Linux command-line utilities

## Progress
- [x] VirtualBox and network setup
- [x] Kali Linux configuration
- [x] Metasploitable 2 deployment
- [x] Metasploitable 3 deployment
- [ ] Service enumeration
- [ ] Exploitation walkthroughs
- [ ] Post-exploitation analysis

## Disclaimer
All activities are performed in a legal, isolated lab environment using intentionally vulnerable machines for educational purposes only.
