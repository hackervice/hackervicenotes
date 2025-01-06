# 🐚 Shells

* A shell is an interface for interacting with a Command Line Environment (CLI). Common examples include:
  * **Linux**: bash, sh
  * **Windows**: cmd.exe, PowerShell
* **Remote System Targeting**:
  * It's possible to exploit applications on a remote server (e.g., web servers) to execute arbitrary code.
  * This initial access is crucial for obtaining a shell on the target system.
*   **Types of Shells**:

    * **Reverse Shell**: The remote server sends command line access back to the attacker.
      * The target executes code that connects back to your computer.
      * You set up a listener on your computer using tools from previous tasks to receive the connection.
      * Effective for bypassing firewall rules that may block connections to arbitrary ports on the target.
      * Drawback: Requires configuration of your own network to accept the incoming shell connection.
    * **Bind Shell**: The server opens a port that the attacker can connect to for executing commands.
      * The code executed on the target starts a listener attached to a shell directly on that target.
      * This listener is opened to the internet, allowing you to connect to the port and obtain remote code execution.
      * Advantage: No configuration needed on your own network.
      * Potential drawback: May be blocked by firewalls protecting the target.



## Tools

To receive reverse shells and send bind shells, we need malicious shell code and a method to interface with the resulting shell. Here are some key tools used for these purposes:

**Netcat**:

* Known as the "Swiss Army Knife" of networking.
* Used for various network interactions, including banner grabbing and receiving reverse shells.
* Can connect to remote ports for bind shells.
* **Limitations**: Netcat shells are unstable by default but can be improved with specific techniques (to be covered later).

**Socat**:

* An enhanced version of Netcat with more capabilities.
* Generally provides more stable shells compared to Netcat.
* **Drawbacks**:
  * More complex syntax.
  * Not typically installed by default on many systems.
* Both Netcat and Socat have .exe versions for Windows.

**Metasploit -- multi/handler**:

* Part of the Metasploit framework, used to receive reverse shells.
* Offers a robust way to obtain stable shells with various options for enhancement.
* Essential for interacting with Meterpreter shells and handling staged payloads (to be explored in task 9).

**Msfvenom**:

* A standalone tool within the Metasploit Framework for generating payloads on the fly.
* Focuses on creating reverse and bind shell payloads.
* A powerful tool that will be discussed in detail in a dedicated task.

**Additional Resources**:

* **Payloads All the Things**: A [repository](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md) of shells in various programming languages.
* **PentestMonkey Reverse Shell Cheatsheet**: A commonly used [resource](https://web.archive.org/web/20200901140719/http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet) for reverse shells.
* **Kali Linux**: Comes pre-installed with various web shells located at `/usr/share/webshells`.
* **SecLists Repo**: Primarily for wordlists but also contains[ useful code ](https://github.com/danielmiessler/SecLists)for obtaining shells.
