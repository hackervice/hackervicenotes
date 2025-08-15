---
description: >-
  Use what you learned in this section to execute the command 'ls -la'. What is
  the size of the 'index.php' file?
---

# Bypassing Space Filters

Even we if we found a way to bypass a blacklist, it does mean we can successfully append any command.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

As this section suggest, we should use Bash Brace Expansion, which automatically adds spaces between arguments wrapped between braces: `{ls,-la}`

And the correct answer is `1613`.&#x20;
