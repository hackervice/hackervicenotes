# WordPress User Roles

In a standard WordPress installation, there are **five types of user roles**, each with specific permissions and capabilities:

<table><thead><tr><th width="133">Role</th><th>Description</th></tr></thead><tbody><tr><td><strong>Administrator</strong></td><td>Has full access to all administrative features, including adding and deleting users and posts, as well as editing source code.</td></tr><tr><td><strong>Editor</strong></td><td>Can publish and manage posts, including those created by other users.</td></tr><tr><td><strong>Author</strong></td><td>Can publish and manage their own posts.</td></tr><tr><td><strong>Contributor</strong></td><td>Can write and manage their own posts but cannot publish them.</td></tr><tr><td><strong>Subscriber</strong></td><td>Normal users who can browse posts and edit their profiles.</td></tr></tbody></table>

#### Access and Security Implications

* **Administrator Access**: Gaining access as an administrator is crucial for executing code on the server, as this role has the highest level of permissions.
* **Editor and Author Roles**: While they have fewer permissions than administrators, editors and authors may still have access to certain vulnerable plugins that could be exploited, making them potential targets for attacks.
