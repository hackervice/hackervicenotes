# File upload vulnerabilities

### What are file upload vulnerabilities? <a href="#id-4974f48e-130e-4467-ad4c-81ffe360bcb1" id="id-4974f48e-130e-4467-ad4c-81ffe360bcb1"></a>

File upload vulnerabilities are when a web server allows users to upload files to its filesystem without sufficiently validating things like their name, type, contents, or size. Failing to properly\
enforce restrictions on these could mean that even a basic image upload function can be used to upload arbitrary and potentially dangerous files instead. This could even include server-side script files that enable remote code execution.\


In some cases, the act of uploading the file is in itself enough to cause damage. Other attacks may involve a follow-up HTTP request for the file, typically to trigger its execution by the server.

How do file upload vulnerabilities arise?

Exploiting unrestricted file uploads to deploy a web shell

🧪Lab: Remote code execution via web shell upload

Exploiting flawed validation of file uploads

Flawed file type validation

Flawed file type validation - Continued

Flawed file type validation - Continued

🧪Lab: Web shell upload via Content-Type restriction bypass
