# Metasploit multi/handler

Multi/Handler is a key tool for catching reverse shells, especially when using Meterpreter and staged payloads. Here’s how to use it:

1. **Start Metasploit**: Open `msfconsole`.
2. **Load Multi/Handler**: Type `use multi/handler` and press enter.
3.  **Set Options**: Use the `options` command to view settings. You need to configure:

    * `PAYLOAD`: The specific payload for your target.
    * `LHOST`: The listening address (e.g., your `tun0` address on TryHackMe).
    * `LPORT`: The listening port.

    Set these with:

    ```bash
    set PAYLOAD <payload>
    set LHOST <listen-address>
    set LPORT <listen-port>
    ```
4. **Start the Listener**: Use `exploit -j` to run the handler in the background.
5. **Receive the Shell**: When the staged payload connects, Metasploit sends the rest of the payload, giving you a reverse shell.
6. **Manage Sessions**: If the handler is backgrounded, use `sessions 1` to bring it to the foreground. If multiple sessions exist, list them with `sessions` and select the appropriate one.
