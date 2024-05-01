# Passive Infrastructure Identification

* Netcraft - `https://sitereport.netcraft.com`
  *   [Netcraft](https://www.netcraft.com/) can offer us information about the servers without even interacting with them, and this is something valuable from a passive information gathering point of view.

      Details we can observe from the report:

      | `Background`      | General information about the domain, including the date it was first seen by Netcraft crawlers. |
      | ----------------- | ------------------------------------------------------------------------------------------------ |
      | `Network`         | Information about the netblock owner, hosting company, nameservers, etc.                         |
      | `Hosting history` | Latest IPs used, webserver, and target OS.                                                       |
* Wayback Machine - `http://web.archive.org/`
  * This tool can be used to find older versions of a website at a point in time. Let's take a website running WordPress, for example. We may not find anything interesting while assessing it using manual methods and automated tools, so we search for it using Wayback Machine and find a version that utilizes a specific (now vulnerable) plugin. Heading back\
    to the current version of the site, we find that the plugin was not removed properly and can still be accessed via the\
    `wp-content` directory. We can then utilize it to gain remote code execution on the host and a nice bounty.
*   Waybackurls

    `go install github.com/tomnomnom/waybackurls@latest`

    ```
    export GOPATH="$HOME/go" 
    PATH="$GOPATH/bin:$PATH"  
    ```

    ```
    waybackurls -dates https://facebook.com > waybackurls.txt
    cat waybackurls.txt
    ```
