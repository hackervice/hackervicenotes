# Brute-Forcing Passwords

After identifying valid users, password-based authentication relies on passwords for user verification. Attackers can exploit this by guessing or brute-forcing passwords, especially since users often choose easy-to-remember passwords.

***

**Common Issues with Passwords:**

1. **Password Reuse:**
   * Users frequently use the same password across multiple accounts. If one account is compromised, attackers can access others using the same credentials (known as "Password Spraying").
2. **Weak Passwords:**
   * Many users select weak passwords based on common phrases, dictionary words, or simple patterns, making them susceptible to brute-force attacks.

***

**Brute-Force Attack Dynamics:**

* The effectiveness of a brute-force attack depends on:
  * The number of attempts an attacker can make.
  * The time taken for each attempt.
* Using a well-curated wordlist that matches the target web application's password policy is crucial to avoid wasting time on invalid passwords.

**Example Password Policy:**

* A sample web application may require:
  * At least one uppercase character.
  * At least one lowercase character.
  * At least one digit.
  * Minimum length of 10 characters.

***

**Creating a Custom Wordlist:**

*   Using a large password wordlist (e.g., `rockyou.txt` with over 14 million passwords), we can filter it to match the target's password policy:

    {% code overflow="wrap" %}
    ```bash
    grep '[[:upper:]]' /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt | grep '[[:lower:]]' | grep '[[:digit:]]' | grep -E '.{10}' > custom_wordlist.txt
    ```
    {% endcode %}
* This reduces the wordlist to approximately 150,000 valid passwords, a significant reduction of about 99%.

***

**Brute-Forcing Process:**

1. **Identify Target User:**
   * Using previous enumeration techniques, we determine that "admin" is a valid username.
2. **Intercept Login Request:**
   * Capture the login request to identify POST parameters and error messages. For example, an incorrect username might return: "Invalid username or password."
3.  **Construct Brute-Force Command:**

    * Using `ffuf`, we can automate the brute-forcing process:

    {% code overflow="wrap" %}
    ```bash
    ffuf -w ./custom_wordlist.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=admin&password=FUZZ" -fr "Invalid username"
    ```
    {% endcode %}

    * This command attempts to log in with the username "admin" and each password from the custom wordlist, filtering out responses that contain "Invalid username."
4. **Successful Login:**
   * After several attempts, we may successfully find the password (e.g., "Password1"), allowing access to the admin panel of the web application.

***

#### Summary:

Brute-forcing passwords is a common attack method that exploits weak password practices and password reuse. By creating a targeted wordlist that adheres to the specific password policy of a web application, attackers can efficiently attempt to gain unauthorized access. Understanding the dynamics of brute-force attacks and employing tools like `ffuf` can significantly enhance the effectiveness of these attempts.
