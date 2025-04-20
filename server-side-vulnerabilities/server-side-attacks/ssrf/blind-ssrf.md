# Blind SSRF

Even if we can’t see the response, we can still confirm Blind SSRF using these steps:

1.  **Set up a listener** on your machine to catch incoming requests (e.g., using `netcat`):

    ```bash
    bashCopyEditnc -lnvp 8000
    ```
2. **Send a URL** pointing to your listener to the vulnerable app.
3. **Check your listener’s output**:
   * If it shows a connection and request (like a GET request), SSRF is happening!
   * If you try pointing it to the server itself and get a generic error like "date unavailable," you’ve found _blind SSRF_.

***

### 🛠️ How to Exploit Blind SSRF

Exploitation is limited but still possible depending on how the app behaves. Here are a few techniques:

#### 🔍 Local Port Scanning

1. Change the URL to `http://localhost:PORT`.
2. Watch how the app responds:
   *   If the **port is closed**, you may get a generic error like:

       ```
       Something went wrong!
       ```
   * If the **port is open**, the error message is **different**, even if you don’t get full content.
3. Use these differences to infer which ports are open.

#### 📁 Detecting Existing Files

You can also use SSRF to guess files on the server:

1. Send a request to `file:///etc/passwd` (or other known file paths).
2. If the file **exists**, the error will look different than if the file **doesn’t exist**.
3. Based on this, you can confirm whether a file exists, even if you can’t read it.

***

### ⚠️ Limitations

* You _can’t_ view file contents.
* You _can’t_ interact with services like MySQL unless they return an HTTP response.
* You're mostly relying on **differences in error messages** to infer what's going on.
