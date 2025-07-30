# Basic Bypasses

When dealing with Local File Inclusion (LFI) vulnerabilities, web applications often implement various protections. However, these protections can sometimes be bypassed if the application is not properly secured. Here are some common techniques to exploit LFI vulnerabilities despite these defenses.

***

### 🛡️ Non-Recursive Path Traversal Filters

#### Understanding Basic Filters

Many applications use simple filters to prevent path traversal by removing instances of `../` from user input. For example:

```php
$language = str_replace('../', '', $_GET['language']);
```

While this may seem effective, it only removes the substring once, allowing for potential bypasses. For instance, using a payload like `....//` can still lead to successful path traversal:

* **Malicious URL**: `http://<SERVER_IP>:<PORT>/index.php?language=....//....//....//....//etc/passwd`

This approach demonstrates that recursive patterns can still exploit the vulnerability.

#### Alternative Payloads

Other variations like `..././` or `....\/` can also be used to bypass filters that only look for `../`.

***

### 🔄 Encoding Techniques

#### URL Encoding

Some filters may block characters like `.` and `/`. However, URL encoding can help bypass these restrictions. For example, encoding `../` as `%2e%2e%2f` can allow traversal:

* **Encoded URL**: `http://<SERVER_IP>:<PORT>/index.php?language=%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%65%74%63%2f%70%61%73%73%77%64`

This technique can be particularly effective against older PHP versions or custom filters.

***

### 📂 Approved Paths

#### Regular Expression Filters

Some applications restrict file inclusion to specific directories using regular expressions. For example:

```php
if(preg_match('/^\.\/languages\/.+$/', $_GET['language'])) {
    include($_GET['language']);
}
```

To bypass this, attackers can start their payload with the approved path and then use `../` to navigate back:

* **Malicious URL**: `http://<SERVER_IP>:<PORT>/index.php?language=./languages/../../../../etc/passwd`

This method can be combined with other techniques for added effectiveness.

***

### 📜 Appended Extensions

#### Handling File Extensions

When applications append extensions like `.php`, it can limit the ability to read other files. However, older PHP versions had vulnerabilities that allowed for bypassing this restriction through techniques like path truncation or null byte injection.

#### Path Truncation

In older PHP versions, strings longer than 4096 characters would be truncated. By crafting a long payload, attackers could bypass the appended extension:

* **Example Payload**: `?language=non_existing_directory/../../../etc/passwd/./././././ REPEATED ~2048 times`

This approach requires careful calculation to ensure the correct path is preserved.

#### Null Byte Injection

Versions of PHP prior to 5.5 were vulnerable to null byte injection, allowing attackers to terminate strings early:

* **Malicious Payload**: `/etc/passwd%00`

This would effectively ignore the appended `.php` extension, allowing access to the sensitive file.
