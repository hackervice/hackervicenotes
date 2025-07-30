# Local File Inclusion (LFI)

Local File Inclusion (LFI) vulnerabilities allow attackers to read files on a server by manipulating file inclusion mechanisms in web applications. Understanding how to exploit these vulnerabilities is crucial for both security professionals and developers.

***

### 🛠️ Basic LFI Exploitation

#### Language Parameter Example

A common scenario involves a web application that allows users to select a language, which is then passed as a parameter in the URL. For instance:

* **URL**: `http://<SERVER_IP>:<PORT>/index.php?language=es.php`

If the application includes files based on this parameter, an attacker can manipulate it to read sensitive files, such as:

* **Malicious URL**: `http://<SERVER_IP>:<PORT>/index.php?language=/etc/passwd`

This allows the attacker to view the contents of the `/etc/passwd` file, revealing user accounts on the server.

***

### 🔍 Path Traversal Techniques

#### Bypassing Restrictions

Sometimes, developers append or prepend strings to the input parameter, which can prevent direct access to sensitive files. For example:

* **Code**: `include("./languages/" . $_GET['language']);`

In this case, attempting to access `/etc/passwd` directly would fail. However, attackers can use directory traversal techniques by adding `../` to navigate up the directory structure:

* **Successful URL**: `http://<SERVER_IP>:<PORT>/index.php?language=../../../../etc/passwd`

This method allows access to files regardless of the directory structure.

#### Filename Prefix and Appended Extensions

If the application uses a prefix or appends an extension to the parameter, attackers can still exploit LFI by adjusting their input. For example:

* **Prefix Example**: `include("lang_" . $_GET['language']);`
  * **Malicious URL**: `http://<SERVER_IP>:<PORT>/index.php?language=/../../../etc/passwd`
* **Extension Example**: `include($_GET['language'] . ".php");`
  * **Malicious URL**: `http://<SERVER_IP>:<PORT>/index.php?language=/etc/passwd`

***

### ⚠️ Second-Order Attacks

#### Understanding Second-Order LFI

Second-order attacks occur when an application indirectly includes files based on user-controlled parameters that have been stored in a database. For instance, if a user can set their username to a malicious value like `../../../etc/passwd`, and later that username is used in a file inclusion context, the attack can succeed.

* **Example**: A profile URL like `/profile/$username/avatar.png` could be exploited if the username is maliciously crafted.
