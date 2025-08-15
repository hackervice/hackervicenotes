# WordPress Structure

#### Default WordPress File Structure

* **Installation Environment**: WordPress can be installed on **Windows**, **Linux**, or **Mac OSX**. This module focuses on a default installation on an **Ubuntu Linux** server.
* **Requirements**: A fully configured **LAMP stack** (Linux, Apache, MySQL, PHP) is necessary before installation. After installation, files are located in the webroot at **/var/www/html**.

#### Directory Structure

The following is the key directory structure of a default WordPress installation:

```
var/www/html
├── index.php
├── license.txt
├── readme.html
├── wp-activate.php
├── wp-admin
├── wp-blog-header.php
├── wp-comments-post.php
├── wp-config.php
├── wp-config-sample.php
├── wp-content
├── wp-cron.php
├── wp-includes
├── wp-links-opml.php
├── wp-load.php
├── wp-login.php
├── wp-mail.php
├── wp-settings.php
├── wp-signup.php
├── wp-trackback.php
└── xmlrpc.php
```

#### Key WordPress Files

* **index.php**: The homepage of the WordPress site.
* **license.txt**: Contains version information of the installed WordPress.
* **wp-activate.php**: Used for email activation during site setup.
* **wp-admin**: Contains the login page and backend dashboard for administrators. Login can be accessed via:
  * `/wp-admin/login.php`
  * `/wp-admin/wp-login.php`
  * `/login.php`
  * `/wp-login.php`
  * This file can be renamed for security.
* **xmlrpc.php**: Facilitates data transmission using HTTP and XML, now largely replaced by the WordPress REST API.

#### WordPress Configuration File

* **wp-config.php**: Essential for database connection, containing:
  * Database name, host, username, and password.
  * Authentication keys and salts.
  * Database table prefix.
  * Option to enable **DEBUG mode** for troubleshooting.

Example snippet:

```php
define( 'DB_NAME', 'database_name_here' );
define( 'DB_USER', 'username_here' );
define( 'DB_PASSWORD', 'password_here' );
define( 'DB_HOST', 'localhost' );
```

#### Key WordPress Directories

*   **wp-content**: Main directory for plugins and themes. The **uploads/** subdirectory stores uploaded files, which may contain sensitive data leading to vulnerabilities.

    ```
    /var/www/html/wp-content
    ├── index.php
    ├── plugins
    └── themes
    ```
*   **wp-includes**: Contains core files excluding administrative components and themes, including certificates, fonts, JavaScript files, and widgets.

    ```
    /var/www/html/wp-includes
    ├── theme.php
    ├── update.php
    ├── user.php
    ├── vars.php
    ├── version.php
    ├── widgets
    ├── widgets.php
    ├── wlwmanifest.xml
    ├── wp-db.php
    └── wp-diff.php
    ```
