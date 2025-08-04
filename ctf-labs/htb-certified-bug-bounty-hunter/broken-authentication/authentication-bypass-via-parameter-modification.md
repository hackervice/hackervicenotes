# Authentication Bypass via Parameter Modification

### Apply what you learned in this section to bypass authentication to obtain the flag.

For this exercise we were given low level credentials to the web application. The goal is to obtain full access to the admin panel and to do that we'll need to create a numeric wordlist and brute-force the `user_id` parameter.

**Step 1:** Capture the Request

* Login into the application
* Use Burp Suite to capture the Request of the `/admin.php?user_id=183 endpoint`



<figure><img src="../../../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption></figcaption></figure>

We can see the after we logged in we get the message **"Could not load admin data. Please check your privileges"**, we will use this to filter our brute-force attack.

<figure><img src="../../../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

The Request looks like this. The only information we will need is the host, endpoint and the php session id.

**Step 2:** Craft the payload and attack:

* Create a numeric wordlist from 0 to 500:
  * ```bash
    seq -w 0 500 > uid.txt
    ```
* Brute-force with ffuf:
  * {% code overflow="wrap" %}
    ```bash
    ffuf -w ./uid.txt -u http://SERVER_IP:SERVER_PORT/admin.php?user_id=FUZZ -X POST -b "PHPSESSID=PHP_SESSION_ID" -fr "Could not"
    ```
    {% endcode %}

<figure><img src="../../../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

And we manage to get a result!

**Step 3:** Access the page with the new `user_id`:

* Modify the `user_id` to the one we just have found and we get access to the admin panel!

<figure><img src="../../../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>
