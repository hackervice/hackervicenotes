# Virtual Hosts

For this lab we are asked to enumerate 5 vHosts targets. The web server is 10.129.194.60 and vHosts to be fuzzed s www.inlanefreight.htb

The most efficient way to enumerate this vHost is by using the `ffuf` tool with this [wordlist](https://github.com/danielmiessler/SecLists/blob/master/Discovery/DNS/namelist.txt) by SecList.

The following command is the first time I tried to identified the flags within the vHosts found

```bash
ffuf -w ./vhosts -u http://10.129.194.60 -H "HOST: FUZZ.inlanefreight.htb"
```

<figure><img src="../../../.gitbook/assets/Screenshot 2024-04-20 012011.png" alt=""><figcaption></figcaption></figure>

As we can see from the output, almost all of the vHosts have a Content Length 10918 size. With a very large list (more than 150000 words)!

It would be a very tedious job to lookup in all of these results.

We are also assuming the Flag is not in a vHost with a 10918 sized Content Lenght. But typically what draws our attention are uncommon results. So let’s try to filter all vHosts by adding a filter to the end of the last command.

```bash
ffuf -w ./vhosts -u http://10.129.194.60 -H "HOST: FUZZ.inlanefreight.htb" -fs 10918
```

<figure><img src="../../../.gitbook/assets/Screenshot 2024-04-20 012037.png" alt=""><figcaption></figcaption></figure>

As we can see, with the filter we narrowed our results to just 6 targets. Much more doable

Then instead of manually lookup individually for each vHost let’s try to automatize this process. First lets gather all the results to a single file:

```
ap
app
citrix
customers
dmz
www2
```

Then let’s adapt the script from HTB with a few teaks in order to lookup to the desire information:

```javascript
cat ./results | while read results;do echo "\n********\nFUZZING: ${results}\n********";curl -s http://inlanefreight.htb -H "HOST: ${results}.inlanefreight.htb" | grep "FLAG\|HTB";done
```

<figure><img src="../../../.gitbook/assets/Screenshot 2024-04-20 012528.png" alt=""><figcaption></figcaption></figure>

**Enumerate the target and find a vHost that contains flag No. 1. Submit the flag value as your answer (in the format HTB{DATA}).**

`HTB{h8973hrpiusnzjoie7zrou23i4zhmsxi8732zjso}`

**Enumerate the target and find a vHost that contains flag No. 2. Submit the flag value as your answer (in the format HTB{DATA}).**

`HTB{u23i4zhmsxi872z3rn98h7nh2sxnbgriusd32zjso}`

**Enumerate the target and find a vHost that contains flag No. 3. Submit the flag value as your answer (in the format HTB{DATA}).**

`HTB{Fl4gF0uR_o8763tznb4xou7zhgsniud7gfi734}`

**Enumerate the target and find a vHost that contains flag No. 4. Submit the flag value as your answer (in the format HTB{DATA}).**

`HTB{bzghi7tghin2u76x3ghdni62higz7x3s}`

**Find the specific vHost that starts with the letter "d" and submit the flag value as your answer (in the format HTB{DATA}).**

`HTB{7zbnr4i3n7zhrxn347zhh3dnrz4dh7zdjfbgn6d}`
