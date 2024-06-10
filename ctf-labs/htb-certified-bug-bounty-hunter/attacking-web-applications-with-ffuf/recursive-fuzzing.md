# Recursive Fuzzing

### Try to repeat what you learned so far to find more files/directories. One of them should give you a flag. What is the content of the flag?

In this lab we just have to apply pretty much what we've learned in this section

{% code overflow="wrap" %}
```shell
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://94.237.49.182:57951/FUZZ -recursion -recursion-depth 1 -e .php -v
```
{% endcode %}

After we ran the tool we were able to find the page with the flag we are looking for.

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Just paste the full URL on the web browser, and that's it for this lab!
