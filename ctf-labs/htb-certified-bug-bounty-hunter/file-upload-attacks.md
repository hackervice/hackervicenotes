# File Upload Attacks

Try to upload a PHP script that executes the (hostname) command on the back-end server, and submit the first word of it as the answer.

In this lab we are presented with a web application that allow us to upload files to the web server.

<figure><img src="../../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

Our goal is to get the server's hostname, and to do that we'll try to exploit a simple file upload attack.

<figure><img src="../../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>

In this section we already determined that this is a PHP web application, so create a simple .php file to get the server's hostname:

```php
<?php echo gethostname();?>
```

Then within the application we follow the steps to upload our file.

<figure><img src="../../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>

When the file is uploaded successfully we have download button to download our file. Lets click it.

<figure><img src="../../.gitbook/assets/image (128).png" alt=""><figcaption></figcaption></figure>

And here it is! The application is vulnerable to a file upload attacks, since we manage to get the hostname.
