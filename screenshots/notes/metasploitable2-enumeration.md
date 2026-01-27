# Metasploitable 2 – Enumeration

## Target Information
- Target Machine: Metasploitable 2
- IP Address: 172.30.1.21
- Attacker Machine: Kali Linux
- Network: VulnLabs (Internal Network)

## Scan Performed
```bash
sudo nmap -sS -A -p- 172.30.1.21
## Key Findings

The target system exposes a large attack surface with multiple insecure and legacy services running.

Notable open ports and services include:

- 21/tcp – FTP (vsftpd 2.3.4, anonymous login allowed)
- 22/tcp – SSH
- 23/tcp – Telnet
- 80/tcp – HTTP (Apache 2.2.8)
- 139/445 – Samba (smbd 3.0.20)
- 1524/tcp – Bind shell (root shell)
- 3306/tcp – MySQL
- 8180/tcp – Apache Tomcat

## Initial Assessment

Several exposed services are known to be insecure or vulnerable, including anonymous FTP access, Telnet, outdated Samba, and an exposed bind shell. These findings indicate the system is intentionally vulnerable and suitable for practicing enumeration and exploitation techniques.
