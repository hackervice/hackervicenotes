# Authentication Bypass via Direct Access

* This section covers vulnerabilities that allow attackers to bypass authentication mechanisms entirely, focusing on direct access to protected resources.

**Direct Access Vulnerability:**

* An attacker can bypass authentication checks by directly requesting protected resources if the web application fails to verify authentication properly.
* Example scenario: If a web application redirects users to `/admin.php` after successful login but does not enforce authentication checks on that endpoint, an unauthenticated user can access it directly.

**Illustrative Example:**

*   Consider a PHP snippet that checks user authentication:

    ```php
    if(!$_SESSION['active']) {
        header("Location: index.php");
    }
    ```
* This code redirects unauthenticated users to `index.php`, but it does not stop script execution, allowing the protected content of `/admin.php` to be sent in the response body.

**Exploiting the Vulnerability:**

* When accessing `/admin.php`, the browser follows the redirect and shows the login prompt instead of the protected content.
* An attacker can intercept the response using a tool like Burp Suite:
  1. Enable Intercept in Burp.
  2. Access `/admin.php` in the browser.
  3. Intercept the response and change the status code from `302 Found` to `200 OK`.
  4. Forward the modified response to the browser.
* This manipulation allows the attacker to view the protected information directly in the browser.

**Prevention Measures:**

*   To prevent this vulnerability, ensure that the PHP script exits after issuing a redirect:

    ```php
    if(!$_SESSION['active']) {
        header("Location: index.php");
        exit;
    }
    ```
* This change ensures that no protected content is sent in the response body if the user is not authenticated, effectively mitigating the risk of unauthorized access.

**Conclusion:**

* Always validate authentication before serving protected resources and ensure proper handling of redirects to safeguard against authentication bypass vulnerabilities.
