# Obtaining Session Identifiers without User Interaction

As a **pentester**, there are various techniques to obtain session identifiers, which can be categorized into two groups: those requiring user interaction and those that do not. This section focuses on **session ID-obtaining attacks that do not require user interaction**.

#### 🔍 Obtaining Session Identifiers via Traffic Sniffing

**Traffic sniffing** is a common technique used by penetration testers to assess network security. By connecting their devices to available Ethernet sockets, testers can monitor network traffic and identify potential vulnerabilities. However, this method requires the attacker and the victim to be on the same local network, as remote traffic sniffing is not feasible.

**Key Requirements for Traffic Sniffing:**

* The attacker must be on the same local network as the victim.
* The traffic must be unencrypted (HTTP).

Since HTTP transmits data in plaintext, an attacker can capture sensitive information, including usernames, passwords, and session identifiers. In contrast, encrypted traffic (via SSL or IPsec) makes it significantly more challenging to obtain such data.

#### 🛠️ Testing for Session Identifiers via Traffic Sniffing

1. **Simulate the Attacker**:
   * Navigate to the target application (e.g., `http://example.com`).
   * Use developer tools (Shift+Ctrl+I in Firefox) to identify session cookies, such as `auth-session`.
2. **Start Traffic Sniffing**:
   *   Launch Wireshark to monitor network traffic:

       ```bash
       bashCopy Codesudo -E wireshark
       ```
   * Select the appropriate network interface (e.g., `tun0`) and start capturing packets.
3. **Simulate the Victim**:
   * Open a new private browsing window and log in to the application using valid credentials.
4. **Obtain the Victim's Cookie through Packet Analysis**:
   * In Wireshark, apply a filter to display only HTTP traffic. This can be done by entering `http` in the filter bar and pressing Enter.
   * Search for the `auth-session` cookie within the captured packets. Use the "Find Packet" feature to locate packets containing the session identifier.
   * Right-click on the relevant packet and select "Copy" > "Value" to extract the session identifier.
5. **Hijack the Victim's Session**:
   * Return to the browser where you initially accessed the application.
   * Open developer tools, navigate to storage, and replace the current cookie's value with the one obtained from Wireshark (removing the `auth-session=` part).
   * Refresh the page to see if you are now logged in as the victim.

#### 📝 Obtaining Session Identifiers Post-Exploitation (Web Server Access)

During the **post-exploitation phase**, session identifiers and session data can be retrieved from a compromised web server's disk or memory. While an attacker can do more than just obtain session data, they may prefer to limit their actions to avoid detection.

**PHP Session Identifiers**

*   PHP session identifiers are typically stored in a directory specified by the `session.save_path` entry in the `php.ini` file. To locate this path, you can run:

    ```bash
    locate php.ini
    cat /etc/php/7.4/apache2/php.ini | grep 'session.save_path'
    ```
* The default path is often `/var/lib/php/sessions`. The session files follow the naming convention `sess_<sessionID>`.

**Java Session Identifiers**

* In Java applications, session identifiers are managed by the session manager. The default implementation stores active sessions, while an optional implementation can save sessions across server restarts. The session data file is typically named `SESSIONS.ser`.

**.NET Session Identifiers**

* In .NET applications, session data can be found in:
  * The application worker process (InProc Session mode).
  * StateServer (OutProc Session mode).
  * An SQL Server.

#### 📝 Obtaining Session Identifiers Post-Exploitation (Database Access)

If you have direct access to a database (e.g., through SQL injection or valid credentials), check for stored user sessions. Here’s an example of how to extract session data:

```sql
SHOW DATABASES;
USE project;
SHOW TABLES;
SELECT * FROM users;
```

If you find a table named `all_sessions`, you can extract session data with:

```sql
SELECT * FROM all_sessions;
SELECT * FROM all_sessions WHERE id=3;
```

By successfully extracting session data, you can authenticate as the user associated with that session.

By understanding these techniques, pentesters can effectively identify and exploit vulnerabilities related to session identifiers, helping organizations improve their security measures.
