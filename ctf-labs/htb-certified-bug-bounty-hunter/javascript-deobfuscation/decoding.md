# Decoding

### Using what you learned in this section, determine the type of encoding used in the string you got at previous exercise, and decode it. To get the flag, you can send a 'POST' request to 'serial.php', and set the data as "serial=YOUR\_DECODED\_OUTPUT".

For this lab we'll use the string founded in the previous exercise.

First let's do a base64 decoding

```bash
echo N2gxNV8xNV9hX3MzY3IzN19tMzU1NGcz | base64 -d
```

<figure><img src="../../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

We managed to decode the string. Now lets send the data as a POST request

{% code overflow="wrap" %}
```bash
curl http://94.237.58.102:36075/serial.php -X POST -d "serial=7h15_15_a_s3cr37_m3554g3"
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

And we got the flag!
