# Active Subdomain Enumeration

### Assetfinder

{% code title="Enumerate a single domain" %}
```shell
assetfinder example.com
```
{% endcode %}

Enumerate multiple domains with assetfinder from a single file

```sh
nano domains.txt
```

```sh
example1.com
example2.com
example3.com
```

```sh
assetfinder < domains.txt
```

Enumerate multiple domains with assetfinder a single file and .txt output for each domain.

{% code title="Create a bash script" %}
```sh
nano enumerate_subdomains.sh
```
{% endcode %}

{% code title="Code for bash script" %}
```bash
#!/bin/bash

# Read each domain from domains.txt
while IFS= read -r domain; do
    echo "Enumerating subdomains for $domain..."
    assetfinder "$domain" > "$domain.txt"
    echo "Results saved to $domain.txt"
done < domains.txt
```
{% endcode %}

{% code title="Make the script executable" %}
```sh
chmod +x enumerate_subdomains.sh
```
{% endcode %}

{% code title="Run the script" %}
```shell
./enumerate_subdomains.sh
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### ZoneTransfer

*   ZoneTransfers - [https://hackertarget.com/zone-transfer/](https://hackertarget.com/zone-transfer/)

    The zone transfer is how a secondary DNS server receives information from the primary DNS server and updates it.

    * Manual approach
      1. Identifying Namservers - `nslookup -type=NS zonetransfer.me`
      2. Testing for ANY and AXFR Zone Tranfer - `nslookup -type=any -query=AXFR zonetransfer.me nsztm1.digi.ninja`

    If we manage to perform a successful zone transfer for a domain, there is no need to continue enumerating this particular domain as this will extract all the available information.

### Gobuster

Gobuster is a tool that we can use to perform subdomain enumeration. It is especially interesting for us the patterns options as we have learned some naming conventions from the passive information gathering we can use to discover new subdomains following the same pattern.

{% code title="pattern.txt" %}
```
lert-api-shv-{GOBUSTER}-sin6
atlas-pp-shv-{GOBUSTER}-sin6
```
{% endcode %}

{% code title="wordlist by SecLists" overflow="wrap" %}
```sh
export TARGET="[example.com](<http://example.com/>)"
export NS="[d.ns.example.com](<http://d.ns.example.com/>)"
export WORDLIST="numbers.txt"
gobuster dns -q -r "${NS}" -d "${TARGET}" -w "${WORDLIST}" -p ./patterns.txt -o "gobuster_${TARGET}.txt"
```
{% endcode %}

* `dns`: Launch the DNS module
* `q`: Don't print the banner and other noise.
* `r`: Use custom DNS server
* `d`: A target domain name
* `p`: Path to the patterns file
* `w`: Path to the wordlist
* `o`: Output file
