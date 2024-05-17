# Skills Assessment - Web Fuzzing (in progress)

### Run a sub-domain/vhost fuzzing scan on '\*.academy.htb' for the IP shown above. What are all the sub-domains you can identify? (Only write the sub-domain name)



Subdomain fuzzing didn't lead to any results. Ffuf ran for over 30 min and ended up returning just errors. You might want skip to vhost fuzzing and save some time :thumbsup:

{% code title="Subdomain fuzzing" overflow="wrap" %}
```shell
ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://FUZZ.academy.htb/
```
{% endcode %}

With vhost fuzzing I notice a pattern, so I decided to add the flag `-fs 985` to see if I catch something interesting .

{% code title="VHost fuzzing" overflow="wrap" %}
```sh
ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:46699 -H 'Host: FUZZ.academy.htb' -fs 985
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

And we successfully enumerate three VHosts!

### Before you run your page fuzzing scan, you should first run an extension fuzzing scan. What are the different extensions accepted by the domains?



{% code overflow="wrap" %}
```sh
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt -u http://test.academy.htb:46699/indexFUZZ 

ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt -u http://archive.academy.htb:46699/indexFUZZ 

ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt -u http://faculty.academy.htb:46699/indexFUZZ 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>



### One of the pages you will identify should say 'You don't have access!'. What is the full page URL?



### In the page from the previous question, you should be able to find multiple parameters that are accepted by the page. What are they?



### Try fuzzing the parameters you identified for working values. One of them should return a flag. What is the content of the flag?
