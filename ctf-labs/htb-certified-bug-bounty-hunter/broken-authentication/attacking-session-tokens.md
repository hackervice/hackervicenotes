# Attacking Session Tokens

### Obtain administrative access on the target to obtain the flag.

**Step 1** - Decode the value of the parameter `session`:

* Login into the application
* Get the session id:

<figure><img src="../../../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>

* Use [DenCode](https://dencode.com/en/) to detect the type of encoding

<figure><img src="../../../.gitbook/assets/image (15) (1).png" alt=""><figcaption></figcaption></figure>

* You can confirm with the following command:
  * ```bash
    echo -n 757365723d6874622d7374646e743b726f6c653d75736572 | xxd -r -p
    ```

<figure><img src="../../../.gitbook/assets/image (16) (1).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Modify the role:

* Change the role to admin:
  * `user=htb-stdnt;role=user`
* Encode it:
  * <pre class="language-bash"><code class="lang-bash"><strong>echo -n 'user=htb-stdnt;role=admin' | xxd -p
    </strong></code></pre>

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Change the session id of the Request and get the flag:

* Modify the session id
* Resend the Request
* Search for the flag on the Response:

<figure><img src="../../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

