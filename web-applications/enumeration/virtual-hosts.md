# Virtual Hosts

`curl -s http://example.com` downloads html page

`curl -s http://example.com -H "Host: exampleheader.com"` sends a curl request to a domain previously identified during the information gathering in the HOST header.

**Automate vhosts names discovery with a dictionary file**

```
app
blog
dev-admin
forum
help
m
my
shop
some
store
support
www
```

**vHost Fuzzing**

```
cat ./vhosts | while read vhost;do echo "\n********\nFUZZING: ${vhost}\n********";curl -s -I http://example.com -H "HOST: ${vhost}.exampleheader.com" | grep "Content-Length: ";done
```

Use cURL to access the identified virtual host (ex: `dev-admin`)

`curl -s http://example.com -H "Host: dev-admin.exampleheader.com"`

**Automating Virtual Hosts Discovery with ffuf**

```
ffuf -w ./vhosts -u http://example.com -H "HOST: FUZZ.exampleheader.com" -fs 612
```

where:

* `w`: Path to our wordlist
* `u`: URL we want to fuzz
* `H "HOST: FUZZ.exampleheader.com"`: This is the `HOST` Header, and the word `FUZZ` will be used as the fuzzing point.
* `fs 612`: Filter responses with a size of 612, default response size in this case.
