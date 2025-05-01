# SQLMap Essentials

### Skills Assessment

***

You are given access to a web application with basic protection mechanisms. Use the skills learned in this module to find the SQLi vulnerability with SQLMap and exploit it accordingly. To complete this module, find the flag and submit it here.



The application presented looks a online shoe store. Let's explore if we can find a possible entry point like an ID parameter.

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

At the page /shop.html I manage to intercept a POST request with the ID parameter. I was able to do this with Burp Suite, to replicate we just have to go to /shop.html and hover your mouse over a product, and click on "ADD TO CART"

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Copy the POST request, save it to a file and run the following command:

```batch
sqlmap -r post.txt
```

At the middle of the process we are advised to user the --tamper=between, so lets change the command:

```batch
sqlmap -r post.txt --tamper=between --batch
```

<figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

The `id` parameter is indeed vulnerable, and sqlmap was able to find a working payload.

To accelerate the finding process we can use the following command:

```batch
sqlmap -r post.txt --tamper=between --search -T flag
```

The `search` argument will accelerate the flag finding process, instead of dumping all the data.

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Answer the prompts accordingly

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And after a few seconds we manage to find our flag!
