# Skills Assessment - Web Fuzzing

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

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And we successfully enumerate three VHosts!

### Before you run your page fuzzing scan, you should first run an extension fuzzing scan. What are the different extensions accepted by the domains?



{% code overflow="wrap" %}
```sh
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt -u http://test.academy.htb:46699/indexFUZZ 

ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt -u http://archive.academy.htb:46699/indexFUZZ 

ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt -u http://faculty.academy.htb:46699/indexFUZZ 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>



### One of the pages you will identify should say 'You don't have access!'. What is the full page URL?

{% hint style="info" %}
Note: the port changed to 33056 because I had to reset the target :smile:
{% endhint %}

In order to find the correct answer I had to fuzz each subdomain and their respective extensions.

To find the correct page I used the following command:

{% code overflow="wrap" %}
```bash
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://faculty.academy.htb:33056/FUZZ -ic -recursion -recursion-depth 1 -e .php7 -v
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (11) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### In the page from the previous question, you should be able to find multiple parameters that are accepted by the page. What are they?

#### Get method

{% code overflow="wrap" %}
```sh
fuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://faculty.academy.htb:33056/courses/linux-security.php7?FUZZ=key -fs 774
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (12) (1) (1) (1).png" alt=""><figcaption><p>user parameter found</p></figcaption></figure>

#### Post method

{% code overflow="wrap" %}
```sh
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://faculty.academy.htb:33056/courses/linux-security.php7 -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs 774
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (14) (1) (1) (1).png" alt=""><figcaption><p>user and username parameter found</p></figcaption></figure>

And with the Post method we fuzzed two parameters, user and username

### Try fuzzing the parameters you identified for working values. One of them should return a flag. What is the content of the flag?

{% hint style="info" %}
To complete this challenge we have to user a username [list](https://github.com/danielmiessler/SecLists/blob/master/Usernames/Names/malenames-usa-top1000.txt)
{% endhint %}



{% code overflow="wrap" %}
```sh
ffuf -w /opt/useful/SecLists/Usernames/Names/malenames-usa-top1000.txt:FUZZ -u http://faculty.academy.htb:33056/courses/linux-security.php7 -X POST -d 'username=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -v -fs 781

```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (15) (1) (1) (1).png" alt=""><figcaption><p>Username found</p></figcaption></figure>

Now to see the content of the page we use the curl command to retrieve the HTML code

{% code overflow="wrap" %}
```sh
curl http://faculty.academy.htb:33056/courses/linux-security.php7 -X POST -d 'username=HARRY' 'Content-Type: application/x-www-form-urlencoded'
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (16) (1) (1) (1).png" alt=""><figcaption><p>Flag found</p></figcaption></figure>

As you can see we found the last flag!
