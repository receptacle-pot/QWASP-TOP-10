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

<img width="1918" height="968" alt="Screenshot_2026-05-06_02_34_50" src="https://github.com/user-attachments/assets/d67d59f7-5a2d-4e35-a723-7d784c629d8f" />



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


