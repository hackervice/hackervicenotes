# Bypassing Security Filters

To get the flag, try to bypass the command injection filter through HTTP Verb Tampering, while using the following filename: file; cp /flag.txt ./

**Step 1** - Access the application:

<figure><img src="../../../.gitbook/assets/image (225).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Try to add a file in the input field:

* If we try to insert a file with the naming **test;** we got blocked by the application

<figure><img src="../../../.gitbook/assets/image (226).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Change to POST method:

* The application is using a GET method to insert the file, change is to POST:

<figure><img src="../../../.gitbook/assets/image (228).png" alt=""><figcaption></figcaption></figure>

* And the file is successully uploaded:

<figure><img src="../../../.gitbook/assets/image (229).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Get the flag:

* Now that we can bypass the command injection, lets insert the filename **file; cp /flag.txt ./**

<figure><img src="../../../.gitbook/assets/image (230).png" alt=""><figcaption></figcaption></figure>

* Click on flag.txt

<figure><img src="../../../.gitbook/assets/image (231).png" alt=""><figcaption></figcaption></figure>

And we are done!
