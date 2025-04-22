---
description: >-
  Use what you learned in this section to find name of the user in the '/home'
  folder. What user did you find?
---

# Bypassing Other Blacklisted Characters

In order to display the name of the user in the `/home` folder we must combine several techiniques learned in the last sections.

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We have to use the following command in order to list the user:

```bash
ip=127.0.0.1%0a{ls,-la}${IFS}${PATH:0:1}home
```

Here’s a  breakdown of the command:

#### Simple Breakdown

1. **`ip=127.0.0.1`**:
   * This sets a parameter called `ip` to `127.0.0.1`, which is the address for localhost (the same machine).
2. **`%0a`**:
   * This represents a newline character. It tells the server to treat what comes next as a new command.
3. **`{ls,-la}`**:
   * This is a command to list files in a directory. The `-la` options mean:
     * `-l`: Show detailed information about each file.
     * `-a`: Include hidden files (those starting with a dot).
4. **`${IFS}`**:
   * This is a special variable that represents spaces. It helps separate parts of the command.
5. **`${PATH:0:1}`**:
   * This gets the first character of the `PATH` variable (which usually starts with a colon). It’s used here to help format the command.
6. **`home`**:
   * This specifies the directory you want to list, which is `/home`

When you put it all together, this command is trying to execute:

<pre class="language-bash"><code class="lang-bash"><strong>ping -c 1 127.0.0.1; ls -la /home
</strong></code></pre>

We only manage to put `home` in the command because the WAF is not blocking any letters.

So, 1nj3c70r is the answer!
