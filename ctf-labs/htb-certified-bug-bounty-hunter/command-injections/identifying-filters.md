---
description: >-
  Try all other injection operators to see if any of them is not blacklisted.
  Which of (new-line, &, |) is not blacklisted by the web application?
---

# Identifying Filters

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

In this application the payload `ip=127.0.0.1%3b+whoami` is being blocked. In order to understand this blacklist, lets try on operator at the time to see how the applications responds.

If you inserted the & and got a valid response, and Hack The Box is not accepting as a valid answer don't feel overwhelmed.&#x20;

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

The correct answer is `new-line` as we manage to successfully bypass the filter of the `&` encoded character.
