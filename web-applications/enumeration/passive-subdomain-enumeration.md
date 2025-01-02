# Passive Subdomain Enumeration

### VirusTotal

{% embed url="https://virustotal.com" %}

### Certificates

#### Censys

{% embed url="https://censys.io" %}

#### crt.sh

{% embed url="https://crt.sh" %}

{% code title="Enumerates subdomains listed on crt.sh" fullWidth="false" %}
```javascript
export TARGET="example.com"
export PORT="443"
openssl s_client -ign_eof 2>/dev/null <<<$'HEAD / HTTP/1.0\r\n\r' -connect "${TARGET}:${PORT}" | openssl x509 -noout -text | grep 'DNS' | sed -e 's|DNS:|\n|g' -e 's|^\.||g' | tr -d ',' | sort -u
```
{% endcode %}

<table><thead><tr><th width="342">Argument</th><th>Description</th></tr></thead><tbody><tr><td><code>curl -s</code></td><td>Issue the request with minimal output</td></tr><tr><td><code>https://crt.sh/?q=&#x26;output=json</code> </td><td>Ask for the json output</td></tr><tr><td><code>jq -r '.[]' "(.name_value)\n(.common_name)"' sort -u</code></td><td>Process the json output and print certificate's name value and common name one per line</td></tr><tr><td><code>sort -u</code></td><td>Sort alphabetically the output provided and removes duplicates</td></tr></tbody></table>

### OpenSSL

{% code fullWidth="false" %}
```javascript
export TARGET="example.com"
export PORT="443"
openssl s_client -ign_eof 2>/dev/null <<<$'HEAD / HTTP/1.0\r\n\r' -connect "${TARGET}:${PORT}" | openssl x509 -noout -text | grep 'DNS' | sed -e 's|DNS:|\n|g' -e 's|^\.||g' | tr -d ',' | sort -u
```
{% endcode %}



## Automating Passive Subdomain Enumeration



### TheHarvester

TheHarvester is a simple-to-use yet powerful and effective tool for early-stage penetration testing and red team engagements. We can use it to gather information to help identify a company's attack surface. The tool collects emails, names, subdomains, IP addresses, and URLs from various public data sources for passive information gathering. For now, we will use the following modules:

<table data-header-hidden><thead><tr><th width="230"></th><th></th></tr></thead><tbody><tr><td><a href="http://www.baidu.com/">Baidu</a></td><td>Baidu search engine.</td></tr><tr><td><code>Bufferoverun</code></td><td>Uses data from Rapid7's Project Sonar - <a href="http://www.rapid7.com/research/project-sonar/">www.rapid7.com/research/project-sonar/</a></td></tr><tr><td><a href="https://crt.sh/">Crtsh</a></td><td>Comodo Certificate search.</td></tr><tr><td><a href="https://hackertarget.com/">Hackertarget</a></td><td>Online vulnerability scanners and network intelligence to help organizations.</td></tr><tr><td><code>Otx</code></td><td>AlienVault Open Threat Exchange - <a href="https://otx.alienvault.com/">https://otx.alienvault.com</a></td></tr><tr><td><a href="https://rapiddns.io/">Rapiddns</a></td><td>DNS query tool, which makes querying subdomains or sites using the same IP easy.</td></tr><tr><td><a href="https://github.com/aboul3la/Sublist3r">Sublist3r</a></td><td>Fast subdomains enumeration tool for penetration testers</td></tr><tr><td><a href="http://www.threatcrowd.org/">Threatcrowd</a></td><td>Open source threat intelligence.</td></tr><tr><td><a href="https://www.threatminer.org/">Threatminer</a></td><td>Data mining for threat intelligence.</td></tr><tr><td><code>Trello</code></td><td>Search Trello boards (Uses Google search)</td></tr><tr><td><a href="https://urlscan.io/">Urlscan</a></td><td>A sandbox for the web that is a URL and website scanner.</td></tr><tr><td><code>Vhost</code></td><td>Bing virtual hosts search.</td></tr><tr><td><a href="https://www.virustotal.com/gui/home/search">Virustotal</a></td><td>Domain search.</td></tr><tr><td><a href="https://www.zoomeye.org/">Zoomeye</a></td><td>A Chinese version of Shodan.</td></tr></tbody></table>

Create a file called sources.txt with the follwing contents:

```
baidu
bufferoverun
crtsh
hackertarget
otx
projectdiscovery
rapiddns
sublist3r
threatcrowd
trello
urlscan
vhost
virustotal
zoomeye
```

Execute the following commands to gather information from these sources.

```javascript
export TARGET="example.com"
cat sources.txt | while read source; do theHarvester -d "${TARGET}" -b $source -f "${source}_${TARGET}";done
```

Extract all the subdomains found and sort them via the following command:

```javascript
cat *.json | jq -r '.hosts[]' 2>/dev/null | cut -d':' -f 1 | sort -u > "${TARGET}_theHarvester.txt”
```

Merge all the passive reconnaissance files:

```javascript
cat example.com_*.txt | sort -u > example.com_subdomains_passive.txt
cat example.com_subdomains_passive.txt 
```



|                                         |          |            |        |                                              |                         |
| --------------------------------------- | -------- | ---------- | ------ | -------------------------------------------- | ----------------------- |
|                                         | whois    |            | domain | Exemplo: whois example.com                   | Responde a DNS internos |
| Querying: A Records                     | nslookup |            | domain | Exemplo: nslookup example.com                | Responde a DNS internos |
|                                         | dig      |            | domain | Exemplo: dig example.com                     | Responde a DNS internos |
| Querying: A Records for a Subdomain     | nslookup | -query=A   | domain | Exemplo: nslookup -query=A example.com       | Responde a DNS internos |
|                                         | dig      | a          | domain | Exemplo: dig a example.com                   | Responde a DNS internos |
| Querying: PTR Records for an IP Address | nslookup | -query=PTR | IP     | Exemplo: nslookup -query=PTR 180.xxx.xxx.xxx |                         |
|                                         | dig      | -x         | IP     | Exemplo: dig -x 180.xxx.xxx.xxx              |                         |
| Querying: ANY Existing Records          | nslookup | -query=ANY | domain | Exemplo: nslookup -query=ANY example.com     |                         |
|                                         | dig      | any        | domain | Exemplo: dig any example.com                 |                         |
| Querying: TXT Records                   | nslookup | -query=TXT | domain | Exemplo: nslookup -query=TXT example.com     |                         |
|                                         | dig      | txt        | domain | Exemplo: dig txt example.com                 |                         |
| Querying: MX Records                    | nslookup | -query=MX  | domain | Exemplo: nslookup -query=MX example.com      |                         |
|                                         | dig      | mx         | domain | Exemplo: dig mx example.com                  |                         |
