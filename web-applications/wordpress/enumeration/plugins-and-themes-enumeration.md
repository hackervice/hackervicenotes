# Plugins and Themes Enumeration

**Plugins**

* {% code overflow="wrap" %}
  ```bash
  curl -s -X GET http://example.com | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'wp-content/plugins/*' | cut -d"'" -f2
  ```
  {% endcode %}

**Themes**

* {% code overflow="wrap" %}
  ```bash
  curl -s -X GET http://example.com | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'themes' | cut -d"'" -f2
  ```
  {% endcode %}

**Plugins Active Enumeration**

* ```bash
  curl -I -X GET http://example.com/wp-content/plugins/mail-masta
  ```
* If the content does not exist, a `404 Not Found error`&#x20;
* &#x20;  &#x20;

