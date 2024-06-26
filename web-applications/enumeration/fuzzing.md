# Fuzzing

This technique is used to discover web folders in a application that we are trying to test. A manual approach would be visiting a specific well-known page like `https://www.example.com/login` we will probably get a HTTP code <mark style="color:green;">200 OK</mark> and for `https://www.example.com/randomfolder` a HTTP code <mark style="color:red;">404 Page Not Found</mark>.&#x20;

As you can imagine, fuzzing manually will take forever and this is why we have tools that do this automatically. We should use a commonly know wordlist, and some of the most commonly used can be found under the GitHub [SecLists](https://github.com/danielmiessler/SecLists) repository.

### Directory Fuzzing

Fuzzing with custom wordlist. Option `-w` to specify the wordlist path, and `-u` for the URL.

{% code overflow="wrap" %}
```shell
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://example.com:PORT/FUZZ
```
{% endcode %}

***

### Page Fuzzing

After we found a directory it might return an empty page. To discover common files we can fuzz well-known pages like `.html`, `.aspx`, `.php`, etc.

{% code overflow="wrap" %}
```sh
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://example.com:PORT/blog/indexFUZZ
```
{% endcode %}

***

### Recursive Fuzzing

Scanning recursively allow us to identify sub-directories like /login/user/...etc. Additionally we can specify our extension to find webpages.&#x20;

{% code overflow="wrap" %}
```shell
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://example.com:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v
```
{% endcode %}

***

### Subdomain Fuzzing

{% code overflow="wrap" %}
```shell
ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u https://FUZZ.example.com/
```
{% endcode %}

***

### Vhost Fuzzing

If there are no DNS records mapping a hostname to an IP address, accessing the host directly via its IP address or by manipulating the `Host` header in HTTP requests may still yield a response from the server. We can use the `-H` flag to specify a header and will use the `FUZZ` keyword within it,

{% code overflow="wrap" %}
```shell
ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://example.com:PORT/ -H 'Host: FUZZ.example.com'
```
{% endcode %}

***

### Parameter Fuzzing

#### GET

We can also use ffuf to enumerate parameters. The parameters are usually passed right after the URL, with a `?` symbol, like this:

`http://admin.example.com:PORT/admin/admin.php?param1=key`.

So, all we have to do is to replace `param1` with `FUZZ` like in the previous examples. And we can use this [wordlist](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/burp-parameter-names.txt) along with the following command:

{% code overflow="wrap" %}
```sh
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.example.com:PORT/admin/admin.php?FUZZ=key -fs xxx
```
{% endcode %}

#### POST

Post requests are not passed in the URL because it cannot be appended after a `?` symbol. To fuzz a parameter with `ffuf` we must use de flag `-d`. And we also have to add `-X POST` to send a `POST` request.

{% code overflow="wrap" %}
```sh
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.example.com:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```
{% endcode %}

We can check what we hit (in this example is the `id` parameter) with the last command, by send a POST request with curl.

{% code overflow="wrap" %}
```sh
curl http://admin.example.com:PORT/admin/admin.php -X POST -d 'id=key' -H 'Content-Type: application/x-www-form-urlencoded'
```
{% endcode %}

***

### Value Fuzzing

After we enumerate successfully our target, we now have to fuzz the correct value that would return the `flag` content we need.

We can create a custom wordlist so it can fit our type of fuzzing, like usernames or ids. Or we can look for some well-known wordlists like the [SecLists](https://github.com/danielmiessler/SecLists), for example.

A simple way to create an `id` parameter wordlist from 1-1000 is by using scripting languages like Bash or Python.

{% code title="Creates a file with 1-1000 numbers" overflow="wrap" %}
```shell
for i in $(seq 1 1000); do echo $i >> ids.txt; done
```
{% endcode %}

To use it with `ffuf` we can do the following command:

{% code overflow="wrap" %}
```shell
ffuf -w ids.txt:FUZZ -u http://admin.example.com:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```
{% endcode %}

