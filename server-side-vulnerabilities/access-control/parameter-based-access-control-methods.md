# Parameter-based access control methods

Parameter-based access control methods are commonly used in applications to determine user access rights or roles. However, relying on user-controllable locations for this information can lead to significant security vulnerabilities. Understanding the risks associated with this approach is crucial for implementing effective access control.

* **User-Controllable Locations:**
  * Some applications store access rights or role information in locations that users can manipulate, such as:
    * Hidden fields
    * Cookies
    * Preset query string parameters
* **Access Control Decisions:**
  * The application may make access control decisions based on the values submitted in these locations. For example:

```
https://insecure-website.com/login/home.jsp?admin=true
https://insecure-website.com/login/home.jsp?role=1
```

* **Security Risks:**
  * This method is insecure because users can easily modify the values in the URL or other controllable locations. As a result, they may gain unauthorized access to functionalities, including administrative features.
