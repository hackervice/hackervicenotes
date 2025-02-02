# Prevention

Command injection vulnerabilities arise when an application allows users to execute arbitrary commands on the server. It's crucial to recognize how these vulnerabilities can be exploited and the limitations of common mitigations like character and command filters.

**Avoiding System Commands**

* **Minimize Use**: Avoid using functions that execute system commands, especially with user input. Even indirect user influence can lead to vulnerabilities.
* **Use Built-in Functions**: Opt for built-in functions that provide the required functionality securely. For example, use `fsockopen` in PHP to check if a host is alive instead of executing system commands.
* **Sanitize User Input**: If you must use system commands, always validate and sanitize user input to prevent exploitation.

**Input Validation and Sanitization**

*   **Validation**: Ensure user input matches expected formats. Implement validation on both the front-end and back-end. Use built-in filters (e.g., `filter_var` in PHP) or regular expressions for custom formats.

    ```php
    if (filter_var($_GET['ip'], FILTER_VALIDATE_IP)) {
        // call function
    } else {
        // deny request
    }
    ```
*   **Sanitization**: Remove unnecessary special characters from validated input. This step is critical to prevent injection attacks. Use functions like `preg_replace` in PHP or string replacement in JavaScript.

    ```php
    $ip = preg_replace('/[^A-Za-z0-9.]/', '', $_GET['ip']);
    ```
* **Libraries**: Consider using libraries like DOMPurify for NodeJS to sanitize input effectively.

**Server Configuration Best Practices**

* **Web Application Firewalls**: Utilize both built-in and external WAFs to protect against attacks.
* **Principle of Least Privilege (PoLP)**: Run the web server with minimal privileges to limit potential damage.
* **Function Restrictions**: Disable potentially dangerous functions in your server configuration (e.g., `disable_functions` in PHP).
* **Scope Limitation**: Restrict the web application’s access to its own directory (e.g., `open_basedir` in PHP).
* **Request Filtering**: Reject double-encoded requests and non-ASCII characters in URLs.
* **Library Management**: Avoid using outdated or sensitive libraries that could introduce vulnerabilities.

**Ongoing Security Practices** Even with robust coding practices and server configurations, continuous penetration testing is essential. Regularly test your web application for command injection vulnerabilities, as even a small oversight in code can lead to significant security risks.

For further insights on user input validation and sanitization, refer to the "Secure Coding 101: JavaScript" module, which provides guidance on auditing and patching vulnerabilities in web applications.
