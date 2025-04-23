# Custom Wordlists

**Custom Wordlists**

* **Pre-made Wordlists**: Tools like rockyou and SecLists are broad and may not be efficient for specific targets.
* **Targeted Approach**: For example, when attempting to access system files for "Thomas Edison," generic lists are unlikely to yield results due to unique username patterns enforced by organizations.
* **Creating Custom Wordlists**: Tailor wordlists based on information from social media, company directories, and leaked data to improve efficiency in brute-force attacks.

**Username Anarchy**

* **Username Variability**: Even simple names can lead to complex username combinations. Consider variations like initials, middle names, hobbies, and leetspeak.
* **Tool Usage**: Use the Username Anarchy script to generate potential usernames based on the target's first and last names.
  * Installation:
    *   ```bash
        sudo apt install ruby -y
        git clone https://github.com/urbanadventurer/username-anarchy.git
        cd username-anarchy
        ```


    * Example command:&#x20;
      * `./username-anarchy Ron Smith > ron_smith_usernames.txt`
* **Output**: The generated list includes basic combinations, initials, and creative variations, enhancing the chances of finding the correct username.

**CUPP (Common User Passwords Profiler)**

* **Password Generation**: CUPP creates personalized password lists based on detailed information about the target.
* **Information Gathering**: Collect data from social media, company websites, public records, and news articles to inform the password generation process.
* **Example Profile for Ron Smith**: Include details like birthdate, partner's name, interests, etc.
* **CUPP Execution**: Run CUPP in interactive mode to input the gathered information and generate a comprehensive password list.
  * Example command: `cupp -i`
* **Output**: The generated list includes variations like original, reversed, concatenated, and leetspeak passwords.

**Filtering Passwords**

* **Password Policy Compliance**: Use grep to filter the generated password list to meet specific company policies (e.g., length, character types).
  *   Example command:

      {% code overflow="wrap" %}
      ```bash
      grep -E '^.{6,}$' ron.txt | grep -E '[A-Z]' | grep -E '[a-z]' | grep -E '[0-9]' | grep -E '([!@#$%^&*].*){2,}' > ron-filtered.txt
      ```
      {% endcode %}

**Brute-Force Attack with Hydra**

* **Using Hydra**: Combine the username and filtered password lists to perform a brute-force attack on the login form.
  *   Example command:

      {% code overflow="wrap" %}
      ```bash
      hydra -L ron_smith_usernames.txt -P jane-filtered.txt SERVER_IP -s SERVER_PORT -f http-post-form "/:username=^USER^&password=^PASS^:Invalid credentials"
      ```
      {% endcode %}
* **Outcome**: Upon successful completion, log in using the discovered credentials to access system files.

#### Summary

Utilizing custom wordlists, Username Anarchy, and CUPP significantly enhances the efficiency of brute-force attacks by tailoring approaches to specific targets. This method minimizes wasted effort and maximizes the likelihood of success in accessing system files.
