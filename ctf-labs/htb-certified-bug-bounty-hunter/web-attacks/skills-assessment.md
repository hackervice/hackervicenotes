# Skills Assessment

#### You are performing a web application penetration test for a software development company, and they task you with testing the latest build of their social networking web application. Try to utilize the various techniques you learned in this module to identify and exploit multiple vulnerabilities found in the web application.

#### Try to escalate your privileges and exploit different vulnerabilities to read the flag at '/flag.php'.

This Skills Assessment puts together every key topic of the Web Attacks session. We have to be a bit creative an put our script skills working!

<figure><img src="../../../.gitbook/assets/image (253).png" alt=""><figcaption></figcaption></figure>

When logged in with the credentials `htb-student/Academy_student!` this is the default profile page of the current user. The main functionality of this application with this user will be the changing password feature, located in the `Settings/Account Settings`.

But before that lets refresh the page and capture the traffic.

<figure><img src="../../../.gitbook/assets/image (254).png" alt=""><figcaption></figcaption></figure>

After we forward the `/profile.php` Request we have the GET Request to `/api.php/user/74`. Notice that on the Response we can see information related to the current user. Let's chande the user id to see if can retreive other users information.

<figure><img src="../../../.gitbook/assets/image (255).png" alt=""><figcaption></figcaption></figure>

And it's working! Now let's try enumerate all the users with a simple bash script:

`nano apienum.sh`

```bash
#!/bin/bash
url=SERVER_IP:SERVER_PORT/api.php/user/

for i in {1..100}; do
        response=$(curl -s "$url/$i")  # Get the response from the API
        echo $response
done
```

Don't forget to `chmod +x apienum.sh`, and then run the script to store the output to a file `apienum.sh > results.txt`.

<figure><img src="../../../.gitbook/assets/image (256).png" alt=""><figcaption></figcaption></figure>

If we search for "**admin**" we have matching result for the user **a.corrales** that belongs to the company **Administrator** - that must be his role.

<figure><img src="../../../.gitbook/assets/image (257).png" alt=""><figcaption></figcaption></figure>

At the beginning of this write up I mentioned that one of the main functionalities of this application, was the changing password feature. Click on `Settings` and introduce the new password for the current user.

<figure><img src="../../../.gitbook/assets/image (258).png" alt=""><figcaption></figcaption></figure>

Noticed that before the changing submission, the application generates a token for this user.

<figure><img src="../../../.gitbook/assets/image (259).png" alt=""><figcaption></figcaption></figure>

And on the next Request, the Password is changed for the user `74` using the previous token.

<figure><img src="../../../.gitbook/assets/image (261).png" alt=""><figcaption></figcaption></figure>

Now lets elevate our privileges, by trying to the the same for the user **a.corrales**. If we change to the ID `52`, we manage to get password reset token without any constraints.

<figure><img src="../../../.gitbook/assets/image (262).png" alt=""><figcaption></figcaption></figure>

However, the web application does not allow us to change the password with this POST Request. So lets try a bypass that we learnt on this module. We can try another request methods to see if can bypass the reset function.&#x20;

<figure><img src="../../../.gitbook/assets/image (263).png" alt=""><figcaption></figcaption></figure>

If we right click on the Request and choose `Change request method`, it changes to GET method. And as we can see, the password got successfully changed! Now lets login with new credentials.

<figure><img src="../../../.gitbook/assets/image (264).png" alt=""><figcaption></figcaption></figure>

Having logged in with the user `a.corrales`, we can see immediately a new panel on the right that allow us to add an event. Lets explore it.

<figure><img src="../../../.gitbook/assets/image (265).png" alt=""><figcaption></figcaption></figure>

Fill the form and click **Submit**.

<figure><img src="../../../.gitbook/assets/image (266).png" alt=""><figcaption></figcaption></figure>

When we captured the Request the first thing we should notice when submitting a XML form, is if there is any value being reflected on the Response. And by the look of it, the value Event is being reflected.

<figure><img src="../../../.gitbook/assets/image (267).png" alt=""><figcaption></figcaption></figure>

One proof of concept that we can do is create a XML entity. And it works too! The entity `&hack;` is reproduced on the Response.

Now lets try to read a common php file, `index.php`.&#x20;

```php
<!DOCTYPE email [
  <!ENTITY hack SYSTEM "php://filter/convert.base64-encode/resource=index.php">
]>
```

Don't forget to add the entity `&hack;` to the name tag.

<figure><img src="../../../.gitbook/assets/image (268).png" alt=""><figcaption></figcaption></figure>

And we manage to read the index.php file without any problem.&#x20;

Now lets do the same for the `/flag.txt`:

```php
<!DOCTYPE email [
  <!ENTITY hack SYSTEM "php://filter/convert.base64-encode/resource=/flag.php">
]>
```

<figure><img src="../../../.gitbook/assets/image (269).png" alt=""><figcaption></figcaption></figure>

And after several steps we finally made it! By selecting the encoded string, Burp Suite automatically decodes it, and we manage to get the Skills Assessment flag!



