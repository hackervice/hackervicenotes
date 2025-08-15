# IDOR in Insecure APIs

### Try to read the details of the user with 'uid=5'. What is their 'uuid' value?

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

When accessing the web application, click on "Edit Profile" and Intercept the Requests. We can see that this profile points to the user ID 1.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

If we send the Request to the Repeater and change the ID to 5, we get the `uuid` of the corresponding user, confirming and IDOR vulnerability
