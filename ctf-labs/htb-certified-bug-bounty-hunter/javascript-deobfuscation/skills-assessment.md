# Skills Assessment

### Try to study the HTML code of the webpage, and identify used JavaScript code within it. What is the name of the JavaScript file being used?

We can check the JavaScript file name by accessing the target on a web browser, doing an Inspect and going to the Debugger tab.

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Once you find the JavaScript code, try to run it to see if it does any interesting functions. Did you get something in return?

We can copy the JavaScript code and run in it on [JSConsole](https://jsconsole.com/).

Bellow we can see the output:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### As you may have noticed, the JavaScript code is obfuscated. Try applying the skills you learned in this module to deobfuscate the code, and retrieve the 'flag' variable.

Copying the previous code to UnPacker

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We just have to put the string together

### Try to Analyze the deobfuscated JavaScript code, and understand its main functionality. Once you do, try to replicate what it's doing to get a secret key. What is the key?

The code is sending a POST request to '/keys.php'

We can try to replicate it with a empty POST request to see if we manage to get any information

```bash
curl http://94.237.59.179:48281/keys.php -X POST
```

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And we managed to get a secret key!

### Once you have the secret key, try to decide it's encoding method, and decode it. Then send a 'POST' request to the same previous page with the decoded key as "key=DECODED\_KEY". What is the flag you got?

The key found previously is enconded with Hex. We can decode it with the follwing command:

```bash
echo 4150495f70336e5f37333537316e365f31355f66756e | xxd -p -r
```

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Now that we decoded it, all we have to do is send it as a POST request:

{% code overflow="wrap" %}
```bash
curl http://94.237.59.179:48281/keys.php -X POST -d "key=API_p3n_73571n6_15_fun"
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And we managed to get last key!
