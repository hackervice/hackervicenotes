# DNS

This lab is composed with three question

**The first one is: Which IP address maps to** [**inlanefreight.com**](http://inlanefreight.com/)**?**

<figure><img src="../../.gitbook/assets/Screenshot 2024-03-22 183252.png" alt=""><figcaption></figcaption></figure>

To retrieve the IP information we can simply use the command `nslookup [inlanefreight.com](<http://inlanefreight.com/>)` and we got the IP 134.209.24.248.

Another command would be `dig [inlanefreight.com](<http://inlanefreight.com/>)`



Second question: Which domain is returned when querying the PTR record for 134.209.24.248?

<figure><img src="../../.gitbook/assets/Screenshot 2024-03-22 183805 (1).png" alt=""><figcaption></figcaption></figure>

For querying PTR records for a specific IP we can use the command `nslookup -query=PTR 134.209.24.248` or alternately `dig -x 134.209.24.248` . The answer is [inlanefreight.com](http://inlanefreight.com/)

**Third question: What is the first mailserver returned when querying the MX records for** [**paypal.com**](http://paypal.com/)**?**

<figure><img src="../../.gitbook/assets/Screenshot 2024-03-22 184355.png" alt=""><figcaption></figcaption></figure>

For querying PTR records for a specific IP we can use the command `nslookup -query=PTR 134.209.24.248` or alternately `dig -x 134.209.24.248` . The answer is [inlanefreight.com](http://inlanefreight.com/)

**Third question: What is the first mailserver returned when querying the MX records for** [**paypal.com**](http://paypal.com/)**?**
