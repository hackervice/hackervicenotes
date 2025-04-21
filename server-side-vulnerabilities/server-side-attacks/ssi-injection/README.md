# SSI Injection

#### Introduction to SSI Injection

Server-Side Includes (SSI) is a technology used by web applications to generate dynamic content within HTML pages, supported by popular web servers like Apache and IIS. While common file extensions for SSI include `.shtml`, `.shtm`, and `.stm`, it's important to note that web servers can be configured to process SSI directives in various file types, making it challenging to definitively identify SSI usage based solely on file extensions.

**SSI Directives**

SSI employs directives to incorporate dynamically generated content into static HTML pages. Each directive consists of a name, parameters, and corresponding values, following this syntax:

```html
<!--#name param1="value1" param2="value2" -->
```

Some common SSI directives include:

*   **printenv**: Displays environment variables.

    ```html
    <!--#printenv -->
    ```
*   **config**: Modifies SSI configuration settings, such as error messages.

    ```html
    <!--#config errmsg="Error!" -->
    ```
*   **echo**: Outputs the value of specified variables, such as the current file's name or local server time.

    ```html
    <!--#echo var="DOCUMENT_NAME" var="DATE_LOCAL" -->
    ```
*   **exec**: Executes a command provided in the `cmd` parameter.

    ```html
    <!--#exec cmd="whoami" -->
    ```
*   **include**: Includes a specified file from the web root directory.

    ```html
    <!--#include virtual="index.html" -->
    ```

**SSI Injection**

SSI injection occurs when an attacker successfully injects SSI directives into a file served by the web server, leading to the execution of those directives. This vulnerability can arise in various scenarios, such as through insecure file uploads that allow attackers to upload files containing malicious SSI directives or when user input is improperly written to files in the web root directory.
