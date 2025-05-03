# Authentication Bypass via Parameter Modification

* Authentication mechanisms can be flawed if they rely on HTTP parameters, leading to vulnerabilities that allow for authentication and authorization bypasses, including privilege escalation.

**Parameter Modification Vulnerability:**

* This vulnerability is often related to authorization issues, such as Insecure Direct Object Reference (IDOR), which can allow unauthorized access to resources.

**Example Scenario:**

*   Consider a web application where a user logs in with credentials for `htb-stdnt` and is redirected to:

    ```html
    /admin.php?user_id=123
    ```
* Upon accessing this URL, the user notices limited privileges and data visibility.

**Investigating the `user_id` Parameter:**

* Removing the `user_id` parameter from the request results in a redirect to the login screen (`/index.php`), indicating that the parameter is crucial for authentication.
* The valid session (indicated by the `PHPSESSID` cookie) does not prevent the redirect, suggesting that the application relies on the `user_id` parameter for access control.

**Exploiting the Vulnerability:**

*   By directly accessing the URL with the `user_id` parameter:

    ```
    /admin.php?user_id=123
    ```
* The attacker can bypass authentication checks.
* Since the parameter is named `user_id`, it likely corresponds to the ID of the user accessing the page. If the attacker can guess or brute-force the user ID of an administrator, they can gain access to administrative privileges.

**Brute-Forcing User IDs:**

* Techniques from the Brute-Force Attacks section can be applied to discover valid administrator user IDs, allowing the attacker to specify the admin's ID in the `user_id` parameter and access protected information.

**Final Remarks:**

* More advanced vulnerabilities can also lead to authentication bypass, including:
  * **Type Juggling**
  * **Injection Vulnerabilities**
  * **Logic Bugs**

**Conclusion:**

* Always validate and secure parameters used in authentication and authorization processes to prevent unauthorized access and privilege escalation. Regularly review and test for parameter manipulation vulnerabilities in web applications.
