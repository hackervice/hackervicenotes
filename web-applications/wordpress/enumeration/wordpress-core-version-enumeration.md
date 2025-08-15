# WordPress Core Version Enumeration

**WP Version - Source Code**

* View Source Code - search for meta `generator` tag
* ```bash
  curl -s -X GET http://example.com | grep '<meta name="generator"'
  ```

**WP Version - CSS**

* View Source Code - search for link rel `stylesheet` tag
* ```bash
  curl -s -X GET http://example.com | grep '<link rel="stylesheet"'
  ```

**WP Version - JS**

* View Source Code - search for script type `javascript` tag
* ```bash
  curl -s -X GET http://example.com | grep '<script type="text/javascript"'
  ```

\
