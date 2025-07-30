# PHP Filters

PHP web applications, including those built with frameworks like Laravel and Symfony, can be vulnerable to Local File Inclusion (LFI) attacks. By leveraging PHP filters and wrappers, attackers can extend their exploitation capabilities, potentially leading to remote code execution. This section explores how to utilize PHP filters to read source code and gain insights into the application.

***

### 🔍 Understanding PHP Filters

#### What are PHP Filters?

PHP filters are a type of wrapper that allows manipulation of input data. By using the `php://filter` wrapper, we can apply various filters to resources, such as local files. The key parameters for these filters are:

* **resource**: Specifies the target file.
* **read**: Defines the filter to apply, such as encoding or compression.

Among the available filters, the **convert.base64-encode** filter is particularly useful for LFI attacks, as it allows us to read PHP source code without executing it.

***

### 🔎 Fuzzing for PHP Files

#### Discovering PHP Files

To identify potential PHP files for exploitation, tools like **ffuf** or **gobuster** can be used to fuzz for existing PHP pages:

{% code overflow="wrap" %}
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://<SERVER_IP>:<PORT>/FUZZ.php
```
{% endcode %}

When fuzzing, it's important to check for all HTTP response codes, including `301`, `302`, and `403`, as these may still provide valuable information.

***

### 📄 Standard PHP Inclusion

#### Executing PHP Files

When including PHP files through LFI, the files are executed, and their output is rendered as HTML. For example:

* **Malicious URL**: `http://<SERVER_IP>:<PORT>/index.php?language=config`

If the included file does not produce HTML output, it may return an empty result. This is where the base64 filter becomes useful, as it allows us to retrieve the source code instead of executing the file.

***

### 🔑 Source Code Disclosure with Base64 Filter

#### Using the Base64 Filter

To read the source code of a PHP file, we can use the base64 filter as follows:

* **Malicious URL**: `http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=config`

This request will return the base64-encoded content of `config.php`. The `.php` extension is automatically appended, allowing us to specify the resource without needing to include the extension.

#### Decoding the Source Code

Once we receive the encoded string, we can decode it to reveal the source code:

```bash
echo 'PD9waHAK...SNIP...KICB9Ciov' | base64 -d
```

This process allows us to gain insights into the application's configuration and logic, which can be critical for further exploitation.
