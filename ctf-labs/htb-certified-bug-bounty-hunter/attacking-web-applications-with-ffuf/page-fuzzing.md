# Page Fuzzing

### Try to use what you learned in this section to fuzz the '/blog' directory and find all pages. One of them should contain a flag. What is the flag?

In this section we discovered that the application is using `.php`. In order to discover more pages we'll simply fuzz the file with the extension `.php` specified.

{% code overflow="wrap" %}
```shell
ffuf -w /opt/useful/Seclist/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://83.136.254.223:50360/blog/FUZZ.php
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>ffuf found some php files</p></figcaption></figure>

Lets try to open the `home` page in a web browser.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And `home.php` is the page that contains the flag!
