# Skills Assessment

You are contracted to perform a penetration test for a company, and through your pentest, you stumble upon an interesting file manager web application. As file managers tend to execute system commands, you are interested in testing for command injection vulnerabilities.

Use the various techniques presented in this module to detect a command injection vulnerability and then exploit it, evading any filters in place.

***

If you stumble across this page in order to find some guidance, I hope it the right place to you. I must say that I struggle a bit this this skill assessment, the hardest part was definitely to find the vulnerable parameter to command injection. If this is also you, I'll give you some tips before you jump right to the answer.

In this section we learned to test command injection with the following operators

<table data-header-hidden><thead><tr><th></th><th width="112"></th><th width="161"></th><th></th></tr></thead><tbody><tr><td><strong>Injection Operator</strong></td><td><strong>Injection Character</strong></td><td><strong>URL-Encoded Character</strong></td><td><strong>Executed Command</strong></td></tr><tr><td>Semicolon</td><td><code>;</code></td><td><code>%3b</code></td><td>Both</td></tr><tr><td>New Line</td><td><code>\n</code></td><td><code>%0a</code></td><td>Both</td></tr><tr><td>Background</td><td><code>&#x26;</code></td><td><code>%26</code></td><td>Both (second output generally shown first)</td></tr><tr><td>Pipe</td><td><code>|</code></td><td><code>%7c</code></td><td>Both (only second output is shown)</td></tr><tr><td>AND</td><td><code>&#x26;&#x26;</code></td><td><code>%26%26</code></td><td>Both (only if first succeeds)</td></tr><tr><td>OR</td><td><code>||</code></td><td><code>%7c%7c</code></td><td>Second (only if first fails)</td></tr><tr><td>Sub-Shell</td><td><code>``</code></td><td><code>%60%60</code></td><td>Both (Linux-only)</td></tr><tr><td>Sub-Shell</td><td><code>$()</code></td><td><code>%24%28%29</code></td><td>Both (Linux-only)</td></tr></tbody></table>

Credits: HackTheBox.com

My suggestion is to learn how the web application works, intercept every Request when you are interacting with the web app and to take this sixteen operators through the Intruder in every parameter. Analyse the Response, compare the their length and then try to reproduce it with Repeater.



<figure><img src="../../../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>

The application seems to be a simple File Manager, where you can simply view files content, download and move the files to other directory.

<figure><img src="../../../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

To find the vulnerable parameter lets pick a file and select its copy functionality.



<figure><img src="../../../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

We are referred to other page where we are able to select the file's destination. Make sure your Burp has intercept on, and click on Move.

Your Request should look like this:

<figure><img src="../../../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

As you see I added a `;` to see how the application was going to handle this operator, and then forwarded the Request.



<figure><img src="../../../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

Once the Request is forwarded we can see that a message appears on the web application saying "Malicious request denied!". This is interesting because we have an indication that our operator is being filtered.

<figure><img src="../../../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

Having identified a possible entry point, the next step was to find an operator that was not being filtered.

<figure><img src="../../../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

After several attempts I found the `%26` was possible to bypass the security filtering. Then I kept evolving the exploit, the `$()`  sub-shell operator was also being accepted so i added the `whoami` command to it. But the command was blacklisted and I ended up with the payload as you can see in the image above and able to successfully enumerate the current user, `www-data`.

```bash
%26$(w'ho'ami)
```

<figure><img src="../../../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

Then I tried what almost every pentester would do, adapt the ls command with the single quotes: `l's`. For some reason it was giving me an output error, and after several tries I ended up make it work the reverse function. This allow us to type the command backwards and when executed within the function it is interpreted normally by the application.

As you can see from the image above, it this payload we can enumerate successfully the files in the current directory.

```bash
%26$(rev<<<'sl')
```

<figure><img src="../../../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

Having found a working payload, now all we have to do is navigate through the directories backwards in order to list their specific files. What I found to be working for me was using ${IFS} to be interpreted as a blank space, and ${PATH:0:1} for a slash. What we normally would type as `ls ../html` is being inserted like this:

```bash
%26$(rev<<<'sl')${IFS}..${PATH:0:1}html
```

<figure><img src="../../../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

We keep going through to directories backwards and we ended up found the flag.txt file that we are looking for.

```bash
%26$(rev<<<'sl')${IFS}..${PATH:0:1}..${PATH:0:1}..
```

<figure><img src="../../../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

Now all we have to do is adapt our payload and use the cat command to read the flag.txt content:

```bash
%26$(rev<<<'tac')${IFS}..${PATH:0:1}..${PATH:0:1}..${PATH:0:1}flag.txt
```

And we finally got our flag!&#x20;

This was truly a challenging skills assessment. I was curious to see how other fellow pentesters made it work and I found interesting how simple and different was some of their answers.&#x20;

At the end what it really matters is understand the key concepts and create a methodology that works for you and your end goal.&#x20;

Happy hacking!\
