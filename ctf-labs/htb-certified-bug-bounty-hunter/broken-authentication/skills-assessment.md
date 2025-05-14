# Skills Assessment

Scenario

You are tasked to perform a security assessment of a client's web application. For the assessment, the client has not provided you with credentials. Apply what you have learned in this module to obtain the flag.

For this assessment we are tasked to test the client's web application, and the goal is to try to any sort of bypass/brute-force the login form to obtain the flag.

<figure><img src="../../../.gitbook/assets/image (199).png" alt=""><figcaption></figcaption></figure>

This is the client's web application and we noticed right away that has a Login button at the top right corner. Let's click it.

<figure><img src="../../../.gitbook/assets/image (213).png" alt=""><figcaption></figcaption></figure>

One of the first tries we can do is try to insert a random username expecting an output like "Username or password does not exist", or for the password something like "Wrong password". Since we got a generic error lets explore the application a little bit.

<figure><img src="../../../.gitbook/assets/image (211).png" alt=""><figcaption></figcaption></figure>

Since no credentials were given to this assessment, let's register a new account a then login with the new credentials to see if we can elevate our privileges.

<figure><img src="../../../.gitbook/assets/image (214).png" alt=""><figcaption></figcaption></figure>

Let's keep in mind when we fail to register we are prompt with a password policy.

<figure><img src="../../../.gitbook/assets/image (201).png" alt=""><figcaption></figcaption></figure>

The web application looks pretty simple and there is nothing too much to look for. Let's try to login again, but this time we will intercept the Requests.

<figure><img src="../../../.gitbook/assets/image (200).png" alt=""><figcaption></figcaption></figure>

When the POST Request is sent with the new user, we can noticed that at the Response we get a redirection code 302 Found, and it's redirecting us the profile.php.

Since we didnt't get anything useful, let's explorer the Login form again.

\[Image]

One of the things I could notice is actual enter a valid user we get a different message. Instead of of the generic message "Username or password does not exist" we get the message "Invalid credentials". This means if that isn't any kind of protection we can actually brute-force the username and see if we get more privileges.

We can use this wordlist for the usernames, and use the following command:

{% code overflow="wrap" %}
```bash
ffuf -w ./xato-net-10-million-usernames.txt -u http://SERVER_IP:SERVER_PORT/login.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=1234" -fr "Unknown username or password"
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (202).png" alt=""><figcaption></figcaption></figure>

After a few seconds we manage to get a register user in the web application.&#x20;

Now we need to brute-force this user's password. To do that we'll use the well-known password wordlist, rockyou.txt. In penetration testing we want to be efficient, so to avoid sending necessary Requests, let's shorten the password wordlist to match the web application's password policy:

{% code overflow="wrap" %}
```bash
grep '^[[:alnum:]]*$' /usr/share/wordlists/rockyou.txt  | grep '[[:upper:]]' | grep '[[:lower:]]' | grep '[[:digit:]]' | grep -E '^.{12}$' > custom_wordlist.txt
```
{% endcode %}

Now, let's craft our ffuf command and brute-force the password:

{% code overflow="wrap" %}
```bash
ffuf -w ./custom_wordlist.txt -u http://SERVER_IP:SERVER_PORT/login.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=<username>&password=FUZZ" -fr "Invalid credentials"
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (203).png" alt=""><figcaption></figcaption></figure>

And now that we have both username and password we can login to the application!

<figure><img src="../../../.gitbook/assets/image (204).png" alt=""><figcaption></figcaption></figure>

When we enter the brute-forced credentials, we are redirected to 2FA page. One of the first test we can do is create a numeric wordlist of 4 digit tokens, and try to brute-force this code.

If you are were because you are stuck for days in this exact point, join the club because I also been there. No matter what we try, we'll not get through with the OTP brute-force. After two tries we get redirected again to the login page.

So, since brute-forcing the OTP is not the way, let's go a step back and analyse login POST request of the founded user.

<figure><img src="../../../.gitbook/assets/image (205).png" alt=""><figcaption></figcaption></figure>

Similiar to our user `hackervice` we also get a **302 Found Response**. However, if we look closely with the new user the **Location** as the value **/2fa.php**. If we go back, we'll see that the user `hackervice` gets redirected to **/profile.php**.&#x20;

So let's authenticate again and intercept the POST Response to manually change the **Location**.

<figure><img src="../../../.gitbook/assets/image (207).png" alt=""><figcaption></figcaption></figure>

We change the location from **/2fa.php** to **/profile.php**. and we forward the **Request**.

<figure><img src="../../../.gitbook/assets/image (208).png" alt=""><figcaption></figcaption></figure>

We are successfully redirected to the **profile.php**. However the GET Response is forcing us again to the **/2fa.php** page, so before forwarding the **Request** lets change the code to **200 OK** and the location to **/profile.php** again.

<figure><img src="../../../.gitbook/assets/image (209).png" alt=""><figcaption></figcaption></figure>

After we forward the GET Request we were able to load the profile page of the new user and get the flag!

This was very fun skills assessment, where it was possible to apply various broken authentication techniques. It was also a little bit time consuming, but we gained some experience to pay more attention to the small details.
