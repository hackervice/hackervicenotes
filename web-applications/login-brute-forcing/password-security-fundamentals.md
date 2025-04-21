# Password Security Fundamentals

**The Importance of Strong Passwords**

* **First Line of Defense:** Passwords protect sensitive information and systems from unauthorized access.
* **Strength Matters:** Longer and more complex passwords exponentially increase the difficulty for attackers attempting brute-force methods.

**Characteristics of a Strong Password**

* **Length:** Aim for a minimum of 12 characters; longer passwords significantly increase security.
* **Complexity:** Use a mix of uppercase and lowercase letters, numbers, and symbols to enhance unpredictability.
* **Uniqueness:** Avoid reusing passwords across different accounts to limit the impact of a breach.
* **Randomness:** Steer clear of dictionary words and personal information to reduce vulnerability to common attack methods.

**Common Password Weaknesses**

* **Short Passwords:** Easily cracked due to limited combinations.
* **Common Words/Phrases:** Susceptible to dictionary attacks.
* **Personal Information:** Makes passwords easier to guess.
* **Reused Passwords:** Compromises multiple accounts if one is breached.
* **Predictable Patterns:** Simple sequences are easily guessed.

**Password Policies**

* Organizations often implement policies to enforce strong password practices, including:
  * Minimum length requirements.
  * Complexity rules.
  * Password expiration and history guidelines.

**The Perils of Default Credentials**

* **Default Passwords:** Pre-set passwords are often simple and widely known, making them prime targets for attackers.
* **Default Usernames:** Common usernames like "admin" or "root" provide attackers with a predictable starting point, making brute-force attacks easier.

**Brute-Forcing and Password Security**

* **Weak vs. Strong Passwords:** Weak passwords are easily breached, while strong passwords require significant time and resources to crack.
* **Pentester Insights:**
  * Evaluate system vulnerabilities based on password policies and user behavior.
  * Choose appropriate tools based on password complexity.
  * Identify and exploit weak points, such as default credentials.

**Example of Default Credentials**

| Device/Manufacturer  | Default Username | Default Password | Device Type                  |
| -------------------- | ---------------- | ---------------- | ---------------------------- |
| Linksys Router       | admin            | admin            | Wireless Router              |
| D-Link Router        | admin            | admin            | Wireless Router              |
| Netgear Router       | admin            | password         | Wireless Router              |
| TP-Link Router       | admin            | admin            | Wireless Router              |
| Cisco Router         | cisco            | cisco            | Network Router               |
| Asus Router          | admin            | admin            | Wireless Router              |
| Belkin Router        | admin            | password         | Wireless Router              |
| Zyxel Router         | admin            | 1234             | Wireless Router              |
| Samsung SmartCam     | admin            | 4321             | IP Camera                    |
| Hikvision DVR        | admin            | 12345            | Digital Video Recorder (DVR) |
| Axis IP Camera       | root             | pass             | IP Camera                    |
| Ubiquiti UniFi AP    | ubnt             | ubnt             | Wireless Access Point        |
| Canon Printer        | admin            | admin            | Network Printer              |
| Honeywell Thermostat | admin            | 1234             | Smart Thermostat             |
| Panasonic DVR        | admin            | 12345            | Digital Video Recorder (DVR) |

_Credits to HTB_

Understanding password security is crucial for both pentesters and organizations aiming to defend against brute-force attacks. By implementing strong password practices, we can significantly enhance our security posture and protect sensitive information.
