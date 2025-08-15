# User Enumeration

#### Methods

* Review post to uncover the assigned ID
  * the admin is usually assigned the user ID 1
  * Confirm with [http://example.com/?author=1](http://example.com/?author=1)
  * ```shell-session
    curl -s -I http://example.com/?author=1
    ```
* JSON Endpoint
  * ```shell-session
    curl http://example.com/wp-json/wp/v2/users | jq
    ```

