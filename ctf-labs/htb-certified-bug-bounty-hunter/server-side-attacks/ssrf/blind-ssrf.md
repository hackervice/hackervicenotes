# Blind SSRF

Exploit the SSRF to identify open ports on the system. Which port is open in addition to port 80?

As the previous exercises, we capture the POST Request from the button Check Availability.

<figure><img src="../../../../.gitbook/assets/image (17) (1).png" alt=""><figcaption></figcaption></figure>

We sent the Request to Repeater, and one of the first test we can do to confirm a Blund SSRF is setup a netcat listener.

<figure><img src="../../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And as we can see when sent the Request we can establish a connection.

<figure><img src="../../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

However if we send the Request to the port 8000 we dont get anything interesting, but we can see a pattern. The Request to the port 80 returns the message "Date is unavailable. Please choose a different date!" and for the port 8000 we get the message "Something went wrong!", we can conclude that the server is returning different messages in case the ports are open/closed.

Since our task is to discover what port is open in addition to port 80, lets use the ports.txt file in the **Identifying SSRF**. Then we adapt to the following ffuf command:

{% code overflow="wrap" %}
```bash
ffuf -w ./ports.txt -u http://SERVER_IP/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://127.0.0.1:FUZZ/&date=2024-01-01" -fr "Something went wrong"
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (150).png" alt=""><figcaption></figcaption></figure>

And we successfuly found the second open port!
