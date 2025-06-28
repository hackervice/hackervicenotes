# HTTP Verb Tampering

* The HTTP protocol allows various methods (verbs) at the start of an HTTP request, enabling web applications to perform specific actions based on the request type.
* Commonly used methods include GET and POST, but clients can send other methods to see how the server responds.

**Server Response to Unrecognized Methods:**

* If a web server is configured to accept only GET and POST, sending other methods may result in an error page, which is not a severe vulnerability but can lead to information disclosure.
* If the server accepts additional methods (e.g., HEAD, PUT) without proper handling, it can be exploited to access unauthorized functionalities or bypass security controls.

#### Common HTTP Methods:

* **HEAD:** Similar to GET but returns only headers, no body.
* **PUT:** Writes the request payload to a specified location.
* **DELETE:** Deletes the resource at a specified location.
* **OPTIONS:** Shows accepted options by the server, including HTTP verbs.
* **PATCH:** Applies partial modifications to a resource.

**Potential Risks:**

* Sensitive functionalities (like writing or deleting files) can be exploited if the server is not securely configured.
* HTTP Verb Tampering vulnerabilities often arise from misconfigurations in the web server or application.

#### Types of Insecure Configurations:

1. **Insecure Web Server Configurations:**
   * Authentication may be limited to specific HTTP methods, leaving others accessible without authentication.
   *   Example configuration:

       <pre class="language-xml"><code class="lang-xml"><strong>&#x3C;Limit GET POST>
       </strong>    Require valid-user
       &#x3C;/Limit>
       </code></pre>
   * An attacker could use an unprotected method (like HEAD) to bypass authentication and access restricted pages.
2. **Insecure Coding Practices:**
   * Developers may apply filters to mitigate vulnerabilities but fail to cover all HTTP methods.
   *   Example of a flawed SQL Injection mitigation:

       {% code overflow="wrap" %}
       ```php
       $pattern = "/^[A-Za-z\s]+$/";
       if(preg_match($pattern, $_GET["code"])) {
           $query = "Select * from ports where port_code like '%" . $_REQUEST["code"] . "%'";
           ...SNIP...
       }
       ```
       {% endcode %}
   * The filter only checks GET parameters, allowing POST parameters to bypass the filter and potentially execute SQL injection.

**Conclusion:**

* Insecure coding practices are more common than misconfigurations and often lead to vulnerabilities.
* Understanding these vulnerabilities is crucial for developing secure web applications and preventing HTTP Verb Tampering attacks.
