Path Traversal (Directory Traversal) 
1. Vulnerability Summary
A Path Traversal vulnerability (also known as a Dot-Dot-Slash Attack) occurs when a web application fails to sanitize user input used to access system files. This allows an attacker to navigate outside the intended directory and access sensitive files on the hosting server (such as configuration files, password files, or system files).

2. Test Environment 
Path: vulnerabilities/fi/ (File Inclusion).

Mechanism: The application selects files from a dropdown list to display their content. The filename is passed via a URL parameter, for example: http://localhost/vulnerabilities/fi/?page=include.php

3. Exploitation Steps (PoC)
Step 1: Understanding the Application
The application takes the value of the page parameter and concatenates it with the default directory path to open the file.

Step 2: Payload Injection
We use the ../ (dot-dot-slash) sequence to move backward through the system directories until we reach the root, then request a sensitive file.

Payload used (to access the Linux user file):
../../../../etc/passwd

Final URL:
http://localhost/vulnerabilities/fi/?page=../../../../etc/passwd

Step 3: Result
Instead of displaying the site's local files, the server reads the /etc/passwd file and displays its content on the page, revealing the usernames existing on the system.

4. Common Payloads for Testing
To access the site's configuration file (may contain database credentials):
../../../../config/config.inc.php

To access system logs:
../../../../var/log/apache2/access.log

If the server is running on Windows:
..\..\..\..\windows\win.ini

5. Technical Root Cause
At the "Low" security level, the code takes the input directly without any validation:

PHP
$file = $_GET['page'];
include($file);
Since the include function in PHP handles file paths, it follows the attacker's instructions to move backward and access any file the server has read permissions for.

6. Remediation (How to fix)
Avoid Passing Paths Directly: Never allow users to pass the full filename. Use a "Whitelist" of allowed filenames that can be accessed.

Use basename(): Use the basename() function in PHP, which strips any directory paths and keeps only the filename.

Chroot/Jail: Run the application in an isolated environment that prevents it from accessing core system directories.

Enable open_basedir: Restrict the directories that PHP is allowed to open files from in the server configuration (php.ini).

