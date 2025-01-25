# OS command injection

### What is OS command injection? <a href="#id-114ef81f-1e79-4601-88f0-1ee6aa6d3543" id="id-114ef81f-1e79-4601-88f0-1ee6aa6d3543"></a>

OS command injection is also known as shell injection. It allows an attacker to execute operating system (OS) commands on the server that is running an application, and typically fully compromise\
the application and its data. Often, an attacker can leverage an OS command injection vulnerability to compromise other parts of the hosting infrastructure, and exploit trust relationships to pivot the attack to other systems within the organization.\


Useful commands

PHP Example

```php
<?php
if (isset($_GET['filename'])) {
    system("touch /tmp/" . $_GET['filename'] . ".pdf");
}
?>
```

NodeJS Example

```n4js
app.get("/createfile", function(req, res){
    child_process.exec(`touch /tmp/${req.query.filename}.txt`);
})
```



Injecting OS commands

Injecting OS commands - Continued

🧪Lab: OS command injection, simple case
