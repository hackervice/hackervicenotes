# Reading Files

We see in the above PHP code that '$conn' is not defined, so it must be imported using the PHP include command. Check the imported page to obtain the database password.



This exercise is a follow up to the example provided in the lesson. It was shown that the web application is vulnerable to SQL Injection.

With the query it was demonstrated that we could read files from the source code:

<figure><img src="../../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

The page just ends up rendering HTML code. But while viewing the source code (CTRL + U) we can see something interesting:

<figure><img src="../../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

As we can see the variable $conn is not defined in this code, it must be defined somewhere. Since we are looking for the database password, we can try to look to config.php. The config.php file provides the PHP scripts with the necessary configuration settings to connect to the database and validate various parameters.

<figure><img src="../../../.gitbook/assets/Screenshot from 2024-12-07 19-49-16 (1).png" alt=""><figcaption></figcaption></figure>

This time we use the query bellow, and we were able to see the database password.

```sql
cn' UNION SELECT 1, LOAD_FILE("/var/www/html/search.php"), 3, 4-- -
```
