# Web Attacks

* Web applications are increasingly common in business, making their protection against attacks critical.
* As web applications grow in complexity, so do the types of attacks, leading to a vast attack surface.
* Web attacks are the most prevalent type of attack against companies, making their protection a top priority for IT departments.

**Impact of Web Attacks:**

* Attacks on external-facing web applications can compromise internal networks, leading to stolen assets or service disruptions.
* Financial disasters can result from such compromises.
* Even companies without external web applications are at risk due to internal applications and external APIs.

**Module Focus:**

* This section covers three specific web attacks, including detection, exploitation, and prevention strategies.

#### Key Web Attacks:

1. **HTTP Verb Tampering:**
   * Exploits web servers that accept multiple HTTP verbs/methods.
   * Malicious requests using unexpected methods can bypass authorization and security controls.
   * Part of a broader category of HTTP attacks targeting web server configurations.
2. **Insecure Direct Object References (IDOR):**
   * A common vulnerability allowing unauthorized access to data.
   * Often results from weak access control systems on the back-end.
   * Attackers can access other users' files by guessing or calculating file IDs due to exposed direct references.
3.  **XML External Entity (XXE) Injection:**

    * Targets web applications that process XML data.
    * Outdated XML libraries can be exploited by sending malicious XML data.
    * Potential outcomes include disclosing sensitive local files (e.g., configuration files) and stealing server credentials, leading to remote code execution.
    *

    #### Conclusion:

    * Understanding these web attacks is essential for developing effective security measures to protect web applications and the sensitive data they handle.
