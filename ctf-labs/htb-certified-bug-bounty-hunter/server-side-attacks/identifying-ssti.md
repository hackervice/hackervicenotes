# Identifying SSTI

Apply what you learned in this section and identify the Template Engine used by the web application. Provide the name of the template engine as the answer.

For this simple exercise we can use the cheat sheet referenced in this [SSTI](https://app.gitbook.com/o/eXpdkTjmLVS0nzVHsMPS/s/t4gp37tj8QBnGMMf3DZs/~/changes/137/server-side-vulnerabilities/server-side-attacks/ssti/identifying) section.

<figure><img src="../../../.gitbook/assets/image (152).png" alt=""><figcaption></figcaption></figure>

This is the look of the application. The first value we must test is **`${7*7}`**, but it wasn't executed so we must follow to the next value which is **`{{7*7}}`**.

<figure><img src="../../../.gitbook/assets/image (153).png" alt=""><figcaption></figcaption></figure>

Since the **`{{7*7}}`** returned 49, we can assure that the template engine being used is Twig!
