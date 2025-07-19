# File Inclusion Prevention

#### What is the full path to the php.ini file for Apache?

/etc/php/7.4/apache2/php.ini

#### Edit the php.ini file to block system(), then try to execute PHP Code that uses system. Read the /var/log/apache2/error.log file and fill in the blank: system() has been disabled for \_\_\_\_\_\_\_\_ reasons.

**Step 1** - SSH to target machine:



**Step 2** - Find the php.ini file:

* Use `find /etc -name php.ini 2>/dev/null`

<figure><img src="../../../.gitbook/assets/image (299).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Edit the **/etc/php/7.4/apache2/php.ini**:

* Open the **/etc/php/7.4/apache2/php.ini f**ile
* Find the **disable\_functions** section and add **system:**

<figure><img src="../../../.gitbook/assets/image (300).png" alt=""><figcaption></figcaption></figure>

**Step 4** - Create a **.php** file in **/var/www/html**:

* Create a shell.php with the following code:
  * ```php
    <?php system($GET["cmd"]);?>
    ```
* Restart apache `sudo systemctl restart apache2`

**Step 5** - Execute the **shell.php** and look the logs:

* Run `tail -f /var/log/apache2/error.log`

<figure><img src="../../../.gitbook/assets/image (301).png" alt=""><figcaption></figcaption></figure>

* On the attack machine curl the .php file that we created:
  * ```bash
    curl 10.129.29.112/shell.php?cmd=ls
    ```
* Look for the logs again

<figure><img src="../../../.gitbook/assets/image (302).png" alt=""><figcaption></figcaption></figure>

The word we looking for is **security**. Since we add **system** to the **disable\_function** we prevented LFI with success!
