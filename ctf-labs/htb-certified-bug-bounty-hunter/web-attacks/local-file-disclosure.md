# Local File Disclosure

#### Try to read the content of the 'connection.php' file, and submit the value of the 'api\_key' as the answer.

**Step 1** - Get the XML data

* Fill the form click on **Send Message**.

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

* Capture the POST Request and send it with Repeater

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

You can see that it looks the value of the tag `email` is being reflected on the Response.

**Step 2** - Exploit the `email` tag:

In order to see if there is an actual vulnerability in the `email` tag, lets insert a DTD with a entity declared.

* Insert the DTD
* ```xml
  <!DOCTYPE email [
    <!ENTITY test "HackerVice">
  ]>
  ```
* Place the value `&test;` inside the email tags.

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

As we can see the value declared was reproduced in the Response. The system is definitely vulnerable.

**Step 3** - Get the api\_key:

The `api_key` is inside the file connection.php. Since php files contain special characters like `<`/`>`/`&` , we will have wrap the content with base64 encoding to avoid breaking the XML form. We will use`php://filter/` wrapper,  then use the encoder `convert.base64-encode` and lastly use an input resource resource=connection.php: `php://filter/convert.base64-encode/resource=connection.php`&#x20;

```xml
<!DOCTYPE email [
  <!ENTITY test SYSTEM "php://filter/convert.base64-encode/resource=connection.php">
]>
```

<figure><img src="../../../.gitbook/assets/image (248).png" alt=""><figcaption></figcaption></figure>

When the XML is sent, we can see that the Response gives us the `connection.php` file encoded. If we just select the encoded characters, then on the right panel Burp Suite automatically detects the **base64** encoding and decodes it automatically. And we got the **api\_key**!
