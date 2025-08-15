# Cross-Site Scripting (XSS)

Cross-Site Scripting (XSS) vulnerabilities affect web applications and APIs alike. An XSS vulnerability may allow an attacker to execute arbitrary JavaScript code within the target's browser and result in complete web application compromise if chained together with other vulnerabilities.

#### Interact with the API with a random value

`http://SERVER_IP:PORT/api/download/randomvalue`

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Since the value is reflected in the response, try to enter a payload.

#### XSS payload

```javascript
<script>alert(document.domain)</script>
```

Or the encoded version

```javascript
%3Cscript%3Ealert%28document.domain%29%3C%2Fscript%3E
```
