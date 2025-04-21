# Login Brute Forcing

**What is Brute Forcing?**

* **Definition:** A trial-and-error method used to crack passwords, login credentials, or encryption keys by systematically trying every possible combination until the correct one is found.
* **Analogy:** Similar to a thief trying every key on a keyring to unlock a treasure chest.

**Factors Influencing Brute Force Success:**

* **Password Complexity:** Longer passwords with a mix of characters are harder to crack.
* **Computational Power:** Modern hardware can attempt billions of combinations per second.
* **Security Measures:** Account lockouts, CAPTCHAs, and other defenses can hinder brute-force attempts.

**How Brute Forcing Works:**

1. **Start:** The attacker initiates the process using specialized software.
2. **Generate Combination:** The software creates potential password combinations based on parameters.
3. **Apply Combination:** The generated combination is tested against the target system.
4. **Check Success:** The system checks if the combination matches the stored password.
5. **Access Granted:** If successful, the attacker gains unauthorized access.
6. **Repeat:** The process continues until the correct combination is found or the attacker gives up.

**Types of Brute Forcing Techniques:**

| Method                    | Description                                                                                                                   | Example                                                                                                                                                  | Best Used When...                                                                                                                           |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `Simple Brute Force`      | Systematically tries all possible combinations of characters within a defined character set and length range.                 | Trying all combinations of lowercase letters from 'a' to 'z' for passwords of length 4 to 6.                                                             | No prior information about the password is available, and computational resources are abundant.                                             |
| `Dictionary Attack`       | Uses a pre-compiled list of common words, phrases, and passwords.                                                             | Trying passwords from a list like 'rockyou.txt' against a login form.                                                                                    | The target will likely use a weak or easily guessable password based on common patterns.                                                    |
| `Hybrid Attack`           | Combines elements of simple brute force and dictionary attacks, often appending or prepending characters to dictionary words. | Adding numbers or special characters to the end of words from a dictionary list.                                                                         | The target might use a slightly modified version of a common password.                                                                      |
| `Credential Stuffing`     | Leverages leaked credentials from one service to attempt access to other services, assuming users reuse passwords.            | Using a list of usernames and passwords leaked from a data breach to try logging into various online accounts.                                           | A large set of leaked credentials is available, and the target is suspected of reusing passwords across multiple services.                  |
| `Password Spraying`       | Attempts a small set of commonly used passwords against a large number of usernames.                                          | Trying passwords like 'password123' or 'qwerty' against all usernames in an organization.                                                                | Account lockout policies are in place, and the attacker aims to avoid detection by spreading attempts across multiple accounts.             |
| `Rainbow Table Attack`    | Uses pre-computed tables of password hashes to reverse hashes and recover plaintext passwords quickly.                        | Pre-computing hashes for all possible passwords of a certain length and character set, then comparing captured hashes against the table to find matches. | A large number of password hashes need to be cracked, and storage space for the rainbow tables is available.                                |
| `Reverse Brute Force`     | Targets a single password against multiple usernames, often used in conjunction with credential stuffing attacks.             | Using a leaked password from one service to try logging into multiple accounts with different usernames.                                                 | A strong suspicion exists that a particular password is being reused across multiple accounts.                                              |
| `Distributed Brute Force` | Distributes the brute forcing workload across multiple computers or devices to accelerate the process.                        | Using a cluster of computers to perform a brute-force attack significantly increases the number of combinations that can be tried per second.            | The target password or key is highly complex, and a single machine lacks the computational power to crack it within a reasonable timeframe. |

_Credits to HTB_



**Role of Brute Forcing in Penetration Testing:**

* **Purpose:** Brute forcing is a vital tool in penetration testing, simulating attacks to identify vulnerabilities.
* **When to Use:**
  * When other access methods fail.
  * To expose weak password policies.
  * To target specific accounts, especially those with elevated privileges.

Join us in exploring the intricacies of brute forcing and learn how to protect against these attacks effectively!
