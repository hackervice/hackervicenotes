# SSRF

In an SSRF attack, an attacker can manipulate a web server into making requests to arbitrary URLs that they provide. While this may seem harmless at first, the implications can be severe, depending on the web application's configuration. SSRF vulnerabilities can lead to unauthorized access to internal systems, data leakage, and other serious security breaches.

**Common URL Schemes Exploited in SSRF:**

* **http:// and https://**: These schemes allow attackers to fetch content via HTTP/S requests, potentially bypassing Web Application Firewalls (WAFs) and accessing restricted or internal endpoints.
* **file://**: This scheme enables attackers to read files from the local file system of the web server, leading to Local File Inclusion (LFI) vulnerabilities.
* **gopher://**: This protocol can send arbitrary bytes to specified addresses, allowing attackers to send HTTP POST requests with malicious payloads or communicate with other services like SMTP servers or databases.



