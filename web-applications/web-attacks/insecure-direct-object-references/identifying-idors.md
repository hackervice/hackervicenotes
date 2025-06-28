# Identifying IDORs

The first step in exploiting Insecure Direct Object References (IDOR) vulnerabilities is to identify direct object references in HTTP requests. This often involves examining URL parameters or APIs that contain object references, such as `?uid=1` or `?filename=file_1.pdf`. These references can also appear in other HTTP headers, including cookies.

To test for vulnerabilities, you can increment the values of these object references (e.g., changing `?uid=1` to `?uid=2`) to see if you can access other data. Additionally, using fuzzing tools to generate numerous variations can help identify any unauthorized access. Successful retrieval of files that do not belong to you indicates an IDOR vulnerability.

#### AJAX Calls

Another method for identifying IDOR vulnerabilities is through JavaScript AJAX calls. Some web applications may expose unused parameters or APIs in their front-end code. For instance, a web application might restrict access to admin functions based on user roles, but these functions may still be present in the JavaScript code.

If you find AJAX calls in the front-end code, you can test them for vulnerabilities. For example, consider the following AJAX function:

```javascript
function changeUserPassword() {
    $.ajax({
        url: "change_password.php",
        type: "post",
        dataType: "json",
        data: { uid: user.uid, password: user.password, is_admin: is_admin },
        success: function(result) {
            //
        }
    });
}
```

Even if this function is not called for non-admin users, locating it in the code allows you to test it for potential IDOR vulnerabilities.

#### Understanding Hashing and Encoding

Some applications may use encoded or hashed values instead of simple sequential numbers for object references. If you encounter such parameters, you can still exploit them if the backend lacks proper access control.

For example, if a parameter is base64 encoded, you can decode it to reveal the plaintext reference, modify it, and re-encode it to access other data. If you see a reference like `?filename=ZmlsZV8xMjMucGRm`, decoding it reveals `file_123.pdf`. You can then encode a different reference (e.g., `file_124.pdf`) and test it.

In cases where the reference is hashed (e.g., `download.php?filename=c81e728d9d4c2f636f067f89cc14862c`), you may initially think it is secure. However, if you find the hashing method in the source code, such as:

```javascript
$.ajax({
    url: "download.php",
    type: "post",
    dataType: "json",
    data: { filename: CryptoJS.MD5('file_1.pdf').toString() },
    success: function(result) {
        //
    }
});
```

You can calculate the hash for other filenames and attempt to access them, revealing potential IDOR vulnerabilities.

#### Comparing User Roles

For more advanced IDOR attacks, registering multiple user accounts can be beneficial. By comparing HTTP requests and object references between different users, you can gain insights into how URL parameters and identifiers are generated.

For instance, if User1 can access their salary with the following API call:

```json
{
  "attributes": {
    "type": "salary",
    "url": "/services/data/salaries/users/1"
  },
  "Id": "1",
  "Name": "User1"
}
```

You can check if User2, who should not have access to this information, can replicate the call. If the application only requires a valid session to make the API call without backend access control checks, you may successfully retrieve data for User1 while logged in as User2, indicating an IDOR vulnerability.
