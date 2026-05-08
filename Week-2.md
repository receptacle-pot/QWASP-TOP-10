# Web Application Penetration Testing (OWASP Top 10 Focus)

# Week-2 : Core Vulnerability Testing

## 1. SQL Injection (SQLi)

Description : Injection of malicious SQL queries to manipulate database. An attacker inserts malicious SQL commands into application input. The Applications executes unintended queries on the database.

Payload Example : ' OR '1'='1
SQL statements Becomes : Select count(*) from users where username =’user’ and password = ' OR '1'='1

Impact : 
- Bypass login
- Extract database data

What we can do  :
- Use prepared statements
- Input validation

### Lab on SQL Injection :
Performed SQL Injection on a shopping website to get access to all the items present in this website by changing the parameter from the users side :
- Used Burp Suite to intercept and modify the request that sets the product category filter.
- Modify the category parameter, giving it the value '+OR+1=1--
- Submit the request, and verify that the response now contains one or more unreleased products.
- The SQL Injection get executed from the server side and shows all the items in the website.

### Screenshot :

### Brupe-suite : 

<img width="1918" height="968" alt="Screenshot_2026-05-06_02_34_50" src="https://github.com/user-attachments/assets/d67d59f7-5a2d-4e35-a723-7d784c629d8f" />

### DVWA :

<img width="1918" height="968" alt="Screenshot_2026-05-08_06_21_53" src="https://github.com/user-attachments/assets/f7c70a59-38d6-4c3c-8ffa-15a86708a82e" />

### OWASP Juice Shop : 

<img width="1920" height="1080" alt="Screenshot_2026-05-08_07_02_10" src="https://github.com/user-attachments/assets/f36eb449-5053-432c-a533-c5d46234ef82" />




\  
## 2. Broken Authentication

Description : Broken authentication is a critical security vulnerability occurring when applications improperly manage user sessions and identities, allowing attackers to compromise passwords, keys, or session tokens.

Key Aspects of Broken Authentication :

- Threat Agents: Attackers use automated tools to perform credential stuffing (using stolen passwords) and brute-force attacks.
- Session Management Errors: Session tokens are exposed in URLs, not invalidated after logout, or are predictable
- Weak Password Policies: Allowing weak passwords or failing to protect against automated credential stuffing.
- API Vulnerabilities: Improper authentication on API endpoints.

Impact :
- Attackers can gain the same privileges as the user.
- Ranging from low-level access to full administrator control.
- Unauthorized access

### Lab on Broken Authentication Vulnerabilities :

Lab: Username enumeration via different responses : 
- The lab is vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password.
- To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

Solution : Burp Suite Intruder Attack

- Open login page and enter invalid username/password.
- In Burp → Proxy > HTTP History, find POST /login request.
- Send request to Intruder.
- Keep payload position on:
- username=§invalid-user§
- Select Sniper Attack.
- In Payloads, paste candidate usernames list.
- Start attack.
- Check Length column:
  - Most responses → Invalid username
  - One response → Incorrect password
- Note that valid username.
- Clear old payloads.
- Set request like:
- username=valid-user&password=§invalid-password§
- Paste candidate passwords list.
- Start attack again.
- Check Status column:
  - Most → 200
  - One → 302 (successful login)
- Note the correct password.
- Login with discovered username & password.
- Access user account page and complete the lab.

### Screenshot :

### Portswigger : 

<img width="1918" height="968" alt="Screenshot_2026-05-06_13_41_08" src="https://github.com/user-attachments/assets/e1c05b88-9621-4631-890b-8077eb0f4c51" />

### OWASP Juice Shop :

<img width="1920" height="1080" alt="Screenshot_2026-05-08_08_02_13" src="https://github.com/user-attachments/assets/02fe9052-b572-4cbe-bf78-cfbd47cf0f08" />



## 3. Cross-Site Scripting (XSS)

Description : Injecting malicious JavaScript into web pages. Cross-Site Scripting (XSS) is a web security vulnerability that allows an attacker to inject malicious client-side scripts—usually JavaScript—into web pages viewed by other users

Payload : <script>alert('XSS')</script>

Impact :
- Cookie theft
- Session hijacking

Potential Impacts : When a successful XSS attack occurs, the attacker can typically perform any action the victim is authorized to do:

Session Hijacking : Stealing session cookies to impersonate the user.

