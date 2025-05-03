# Further Session Attacks

* This section discusses two advanced attack vectors related to the flawed handling of session tokens in web applications: Session Fixation and Improper Session Timeout.

***

**1. Session Fixation:**

* **Definition:** [Session fixation](https://owasp.org/www-community/attacks/Session_fixation) is an attack that allows an attacker to take over a victim's session by coercing them into using a session token chosen by the attacker.
* **Mechanism:**
  * A vulnerable web application does not generate a new session token after successful authentication.
  * The attacker first obtains a valid session token (e.g., `a1b2c3d4e5f6`) by authenticating to the application.
  * The attacker then logs out, invalidating their session but retaining knowledge of the session token.
  *   The attacker sends a link to the victim that includes the session token as a parameter:

      ```
      http://example.com/?sid=a1b2c3d4e5f6
      ```
  * When the victim clicks the link, the web application sets the session cookie to the attacker-provided token.
  * Upon authentication, the victim's browser sends the attacker’s session token, allowing the attacker to hijack the victim's session.
* **Prevention:**
  * To mitigate session fixation attacks, web applications must generate a new, randomly assigned session token after successful authentication.

***

**2. Improper Session Timeout:**

* **Definition:** Improper session timeout occurs when a web application does not define a session timeout for session tokens, allowing them to remain valid indefinitely.
* **Risks:**
  * If a session token is valid forever, an attacker who hijacks a session can maintain access indefinitely, posing a significant security risk.
* **Best Practices:**
  * Implement a session timeout that is appropriate for the application's context.
  * For sensitive applications (e.g., those handling health data), a shorter session timeout (in minutes) is advisable.
  * For less sensitive applications (e.g., social media), a longer session timeout (in hours) may be acceptable.

***

**Conclusion:**

* Proper handling of session tokens is critical for web application security. Implementing measures against session fixation and ensuring appropriate session timeouts can significantly reduce the risk of session hijacking and unauthorized access. Regularly review session management practices to align with security best practices and the specific needs of the application
