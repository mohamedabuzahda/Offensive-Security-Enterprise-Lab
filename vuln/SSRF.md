Server-Side Request Forgery (SSRF) 
1. Vulnerability Summary
SSRF is a powerful vulnerability that allows an attacker to force a server to make HTTP requests to a destination of the attacker's choosing. In a normal scenario, the server communicates with the internet or internal network devices. In an SSRF attack, the attacker exploits the server’s trust to make it scan the internal network, access protected services, or retrieve files from systems that are not directly reachable from the attacker's own machine.

2. Test Environment 
Path: vulnerabilities/ssrf/

Mechanism: The page allows the user to load an image via an external URL. The server then fetches this image and displays it.

URL Structure: .../vulnerabilities/ssrf/?file=

3. Exploitation (PoC)
Step 1: Normal Operation
The application expects a URL for an image, such as http://localhost/images/logo.png. The server opens the URL and retrieves the content.

Step 2: SSRF Exploitation
We attempt to force the server to retrieve data it should not have access to, or to perform internal port scanning.

Payload to access a local system file (on a Linux server):

Plaintext
file:///etc/passwd
Final URL: .../vulnerabilities/ssrf/?file=file:///etc/passwd

Payload to scan for internal services (e.g., checking if port 80 is open):

Plaintext
http://127.0.0.1:80
Step 3: Result
Instead of fetching an image, the server reads system files or interacts with services running internally on 127.0.0.1 (like router admin panels or internal databases), and the application displays the result of this interaction.

4. Technical Root Cause
At the "Low" security level, the application does not validate the input URL:

PHP
$file = $_GET['file'];
$content = file_get_contents($file);
echo $content;
Here, PHP's file_get_contents function supports multiple protocols (http://, file://, ftp://, etc.). The server does not distinguish between a request for an "image from the internet" and a request for a "sensitive system file."

5. Remediation (How to fix)
Whitelisting: Do not allow the server to connect to arbitrary URLs. Define a strict whitelist of allowed domains or base URLs.

Restrict Protocols: Allow only http:// and https:// and block dangerous protocols like file://, gopher://, or dict://.

Validate IP Addresses: Prevent the server from connecting to private/internal IP ranges (e.g., 127.0.0.1, 192.168.x.x, or 10.x.x.x).

Network Segmentation: Run the application in a restricted network environment that does not have access to sensitive internal resources.