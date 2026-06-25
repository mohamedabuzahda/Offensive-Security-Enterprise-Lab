Reflected XSS 


1. Vulnerability Summary
A Reflected Cross-Site Scripting (XSS) vulnerability occurs when a web application receives data from a user (via a URL parameter or a form field) and immediately includes that data in the web page response without proper sanitization or encoding. This allows an attacker to execute arbitrary JavaScript in the victim's browser.

2. Test Environment 
Security Level: Low

Path: vulnerabilities/xss_r/

Mechanism: The page contains an input field asking for a name. When the user clicks "Submit," the input is reflected back on the page and embedded in the URL.

3. Exploitation Steps (PoC)
Step 1: Basic Testing
Input a standard string like User.



Step 2: Payload Injection
Instead of a name, input a JavaScript payload into the field:

HTML
<script>alert('XSS_Success')</script>
Step 3: Execution
Upon clicking "Submit," the browser interprets the script tag and executes the JavaScript, triggering an alert box. This confirms that the input was not sanitized and was executed by the browser.

4. Common Payloads for Testing
Cookie Stealing (Alerting user session):
<script>alert(document.cookie)</script>

Content Manipulation:
<script>document.body.innerHTML = "<h1>Page Defaced</h1>"</script>

Event Handler (If <script> tags are filtered):
<img src=x onerror=alert(1)>

5. Technical Root Cause
At the "Low" security level, the PHP code takes the input directly from $_GET['name'] and echoes it into the HTML document without any validation:

PHP
echo "<pre>Hello " . $_GET['name'] . "</pre>";
6. Remediation (How to fix)
To secure the application, you must follow the principle of "Never trust user input":

HTML Encoding: Convert sensitive characters into their HTML entities (e.g., < becomes &lt;). In PHP, use the htmlspecialchars() function.

Content Security Policy (CSP): Implement a CSP header to restrict where scripts can be loaded from and prevent the execution of inline scripts.