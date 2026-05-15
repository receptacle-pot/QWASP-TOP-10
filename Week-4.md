# FINAL DETAILED PENETRATION TESTING REPORT
## Web Application Penetration Testing (OWASP Top 10 Focus)
### Submitted By:

- Anshu Selwatkar
- Shreya Gedam
- Vaishnavi Patil
- Kadhiravan
  
### Domain: Cybersecurity

### Organization: Zaalima Development Pvt Ltd

## 1. Executive Summary

This project focused on performing web application penetration testing using vulnerable environments aligned with the OWASP Top 10 security risks. The primary objective was to simulate real-world security testing by identifying, exploiting, and analyzing common web application vulnerabilities.

The testing was performed on intentionally vulnerable applications such as:

- OWASP Juice Shop
- DVWA (Damn Vulnerable Web Application)

The project utilized industry-standard tools such as:

- Burp Suite Community Edition
- nOWASP ZAP
- Browser Developer Tools
- XAMPP
- Kali Linux / Windows Testing Environment

The penetration testing lifecycle included:

- Environment Setup
- Reconnaissance & Mapping
- Vulnerability Assessment
- Exploitation & Proof of Concept (PoC)
- Risk Analysis
- Remediation Recommendations

Final Security Review

The assessment successfully identified and exploited multiple OWASP Top 10 vulnerabilities, improving understanding of practical cybersecurity concepts and real-world penetration testing methodologies.


## 2. Project Objectives

The main objectives of this penetration testing project were:

- Understand the OWASP Top 10 vulnerabilities
- Build a vulnerable testing lab
- Perform hands-on penetration testing
- Use security testing tools professionally
- Learn request interception and manipulation
- Identify security flaws in web applications
- Demonstrate practical exploitation techniques
- Document vulnerabilities with Proof of Concept (PoC)
- Recommend mitigation and remediation techniques


## 3. Scope of Testing

- Target Applications
- OWASP Juice Shop
- DVWA (Damn Vulnerable Web Application)
- Testing Scope Included
- Authentication Mechanisms
- Session Management
- Input Validation
- Authorization Controls
- Client-Side Security
- HTTP Headers
- Security Configurations
- Sensitive Data Handling
- Testing Scope Excluded
- Denial of Service (DoS)
- Real-world production systems
- Third-party APIs
- External Infrastructure Testing


## 4. Environment Setup (Week 1)

### 4.1 OWASP Top 10 Study

The project began with studying the major web application risks from the OWASP Top 10, including:

- Injection
- Broken Authentication
- Sensitive Data Exposure
- XML External Entities (XXE)
- Broken Access Control
- Security Misconfiguration
- Cross-Site Scripting (XSS)
- Insecure Deserialization
- Vulnerable Components
- Insufficient Logging & Monitoring

### 4.2 Vulnerable Lab Setup

OWASP Juice Shop Setup

Steps Performed:

- git clone https://github.com/juice-shop/juice-shop.git --depth 1
- cd juice-shop
- npm install
- npm start

Application launched successfully at:

- http://localhost:3000
- DVWA Setup

Installed:

- XAMPP
- Apache Server
- MySQL Database

Security level configured to:

- LOW

Alternative installation:

- git clone https://github.com/digininja/DVWA.git
- wget https://raw.githubusercontent.com/IamCarron/DVWA-Script/main/Install-DVWA.sh
- chmod +x Install-DVWA.sh
- sudo ./Install-DVWA.sh

### 4.3 Burp Suite Configuration

Proxy configured:

- 127.0.0.1:8080

Activities performed:

- HTTP interception
- Request modification
- Spidering
- Repeater testing
- Intruder attacks


## 5. Reconnaissance Phase

Reconnaissance was conducted to understand application behavior and identify attack surfaces.

- Activities Performed
- Intercepted HTTP requests
- Application spidering
- URL discovery
- Session cookie inspection
- Endpoint mapping
- Parameter analysis

Outcome

The application structure was successfully mapped and multiple attack vectors were identified.


## 6. Vulnerability Assessment & Exploitation (Week 2–3)

### 6.1 SQL Injection (SQLi)

Severity:

- Critical

Description

SQL Injection occurs when unsanitized user input manipulates database queries.

Payload Used: 
  ' OR '1'='1
  
Exploitation Performed
Modified request parameters using Burp Suite
Changed category parameter:
  '+OR+1=1--
Result
- Authentication bypass achieved
- Unauthorized data access observed
- Hidden products displayed
Impact :
- Database compromise
- Data leakage
- Authentication bypass
Remediation :
- Prepared Statements
- Parameterized Queries
- Input Validation

### 6.2 Broken Authentication

Severity:

