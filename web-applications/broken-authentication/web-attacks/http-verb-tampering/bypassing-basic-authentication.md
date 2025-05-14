# Bypassing Basic Authentication

* Exploiting HTTP Verb Tampering vulnerabilities is generally straightforward; it involves testing alternate HTTP methods to see how the web server responds.
* Automated vulnerability scanners can often identify insecure server configurations but may miss vulnerabilities due to insecure coding practices, which require active testing.

#### Identifying the Vulnerability:

* In a basic File Manager web application, certain functionalities (like deleting files) are restricted to authenticated users, prompting a Basic Authentication login.
* Attempting to access the restricted functionality without credentials results in a 401 Unauthorized error.

**Steps to Identify Restricted Pages:**

1. **Examine the Application:** Identify the URL of the restricted functionality (e.g., `/admin/reset.php`).
2. **Confirm Restrictions:** Access the `/admin` directory to verify that it requires authentication.

#### Exploiting the Vulnerability:

1. **Identify the HTTP Request Method:**
   * Use a tool like Burp Suite to intercept the request to `/admin/reset.php`, which is a GET request.
2. **Test Alternate HTTP Methods:**
   * Change the request method to POST and forward it. If the server still prompts for authentication, it indicates that both GET and POST requests are protected.
3. **Utilize Other HTTP Methods:**
   *   Since the server accepts multiple methods, send an OPTIONS request to check which methods are allowed:

       ```bash
       curl -i -X OPTIONS http://SERVER_IP:PORT/
       ```
   * The response indicates allowed methods (e.g., POST, OPTIONS, HEAD, GET).
4. **Attempt a HEAD Request:**
   * Change the intercepted request method to HEAD and forward it. A HEAD request is similar to GET but does not return a response body.
   * If successful, the server will not prompt for authentication, and the reset functionality will still execute.

#### Outcome:

* After sending the HEAD request to `/admin/reset.php`, the application does not return a login prompt or a 401 Unauthorized page.
* Upon returning to the File Manager interface, all files are deleted, confirming that the reset functionality was successfully triggered without needing admin access or credentials.

#### Conclusion:

* This exercise demonstrates how HTTP Verb Tampering can be exploited to bypass Basic Authentication due to insecure server configurations. Understanding these vulnerabilities is crucial for securing web applications against unauthorized access.

For a pratical example see the [Bypassing Basic Authentication](https://app.gitbook.com/o/eXpdkTjmLVS0nzVHsMPS/s/t4gp37tj8QBnGMMf3DZs/~/changes/162/ctf-labs/htb-certified-bug-bounty-hunter/web-attacks/bypassing-basic-authentication).
