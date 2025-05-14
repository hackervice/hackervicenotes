# Verb Tampering Prevention

* HTTP Verb Tampering vulnerabilities often arise from insecure configurations and coding practices. Here are some tips in how to identify and patch these vulnerabilities to enhance web application security.

#### Insecure Configuration

**Common Vulnerabilities in Web Server Configurations:**

* Vulnerabilities can occur in various web servers (e.g., Apache, Tomcat, ASP.NET) when authorization is limited to specific HTTP methods, leaving other methods unprotected.

1.  **Apache Configuration Example:**

    ```xml
    <Directory "/var/www/html/admin">
        AuthType Basic
        AuthName "Admin Panel"
        AuthUserFile /etc/apache2/.htpasswd
        <Limit GET>
            Require valid-user
        </Limit>
    </Directory>
    ```

    * This configuration restricts access only to GET requests, allowing POST and other methods to bypass authentication.
2.  **Tomcat Configuration Example:**

    ```xml
    <security-constraint>
        <web-resource-collection>
            <url-pattern>/admin/*</url-pattern>
            <http-method>GET</http-method>
        </web-resource-collection>
        <auth-constraint>
            <role-name>admin</role-name>
        </auth-constraint>
    </security-constraint>
    ```

    * Similar to Apache, this configuration limits authorization to GET requests, leaving other methods accessible.
3.  **ASP.NET Configuration Example:**

    ```xml
    <system.web>
        <authorization>
            <allow verbs="GET" roles="admin">
                <deny verbs="GET" users="*">
            </deny>
            </allow>
        </authorization>
    </system.web>
    ```

    * Again, this configuration restricts access only to GET requests, allowing other methods to be exploited.

**Best Practices for Secure Configuration:**

* Avoid limiting authorization to specific HTTP methods. Instead, allow or deny all HTTP verbs.
* Use safe keywords to specify exceptions, such as:
  * **Apache:** `LimitExcept`
  * **Tomcat:** `http-method-omission`
  * **ASP.NET:** `add/remove`
* Consider disabling or denying all HEAD requests unless explicitly required by the application.

#### Insecure Coding

**Challenges in Identifying Insecure Code:**

* Insecure coding practices are harder to identify than configuration issues because they often involve inconsistencies in the use of HTTP parameters across different functions.

**Example of Vulnerable PHP Code:**

```php
if (isset($_REQUEST['filename'])) {
    if (!preg_match('/[^A-Za-z0-9. _-]/', $_POST['filename'])) {
        system("touch " . $_REQUEST['filename']);
    } else {
        echo "Malicious Request Denied!";
    }
}
```

* The `preg_match` function checks for unwanted characters in POST parameters, but the final command uses `$_REQUEST['filename']`, which includes both GET and POST parameters.
* This inconsistency allows an attacker to bypass the filter by sending a malicious input through a GET request.

**Best Practices for Secure Coding:**

* Ensure consistent use of HTTP methods across the application. Always use the same method for specific functionalities.
* Expand the scope of security filters to cover all request parameters, using the following functions:
  * **PHP:** `$_REQUEST['param']`
  * **Java:** `request.getParameter('param')`
  * **C#:** `Request['param']`
* By covering all methods in security-related functions, the risk of HTTP Verb Tampering vulnerabilities can be significantly reduced.

#### Conclusion:

* Addressing both insecure configurations and coding practices is essential for preventing HTTP Verb Tampering vulnerabilities. By implementing best practices and ensuring consistency in HTTP method usage, web applications can be better protected against unauthorized access and exploitation.
