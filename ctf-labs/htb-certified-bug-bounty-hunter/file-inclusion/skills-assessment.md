# Skills Assessment

### Scenario

#### The company `INLANEFREIGHT` has contracted you to perform a web application assessment against one of their public-facing websites. They have been through many assessments in the past but have added some new functionality in a hurry and are particularly concerned about file inclusion/path traversal vulnerabilities.

#### They provided a target IP address and no further information about their website. Perform a full assessment of the web application checking for file inclusion and path traversal vulnerabilities.

#### Find the vulnerabilities and submit a final flag using the skills we covered in the module sections to complete this module.

Don't forget to think outside the box!

#### Assess the web application and use a variety of techniques to gain remote code execution and find a flag in the / root directory of the file system. Submit the contents of the flag as your answer.



I can't tell you enough how much I spend around this skills assessment. It took me over a week to figure out the solution, but what really trow me off off was the _"Don't forget to think outside the box!"_ sentence. I thought I had to come up with a really crazy solution, and also it says they add a new functionality in a hurry, and I ended up focus too much on the contact form of the application. But the positives for me is that I had the right mindset and methodology through the assessment I was able to enumerate the following applications files:

* index
* about
* contact
* main
* industries
* error

And then I read it's content using php wrapper, however I completly ignored the index file for a long time because I was expecting something more complex. So lets go with the walkthrough:

<figure><img src="../../../.gitbook/assets/image (303).png" alt=""><figcaption></figcaption></figure>

As we can see we already have 4 resources to explore. If you navigate through the application you'll notice that uses the following structure to access the resources `http://SERVER_IP:PORT/index.php?page=<resource>`.

So let's start by using PHP Wrappers to read the index file:

{% code overflow="wrap" %}
```bash
curl http://SERVER_IP:PORT/index.php?page=php://filter/read=convert.base64-encode/resource=index
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (304).png" alt=""><figcaption></figcaption></figure>

You should get something like this. Copy the base64 string and decoded with:

```
echo <base64 string> | base64 -d
```

<figure><img src="../../../.gitbook/assets/image (305).png" alt=""><figcaption></figcaption></figure>

It should return a html code, and if you scroll up a bit you'll see the following line commented. We got a new endpoint, let's paste lets have a look at it.

<figure><img src="../../../.gitbook/assets/image (306).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (307).png" alt=""><figcaption></figcaption></figure>

We now have access to an administration panel. If we click on the buttons Chat Log, Service Log and System Log we can see that we have a new parameter. Fuzz for common files:

{% code overflow="wrap" %}
```bash
ffuf -w LFI-Jhaddix.txt:FUZZ -u 'http://SERVER_IP:PORT/ilf_admin/index.php?log=FUZZ' -fs 2046

```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (308).png" alt=""><figcaption></figcaption></figure>

By filtering for the lenght 2046, the results are more trustfull. Let's try to read the `../../../../../etc/passwd`

<figure><img src="../../../.gitbook/assets/image (311).png" alt=""><figcaption></figcaption></figure>

And we are able to read the **/etc/passwd** file!&#x20;

Since this endpoint deals with logs, lets try to do log poisoning. Notice that this as nginx, so we must specify the correct directory.

<figure><img src="../../../.gitbook/assets/image (312).png" alt=""><figcaption></figcaption></figure>

If we specify the **../../../../../var/log/nginx/access.log** we can read the logs on the browser. So lets capture this request with Burp Suite and try Log Poisoning. Notice that one of the logs is the used User Agent.

<figure><img src="../../../.gitbook/assets/image (314).png" alt=""><figcaption></figcaption></figure>

And look what we've got! A successful code execution!

Now all we have to do is go to the root directory and retrieve the flag.&#x20;

<figure><img src="../../../.gitbook/assets/image (316).png" alt=""><figcaption></figcaption></figure>

And another Skills Assessment completed! Very fun section, with a lot of new techiniques learned!
