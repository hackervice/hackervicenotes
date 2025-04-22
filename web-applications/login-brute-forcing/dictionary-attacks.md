# Dictionary Attacks

**Overview of Dictionary Attacks:**

* Dictionary attacks are a more efficient alternative to brute-force attacks, especially against common or weak passwords.
* They exploit the tendency of users to choose memorable passwords, often based on dictionary words, phrases, or predictable patterns.

**Key Concepts:**

* **Wordlist Quality:** The effectiveness of a dictionary attack depends on the quality and specificity of the wordlist. Tailoring the wordlist to the target (e.g., using gaming terms for a gaming community) increases success rates.
* **Human Psychology:** Dictionary attacks leverage common password practices, making them faster and more efficient than brute-force methods.

**Comparison: Brute Force vs. Dictionary Attack:**

| Feature       | Dictionary Attack                                          | Brute Force Attack                                  | Explanation                                                     |
| ------------- | ---------------------------------------------------------- | --------------------------------------------------- | --------------------------------------------------------------- |
| Efficiency    | Faster and more resource-efficient.                        | Time-consuming and resource-intensive.              | Dictionary attacks narrow the search space significantly.       |
| Targeting     | Highly adaptable to specific targets.                      | No inherent targeting capability.                   | Wordlists can be customized based on target information.        |
| Effectiveness | Effective against weak or commonly used passwords.         | Effective against all passwords given enough time.  | If the password is in the dictionary, it will be found quickly. |
| Limitations   | Ineffective against complex, randomly generated passwords. | Often impractical for lengthy or complex passwords. | Truly random passwords are unlikely to be in any dictionary.    |

**Example Scenario:**

* An attacker targeting a company's employee login might create a wordlist with:
  * Common weak passwords (e.g., "password123")
  * Variations of the company name
  * Employee names or department names
  * Industry-specific jargon

**Building and Utilizing Wordlists:**

* **Sources for Wordlists:**
  * **Publicly Available Lists:** Free lists from the internet, including leaked credentials (e.g., [SecLists](https://github.com/danielmiessler/SecLists/tree/master/Passwords)).
  * **Custom-Built Lists:** Created based on reconnaissance data about the target.
  * **Specialized Lists:** Focused on specific industries or applications.
  * **Pre-existing Lists:** Tools like ParrotSec come with built-in wordlists (e.g., rockyou.txt).

**Useful Wordlists for Login Brute-Forcing:**

| Wordlist                                  | Description                                              | Typical Use                             | Source                                                                                                                           |
| ----------------------------------------- | -------------------------------------------------------- | --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| rockyou.txt                               | Contains millions of leaked passwords.                   | Commonly used for password brute force. | [RockYou breach dataset](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt)                      |
| top-usernames-shortlist.txt               | List of the most common usernames.                       | Quick brute force username attempts.    | [SecLists](https://github.com/danielmiessler/SecLists/blob/master/Usernames/top-usernames-shortlist.txt)                         |
| xato-net-10-million-usernames.txt         | Extensive list of 10 million usernames.                  | Thorough username brute forcing.        | [SecLists](https://github.com/danielmiessler/SecLists/blob/master/Usernames/xato-net-10-million-usernames.txt)                   |
| 2023-200\_most\_used\_passwords.txt       | List of the 200 most commonly used passwords as of 2023. | Targeting commonly reused passwords.    | [SecLists](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt) |
| Default-Credentials/default-passwords.txt | List of default usernames and passwords for devices.     | Ideal for trying default credentials.   | [SecLists](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/default-passwords.txt)           |

**Practical Example: Dictionary Attack Script**

* The following Python script demonstrates how to perform a dictionary attack against a target system:

{% code overflow="wrap" %}
```python
import requests

ip = "SERVER_IP"  # Change to your instance IP
port = SERVER_PORT       # Change to your instance port

# Download a list of common passwords
passwords = requests.get("https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/500-worst-passwords.txt").text.splitlines()

# Attempt each password
for password in passwords:
    print(f"Attempted password: {password}")
    response = requests.post(f"http://{ip}:{port}/dictionary", data={'password': password})

    if response.ok and 'flag' in response.json():
        print(f"Correct password found: {password}")
        print(f"Flag: {response.json()['flag']}")
        break
```
{% endcode %}

**Script Functionality:**

* **Downloads a Wordlist:** Fetches a list of weak passwords.
* **Iterates Through Passwords:** Sends each password to the target system.
* **Analyzes Responses:** Checks for success and captures the flag if the password is correct.
* **Continues Until Success:** Proceeds through the list until the correct password is found or the list is exhausted.

**Conclusion:**

* Understanding the differences between dictionary and brute-force attacks is crucial for effective cybersecurity practices. Tailoring attacks based on human behavior and password choices can significantly enhance the likelihood of success.
