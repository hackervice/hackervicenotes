# What is Authentication

**Definition of Authentication:**

* Authentication is the process of verifying the identity of a system entity or resource, as defined in [RFC 4949](https://datatracker.ietf.org/doc/rfc4949/). It confirms that an entity is who they claim to be.
* **Key Distinction:** Authentication is different from authorization, which grants access to resources. Understanding this difference is crucial for approaching security topics effectively.

**Authentication vs. Authorization:**

| Authentication                                                                                                                    | Authorization                                                 |
| --------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| Determines whether users are who they claim to be                                                                                 | Determines what users can and cannot access                   |
| Challenges the user to validate credentials (for example, through passwords, answers to security questions, or facial recognition | Verifies whether access is allowed through policies and rules |
| Usually done before authorization                                                                                                 | Usually done after successfull authentication                 |
| It usually needs the user's login details                                                                                         | While it need's privilege or security levels                  |
| Generally, transmits info through an ID Token                                                                                     | Generally, transmits info through an Access Token             |

**Common Authentication Method:**

* The most prevalent method in web applications is the login form, where users enter a username and password. This is seen across various platforms like email services, online banking, and educational sites.
* Authentication serves as the primary defense against unauthorized access, and penetration testers focus on assessing the security of these implementations.

**Categories of Authentication Methods:**

1. **Knowledge-based Authentication:**
   * Relies on something the user knows (e.g., passwords, PINs, answers to security questions).
2. **Ownership-based Authentication:**
   * Based on something the user possesses (e.g., ID cards, security tokens, smartphones with authentication apps).
3. **Inherence-based Authentication:**
   * Involves something the user is or does (e.g., biometrics like fingerprints, facial recognition, voice patterns).

| Knowledge         | Ownership         | Inherence         |
| ----------------- | ----------------- | ----------------- |
| Password          | ID card           | Fingerprint       |
| PIN               | Security Token    | Facial Pattern    |
| Security Question | Authenticator App | Voice Recognition |

**Single-Factor vs. Multi-Factor Authentication:**

* **Single-Factor Authentication:** Uses one method (e.g., just a password).
* **Multi-Factor Authentication (MFA):** Combines multiple methods (e.g., password + time-based one-time password). When two factors are used, it is often referred to as 2-Factor Authentication (2FA).
