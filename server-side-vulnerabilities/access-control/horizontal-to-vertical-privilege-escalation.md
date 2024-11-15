# Horizontal to vertical privilege escalation

Horizontal privilege escalation can sometimes lead to vertical privilege escalation, creating a more severe security risk. By compromising a user with higher privileges, an attacker can gain access to sensitive functionalities and data. Understanding this transition is crucial for implementing comprehensive security measures.

* **Transition from Horizontal to Vertical Escalation:**
  * An attacker may initially exploit horizontal privilege escalation to access another user's account. For example, using parameter tampering:
* ```
  https://insecure-website.com/myaccount?id=456
  ```
  * If the targeted user is an administrator, the attacker can access the administrator's account page.
* **Potential Outcomes:**
  * Accessing an administrative account page may allow the attacker to:
    * View the administrator's password.
    * Change the administrator's password.
    * Gain direct access to privileged functionalities that are typically restricted.
