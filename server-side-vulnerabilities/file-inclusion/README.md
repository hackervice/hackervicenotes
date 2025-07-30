# File Inclusion

### 📁 Introduction to File Inclusions

File inclusions are a common feature in modern back-end languages like **PHP**, **JavaScript**, and **Java**, allowing dynamic content to be displayed on web pages. However, if not properly secured, these features can lead to **Local File Inclusion (LFI)** vulnerabilities, where an attacker can manipulate parameters to access local files on the server.

***

### 🔍 Local File Inclusion (LFI)

LFI vulnerabilities often arise in templating engines, which dynamically load content while maintaining static elements like headers and footers. For example, a URL might look like `/index.php?page=about`, where the `about` parameter controls which file is loaded. If this parameter is not properly validated, an attacker could potentially load any file on the server.

#### ⚠️ Risks of LFI

* **Source Code Disclosure**: Attackers can access the application's source code, revealing other vulnerabilities.
* **Sensitive Data Exposure**: Access to sensitive files may lead to credential leaks or server enumeration.
* **Remote Code Execution**: Under certain conditions, LFI can allow attackers to execute code on the server, compromising the entire system.

***

### 🛠️ Examples of Vulnerable Code

#### PHP

In PHP, the `include()` function can be exploited if it takes user-controlled input without proper sanitization:

```php
if (isset($_GET['language'])) {
    include($_GET['language']);
}
```

This allows any file path to be included, making it vulnerable to LFI.

#### NodeJS

NodeJS applications can also be vulnerable. For instance:

<pre class="language-javascript"><code class="lang-javascript"><strong>if(req.query.language) {
</strong>    fs.readFile(path.join(__dirname, req.query.language), function (err, data) {
        res.write(data);
    });
}
</code></pre>

Here, the `language` parameter directly influences which file is read.

Another example where the render() function in Express.js framework is vulnerable:

```javascript
app.get("/about/:language", function(req, res) {
    res.render(`/${req.params.language}/about.html`);
});
```

#### Java

In Java, similar vulnerabilities exist:

```java
<c:if test="${not empty param.language}">
    <jsp:include file="<%= request.getParameter('language') %>" />
</c:if>
```

This allows for the inclusion of files based on user input.

The `import` function may also be used to render a local file or a URL, such as the following example:

```java
<c:import url= "<%= request.getParameter('language') %>"/>
```

#### .NET

In .NET, the `Response.WriteFile` function can also be exploited:

```csharp
@if (!string.IsNullOrEmpty(HttpContext.Request.Query['language'])) {
    <% Response.WriteFile("<% HttpContext.Request.Query['language'] %>"); %> 
}
```

The `@Html.Partial()` function may also be used to render the specified file as part of the front-end template, similarly to what we saw earlier:

```cs
@Html.Partial(HttpContext.Request.Query['language'])
```

Finally, the `include` function may be used to render local files or remote URLs, and may also execute the specified files as well:

```cs
<!--#include file="<% HttpContext.Request.Query['language'] %>"-->
```



***

### 📊 Read vs Execute

Understanding the difference between functions that read file content and those that execute files is crucial for identifying vulnerabilities. Here's a summary:

| Function                  | Read Content | Execute | Remote URL |
| ------------------------- | ------------ | ------- | ---------- |
| **PHP**                   |              |         |            |
| include()/include\_once() | ✅            | ✅       | ✅          |
| require()/require\_once() | ✅            | ✅       | ❌          |
| file\_get\_contents()     | ✅            | ❌       | ✅          |
| fopen()/file()            | ✅            | ❌       | ❌          |
| **NodeJS**                |              |         |            |
| fs.readFile()             | ✅            | ❌       | ❌          |
| fs.sendFile()             | ✅            | ❌       | ❌          |
| res.render()              | ✅            | ✅       | ❌          |
| **Java**                  |              |         |            |
| include                   | ✅            | ❌       | ❌          |
| import                    | ✅            | ✅       | ✅          |
| **.NET**                  |              |         |            |
| @Html.Partial()           | ✅            | ❌       | ❌          |
| @Html.RemotePartial()     | ✅            | ❌       | ✅          |
| Response.WriteFile()      | ✅            | ❌       | ❌          |
| include                   | ✅            | ✅       | ✅          |

***

### ⚠️ Conclusion

File Inclusion vulnerabilities are critical and can lead to severe consequences, including full server compromise. Even if only source code is accessible, it may contain sensitive information that can be exploited. Proper validation and sanitization of user inputs are essential to mitigate these risks.
