# Cross-Site Scripting (XSS)

Cross-Site Scripting (XSS) is a type of security vulnerability commonly found in web applications. It occurs when an attacker is able to inject malicious scripts into web pages viewed by other users. These scripts can then execute in the browsers of those users, potentially allowing the attacker to steal information, take control of user sessions, or perform other malicious actions. XSS attacks can be prevented by properly validating and sanitizing user input on websites.

The most common

### Types of XSS

There are three main types of XSS vulnerabilities:

<table><thead><tr><th width="263">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>Stored (Persistent) XSS</code></td><td>The most critical type of XSS, which occurs when user input is stored on the back-end database and then displayed upon retrieval (e.g., posts or comments)</td></tr><tr><td><code>Reflected (Non-Persistent) XSS</code></td><td>Occurs when user input is displayed on the page after being processed by the backend server, but without being stored (e.g., search result or error message)</td></tr><tr><td><code>DOM-based XSS</code></td><td>Another Non-Persistent XSS type that occurs when user input is directly shown in the browser and is completely processed on the client-side, without reaching the back-end server (e.g., through client-side HTTP parameters or anchor tags)</td></tr></tbody></table>

### XSS Testing Payloads

To test whether an applications is vulnerable to a XSS attack, we can use the following payloads:

```html
<script>alert(window.origin)</script>
```

or

```html
<script>alert(document.domain)</script>
```

This will tell us where the code is being run. For example a very popular event is just using `alert(1)`,it's simple and very visual, but it doesn't actually tell us where the script is being executed.

XSS cheat sheets:

[Payload all the things](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection)

[PortSwigger XSS cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)



### DOM XSS

DOM-based Cross-Site Scripting (XSS) is a type of web vulnerability where the attack payload is executed as a result of modifying the Document Object Model (DOM) in a web page. Unlike other types of XSS attacks that involve server-side scripts, DOM XSS is completely processed on the client-side through JavaScript. This vulnerability arises when client-side scripts dynamically update the DOM based on user input without proper validation, allowing an attacker to inject malicious scripts that are then executed in the context of the victim's browser.

#### Source & Sink

In the context of DOM-based Cross-Site Scripting (XSS), a <mark style="color:purple;">source</mark> is where user-controllable data enters the application, and a <mark style="color:purple;">sink</mark> is where this data is used in a potentially unsafe way, leading to a security vulnerability.

<mark style="color:purple;">**Sources**</mark> in DOM-based XSS include locations where user input or untrusted data is reflected in the DOM, such as URL parameters, document.location, document.referrer, document.cookie, etc.

<mark style="color:purple;">**Sinks**</mark> are places in the application where the data from sources is used in a way that can be exploited by an attacker to execute malicious scripts. Sinks can include functions like eval(), innerHTML, document.write(), etc.

To prevent DOM-based XSS attacks, it's important to properly sanitize and validate user input before it is used in sinks to ensure that it does not contain malicious scripts.

### XSS Discovery

We can use automated the discovery process with open-stools like <mark style="color:purple;">XSS Strike</mark>:

```bash
git clone https://github.com/s0md3v/XSStrike.git
cd XSStrike
pip install -r requirements.txt
python xsstrike.py
```

And then we just run the tool:

```shell-session
python xsstrike.py -u "http://www.example.com:PORT/index.php?menu=test" 
```

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

