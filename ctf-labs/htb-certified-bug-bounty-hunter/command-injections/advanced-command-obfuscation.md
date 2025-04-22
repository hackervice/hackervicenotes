---
description: >-
  Find the output of the following command using one of the techniques you
  learned in this section: find /usr/share/ | grep root | grep mysql | tail -n 1
---

# Advanced Command Obfuscation

This was overwhelming. Yes, let me start just by saying that.&#x20;

When we found the final answer, it seems it's not a big of a deal, but when we try to escalate something and we think we have run out of options, thats when the real works starts.

What caught me off guard was the part "using one of the techniques". So, I thought to myself i just need to try each technique and it should be fine. I was wrong...

I started by short commands like `whoami` to see if I could get an output. Like this:

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

So, my first thought was to split the command and try to build up from there:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

I manage to get the first  part of the command `find /usr/share/` but I couldn't find a way o build up, because i didn't find a enviroment variable to the `|` character.

```bash
ip=127.0.0.1%0a$(rev<<<'dnif')${IFS}${PATH:0:1}usr${PATH:0:1}share
```

After several tries, I end up with this working payload:

{% code overflow="wrap" %}
```bash
ip=127.0.0.1%0a$(rev<<<'hsab')<<<$($(rev<<<'46esab')${IFS}-d<<<ZmluZCAvdXNyL3NoYXJlLyB8IGdyZXAgcm9vdCB8IGdyZXAgbXlzcWwgfCB0YWlsIC1uIDE=)
```
{% endcode %}

I encoded the find `/usr/share/ | grep root | grep mysql | tail -n 1` to base64, and then reversed the `bash` and `base64` commands in order  to bypass the filter.

<figure><img src="../../../.gitbook/assets/image (105).png" alt=""><figcaption></figcaption></figure>
