# Bypassing Security Filters

* The more common type of HTTP Verb Tampering vulnerability arises from insecure coding practices during web application development.
* These vulnerabilities occur when security filters do not cover all HTTP methods, allowing attackers to bypass protections against malicious requests.

#### Identifying the Vulnerability:

* In the File Manager web application, attempting to create a new file with special characters (e.g., `test;`) results in a "Malicious Request Denied!" message, indicating that the application has security filters in place to block injection attempts.
* Despite the filters, an HTTP Verb Tampering attack may allow us to bypass these security measures.

#### Exploiting the Vulnerability:

1. **Intercept the Request:**
   * Use Burp Suite to intercept the request when trying to create a new file with a special character.
   * Change the request method to GET, which may not be covered by the security filter.
2. **Send the Modified Request:**
   * Modify the intercepted request to include the filename parameter with special characters (e.g., `test%3B`).
   * Forward the request.
3. **Check for Successful File Creation:**
   * If the request is successful, the file will be created without triggering the security filter, confirming that the filter only checks POST parameters.

#### Confirming the Bypass:

* To further test the vulnerability, attempt a command injection by using a filename that includes a command (e.g., `file1; touch file2;`).
*   Change the request method to GET again and send the modified request with the command injection payload:

    ```bash
    filename parameter: file1%3B+touch+file2%3B
    ```

4. **Verify File Creation:**
   * After sending the request, check the File Manager interface to see if both `file1` and `file2` were created.
   * If both files appear, it confirms that the HTTP Verb Tampering vulnerability allowed the command injection to succeed.

#### Conclusion:

* This exercise illustrates how insecure coding practices can lead to vulnerabilities that allow attackers to bypass security filters through HTTP Verb Tampering.
* Without this vulnerability, the web application might have been secure against command injection attacks, highlighting the importance of comprehensive security measures that account for all HTTP methods.

For a pratical example see the [Bypassing Security Filters](https://app.gitbook.com/o/eXpdkTjmLVS0nzVHsMPS/s/t4gp37tj8QBnGMMf3DZs/~/changes/162/ctf-labs/htb-certified-bug-bounty-hunter/web-attacks/bypassing-security-filters).
