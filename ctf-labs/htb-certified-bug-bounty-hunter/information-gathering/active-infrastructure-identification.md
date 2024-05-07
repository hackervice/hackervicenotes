# Active Infrastructure Identification

vHosts needed for these questions:

* `app.inlanefreight.local`
* `dev.inlanefreight.local`

What Apache version is running on app.inlanefreight.local? (Format: 0.0.0)

For this question we just have to execute the command `whatweb -a3 app.inlanefreight.local -v`

<figure><img src="../../../.gitbook/assets/Screenshot 2024-03-24 001135.png" alt=""><figcaption></figcaption></figure>

And the Apache version is 2.4.41

Which CMS is used on app.inlanefreight.local? (Format: word)

For this question we can use again the whatweb command:

`whatweb -a3 app.inlanefreight.local -v`

<figure><img src="../../../.gitbook/assets/Screenshot 2024-03-24 003024.png" alt=""><figcaption></figcaption></figure>

or the Wappalyzer plugin:

<figure><img src="../../../.gitbook/assets/Screenshot 2024-03-24 003236.png" alt=""><figcaption></figcaption></figure>

And we are able to identify the CMS, Joomla

On which operating system is the dev.inlanefreight.local webserver running on? (Format: word)

There are many ways to answer this question, this time we’ll use curl with the commands:

```bash
export TARGET="dev.inlanefreight.local" 
curl -I $TARGET
```

<figure><img src="../../../.gitbook/assets/Screenshot 2024-03-24 003547.png" alt=""><figcaption></figcaption></figure>

And by the output, we could enumerate the OS, Ubuntu
