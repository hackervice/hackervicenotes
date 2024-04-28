# Authentication vulnerabilities

Conceptually, authentication vulnerabilities are easy to understand. However, they are usually critical because of the clear relationship between authentication and security.

Authentication vulnerabilities can allow attackers to gain access to sensitive data and functionality. They also expose additional attack surface for further exploits. For this reason, it's important to\
learn how to identify and exploit authentication vulnerabilities, and how to bypass common protection measures.\


In this section, we explain:

* The most common authentication mechanisms used by websites.
* Potential vulnerabilities in these mechanisms.
* Inherent vulnerabilities in different authentication mechanisms.
* Typical vulnerabilities that are introduced by their improper implementation.
* How you can make your own authentication mechanisms as robust as possible.



### What is the difference between authentication and authorization? <a href="#id-1db0f5e0-3e8e-46da-afd8-645feae77980" id="id-1db0f5e0-3e8e-46da-afd8-645feae77980"></a>

Authentication is the process of verifying that a user is who they claim to be. Authorization involves verifying whether a user is allowed to do something.

For example, authentication determines whether someone attempting to access a website with the username `Carlos123` really is the same person who created the account.

Once `Carlos123` is authenticated, their permissions determine what they are authorized to do. For example, they may be authorized to access personal information about other users, or perform actions such as deleting another user's account.

Brute-force attacks

Brute-forcing usernames

Brute-forcing passwords

Username enumeration

🧪Lab: Username enumeration via different responses

Bypassing two-factor authentication

🧪lab
