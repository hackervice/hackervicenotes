# Login Forms

**Overview of Login Forms**

* Custom login forms are common in web applications, serving as primary authentication mechanisms.
* They consist of HTML forms with input fields for username and password, and a submit button.

**Basic Structure of a Login Form**

*   Example HTML structure:

    ```html
    <form action="/login" method="post">
      <label for="username">Username:</label>
      <input type="text" id="username" name="username"><br><br>
      <label for="password">Password:</label>
      <input type="password" id="password" name="password"><br><br>
      <input type="submit" value="Submit">
    </form>
    ```
* Submits a POST request to the server with the credentials.

**Understanding HTTP POST Requests**

* POST request includes:
  * URL endpoint (e.g., `/login`)
  * Content-Type header (e.g., `application/x-www-form-urlencoded`)
  * Body containing username and password as key-value pairs.

**Brute-Forcing with Hydra**

* **Hydra** is a tool for automating login attempts against forms.
*   Command structure:

    ```bash
    hydra [options] target http-post-form "path:params:condition_string"
    ```

**Condition Strings in Hydra**

* **Failure Condition (F=...)**: Identifies failed login attempts by checking for specific error messages.
* **Success Condition (S=...)**: Identifies successful logins, often through HTTP status codes or specific content in the response.

**Manual Inspection of Login Forms**

* Use browser developer tools to inspect the HTML and network requests.
* Key points to note:
  * Method: POST
  * Input field names: username and password

**Proxy Interception**

* Tools like Burp Suite or OWASP ZAP can intercept and analyze network traffic for more complex forms.

**Constructing the Params String for Hydra**

* The params string includes:
  * Path to the form
  * Key-value pairs for username and password
  * Failure condition for identifying unsuccessful attempts

**Example Params String**

*   For a form with:

    * Path: `/`
    * Username field: `username`
    * Password field: `password`
    * Failure message: "Invalid credentials"

    The params string would be:

    ```
    /:username=^USER^&password=^PASS^:F=Invalid credentials
    ```

**Running Hydra**

* Download necessary wordlists for [usernames](https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Usernames/top-usernames-shortlist.txt) and [passwords](https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt).
*   Example Hydra command:

    {% code overflow="wrap" %}
    ```bash
    hydra -L top-usernames-shortlist.txt -P 2023-200_most_used_passwords.txt -f SERVER_IP -s SERVER_PORT http-post-form "/:username=^USER^&password=^PASS^:F=Invalid credentials"
    ```
    {% endcode %}

**Important Notes**

* Ensure the params string is accurate for successful attacks.
* After finding valid credentials, log in to retrieve any necessary flags or information.
