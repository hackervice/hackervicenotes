# Brute-Forcing Password Reset Tokens

* Many web applications offer a password-recovery feature that allows users to reset their passwords if forgotten. This typically involves a one-time reset token sent via SMS or email, which the user can use to authenticate and reset their password.

***

**Risks of Weak Reset Tokens:**

* If a reset token is weak (e.g., predictable or easily brute-forced), attackers can exploit this vulnerability to take over a victim's account.
* Reset tokens are secret data generated when a user requests a password reset, allowing password changes without knowledge of the original password.

**Example of a Password Reset Flow:**

* A typical password reset flow involves several steps, including requesting a reset token and using it to change the password.

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Credits to HTB</p></figcaption></figure>

**Identifying Weak Reset Tokens:**

* To identify weak reset tokens, one must create an account on the target application, request a password reset, and analyze the token received.
* For example, a password reset email might contain a link with a token like `http://example.com/reset_password.php?token=7351`. If the token is a simple 4-digit number, there are only 10,000 possible combinations (0000 to 9999), making it vulnerable to brute-force attacks.

***

**Attacking Weak Reset Tokens:**

1.  **Create a Token Wordlist:**

    * Generate a list of all possible 4-digit tokens using the `seq` command:

    ```bash
    seq -w 0 9999 > tokens.txt
    ```

    * The `-w` flag ensures all numbers are padded to the same length.
2.  **Brute-Force the Tokens:**

    * If users are actively resetting their passwords, you can brute-force the reset tokens. First, send a password reset request for the target user to generate a reset token.
    * Use `ffuf` to automate the brute-forcing process:

    {% code overflow="wrap" %}
    ```bash
    ffuf -w ./tokens.txt -u http://weak_reset.htb/reset_password.php?token=FUZZ -fr "The provided token is invalid"
    ```
    {% endcode %}

    * This command attempts each token from the wordlist against the reset password endpoint, filtering out invalid tokens.
3. **Successful Token Guess:**
   * If a valid token is found (e.g., `FUZZ: 6182`), you can reset the password for the corresponding account, gaining unauthorized access.

***

#### Summary:

Weak password reset tokens pose a significant security risk, especially when they are predictable or limited in range. By generating a list of possible tokens and using automated tools like `ffuf`, attackers can easily brute-force these tokens to take over accounts. It is crucial for developers to implement strong, unpredictable reset tokens to mitigate this vulnerability.
