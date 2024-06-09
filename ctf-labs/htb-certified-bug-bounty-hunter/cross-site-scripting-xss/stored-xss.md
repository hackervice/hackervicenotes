# Stored XSS

### To get the flag, use the same payload we used above, but change its JavaScript code to show the cookie instead of showing the url.

To solve this lab we just have to use the following payload: &#x20;

```html
<script>alert(document.cookie)</script>
```

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

And a pop appears with the flag!
