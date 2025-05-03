# Default Credentials

Default credentials are pre-set usernames and passwords for web applications, intended for initial access post-installation.

It's crucial to change these credentials after setup to prevent unauthorized access by attackers.

**Importance in Security Testing:**

* Testing for default credentials is a key aspect of authentication testing, as highlighted in [OWASP's Web Application Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials).
* Common default credentials often include combinations like "admin" and "password."

**Resources for Testing:**

* Various platforms maintain lists of default credentials, such as:
  * [**CIRT.net**](https://www.cirt.net/passwords): A web database where you can find default credentials for specific devices (e.g., Cisco).
  * [**SecLists**](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Default-Credentials): A collection of default credentials.
  * [**SCADA GitHub Repository**](https://github.com/scadastrangelove/SCADAPASS/tree/master): Contains default passwords for various vendors.

**Practical Example:**

* During a penetration test, if a Cisco device is identified, we can reference [**CIRT.net**](https://www.cirt.net/passwords) for its default credentials.
* For web applications like BookStack, a targeted internet search can yield default credentials. For instance, searching "bookstack default credentials" can lead to installation instructions revealing the default admin login as [admin@admin.com](mailto:admin@admin.com) with the password "password."

**Conclusion:**

* Always ensure default credentials are changed to enhance security and mitigate risks associated with unauthorized access.
