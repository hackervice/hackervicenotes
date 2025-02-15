# Directory Fuzzing

### In addition to the directory we found above, there is another directory that can be found. What is it?

In this lab the target is `83.136.254.223:47319` and in addition to the folder found previously (blog), we can run the same command to detect other content.

With the following command we can enumerate directories for the application

```shell
ffuf -w /opt/useful/Seclist/Discovery/Web-Content/directory-list-2.3-small.txt -u http://83.136.254.223:47319/FUZZ
```

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

And _forum_ is the folder we are looking for!
