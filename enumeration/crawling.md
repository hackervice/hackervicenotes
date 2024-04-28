# Crawling

We can use\
[ffuf](https://github.com/ffuf/ffuf) to discover files and folders that we cannot spot by simply browsing the website. All we need to do is launch `ffuf` with a list of folders names and instruct it to look recursively through them.

```
ffuf -recursion -recursion-depth 1 -u http://example.com/FUZZ -w /opt/useful/SecLists/Discovery/Web-Content/raft-small-directories-lowercase.txt
```

* `recursion`: Activates the recursive scan.
* `recursion-depth`: Specifies the maximum depth to scan.
* `u`: Our target URL, and `FUZZ` will be the injection point.
* `w`: Path to our wordlist.

Save the results to a file called `folders.txt`

**Sensitive Information Disclosure**

In this example CeWL is used to extract words with a minimum length of 5 characters `-m5`, convert them to lowercase `--lowercase` and save them into a file called wordlist.txt `-w <FILE>`:

```
cewl -m5 --lowercase -w wordlist.txt http://192.168.10.10
```

Save the results to a file called `worlist.txt`

Then we combine the previous findings into the following command

```
ffuf -w ./folders.txt:FOLDERS,./wordlist.txt:WORDLIST,./extensions.txt:EXTENSIONS -u http://192.168.10.10/FOLDERS/WORDLISTEXTENSIONS
```

* `w`: We separate the wordlists by comma and add an alias to them to inject them as fuzzing points later
* `u`: Our target URL with the fuzzing points.

Lastly, we can use the curl command to retrieve any findings

```
curl http://example.com/wp-content/secret~
```
