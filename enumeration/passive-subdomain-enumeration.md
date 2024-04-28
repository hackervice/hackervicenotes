# Passive Subdomain Enumeration

* VirusTotal
  * virustotal.com
* Certificates
  * https://censys.io
  *
  *
* Automating Passive Subdomain Enumeration
  *
  *   Create a file called sources.txt with the follwing contents:

      ```
      baidu
      bufferoverun
      crtsh
      hackertarget
      otx
      projectdiscovery
      rapiddns
      sublist3r
      threatcrowd
      trello
      urlscan
      vhost
      virustotal
      zoomeye
      ```
  *   Execute the following commands to gather information from these sources.

      ```
      export TARGET="example.com"
      cat sources.txt | while read source; do theHarvester -d "${TARGET}" -b $source -f "${source}_${TARGET}";done
      ```
  *   Extract all the subdomains found and sort them via the following command:

      ```
      cat *.json | jq -r '.hosts[]' 2>/dev/null | cut -d':' -f 1 | sort -u > "${TARGET}_theHarvester.txt”
      ```
  *   Merge all the passive reconnaissance files:

      ```
      cat example.com_*.txt | sort -u > example.com_subdomains_passive.txt
      cat example.com_subdomains_passive.txt 
      ```
