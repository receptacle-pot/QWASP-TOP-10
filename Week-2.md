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
