# Identifying SSRF

Exploit a SSRF vulnerability to identify an internal web application. Access the internal application to obtain the flag.

The first step to confirm a SSRF vulnerability is to find a POST Request and locate a parameter that might indicate the data is being sent to another web server.

<figure><img src="../../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

The vulnerable has a functionallity to check the dates available. Let's click on the button and intercept the request on Burp Suite.

<figure><img src="../../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

As we can see the application is sending data to the **dateserver.htb**, lets modify it ot 127.0.0.1 to see if we get a Response from the server.

<figure><img src="../../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

After modifying the URL the request is successfully sent.

The next step would be enumerate the server ports to see if we can find something interesting.

First we create a wordlist of the ports we want to scan:

```bash
seq 1 10000 > ports.txt
```

Then we can use the following ffuf command to do it:

{% code overflow="wrap" %}
```bash
ffuf -w ./ports.txt -u http://SERVER_IP/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://127.0.0.1:FUZZ/&date=2024-01-01" -fr "Failed to connect to"
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

From the ffuf output above we can see that we have three ports open.

<figure><img src="../../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And with trial and error we manage to get the flag on the port 8000!
