# Parameter Fuzzing

### Using what you learned in this section, run a parameter fuzzing scan on this page. what is the parameter accepted by this webpage?



First of all, edit the /etc/hosts file and add the admin.academy.htb VHost. Otherwise you'll get stuck forever in this exercise.

Let's make sure we add the option `-fs 798`at the end to avoid an output like this:

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Let's gather what we learn in this lesson and use the following command:

{% code overflow="wrap" %}
```shell
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:31472/admin/admin.php?FUZZ=key -fs 798
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And the application responded with the `user` parameter!
