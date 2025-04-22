# Hybrid Attacks

**Hybrid Attacks Overview:**

* Organizations often require periodic password changes to enhance security, but this can lead to predictable password patterns if users aren't educated on proper password hygiene.

<figure><img src="../../../.gitbook/assets/image (173).png" alt=""><figcaption><p>Credits to HTB</p></figcaption></figure>

* A common insecure practice is for users to make minor modifications to their passwords (e.g., appending a number or special character), such as changing "Summer2023" to "Summer2023!" or "Summer2024."
* This predictability creates vulnerabilities that hybrid attacks can exploit.

**How Hybrid Attacks Work:**

<figure><img src="../../../.gitbook/assets/image (171).png" alt=""><figcaption><p>Credits to HTB</p></figcaption></figure>

* **Initial Phase:** Attackers start with a dictionary attack using a wordlist that includes common passwords, industry terms, and personal information related to the target organization.
* **Transition to Brute Force:** If the dictionary attack fails, the hybrid attack shifts to a targeted brute-force approach. Instead of random combinations, it modifies known passwords by appending numbers or special characters, significantly reducing the search space.

**Advantages of Hybrid Attacks:**

* Hybrid attacks combine the strengths of both dictionary and brute-force techniques, making them highly effective against predictable password patterns.
* They can be tailored to exploit any observed password behaviors within a target organization.

**Example of Filtering Passwords:**

* To target an organization with specific password policies (e.g., minimum length, inclusion of uppercase, lowercase, and numbers), attackers can filter a wordlist using command-line tools like `grep`:
  1.  **Download Wordlist:**

      {% code overflow="wrap" %}
      ```bash
      wget https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/darkweb2017_top-10000.txt
      ```
      {% endcode %}
  2.  **Filter for Minimum Length:**

      ```bash
      grep -E '^.{8,}$' darkweb2017_top-10000 > darkweb2017-minlength.txt
      ```
  3.  **Filter for Uppercase Letters:**

      ```bash
      grep -E '[A-Z]' darkweb2017-minlength.txt > darkweb2017-uppercase.txt
      ```
  4.  **Filter for Lowercase Letters:**

      ```bash
      grep -E '[a-z]' darkweb2017-uppercase.txt > darkweb2017-lowercase.txt
      ```
  5.  **Filter for Numbers:**

      ```bash
      grep -E '[0-9]' darkweb2017-lowercase.txt > darkweb2017-number.txt
      ```
  6.  **Count Results:**

      ```bash
      wc -l darkweb2017-number.txt
      ```
* This process narrows down the potential passwords significantly, enhancing the efficiency of subsequent cracking attempts.

**Credential Stuffing:**

<figure><img src="../../../.gitbook/assets/image (170).png" alt=""><figcaption><p>Credits to HTB</p></figcaption></figure>

* Credential stuffing attacks exploit the common practice of password reuse across multiple accounts, making it easier for attackers to gain unauthorized access.
* Attackers acquire lists of compromised usernames and passwords from data breaches or phishing scams.
* They then target online services (e.g., social media, email, banking) and use automated tools to test the stolen credentials against these services.
* A successful match allows attackers to access accounts, leading to data theft, identity fraud, and other malicious activities.

**The Password Reuse Problem:**

* The widespread reuse of passwords is a critical issue that facilitates credential stuffing. A breach on one platform can compromise multiple accounts.
* Emphasizes the need for users to create strong, unique passwords for each service and implement security measures like multi-factor authentication to mitigate risks.

**Conclusion:**

* Understanding hybrid attacks and credential stuffing is essential for improving cybersecurity practices. Organizations must educate users on password hygiene and implement robust security measures to protect against these threats.
