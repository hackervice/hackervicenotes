# Brute-Forcing 2FA Codes

Two-factor authentication (2FA) adds an extra layer of security to user accounts by requiring two forms of authentication. Typically, this involves combining knowledge-based authentication (like a password) with ownership-based authentication (like a device generating a one-time code).

2FA significantly reduces the risk of unauthorized access, making it more difficult for attackers to breach accounts even if they have the user's credentials.

***

**Common Implementation of 2FA:**

* A typical 2FA setup involves a password and a time-based one-time password (TOTP) generated by an authenticator app or sent via SMS.
* TOTPs are usually numeric and can be vulnerable if they are short and the application does not limit the number of incorrect attempts.

***

**Attacking Two-Factor Authentication:**

1. **Scenario Setup:**
   * Assume valid credentials were obtained through a phishing attack (e.g., `admin:admin`).
   * After logging in, the user is prompted for a TOTP, indicating that 2FA is enabled.
2. **Identifying the TOTP Request:**
   * The TOTP is passed in the `otp` POST parameter, and a session token (e.g., `PHPSESSID`) is required to maintain the authenticated session.
3.  **Generating a Token Wordlist:**

    * Create a wordlist of all possible 4-digit TOTP values (0000 to 9999):

    ```bash
    seq -w 0 9999 > tokens.txt
    ```
4.  **Brute-Forcing the TOTP:**

    * Use `ffuf` to automate the brute-forcing of the TOTP:

    {% code overflow="wrap" %}
    ```bash
    ffuf -w ./tokens.txt -u http://example.com/2fa.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=<PHP_SESSION_ID>" -d "otp=FUZZ" -fr "Invalid 2FA Code"
    ```
    {% endcode %}

    * This command attempts each TOTP from the wordlist, filtering out responses that indicate an invalid code.
5. **Successful Authentication:**
   * If a valid TOTP is found (e.g., `FUZZ: 6513`), the session is marked as fully authenticated.
   * Subsequent requests using the session cookie can access protected resources, such as `/admin.php`.

***

#### Summary:

Two-factor authentication enhances account security by requiring two forms of verification. However, if the TOTP is short and the application does not limit incorrect attempts, it can be vulnerable to brute-force attacks. By generating a list of possible TOTPs and using automated tools like `ffuf`, attackers can potentially bypass 2FA and gain unauthorized access to accounts. It is essential for developers to implement robust security measures to protect against such vulnerabilities.
