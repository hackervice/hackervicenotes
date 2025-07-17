# Basic Bypasses

#### The above web application employs more than one filter to avoid LFI exploitation. Try to bypass these filters to read /flag.txt

**Step 1** - Acess the Web Application

* Select a language - the vulnerable parameter is **language**.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Access the flag file on the root directory

* The application is filtering `../` so lets add `...//` to navigate through the directories

<figure><img src="../../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

And flag retrieved!
