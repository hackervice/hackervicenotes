# XML External Entity (XXE) Injection

**What is XXE?**

XML External Entity (XXE) Injection vulnerabilities occur when XML data is processed from user-controlled input without proper sanitization or secure parsing. This can lead to various malicious actions, including the disclosure of sensitive files and even denial of service attacks on the back-end server. Due to its potential for significant damage, XXE is recognized as one of the Top 10 Web Security Risks by OWASP.

**Understanding XML**

**Extensible Markup Language (XML)** is a markup language designed for the flexible transfer and storage of data. Unlike HTML, which focuses on data presentation, XML is primarily concerned with data representation and structure. XML documents consist of element trees, where each element is defined by tags, with the root element being the top-level element.

**Example of an XML Document**:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<email>
  <date>01-01-2022</date>
  <time>10:00 am UTC</time>
  <sender>john@inlanefreight.com</sender>
  <recipients>
    <to>HR@inlanefreight.com</to>
    <cc>
        <to>billing@inlanefreight.com</to>
        <to>payslips@inlanefreight.com</to>
    </cc>
  </recipients>
  <body>
  Hello,
      Kindly share with me the invoice for the payment made on January 1, 2022.
  Regards,
  John
  </body> 
</email>
```

**Key Components of XML**

* **Tag**: The keys of an XML document, wrapped in angle brackets (e.g., `<date>`).
* **Entity**: XML variables, typically wrapped in ampersands (e.g., `&lt;` for `<`).
* **Element**: The root or child elements, with values stored between start and end tags (e.g., `<date>01-01-2022</date>`).
* **Attribute**: Optional specifications for elements stored within tags (e.g., `version="1.0"`).
* **Declaration**: The first line of an XML document that defines the XML version and encoding (e.g., `<?xml version="1.0" encoding="UTF-8"?>`).

**XML Document Type Definition (DTD)**

**XML DTD** allows for the validation of an XML document against a predefined structure. This structure can be defined within the document or referenced externally.

**Example DTD**:

```xml
<!DOCTYPE email [
  <!ELEMENT email (date, time, sender, recipients, body)>
  <!ELEMENT recipients (to, cc?)>
  <!ELEMENT cc (to*)>
  <!ELEMENT date (#PCDATA)>
  <!ELEMENT time (#PCDATA)>
  <!ELEMENT sender (#PCDATA)>
  <!ELEMENT to  (#PCDATA)>
  <!ELEMENT body (#PCDATA)>
]>
```

This DTD defines the structure of the XML document, specifying the root element and its child elements.

**XML Entities**

Custom entities can be defined in XML DTDs to reduce redundancy. This is done using the `ENTITY` keyword.

**Example of Defining an Entity**:

```xml
<!DOCTYPE email [
  <!ENTITY company "Inlane Freight">
]>
```

This allows the entity to be referenced in the XML document (e.g., `&company;`), which will be replaced with its value during parsing.

**External XML Entities** can also be referenced, allowing the XML parser to include data from external files.

**Example of External Entity Reference**:

```xml
<!DOCTYPE email [
  <!ENTITY company SYSTEM "http://localhost/company.txt">
  <!ENTITY signature SYSTEM "file:///var/www/html/signature.txt">
]>
```

When the XML file is parsed, the parser replaces the entity reference with the content of the specified external file.

**Conclusion**

XXE vulnerabilities can lead to severe security risks if XML data is not properly handled. Understanding XML structure, DTDs, and entity references is crucial for developers to mitigate these risks. By ensuring that XML input is sanitized and securely parsed, applications can protect themselves from potential XXE attacks.
