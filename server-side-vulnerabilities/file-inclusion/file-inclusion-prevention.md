# File Inclusion Prevention

To effectively mitigate file inclusion vulnerabilities, it is crucial to avoid passing any user-controlled inputs into file inclusion functions or APIs. The application should dynamically load assets without user interaction. This section outlines strategies to patch vulnerabilities and harden systems against potential attacks.

***

### 🛡️ Avoiding User Input in File Inclusion

The primary method to prevent file inclusion vulnerabilities is to ensure that user input is never directly passed to file inclusion functions. Instead, implement a whitelist of allowed inputs that match specific files to be loaded. This can take various forms, such as:

* A database table mapping IDs to files
* A case-match script linking names to files
* A static JSON map for matching names and files

By using matched files instead of user input, the risk of file inclusion vulnerabilities is significantly reduced.

***

### 🚫 Preventing Directory Traversal

Directory traversal attacks can allow attackers to access sensitive files and configurations. To prevent this, utilize built-in functions to extract only the filename. For example, in PHP, the `basename()` function can be used to ensure only the filename is processed.

#### Example of Using `basename()`

```php
$filename = basename($input);
```

However, be cautious of edge cases where attackers might exploit wildcards. To further secure user input, sanitize it by recursively removing any attempts at directory traversal:

#### Example of Sanitizing Input

```php
while (substr_count($input, '../', 0)) {
    $input = str_replace('../', '', $input);
}
```

This code ensures that any occurrence of `../` is removed, preventing directory traversal attempts.

***

### ⚙️ Web Server Configuration

Proper web server configurations can help reduce the impact of file inclusion vulnerabilities. Key configurations include:

* **Disable Remote File Inclusion**: Set `allow_url_fopen` and `allow_url_include` to `Off` in PHP.
* **Restrict Access to Web Root**: Use `open_basedir` in the `php.ini` file to limit file access to the web root directory, e.g., `open_basedir = /var/www`.
* **Disable Dangerous Modules**: Ensure that potentially dangerous PHP modules, such as `mod_userdir`, are disabled.

These configurations help prevent access to files outside the web application folder, minimizing the impact of any identified vulnerabilities.

***

### 🛡️ Web Application Firewall (WAF)

Implementing a Web Application Firewall (WAF), such as ModSecurity, is a universal method to harden applications. Key considerations include:

* **Minimize False Positives**: Use a permissive mode to report potential issues without blocking legitimate requests. This allows for tuning rules to avoid disruptions.
* **Early Warning System**: Even in permissive mode, a WAF can provide early warnings of attacks, allowing defenders to respond promptly.

The goal of hardening is to create a robust defense that provides time for detection and response during an attack. According to the FireEye M-Trends Report of 2020, the average detection time for breaches was 30 days. Proper hardening can help organizations identify signs of attacks more quickly.

***

### 🔍 Continuous Monitoring and Testing

Hardened systems are not invulnerable; continuous monitoring and testing are essential. Regularly review logs and conduct tests, especially after the release of zero-day vulnerabilities related to your applications. While hardening may generate unique logs during an attack, it is crucial to remain vigilant and proactive in identifying potential threats.

By implementing these strategies, organizations can significantly reduce the risk of file inclusion vulnerabilities and enhance their overall security posture.
