# Basic HTTP authentication

**Overview of Basic HTTP Authentication:**

* Basic HTTP Authentication (Basic Auth) is a simple method used by web applications to secure sensitive data and functionalities.
* It operates as a challenge-response protocol where the server requests user credentials before granting access to protected resources.
* When a user tries to access a restricted area, the server responds with a `401 Unauthorized` status and a `WWW-Authenticate` header, prompting the browser to display a login dialog.

**How Basic Auth Works:**

1. The user enters their username and password.
2. The browser concatenates the credentials into a single string in the format `username:password`.
3. This string is encoded using Base64 and included in the `Authorization` header of subsequent requests, formatted as `Basic <encoded_credentials>`.
4. The server decodes the credentials, verifies them against its database, and either grants or denies access.

**Example of Basic Auth in HTTP GET Request:**

```http
GET /protected_resource HTTP/1.1
Host: www.example.com
Authorization: Basic YWxpY2U6c2VjcmV0MTIz
```

**Exploiting Basic Auth with Hydra:**

* To demonstrate brute-forcing Basic HTTP Authentication, we will use Hydra with the `http-get` service.

**Setup:**

1.  **Download Wordlist (if needed):**

    {% code overflow="wrap" %}
    ```bash
    curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt
    ```
    {% endcode %}
2.  **Hydra Command:**

    {% code overflow="wrap" %}
    ```bash
    hydra -l basic-auth-user -P 2023-200_most_used_passwords.txt 127.0.0.1 http-get / -s 81
    ```
    {% endcode %}

**Command Breakdown:**

* `-l basic-auth-user`: Specifies the username for the login attempt.
* `-P 2023-200_most_used_passwords.txt`: Indicates the password list file to use for the brute-force attack.
* `127.0.0.1`: The target IP address (localhost in this case).
* `http-get /`: Specifies that the target service is an HTTP server and the attack should be performed using HTTP GET requests to the root path (`/`).
* `-s 81`: Overrides the default HTTP port and sets it to 81.

**Execution:**

* Upon running the command, Hydra will systematically attempt each password from the specified list against the Basic Auth login.
* Eventually, it will identify the correct password for `basic-auth-user`, allowing access to the protected resource and enabling the retrieval of the flag.

**Conclusion:**

* Basic HTTP Authentication is straightforward but vulnerable to brute-force attacks. Tools like Hydra can efficiently exploit these vulnerabilities, highlighting the importance of using stronger authentication methods and implementing security best practices.
