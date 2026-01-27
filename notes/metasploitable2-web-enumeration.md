# Metasploitable 2 – Web Enumeration

## Target Information
- Target: Metasploitable 2
- IP Address: 172.30.1.21
- Operating System: Linux
- Web Server: Apache HTTP Server 2.2.8
- Attacker Machine: Kali Linux

## Initial Web Access
The target web server was accessed directly from the attacker machine using a web browser.

URL accessed:
http://172.30.1.21


The default Metasploitable 2 landing page was exposed and accessible without authentication.

## Observations
The web interface openly exposes multiple intentionally vulnerable applications and administrative interfaces, including:

- DVWA (Damn Vulnerable Web Application)
- Mutillidae
- phpMyAdmin
- TWiki
- WebDAV

The presence of these applications confirms the system is deliberately insecure and designed for exploitation practice.

## Quick Analysis
The lack of authentication controls and the exposure of multiple vulnerable web applications significantly increase the attack surface. Each exposed application represents a potential entry point for further enumeration and exploitation, particularly for common web vulnerabilities such as SQL injection, command injection, file inclusion, and authentication bypass.

## Evidence
- Screenshot: `metasploitable2_http_homepage.png`

## Next Steps
- Identify web technologies and frameworks using WhatWeb
- Enumerate directories and hidden resources using Gobuster
- Select a vulnerable web application for exploitation testing
