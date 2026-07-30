  # What are file upload vulnerabilities
File upload vulnerabilities are security weaknesses that occur when a web application allows users to upload files without properly validating or restricting them. Attackers can exploit these flaws to upload malicious files and potentially gain unauthorized access, execute code, steal data, or disrupt services.

## Common Types of File Upload Vulnerabilities
Uploading Executable Files
An attacker uploads a script such as .php, .jsp, .asp, or another server-executable file.
If the server executes the file, the attacker may gain remote control of the system.
File Type Validation Bypass
Applications often check file extensions (e.g., only allowing .jpg).
Attackers may bypass checks using:
Double extensions (shell.php.jpg)
Alternate extensions (shell.phtml)
Manipulated MIME types
Case variations (shell.PHP)
Malicious Content in Legitimate Files
A file may appear to be an image or document but contain embedded malicious code.
Examples include malware hidden in Office documents or PDFs.
Path Traversal Through File Names
Malicious filenames such as ../../../etc/passwd attempt to overwrite or access sensitive files.
Poorly implemented upload handlers may be vulnerable to this.
Denial of Service (DoS)
Uploading extremely large files or many files can exhaust storage, memory, or processing resources.
Client-Side Attacks
Uploading HTML or SVG files containing JavaScript.
When another user views the file, it may trigger a Cross-Site Scripting (XSS) attack.
Example Scenario

Imagine a website that allows profile picture uploads and only checks whether the filename ends with .jpg.

An attacker uploads:

shell.php.jpg

If the server is configured incorrectly and executes the embedded PHP code, the attacker could run commands on the server.

## Web shell
A web shell is a malicious script that enables an attacker to execute arbitrary commands on a remote web server simply by sending HTTP requests to the right endpoint.
If you're able to successfully upload a web shell, you effectively have full control over the server. This means you can read and write arbitrary files, exfiltrate sensitive data, even use the server to pivot attacks against both internal infrastructure and other servers outside the network. For example, the following PHP one-liner could be used to read arbitrary files from the server's filesystem:

<?php echo file_get_contents('/path/to/target/file'); ?>
Once uploaded, sending a request for this malicious file will return the target file's contents in the response.

A more versatile web shell may look something like this:

<?php echo system($_GET['command']); ?>
This script enables you to pass an arbitrary system command via a query parameter as follows:

GET /example/exploit.php?command=id HTTP/1.1

# LAB 1
- While proxying traffic through Burp, log in to your account and notice the option for uploading an avatar image.
- Upload an arbitrary image, then return to your account page. Notice that a preview of your avatar is now displayed on the page.
- In Burp, go to Proxy > HTTP history. Click the filter bar to open the HTTP history filter window. Under Filter by MIME type, enable the Images checkbox, then apply your changes.
- In the proxy history, notice that your image was fetched using a GET request to /files/avatars/<YOUR-IMAGE>. Send this request to Burp Repeater.
- In Burp, after sending the POST request for the image in the repeater, change the file name of the image to exploit.php. Then replace the contents of the image with a php command: <?php echo file_get_contents('/home/carlos/secret'); ?>
- In Burp Repeater, change the path of the request to point to your PHP file:

GET /files/avatars/exploit.php HTTP/1.1
Send the request. Notice that the server has executed your script and returned its output (Carlos's secret) in the response.
Submit the secret to solve the lab.

