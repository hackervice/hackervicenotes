# Authentication Bypass via Direct Access

### Apply what you learned in this section to bypass authentication to obtain the flag.

For this assessment, we are assuming that after a successful login we are redirected to `/admin.php`.  So we'll try to access to `SERVER_IP:SERVER_PORT/admin.php`.

**Step 1** - Intercept the Request

* Access to&#x20;
* Intercept the Response

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Modify the Response:

* Foward the Request to Receive the Response
* Modify the Response to `200 OK`

<figure><img src="../../../.gitbook/assets/image (6) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

* And forward again&#x20;

**Step 2** - Access the admin panel:

* &#x20;/admin.php hanging before we forward the modified Response:

<figure><img src="../../../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Right after we forward it we get the flag!

<figure><img src="../../../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>
