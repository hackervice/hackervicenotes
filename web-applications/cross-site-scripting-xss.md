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
