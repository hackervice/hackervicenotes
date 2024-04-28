# WHOIS

Is this lab we are asked to perform a WHOIS lookup against [paypal.com](http://paypal.com) domain and lookup for the registrar Internet Assigned Numbers Authorizy (IANA) ID number.

All we have to do is enter the command whois [paypal.com](http://paypal.com/) | grep -i iana

<figure><img src="../.gitbook/assets/Screenshot 2024-03-17 222306.png" alt=""><figcaption></figcaption></figure>

And we get the IANA ID: 292

For the second part it ask what is the email contact for the [tesla.com](http://tesla.com/) domain.

<figure><img src="../.gitbook/assets/Screenshot 2024-03-17 222713.png" alt=""><figcaption></figcaption></figure>

With the command whois [tesla.com](http://tesla.com/) | grep -i admin we can easily filter what we are looking for. The contact is [admin@dnstinations.com](mailto:admin@dnstinations.com)
