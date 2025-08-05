# Additional CSRF Protection Bypasses

Understanding various techniques can be beneficial during engagements or bug bounty hunting. Below are several approaches that may help exploit weak CSRF protections in web applications.

***

#### 🔑 Techniques for Bypassing CSRF Protections

1. **Null Value CSRF Token**:
   *   Attempt to send a null or empty CSRF token:

       ```
       CSRF-Token:
       ```
   * This may work if the application only checks for the presence of the header without validating the token's value.
2. **Random CSRF Token**:
   * Create a CSRF token of the same length as the original but with a different value:
     * **Real**: `CSRF-Token: 9cfffd9e8e78bd68975e295d1b3d3331`
     * **Fake**: `CSRF-Token: 9cfffl3dj3837dfkj3j387fjcxmfjfd3`
   * This can bypass protections that only check for the token's length and presence.
3. **Using Another Session’s CSRF Token**:
   * If the application does not validate if the CSRF token is tied to a specific account, you can use a CSRF token from another account:
     * Create two accounts, capture the CSRF token from the first account, and use it in requests from the second account.
4. **Request Method Tampering**:
   *   Change the request method from POST to GET or vice versa:

       ```http
       POST /change_password
       POST body: new_password=pwned&confirm_new=pwned
       ```

       Change to:

       ```http
       GET /change_password?new_password=pwned&confirm_new=pwned
       ```
   * Some applications may not enforce CSRF protections on unexpected request methods.
5. **Omitting the CSRF Token**:
   *   Not sending a CSRF token can often succeed if the application only checks for the token's existence:

       ```http
       POST /change_password
       POST body: new_password=qwerty
       ```
   *   Alternatively, send a blank token:

       ```http
       POST /change_password
       POST body: new_password=qwerty&csrf_token=
       ```
6. **Session Fixation > CSRF**:
   *   If the application uses a double-submit cookie for CSRF protection, and a session fixation vulnerability exists, an attacker can exploit this:

       ```http
       POST /change_password
       Cookie: CSRF-Token=fixed_token;
       POST body: new_password=pwned&CSRF-Token=fixed_token
       ```
7. **Referer Header Manipulation**:
   *   If the application uses the referer header for CSRF protection, try removing it:

       ```html
       <meta name="referrer" content="no-referrer">
       ```
8. **Bypassing Regex Checks on Referer**:
   * If the referer header uses a regex whitelist, try manipulating the referer:
     *   For example, if it checks for `google.com`, use:

         ```
         www.google.com.pwned.m3
         ```
     *   If it checks for the target domain, try:

         ```
         www.target.com.pwned.m3
         ```
