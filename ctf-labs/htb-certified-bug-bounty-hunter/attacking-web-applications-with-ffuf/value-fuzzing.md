# Value Fuzzing

### Try to create the 'ids.txt' wordlist, identify the accepted value with a fuzzing scan, and then use it in a 'POST' request with 'curl' to collect the flag. What is the content of the flag?

{% hint style="info" %}
Don't forget to edit the /etc/host file id needed :smile:
{% endhint %}

As suggested, we'll create a custom ids.txt file to fuzz the application

{% code overflow="wrap" %}
```bash
for i in $(seq 1 1000); do echo $i >> ids.txt; done                                                                                                     
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

We'll then fuzz the application with our custom wordlist with the following command:

{% code overflow="wrap" %}
```shell
ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:37515/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded'        
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

After we successfully fuzzed the application, as the image above shows, we found a working `id`.

Now, all we have to do is send a curl command to see the content of the page:

{% code overflow="wrap" %}
```sh
curl http://admin.academy.htb:37515/admin/admin.php -X POST -d 'id=73' -H 'Content-Type: application/x-www-form-urlencoded'
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

And we found our flag!
