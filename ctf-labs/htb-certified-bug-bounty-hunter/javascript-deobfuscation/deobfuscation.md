# Deobfuscation

### Using what you learned in this section, try to deobfuscate 'secret.js' in order to get the content of the flag. What is the flag?

For this lag we have to access the target using a web browser.&#x20;

Since we already know that we have to deobfuscate 'secret.js', lets do an Inspect to the webpage, and go to the Debugger tab.

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Accessing the target</p></figcaption></figure>

We can see that ,js code is all written in a single line. Lets do a Beautify by clicking in the brackets bellow.

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Code beautified</p></figcaption></figure>

Now the code is more readable, but it's using a packer obfuscation. Lets use [UnPacker ](https://matthewfl.com/unPacker.html)to deobuscate this code.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

As we can see, the code was successfully deobfuscated!
