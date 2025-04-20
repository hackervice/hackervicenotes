# Identifying

<figure><img src="../../../.gitbook/assets/image (151).png" alt=""><figcaption><p>Credits HTB</p></figcaption></figure>

1. **Confirming SSTI Vulnerability**:
   * **Inject Test String**: Use a test string that contains special characters with semantic meaning in template engines:
     * ```
       ${{<%[%'"}}%\.
       ```
   * **Observe Behavior**: Inject this string into the web application and observe the response. If the application throws an error, it may indicate a potential SSTI vulnerability.
2. **Practical Example**:
   *   Access the web application where you can input a name:

       ```
       http://<SERVER_IP>:<PORT>/
       ```
   * Inject the test string into the input field and submit.
   * Check the response for any error messages that suggest a syntax violation.
3. **Identifying the Template Engine**:
   *   **Initial Payload Injection**: Start by injecting a simple arithmetic payload to determine the template engine:

       ```
       ${7*7}
       ```
   * **Analyze the Response**:
     * If the payload executes successfully, follow the green arrow in your decision-making process.
     * If it does not execute, follow the red arrow.
4. **Further Payload Testing**:
   *   If the first payload did not execute, try the following:

       ```
       {{7*7}}
       ```
   * Check the response again:
     * If this payload executes, follow the green arrow.
     * If it does not, continue following the red arrow.
5. **Final Payload Injection**:
   *   If the previous payload executed, test with:

       ```
       {{7*'7'}}
       ```
   * Analyze the result:
     * In **Jinja**, the output will be `7777777`.
     * In **Twig**, the output will be `49`.
   * Based on the output, you can deduce which template engine the web application is using.



