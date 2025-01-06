# What's next?

Once you have successfully established a shell on a target machine, the next steps involve stabilizing your access and escalating your privileges. Here’s a concise guide on what to do next:

### 1. Stabilizing the Shell

While reverse and bind shells provide remote code execution, they are often unstable and non-interactive. To improve your shell experience, consider the following:

* **Use a TTY Shell**: If you have a basic shell, you can upgrade it to a more stable TTY shell. This can be done using commands like `python -c 'import pty; pty.spawn("/bin/bash")'` on Linux.
* **Backgrounding and Foregrounding**: Use `Ctrl+Z` to background the shell and then use `stty raw -echo; fg` to bring it back to the foreground with better control.

### 2. Gaining User Access on Linux

On Linux systems, look for opportunities to access user accounts:

* **SSH Keys**: Check for SSH keys stored in `/home/<user>/.ssh`. These keys can provide direct access to user accounts.
* **Credentials**: In Capture The Flag (CTF) challenges, you may find plaintext credentials or configuration files containing sensitive information.
* **Exploits**: Certain exploits, like Dirty C0w, can allow you to modify system files (e.g., `/etc/shadow` or `/etc/passwd`) to create or elevate user accounts.

### 3. Gaining Access on Windows

Windows systems may have different avenues for privilege escalation:

* **Registry**: Look for plaintext passwords for running services in the Windows registry. For example, VNC servers often store passwords in plaintext.
* **Configuration Files**: Check for credentials in configuration files, such as:
  * `C:\Program Files\FileZilla Server\FileZilla Server.xml`
  * `C:\xampp\FileZilla Server\FileZilla Server.xml`
* **Escalate to SYSTEM or Administrator**: Aim to obtain a shell running as the SYSTEM user or an administrator. This allows you to add your own account to the administrators group.

#### Adding Your Own Account

Once you have sufficient privileges, you can create a new user account with administrative rights using the following commands:

```bash
net user <username> <password> /add
net localgroup administrators <username> /add
```

### Conclusion

While reverse and bind shells are effective for gaining remote code execution, they are not as robust as a native shell. The goal should always be to escalate your access to a more stable and fully featured method, allowing for easier exploitation and further actions on the target system. By focusing on user access and privilege escalation, you can maintain control over the compromised machine and achieve your objectives more effectively.
