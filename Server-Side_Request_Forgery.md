# Server-Side Request Forgery (SSRF)
SSRF is a web vulnerability that allows an attacker to trick a server into making requests to unintended destinations on the attackers behalf

## SSRF attacks against the server
In an SSRF attack against the server, the attacker causes the application to make an HTTP request back to the server that is hosting the application, via its loopback network interface. This typically involves supplying a URL with a hostname like 127.0.0.1 (a reserved IP address that points to the loopback adapter) or localhost (a commonly used name for the same adapter).

## SSRF attacks against other back-end systems
In some cases, the application server is able to interact with back-end systems that are not directly reachable by users. These systems often have non-routable private IP addresses. The back-end systems are normally protected by the network topology, so they often have a weaker security posture. In many cases, internal back-end systems contain sensitive functionality that can be accessed without authentication by anyone who is able to interact with the systems.

In the previous example, imagine there is an administrative interface at the back-end URL http://192.168.0.68/admin. An attacker can submit the following request to exploit the SSRF vulnerability, and access the administrative interface:

POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

Lab: Basic SSRF against another back-end system
APPRENTICE

## LAB
This lab has a stock check feature which fetches data from an internal system.

To solve the lab, use the stock check functionality to scan the internal 192.168.0.X range for an admin interface on port 8080, then use it to delete the user carlos.

ACCESS THE LAB
 Solution
Visit a product, click Check stock, intercept the request in Burp Suite, and send it to Burp Intruder.
Change the stockApi parameter to http://192.168.0.1:8080/admin then highlight the final octet of the IP address (the number 1) and click Add §.
In the Payloads side panel, change the payload type to Numbers, and enter 1, 255, and 1 in the From and To and Step boxes respectively.
Click  Start attack.
Click on the Status column to sort it by status code ascending. You should see a single entry with a status of 200, showing an admin interface.
Click on this request, send it to Burp Repeater, and change the path in the stockApi to: /admin/delete?username=carlos

stockApi=http://192.168.0.68/admin
