# Session Fixation

Understanding **session fixation** is essential for identifying vulnerabilities in web applications. This attack occurs when an attacker can fixate a valid session identifier, tricking a victim into logging in with that identifier. Once the victim logs in, the attacker can hijack the session.

#### 🔍 Stages of Testing for Session Fixation Vulnerabilities

**Stage 1: Obtain a Valid Session Identifier**

* **Testing Method**: Navigate to the target application without logging in. Many applications assign valid session identifiers to users simply by browsing.
* **Note**: If possible, create an account to obtain a valid session identifier.

**Stage 2: Fixate the Valid Session Identifier**

*   **Testing Method**: Check if the application maintains the same session identifier post-login. This can be a vulnerability if:

    * The session identifier remains unchanged after authentication.
    * The application accepts session identifiers from URL query strings or POST data.

    For example, if a session-related parameter is included in the URL and is propagated to the session cookie, this indicates a potential session fixation vulnerability.

    **Stage 3: Trick the Victim into Using the Fixed Session Identifier**

    * **Testing Method**: Craft a URL that includes the fixed session identifier and lure the victim into clicking it. If the victim accesses this URL, the application will assign the session identifier to them, allowing the attacker to hijack the session.

    #### 🛠️ Testing for Session Fixation

    Here’s a structured approach to testing for session fixation vulnerabilities:

    1. **Identify the Session Fixation Vulnerability**:
       * Navigate to the target application (e.g., `http://example.com/?redirect_uri=/complete.html&token=<RANDOM TOKEN VALUE>`).
       * Observe the URL format and use developer tools (Shift+Ctrl+I in Firefox) to inspect the cookies. Look for a session cookie (e.g., `PHPSESSID`) and check if its value matches the token parameter in the URL.
    2. **Simulate Session Fixation**:
       *   Open a new private browsing window and navigate to a crafted URL, such as:

           ```
           http://example.com/?redirect_uri=/complete.html&token=IControlThisCookie
           ```
       * Again, use developer tools to verify that the `PHPSESSID` cookie's value is now `IControlThisCookie`.
       * This confirms the presence of a session fixation vulnerability, as the attacker can send a similar URL to a victim. If the victim logs in, the attacker can hijack their session since the session identifier is already known.

    #### 📝 Example Code Analysis

    To further understand how session fixation vulnerabilities can occur, consider the following PHP code snippet:

    ```php
    <?php
        if (!isset($_GET["token"])) {
            session_start();
            header("Location: /?redirect_uri=/complete.html&token=" . session_id());
        } else {
            setcookie("PHPSESSID", $_GET["token"]);
        }
    ?>
    ```

    * **Code Breakdown**:
      * `if (!isset($_GET["token"])) { session_start(); ... }`: If the token parameter is not defined, a new session is started, generating a valid session identifier.
      * `header("Location: /?redirect_uri=/complete.html&token=" . session_id());`: The user is redirected with the session ID appended to the token parameter.
      * `setcookie("PHPSESSID", $_GET["token"]);`: If the token parameter is set, the session cookie (`PHPSESSID`) is updated with the token's value.

    This code demonstrates how an attacker can exploit the application by crafting a URL that sets the session identifier to a value they control.
