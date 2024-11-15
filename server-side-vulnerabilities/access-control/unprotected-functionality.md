# Unprotected functionality

Unprotected functionality is a significant vulnerability that can lead to vertical privilege escalation. It occurs when sensitive features of an application are not adequately secured, allowing unauthorized users to access them. Recognizing and addressing this issue is crucial for maintaining application security.

**Definition:**

* **Unprotected Functionality:** Refers to sensitive application features that lack proper access controls, making them accessible to users who should not have permission to use them.

**Key Points:**

* Sensitive administrative functions may be linked only from an administrator's welcome page, but if the corresponding URL is known or can be discovered, non-administrative users can access it.
*   Example of an unprotected administrative URL:

    {% code overflow="wrap" %}
    ```javascript
    https://insecure-website.com/admin
    ```
    {% endcode %}

This URL should only be accessible to authorized administrative users.

* **URL Disclosure:**
  *   Sometimes, administrative URLs may be inadvertently disclosed in files like `robots.txt`, which can be accessed by anyone:

      {% code overflow="wrap" %}
      ```javascript
      https://insecure-website.com/robots.txt
      ```
      {% endcode %}
* **Brute-Force Attacks:**
  * Even if URLs are not explicitly disclosed, attackers can use **wordlists** to brute-force and discover sensitive functionality locations.

The concept of unprotected functionality extends beyond just accessible URLs; it also includes the practice of "security by obscurity." While attempting to hide sensitive features through unpredictable URLs may seem like a protective measure, it does not provide true security. Understanding the limitations of this approach is essential for effective access control.

**Key Points:**

* **Security by Obscurity:**
  * This approach involves concealing sensitive functionality by using less predictable URLs. However, it is not a reliable method for securing access, as users may still discover these URLs through various means.
* **Example of Obfuscated URL:**
  * An administrative function might be located at:
* ```
  https://insecure-website.com/administrator-panel-yb556
  ```
  * While this URL may not be easily guessable, it can still be exposed to unauthorized users.
* **URL Disclosure through JavaScript:**
  * Sensitive URLs can be leaked through client-side code. For instance, consider the following JavaScript snippet:

{% code overflow="wrap" %}
```javascript
<script>
    var isAdmin = false;
    if (isAdmin) {
        ...
        var adminPanelTag = document.createElement('a');
        adminPanelTag.setAttribute('href', 'https://insecure-website.com/administrator-panel-yb556');
        adminPanelTag.innerText = 'Admin panel';
        ...
    }
</script>
```
{% endcode %}

* In this example, the script checks the user's role and creates a link to the admin panel if the user is an admin. However, the script itself is visible to all users, exposing the URL even to non-admins.
