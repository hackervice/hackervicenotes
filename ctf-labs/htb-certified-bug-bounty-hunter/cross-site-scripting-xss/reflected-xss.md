# Reflected XSS

### To get the flag, use the same payload we used above, but change its JavaScript code to show the cookie instead of showing the url.



<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

This exercise is very similar to the last one. We are given an input field, and we have to inject JavaScript code to show the cookie.

So, like the last time, we just have to use this payload:

```markup
<script>alert(document.cookie)</script>
```

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And as we can see, the cookie appeared as a popup window. The main difference for the last exercise is that this alert will not appear once the user revisits the application.
