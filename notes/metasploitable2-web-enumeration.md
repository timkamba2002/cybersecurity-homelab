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

## Technology Identification (WhatWeb)
A WhatWeb scan was performed to identify web technologies and frameworks in use on the target system.

### Command Used
```bash
whatweb http://172.30.1.21
```
### Results
- Web Server: Apache HTTP Server 2.2.8 (Ubuntu)
- Operating System: Ubuntu Linux
- Programming Language: PHP 5.2.4
- WebDAV: Enabled
- HTTP Response: 200 OK
- Page Title: Metasploitable2 - Linux

### Analysis
The target is running significantly outdated software components, including Apache 2.2.8 and PHP 5.2.4, both of which are known to contain multiple historical vulnerabilities. The presence of WebDAV further expands the attack surface and may allow file upload or remote file manipulation attacks if improperly configured. These findings confirm that the web server is a high-value target for further exploitation.

## Evidence
- Screenshot: `metasploitable2_http_homepage.png`

## Directory Enumeration (Gobuster)

A directory brute-force scan was conducted to identify hidden directories and web resources on the target system.

### Command Used
```bash
gobuster dir -u http://172.30.1.21 -w /usr/share/wordlists/dirb/common.txt
```
Results

The following notable directories and files were discovered:

/phpMyAdmin – Database administration interface

/phpinfo.php – PHP configuration information disclosure

/twiki – TWiki collaboration platform

/dav/ – WebDAV directory

/test/ – Test directory

/cgi-bin/ – Common CGI directory (restricted)

/.htaccess, /.htpasswd – Protected configuration files (403 Forbidden)

Analysis

The discovery of multiple sensitive directories significantly increases the attack surface. The presence of phpMyAdmin and phpinfo.php may allow information disclosure or database compromise. WebDAV is of particular interest, as misconfigured WebDAV services can allow unauthorized file uploads or remote code execution. TWiki is also a known attack vector due to historical vulnerabilities.

## Next Steps
- Attempt WebDAV enumeration and file upload testing
- Manually inspect phpMyAdmin and phpinfo.php
- Select a viable exploitation path
