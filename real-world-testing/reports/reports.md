Damn Vulnerable Web Application (DVWA) - Security Level: Low
Date: June 26, 2026
Tester: Grok (Automated + Manual Analysis)
Testing Type: Black Box / Gray Box

1. Executive Summary
A comprehensive penetration test was conducted on the DVWA web application at the Low security level. Seven critical and high-severity web vulnerabilities were successfully identified and exploited.
Discovered Vulnerabilities:

Reflected XSS
DOM-based XSS
SQL Injection (SQLi)
Server-Side Request Forgery (SSRF)
Path Traversal / File Inclusion
OS Command Injection
Cross-Site Request Forgery (CSRF)

Overall Risk Level: Critical
These vulnerabilities allow attackers to steal sessions, extract database credentials, read system files, execute arbitrary commands on the server, and perform unauthorized actions on behalf of authenticated users.
Recommendation: Immediately increase the security level to Medium or High and implement proper input validation, output encoding, and secure coding practices.

2. Scope of Testing

Target: DVWA (Low Security Mode)
Methodology: Manual exploitation + automated tools
Tools Used: Browser Developer Tools, SQLMap, custom payloads
Date of Testing: June 2026


3. Detailed Vulnerability Findings
3.1 Reflected XSS
Severity: High
Location: /vulnerabilities/xss_r/
Description:
The application reflects user input from the URL directly into the page without sanitization.
Proof of Concept (PoC):
HTML<script>alert('XSS_Success')</script>
Cookie stealing payload:
HTML<script>alert(document.cookie)</script>
Root Cause:
PHPecho "<pre>Hello " . $_GET['name'] . "</pre>";
Impact: Session hijacking, malware delivery, page defacement.
Remediation:

Use htmlspecialchars() in PHP.
Implement Content Security Policy (CSP).
Output encoding for all user-controlled data.

3.2 DOM-based XSS
Severity: High
Location: /vulnerabilities/xss_d/
Description:
Client-side JavaScript takes data from the URL and inserts it unsafely into the DOM.
PoC:
texthttp://.../xss_d/?default=<script>alert('DOM-XSS')</script>
Root Cause:
JavaScriptvar lang = document.location.search...;
document.write(decodeURIComponent(lang));
Impact: Execution of malicious JavaScript entirely in the browser.
Remediation:

Use .textContent instead of document.write or .innerHTML.
Sanitize/validate all URL parameters before processing.

3.3 SQL Injection (SQLi)
Severity: Critical
Location: /vulnerabilities/sqli/
Description:
User input is concatenated directly into SQL queries.
Manual PoC: ' OR 1=1 --
Automated Exploitation with SQLMap:
Bashsqlmap -u "http://target/vulnerabilities/sqli/?id=1&Submit=Submit" \
--cookie="PHPSESSID=...; security=low" --dbs

sqlmap ... -D dvwa --tables
sqlmap ... -D dvwa -T users --dump
Impact: Full database dump (usernames, password hashes), data manipulation, or deletion.
Remediation:

Use Prepared Statements / Parameterized Queries.
Strict input validation (e.g., integer-only for IDs).
Apply Principle of Least Privilege on database accounts.

3.4 Server-Side Request Forgery (SSRF)
Severity: High
Location: /vulnerabilities/ssrf/?file=
Description:
The server fetches arbitrary URLs provided by the user.
PoC:

file:///etc/passwd
http://127.0.0.1:80 (internal port scanning)

Root Cause:
PHP$content = file_get_contents($_GET['file']);
Impact: Access to internal services, cloud metadata, or local files.
Remediation:

Strict URL whitelisting.
Block dangerous protocols (file://, gopher://, etc.).
Prevent requests to private IP ranges (127.0.0.1, 192.168.x.x, etc.).

3.5 Path Traversal (File Inclusion)
Severity: High
Location: /vulnerabilities/fi/?page=
Description:
User-controlled input allows navigation outside the intended directory.
PoC:
text../../../../etc/passwd
or
text../../../../config/config.inc.php
Root Cause:
PHP$file = $_GET['page'];
include($file);
Impact: Exposure of sensitive system files and configuration.
Remediation:

Whitelist allowed filenames.
Use basename() function.
Enable open_basedir restriction in PHP.

3.6 OS Command Injection
Severity: Critical
Location: /vulnerabilities/exec/
Description:
User input is passed directly to system shell commands.
PoC:
text127.0.0.1; ls -la
127.0.0.1; cat /etc/passwd
127.0.0.1; whoami
Root Cause:
PHP$cmd = shell_exec('ping -c 4 ' . $target);
Impact: Full server compromise, reverse shells, data exfiltration.
Remediation:

Avoid shell_exec(), system(), exec() when possible.
Use escapeshellarg() and strict validation (e.g., FILTER_VALIDATE_IP).

3.7 Cross-Site Request Forgery (CSRF)
Severity: Medium → High
Location: /vulnerabilities/csrf/
Description:
No anti-CSRF token and no current password verification on sensitive actions.
PoC (Malicious HTML page):
HTML<img src="http://target/vulnerabilities/csrf/?password_new=hacked123&password_conf=hacked123&Change=Change" style="display:none;">
Impact: Force authenticated users to change passwords or perform unwanted actions.
Remediation:

Implement Anti-CSRF tokens.
Require re-authentication for sensitive operations.
Set SameSite=Strict or Lax on cookies.


4. Risk Summary & Recommendations


Vulnerability,Severity,Exploitability,Impact
SQL Injection,Critical,High,Critical
OS Command Injection,Critical,High,Critical
Reflected XSS,High,High,High
DOM-based XSS,High,High,High
SSRF,High,Medium,High
Path Traversal,High,High,High
CSRF,Medium-High,Medium,Medium-High


Strategic Recommendations:

Upgrade DVWA security level to High.
Follow OWASP Top 10 mitigation guidelines.
Conduct regular code reviews and SAST/DAST scanning.
Deploy a Web Application Firewall (WAF).
Train developers on secure coding practices.