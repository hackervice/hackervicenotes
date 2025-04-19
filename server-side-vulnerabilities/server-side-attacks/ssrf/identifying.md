# Identifying

1. **Access the Web Application**:
   * Navigate to the web application at `http://<SERVER_IP>:<PORT>/`.
   * Look for functionality that allows you to do a POST request.
2. **Monitor Requests**:
   * Use a tool like Burp Suite to intercept and analyze the requests.
   * Check for a request that includes a parameter (e.g., `dateserver`) containing a URL.
3. **Test for SSRF**:
   * Supply a URL pointing to your own system (e.g., `http://<YOUR_IP>:<PORT>`) in the `dateserver` parameter.
   *   Set up a netcat listener to receive incoming connections:

       ```
       nc -lnvp 8000
       ```
4. **Confirm Connection**:
   * If the web application is vulnerable, you should see a connection in your netcat listener, confirming the SSRF vulnerability.
5. **Check for Response**:
   * To verify that the SSRF is not blind, point the application to itself using `http://127.0.0.1/index.php`.
   * If you receive the web application's HTML code in the response, the SSRF vulnerability is confirmed.

**Enumerating the System**

1. **Conduct a Port Scan**:
   * Use the SSRF vulnerability to scan for open ports on the web server.
   *   Create a list of ports to scan:

       {% code overflow="wrap" %}
       ```
       seq 1 10000 > ports.txt
       ```
       {% endcode %}
2. **Fuzz for Open Ports**:
   *   Use a fuzzer like `ffuf` to test each port:

       {% code overflow="wrap" %}
       ```
       ffuf -w ./ports.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://127.0.0.1:FUZZ/&date=2024-01-01" -fr "Failed to connect"
       ```
       {% endcode %}
3. **Analyze Results**:
   * Look for responses that do not contain the error message.
   * For example, if you see a successful response for `FUZZ: 3306`, it indicates that a service (like a SQL database) is running on that port.
