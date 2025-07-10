# Local File Inclusion (LFI)

#### Try to use what you learned in this section to access the 'reset.php' page and delete all files. Once all files are deleted, you should get the flag.



**Step 1** - Acess the Web Application

* Select a language - the vulnerable parameter is **language**.

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

**Step 2** - Access the /etc/passwd file

* Substitute the **en.php** for **../../../../../etc/passwd**

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

And you got the user starting by 'b'!



#### Submit the contents of the flag.txt file located in the /usr/share/flags directory.

**Step 1** - Access the /usr/share/flags directory

* Access the flag content by specifying the full path **/usr/share/flags/flag.txt**

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

And you got the flag!
