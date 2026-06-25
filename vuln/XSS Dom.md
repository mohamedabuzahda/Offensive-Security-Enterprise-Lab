Here is the comprehensive educational report on DOM-based XSS .

Vulnerability Report: DOM-based XSS
1. Vulnerability Summary
DOM-based XSS is a sophisticated type of XSS vulnerability. Unlike Reflected or Stored XSS, the payload never leaves the browser to interact with the server. The entire vulnerability exists within the "client-side" environment; the application's client-side JavaScript reads data from the browser (like the URL Fragment or Query String) and processes it in an insecure way, leading to the execution of malicious code.

2. Test Environment 
Path: vulnerabilities/xss_d/

Mechanism: The page features a dropdown menu to select a language. The selection is reflected in the URL, for example: .../vulnerabilities/xss_d/?default=English. The page's JavaScript takes this value from the URL and inserts it directly into the page content.

3. Exploitation (PoC)
Step 1: URL Manipulation
Instead of selecting a valid language from the dropdown, we modify the parameter in the URL so that the browser executes JavaScript instead of displaying text.

Step 2: The Payload
We use a tag that executes within the DOM without requiring a full page reload:

Modified URL:
http://10.114.180.239/vulnerabilities/xss_d/?default=<script>alert('DOM-XSS')</script>

Step 3: Execution
Once the user presses Enter, the page's JavaScript (which uses document.write or innerHTML) takes the content from ?default= and injects it into the HTML code, triggering the alert pop-up.

4. Technical Root Cause
At the "Low" security level, the vulnerable JavaScript code in the page is:

JavaScript
// JavaScript code in the page
var lang = document.location.search.substring(default.length + 9);
document.write(decodeURIComponent(lang));
The vulnerability stems from using document.write with a variable retrieved directly from the URL (document.location.search) without any sanitization.

5. Remediation (How to fix)
Avoid Insecure Methods: Avoid using document.write or .innerHTML. Use safer alternatives like .textContent or .innerText, which treat data as plain text rather than executable HTML.

Input Validation: Ensure that input coming from the URL matches a predefined Whitelist of allowed values.

Encoding: If input must be displayed, encode it to convert characters like < and > into safe HTML entities (e.g., &lt; and &gt;).