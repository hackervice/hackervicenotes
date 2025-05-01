# Brute-Forcing 2FA Codes

### Brute-force the admin user's 2FA code on the target system to obtain the flag.

We were given the credentials user "**admin** and password "**admin**" to login to the web application, however it has a 2FA implementation. In order to bypass this lets first capture the Two Factor Authentication Request.

**Step 1** - Capture the 2FA Request:

* Login with the credentials admin:admin
* Insert a random OTP code and capture the Request on Burp Suite

<figure><img src="../../../.gitbook/assets/image (194).png" alt=""><figcaption></figcaption></figure>

* If we send the Request on Repeater, we can see that we got the message "**Invalid 2FA Code**". We'll use this later to filter the results with **ffuf**.

<figure><img src="../../../.gitbook/assets/image (195).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Brute-force the OTP code:

* Create a numeric wordlist with all possible combinations:
  * ```bash
    seq -w 0 9999 > tokens.txt
    ```
* Use the wordlist to brute-force the OTP code with **ffuf**:
  * {% code overflow="wrap" %}
    ```bash
    ffuf -w ./tokens.txt -u http://SERVER_IP:SERVER_PORT/reset_password.php?token=FUZZ -fr "The provided token is invalid"
    ```
    {% endcode %}
* You will get a bunch of codes, because the session successfully passed the 2FA check. Use the first hit on the list:

<figure><img src="../../../.gitbook/assets/image (197).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Retreive the flag:

* Insert the OTP code to complete the authentication:

<figure><img src="../../../.gitbook/assets/image (196).png" alt=""><figcaption></figcaption></figure>

* And get your flag!

<figure><img src="../../../.gitbook/assets/image (198).png" alt=""><figcaption></figcaption></figure>

