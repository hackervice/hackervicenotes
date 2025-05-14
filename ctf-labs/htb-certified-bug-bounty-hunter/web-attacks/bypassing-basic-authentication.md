# Bypassing Basic Authentication

Try to use what you learned in this section to access the 'reset.php' page and delete all files. Once all files are deleted, you should get the flag.

**Step 1** - Access the application:

<figure><img src="../../../.gitbook/assets/image (220).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Intercept the traffic with Burp Suite:

* Click on the Reset Button
*   Change the Request method and forward it\


    <figure><img src="../../../.gitbook/assets/image (221).png" alt=""><figcaption></figcaption></figure>


*   It changed to POST method but no success:\


    <figure><img src="../../../.gitbook/assets/image (222).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Test other methods:

*   Change to HEAD Request and hit forward\


    <figure><img src="../../../.gitbook/assets/image (223).png" alt=""><figcaption></figcaption></figure>

Step 4 - Get the flag

*   Refresh the page and you should be able to see the flag!\




    <figure><img src="../../../.gitbook/assets/image (224).png" alt=""><figcaption></figcaption></figure>

