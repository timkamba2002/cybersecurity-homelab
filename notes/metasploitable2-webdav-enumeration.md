# Metasploitable 2 – WebDAV Enumeration

## Target Information
- Target: Metasploitable 2
- IP Address: 172.30.1.21
- Service: WebDAV
- Attacker Machine: Kali Linux

## WebDAV Discovery
The WebDAV service was discovered during web directory enumeration and was accessible at:

http://172.30.1.21/dav/

## WebDAV Enumeration (davtest)
A WebDAV enumeration test was conducted to determine file upload and execution capabilities.

### Command Used
```bash
davtest -url http://172.30.1.21/dav
```
Results
WebDAV service accessible

Directory creation allowed

Arbitrary file upload permitted

PHP file upload successful

PHP file execution confirmed

### Impact Analysis
The ability to upload and execute PHP files through WebDAV represents a critical security misconfiguration. This can allow an attacker to achieve remote code execution by uploading a malicious web shell.

### Evidence
Screenshot: metasploitable2_davtest.png

### Next Steps
Upload a simple PHP web shell

Execute system commands through the web interface
