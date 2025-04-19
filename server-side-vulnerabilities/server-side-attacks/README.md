# Server-side Attacks

Server-side attacks target the application or service provided by a server, whereas a client-side attack takes place at the client's machine, not the server itself. Understanding and identifying the differences is essential for penetration testing and bug bounty hunting.

For instance, vulnerabilities like Cross-Site Scripting (XSS) target the web browser, i.e., the client. On the other hand, server-side attacks target the web server. In this module, we will discuss four classes of server-side vulnerabilities:

* Server-Side Request Forgery (SSRF)
* Server-Side Template Injection (SSTI)
* Server-Side Includes (SSI) Injection
* eXtensible Stylesheet Language Transformations (XSLT) Server-Side Injection



**Server-Side Request Forgery (SSRF)**: This vulnerability allows attackers to manipulate a web application into sending unauthorized requests from the server. By exploiting SSRF, attackers can access internal systems, bypass firewalls, and retrieve sensitive information.

**Server-Side Template Injection (SSTI)**: When web applications use templating engines to dynamically generate content, they can be vulnerable to SSTI. Attackers can inject template code, leading to risks such as data leakage and potential full server compromise through remote code execution.

**Server-Side Includes (SSI) Injection**: SSI directives enable dynamic content generation in HTML files. If an attacker can inject commands into these directives, it can result in SSI injection, which may lead to data leakage or remote code execution.

**XSLT Server-Side Injection**: This vulnerability occurs when attackers manipulate XSLT transformations on the server. By exploiting weaknesses in how these transformations are handled, attackers can inject and execute arbitrary code, posing significant security risks.
