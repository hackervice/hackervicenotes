# Filtering Results

### Try running a VHost fuzzing scan on 'academy.htb', and see what other VHosts you get. What other VHosts did you get?

The first thing to do is to add the given IP to our /etc/hosts file.

```sh
sudo sh -c 'echo "94.237.63.93  academy.htb" >> /etc/hosts' 
```

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

When I ran the command for the first time I got a bunch of results. Make sure you add the option `-fs 986` at the end to filter out undesired results.

{% code overflow="wrap" %}
```sh
ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:50756/ -H 'Host: FUZZ.academy.htb' -fs 986
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We got two results! For some reason HTB only accepts one VHost, but the answer is in the screenshot above.
