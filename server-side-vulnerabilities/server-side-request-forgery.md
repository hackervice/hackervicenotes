# Server-side request forgery

### What is SSRF? <a href="#id-5bffa049-692b-4644-9b6a-b8d2d51ce02e" id="id-5bffa049-692b-4644-9b6a-b8d2d51ce02e"></a>

Server-side request forgery is a web security vulnerability that allows an attacker to cause the server-side application to make requests to an unintended location.

In a typical SSRF attack, the attacker might cause the server to make a connection to internal-only services within the organization's infrastructure. In other cases, they may be able to force the server to connect to arbitrary external systems. This could leak sensitive data, such as authorization credentials.



Bypass filtering:

<pre class="language-html" data-overflow="wrap"><code class="lang-html">&#x3C;div class="col-xs-4">
    &#x3C;div class="avatar-image" style="background-image: url('/assets/avatars/5.png')">&#x3C;/div>
    &#x3C;input type="radio" name="avatar" value="<a data-footnote-ref href="#user-content-fn-1">exploit</a>">
&#x3C;/div>
                                
</code></pre>



SSRF attacks against the server

🧪Lab: Basic SSRF against the local server

SSRF attacks against other back-end systems

🧪Lab: Basic SSRF against another back-end system

[^1]: Change the value to private, for example
