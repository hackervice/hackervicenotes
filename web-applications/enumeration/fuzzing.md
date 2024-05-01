# Fuzzing

This technique is used to discover web folders in a application that we are trying to test. A manual approach would be visiting a specific well-known  page like `https://www.example.com/login` we will probably get a HTTP code <mark style="color:green;">200 OK</mark> and for `https://www.example.com/randomfolder` a HTTP code <mark style="color:red;">404 Page Not Found</mark>.&#x20;

As you can imagine, fuzzing manually will take forever and this is why we have tools that do this automatically. We should use a commonly know wordlist, and some of the most commonly used can be found under the GitHub [SecLists](https://github.com/danielmiessler/SecLists) repository.
