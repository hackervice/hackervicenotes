# Medusa

**Overview of Medusa**

* Medusa is a powerful, fast, and modular login brute-forcer used in cybersecurity.
* It supports various services for remote authentication, aiding penetration testers in evaluating login system security against brute-force attacks.

**Installation**

* Medusa is often pre-installed on penetration testing distributions.
*   To check if it's installed, run:

    ```bash
    medusa -h
    ```
*   To install on a Linux system:

    ```bash
    sudo apt-get -y update
    sudo apt-get -y install medusa
    ```

**Command Syntax and Parameters**

* Medusa's command-line interface allows for flexible configuration of targets, credentials, and modules.
*   Basic command structure:

    ```bash
    medusa [target_options] [credential_options] -M module [module_options]
    ```

| Parameter                  | Explanation                                    | Usage Example                                        |
| -------------------------- | ---------------------------------------------- | ---------------------------------------------------- |
| `-h HOST` or `-H FILE`     | Target options: single host or file of targets | `medusa -h 192.168.1.10` or `medusa -H targets.txt`  |
| `-u USERNAME` or `-U FILE` | Username options: single username or file      | `medusa -u admin` or `medusa -U usernames.txt`       |
| `-p PASSWORD` or `-P FILE` | Password options: single password or file      | `medusa -p password123` or `medusa -P passwords.txt` |
| `-M MODULE`                | Define the attack module (e.g., ssh, ftp)      | `medusa -M ssh`                                      |
| `-m "MODULE_OPTION"`       | Additional parameters for the module           | `medusa -M http -m "POST /login.php ..."`            |
| `-t TASKS`                 | Number of parallel login attempts              | `medusa -t 4`                                        |
| `-f` or `-F`               | Fast mode: stop after first successful login   | `medusa -f` or `medusa -F`                           |
| `-n PORT`                  | Specify a non-default port                     | `medusa -n 2222`                                     |
| `-v LEVEL`                 | Verbose output level                           | `medusa -v 4`                                        |

**Medusa Modules**

* Each module is tailored for specific authentication mechanisms. Common modules include:

| Module           | Service/Protocol                      | Description                                                                                 | Usage Example                                                                                                             |
| ---------------- | ------------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| FTP              | File Transfer Protocol                | Brute-forcing FTP login credentials, used for file transfers over a network.                | `medusa -M ftp -h 192.168.1.100 -u admin -P passwords.txt`                                                                |
| HTTP             | Hypertext Transfer Protocol           | Brute-forcing login forms on web applications over HTTP (GET/POST).                         | `medusa -M http -h www.example.com -U users.txt -P passwords.txt -m FORM:username=^USER^&password=^PASS^`                 |
| IMAP             | Internet Message Access Protocol      | Brute-forcing IMAP logins, often used to access email servers.                              | `medusa -M imap -h mail.example.com -U users.txt -P passwords.txt`                                                        |
| MySQL            | MySQL Database                        | Brute-forcing MySQL database credentials, commonly used for web applications and databases. | `medusa -M mysql -h 192.168.1.100 -u root -P passwords.txt`                                                               |
| POP3             | Post Office Protocol 3                | Brute-forcing POP3 logins, typically used to retrieve emails from a mail server.            | `medusa -M pop3 -h mail.example.com -U users.txt -P passwords.txt`                                                        |
| RDP              | Remote Desktop Protocol               | Brute-forcing RDP logins, commonly used for remote desktop access to Windows systems.       | `medusa -M rdp -h 192.168.1.100 -u admin -P passwords.txt`                                                                |
| SSHv2            | Secure Shell (SSH)                    | Brute-forcing SSH logins, commonly used for secure remote access.                           | `medusa -M ssh -h 192.168.1.100 -u root -P passwords.txt`                                                                 |
| Subversion (SVN) | Version Control System                | Brute-forcing Subversion (SVN) repositories for version control.                            | `medusa -M svn -h 192.168.1.100 -u admin -P passwords.txt`                                                                |
| Telnet           | Telnet Protocol                       | Brute-forcing Telnet services for remote command execution on older systems.                | `medusa -M telnet -h 192.168.1.100 -u admin -P passwords.txt`                                                             |
| VNC              | Virtual Network Computing             | Brute-forcing VNC login credentials for remote desktop access.                              | `medusa -M vnc -h 192.168.1.100 -u admin -P passwords.txt`                                                                |
| Web Form         | Brute-forcing Web Login Forms         | Brute-forcing login forms on websites using HTTP POST requests.                             | `medusa -M web-form -h www.example.com -U users.txt -P passwords.txt -m FORM:"username=^USER^&password=^PASS^:F=Invalid"` |
| SNMP             | Simple Network Management Protocol    | Brute-forcing SNMP community strings for network devices.                                   | `medusa -M snmp -h 192.168.1.100 -u public -P community_strings.txt`                                                      |
| LDAP             | Lightweight Directory Access Protocol | Brute-forcing LDAP logins for directory services.                                           | `medusa -M ldap -h ldap.example.com -U users.txt -P passwords.txt`                                                        |
| MSSQL            | Microsoft SQL Server                  | Brute-forcing Microsoft SQL Server logins.                                                  | `medusa -M mssql -h 192.168.1.100 -u sa -P passwords.txt`                                                                 |
| Oracle           | Oracle Database                       | Brute-forcing Oracle database logins.                                                       | `medusa -M oracle -h 192.168.1.100 -u admin -P passwords.txt`                                                             |
| Redis            | Remote Dictionary Server              | Brute-forcing Redis server logins.                                                          | `medusa -M redis -h 192.168.1.100 -u default -P passwords.txt`                                                            |

**Example Usage Scenarios**

1. **Targeting an SSH Server**:
   *   To test an SSH server at `192.168.0.100`:

       ```bash
       medusa -h 192.168.0.100 -U usernames.txt -P passwords.txt -M ssh
       ```
2. **Targeting Multiple Web Servers**:
   *   For basic HTTP authentication across multiple servers:

       ```bash
       medusa -H web_servers.txt -U usernames.txt -P passwords.txt -M http -m GET
       ```
3. **Testing for Empty or Default Passwords**:
   *   To check for empty or default passwords on `10.0.0.5`:

       ```bash
       medusa -h 10.0.0.5 -U usernames.txt -e ns -M service_name
       ```
   *   ### The command checks for:

       * **Empty passwords** (`-e n`): Attempts to log in with no password.
       * **Default passwords** (`-e s`): Attempts to log in using the username as the password.
       * Replace `service_name` with the appropriate module name (e.g., `ssh`, `ftp`, etc.) to target the specific service.

       #### Key Takeaways

       * **Medusa** is a versatile and efficient tool for conducting brute-force attacks on various authentication mechanisms.
       * Understanding the command syntax and available modules is crucial for effectively utilizing Medusa in penetration testing.
       * Always ensure you have permission to test the systems you are targeting to avoid legal issues.

