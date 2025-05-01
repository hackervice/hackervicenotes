# Attacks on Authentication

Attacks on authentication can be categorized based on the three types of authentication methods: knowledge-based, ownership-based, and inherence-based.

***

**1. Attacking Knowledge-based Authentication:**

* **Vulnerability:** This method is easy to attack due to its reliance on static personal information (e.g., passwords, security questions).
* **Common Attack Vectors:**
  * **Guessing/Brute-Forcing:** Attackers can guess or use automated tools to brute-force passwords.
  * **Social Engineering:** Manipulating individuals to reveal personal information.
  * **Data Breaches:** Exploiting leaked information from compromised databases.
* **Focus:** This module will primarily address the weaknesses in knowledge-based authentication due to its prevalence and ease of exploitation.

***

**2. Attacking Ownership-based Authentication:**

* **Strengths:** More resistant to common threats like phishing and password guessing since it relies on physical items (e.g., hardware tokens, smart cards).
* **Challenges:**
  * **Cost and Logistics:** Distributing and managing physical tokens can be complex and costly, limiting adoption in large-scale environments.
* **Vulnerabilities:**
  * **Physical Attacks:** Theft or cloning of physical tokens (e.g., NFC badges).
  * **Cryptographic Attacks:** Exploiting weaknesses in the algorithms used for authentication.

***

**3. Attacking Inherence-based Authentication:**

* **Advantages:** Offers convenience as users provide biometric data (e.g., fingerprints, facial scans) instead of remembering passwords or carrying tokens.
* **Concerns:**
  * **Privacy and Data Security:** Biometric data must be securely stored and managed to prevent misuse.
  * **Bias in Algorithms:** Potential biases in biometric recognition systems can lead to unfair treatment of users.
* **Critical Vulnerability:** If a data breach occurs, biometric data cannot be changed (unlike passwords). For example, a breach in a company managing biometric smart locks exposed users' fingerprints and facial patterns, which cannot be altered, posing a significant security risk.
