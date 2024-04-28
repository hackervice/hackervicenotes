# Passive Subdomain Enumeration



### VirusTotal

virustotal.com

### Certificates

#### Censys

{% embed url="https://censys.io" %}

#### crt.sh

{% embed url="https://crt.sh" %}

{% code title="Enumerates subdomains listed on crt.sh" fullWidth="true" %}
```javascript
export TARGET="example.com"
export PORT="443"
openssl s_client -ign_eof 2>/dev/null <<<$'HEAD / HTTP/1.0\r\n\r' -connect "${TARGET}:${PORT}" | openssl x509 -noout -text | grep 'DNS' | sed -e 's|DNS:|\n|g' -e 's|^\.||g' | tr -d ',' | sort -u
```
{% endcode %}

<table><thead><tr><th width="342">Argument</th><th>Description</th></tr></thead><tbody><tr><td><code>curl -s</code></td><td>Issue the request with minimal output</td></tr><tr><td><code>https://crt.sh/?q=&#x26;output=json</code> </td><td>Ask for the json output</td></tr><tr><td><code>jq -r '.[]' "(.name_value)\n(.common_name)"' sort -u</code></td><td>Process the json output and print certificate's name value and common name one per line</td></tr><tr><td><code>sort -u</code></td><td>Sort alphabetically the output provided and removes duplicates</td></tr></tbody></table>

### OpenSSL

{% code fullWidth="true" %}
```javascript
export TARGET="example.com"
export PORT="443"
openssl s_client -ign_eof 2>/dev/null <<<$'HEAD / HTTP/1.0\r\n\r' -connect "${TARGET}:${PORT}" | openssl x509 -noout -text | grep 'DNS' | sed -e 's|DNS:|\n|g' -e 's|^\.||g' | tr -d ',' | sort -u
```
{% endcode %}
