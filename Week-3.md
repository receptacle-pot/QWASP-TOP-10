#  Advanced Testing & PoC

## 1. Security Misconfiguration

Description : Improper configuration of server/application. 
Security Misconfiguration happens when an application, server, or database is configured insecurely, leaving it vulnerable to attacks. This is one of the most common vulnerabilities in the OWASP Foundation Top 10.

Common Examples : 

- Default usernames and passwords
- Unnecessary services enabled
- Improper file permissions
- Missing security headers
- Exposed admin panels
- Debug mode enabled in production
- Testing Performed
- Checked HTTP security headers
- Identified default configurations
- Scanned for exposed directories/files
- Tested insecure server settings
- Verified software versions for outdated components

Tools Used : 
- Burp Suite
- OWASP ZAP
- Browser Developer Tools

Remediation : 
- Disable unnecessary services and features
- Use strong authentication settings
- Configure secure HTTP headers
- Remove default credentials
- Keep software updated regularly

Impact : Full system compromise

Fix :
- Disable debug
- Harden server

### Screenshot :

<img width="1918" height="968" alt="Screenshot_2026-05-09_05_22_06" src="https://github.com/user-attachments/assets/5caa0a8c-32da-4914-88de-b5cd76c80573" />