Data Theft : Accessing sensitive data like login credentials or private messages.

Phishing : Displaying fake login forms to capture passwords.Defacement: Modifying the content and appearance of the website.

Fix :
- Input sanitization
- Output encoding

### Types of XSS attacks :
There are three main types of XSS attacks. These are:

-Reflected XSS : where the malicious script comes from the current HTTP request.
-Stored XSS : where the malicious script comes from the website's database.
-DOM-based XSS : where the vulnerability exists in client-side code rather than server-side code.

### What can XSS be used for?
An attacker who exploits a cross-site scripting vulnerability is typically able to:

- Impersonate or masquerade as the victim user.
- Carry out any action that the user is able to perform.
- Read any data that the user is able to access.
- Capture the user's login credentials.
- Perform virtual defacement of the web site.
- Inject trojan functionality into the web site.

### Lab on Cross-Site Scripting XSS

Lab: Reflected XSS into HTML context with nothing encoded

This lab contains a simple reflected cross-site scripting vulnerability in the search functionality.
To solve the lab, perform a cross-site scripting attack that calls the "alert" function.

### Screenshots :

### Portswigger : 

<img width="1918" height="968" alt="Screenshot_2026-05-07_12_47_45" src="https://github.com/user-attachments/assets/b174c56f-d622-4377-b15d-3ff3e7c4e4d6" />

<img width="1919" height="1035" alt="Screenshot 2026-05-07 184251" src="https://github.com/user-attachments/assets/63ba7dc2-fec7-421a-91b2-60254073796c" />

### OWASP Juice Shop :

<img width="1920" height="1080" alt="Screenshot_2026-05-08_07_12_03" src="https://github.com/user-attachments/assets/e474e26a-07c9-41a0-8c1c-35539d18fc74" />

<img width="1920" height="1080" alt="Screenshot_2026-05-08_07_13_34" src="https://github.com/user-attachments/assets/bdf53c80-8e4c-4d72-b881-d2d531fe9fb9" />

### DVWA :

<img width="1918" height="968" alt="Screenshot_2026-05-08_06_31_32" src="https://github.com/user-attachments/assets/405b2e49-ae3d-48c2-9083-01b3e91548d9" />



## 4. Sensitive Data Exposure

Description : Sensitive data exposure is a critical security vulnerability where applications unintentionally reveal confidential information—such as PII (passwords, credit cards, health records)—to unauthorized parties. Sensitive data transmitted insecurely.

Example : HTTP instead of HTTPS

Impact : Data leakage

Fix : Use HTTPS, Encrypt data

Common Causes: 
- Lack of Encryption: Storing or transmitting data in plaintext (unencrypted).
- Weak Cryptography: Using weak encryption keys or improper key management.
- Improper Configuration: Insecure default settings, including exposed cloud storage or missing security headers.
- Human Error/Errors in Code: Secrets (API keys, passwords) left in code repositories, or in URL query strings.

Example Attack Scenarios : 

 - Scenario #1: An application encrypts credit card numbers in a database using automatic database encryption. However, this data is automatically decrypted when retrieved, allowing a SQL injection flaw to retrieve credit card numbers in clear text.

- Scenario #2: A site doesn’t use or enforce TLS for all pages or supports weak encryption. An attacker monitors network traffic (e.g. at an insecure wireless network), downgrades connections from HTTPS to HTTP, intercepts requests, and steals the user’s session cookie. The attacker then replays this cookie and hijacks the user’s (authenticated) session, accessing or modifying the user’s private data. Instead of the above they could alter all transported data, e.g. the recipient of a money transfer.

- Scenario #3: The password database uses unsalted or simple hashes to store everyone’s passwords. A file upload flaw allows an attacker to retrieve the password database. All the unsalted hashes can be exposed with a rainbow table of pre-calculated hashes. Hashes generated by simple or fast hash functions may be cracked by GPUs, even if they were salted

### Screenshots: 

### OWASP Juice Shop :

<img width="1920" height="1080" alt="Screenshot_2026-05-08_07_50_31" src="https://github.com/user-attachments/assets/9d549e8e-133a-4d16-b092-b5b61bb97e97" />

<img width="1920" height="1080" alt="Screenshot_2026-05-08_07_28_54" src="https://github.com/user-attachments/assets/de2f5294-09e3-4e7e-9fde-27f3d7eba823" />
