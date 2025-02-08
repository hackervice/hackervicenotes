---
description: >-
  You are contracted to perform a penetration test for a company, and through
  your pentest, you stumble upon an interesting file manager web application. As
  file managers tend to execute system commands
---

# Skill Assessment

If you stumble across this page in order to find some guidance, I hope it the right place to you. I must say that I struggle a bit this this skill assessment, the hardest part was definitely to find the vulnerable parameter to command injection. If this is also you, I'll give you some tips before you jump right to the answer.

In this section we learned to test command injection with the following operators

<table data-header-hidden><thead><tr><th></th><th width="112"></th><th width="161"></th><th></th></tr></thead><tbody><tr><td><strong>Injection Operator</strong></td><td><strong>Injection Character</strong></td><td><strong>URL-Encoded Character</strong></td><td><strong>Executed Command</strong></td></tr><tr><td>Semicolon</td><td><code>;</code></td><td><code>%3b</code></td><td>Both</td></tr><tr><td>New Line</td><td><code>\n</code></td><td><code>%0a</code></td><td>Both</td></tr><tr><td>Background</td><td><code>&#x26;</code></td><td><code>%26</code></td><td>Both (second output generally shown first)</td></tr><tr><td>Pipe</td><td><code>|</code></td><td><code>%7c</code></td><td>Both (only second output is shown)</td></tr><tr><td>AND</td><td><code>&#x26;&#x26;</code></td><td><code>%26%26</code></td><td>Both (only if first succeeds)</td></tr><tr><td>OR</td><td><code>||</code></td><td><code>%7c%7c</code></td><td>Second (only if first fails)</td></tr><tr><td>Sub-Shell</td><td><code>``</code></td><td><code>%60%60</code></td><td>Both (Linux-only)</td></tr><tr><td>Sub-Shell</td><td><code>$()</code></td><td><code>%24%28%29</code></td><td>Both (Linux-only)</td></tr></tbody></table>

Credits: HackTheBox.com

My suggestion is to learn how the web application works, intercept every Request when you are interacting with the web app and to take this sixteen operators through the Intruder in every parameter. Analyse the Response, compare the their length and then try to reproduce it with Repeater



The application seems to be a simple File Manager, where you can simply view files content, download and move the files to other directory.

<figure><img src="../../../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>

To find the vulnerable parameter lets pick a file and select its copy functionality

<figure><img src="../../../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

We are referred to other page where we are able to select the file's destination. Make sure your Burp has intercept on, and click on Move.

<figure><img src="../../../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

Your Request should look like this:

<figure><img src="../../../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

As you see I added a `;` to see how the application was going to handle this operator, and then forwarded the Request.



<figure><img src="../../../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

Once the Request is forwarded we can see that a message appears on the web application. This is interesting because we have an indication that our operator is being filtered



<figure><img src="../../../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

Having identified a possible entry point, the next step was to find an operator that was not being filtered.

<figure><img src="../../../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

After several attempts I found the `%26` was possible to bypass the security measures. Then I kept evolving the exploit, the `$()` was also being accepted so i added the `whoami` command to it. But the command was blacklisted and I ended up with the payload above and able to successfully enumerate the current user, `www-data`.

```bash
%26$(w'ho'ami)
```



<figure><img src="../../../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

evolving payload

<figure><img src="../../../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

Flag found

<figure><img src="../../../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

Thank god!

<figure><img src="../../../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

\