- High

Description

Weak authentication allowed account compromise using credential attacks.

Testing Performed
Username Enumeration
Password Brute Force
Session Testing
Tool Used

- Burp Suite Intruder

Result : 

Valid credentials identified successfully.

Impact : 
- Unauthorized account access
- Session compromise

Remediation : 
- Strong password policy
- MFA implementation
- Session expiration
- Rate limiting

### 6.3 Cross-Site Scripting (XSS)
Severity:

- High

Payload Used : 
  <script>alert('XSS')</script>
Types Tested : 
- Reflected XSS
- Stored XSS
- DOM-based XSS

Result

JavaScript executed successfully inside the browser.

Impact : 
- Session hijacking
- Cookie theft
- Phishing attacks
- Website defacement

Remediation
- Input sanitization
- Output encoding
- Content Security Policy (CSP)

### 6.4 Sensitive Data Exposure
Severity:

- High

Description

Sensitive information was transmitted insecurely.

Testing Performed :
- Intercepted HTTP traffic
- Inspected credentials
- Reviewed session tokens

Result

Sensitive data exposed over insecure communication.

Impact : 
- Credential theft
- Session hijacking
- Data leakage

Remediation : 
- HTTPS/TLS
- Data encryption
- Secure storage

### 6.5 Security Misconfiguration
Severity:

- High

Description

Applications contained insecure server settings and default configurations.

Testing Performed : 
- Checked HTTP headers
- Tested CORS misconfiguration
- Inspected exposed directories
- Verified outdated software
- Findings

Missing:

- Content-Security-Policy
- X-Frame-Options

Result

CORS vulnerability exploited successfully.

Impact : 
- System compromise
- Unauthorized API access

Remediation
- Secure headers
- Disable debug mode
- Remove defaults
- Regular patching
  
### 6.6 Broken Access Control
Severity:

- Critical

Description

Users accessed unauthorized resources.

Testing Performed

Modified cookie value:

   Admin=false → Admin=true

Result

Administrative access obtained.

Impact : 
- Privilege escalation
- Unauthorized administrative actions

Remediation
- Server-side authorization
- RBAC implementation
- Access validation


## 7. Risk Rating Summary

- Vulnerability	Severity
- SQL Injection	Critical
- Broken Authentication	High
- Cross-Site Scripting (XSS)	High
- Sensitive Data Exposure	High
- Security Misconfiguration	High
- Broken Access Control	Critical


## 8. Tools Used : 

- Tool	Purpose
- Burp Suite	Request interception & exploitation
- OWASP ZAP	Security scanning
- DVWA	Vulnerable testing environment
- - OWASP Juice Shop	Security testing practice
- XAMPP	Local web server
- Browser Dev Tools	Debugging & inspection

## 9. Proof of Concept (PoC) Summary

The following PoCs were successfully demonstrated:

- Vulnerable environment deployment
- Reconnaissance & spidering
- SQL Injection exploitation
- Broken Authentication attack
- Cross-Site Scripting (XSS) execution
- Sensitive Data Exposure analysis
- Security Misconfiguration exploitation
- Broken Access Control privilege escalation

All vulnerabilities were documented with screenshots and attack steps.

## 10. Challenges Faced

During the testing process, several challenges were encountered:

- Burp proxy configuration issues
- Browser certificate trust problems
- Request interception failures
- Payload filtering
- Session handling complications

These challenges improved troubleshooting and analytical skills in cybersecurity testing.

## 11. Work Experience & Learning Outcomes

This internship project provided hands-on experience in practical penetration testing and improved understanding of real-world cybersecurity methodologies.

- Key Skills Developed
- Web Application Penetration Testing
- Vulnerability Assessment
- Burp Suite Usage
- Request Manipulation
- Authentication Testing
- Security Misconfiguration Analysis
- OWASP Top 10 Understanding
- Documentation & Reporting

The project significantly enhanced practical knowledge regarding how attackers exploit vulnerabilities and how defenders secure applications.

## 12. Final Conclusion

This penetration testing project successfully achieved its objectives by identifying and exploiting major web application vulnerabilities aligned with the OWASP Top 10 framework.

The testing demonstrated practical attacks such as SQL Injection, Cross-Site Scripting (XSS), Broken Authentication, Sensitive Data Exposure, Security Misconfiguration, and Broken Access Control in controlled lab environments.

Through this project, valuable experience was gained in:

- Vulnerability assessment
- Ethical hacking methodologies
- Security tool usage
- Risk analysis
- Mitigation strategies
- Professional report writing

Overall, the project strengthened practical cybersecurity knowledge and provided industry-level exposure to penetration testing workflows used in real-world security assessments.
