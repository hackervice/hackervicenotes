# Horizontal privilege escalation

Horizontal privilege escalation is a security vulnerability that allows a user to access resources or data belonging to another user, rather than just their own. Understanding this type of escalation is essential for implementing effective access controls and protecting user data.

**Definition:**

* **Horizontal Privilege Escalation:** Occurs when a user gains unauthorized access to resources or functionalities associated with another user. For example, if an employee can view the records of other employees in addition to their own, this constitutes horizontal privilege escalation.

**Exploitation Methods:**

* Horizontal privilege escalation can be exploited using similar methods as vertical privilege escalation. For instance, a user might access their account page with a URL like:
* ```
  https://insecure-website.com/myaccount?id=123
  ```
  * If an attacker modifies the `id` parameter to that of another user, they could potentially access that user's account page and the associated data and functions.

#### Note on Insecure Direct Object Reference (IDOR)

* This scenario exemplifies an **Insecure Direct Object Reference (IDOR)** vulnerability, which occurs when user-controllable parameter values are used to directly access resources or functions.

**Complexities of Exploitation:**

* In some applications, the exploitable parameter may not have a predictable value. For example, instead of using incrementing numbers, an application might use globally unique identifiers (GUIDs) to identify users. While this can make it harder for an attacker to guess another user's identifier, GUIDs may still be disclosed in other parts of the application, such as user messages or reviews.
