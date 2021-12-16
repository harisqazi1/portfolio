# Reconnaissance

{% hint style="info" %}
The "Web Hacking" section are notes that I have compiled while learning using the PortSwigger Web Security Academy and from the book _Bug Bounty Bootcamp: The Guide to Finding and Reporting Web Vulnerabilities_ by Vickie Li. A majority of the notes/codes are from them, however I will modify these according to my needs.
{% endhint %}

* Manually walk through the target
  * See what you are able to do at different privilege levels
  * See what you can/can't access
* Google Dorking - advanced Google searches
  * site - searches for pages on only one site
    * `print site:python.org`
  * inurl - searches for pages with the text you choose in the URL
    * `inurl: "admin.php" site:example.com`
  * intitle - searches for specific strings in a page's title
    * `intitle: "index of" site: example.com`
  * link - searches for websites that reference the link
    * `link:"example.com"`
  * filetype - searches for pages with specific file extensions
    * `filetype:log site:example.com`
  * wildcard - to replace the wildcard with anything
    * `how to hack a *`
  * quotes - will make the search more specific
    * `"how to hack"`
  * or - can be used to search for one term or another
    * `"how to hack" site:(reddit.com | stackoverflow.com)`
  * minus - exclude an item from the search
    * `"how to hack" -php`
  * [https://www.exploit-db.com/google-hacking-database](https://www.exploit-db.com/google-hacking-database)
* Scope Discovery
  * whois search
    * `whois google.com`
  * reverse whois search
  * nslookup
  * certificate parsing
    * [https://crt.sh/](https://crt.sh)
    * [https://search.censys.io/?q=](https://search.censys.io/?q=)
  * subdomain enumeration
    * [https://github.com/aboul3la/Sublist3r](https://github.com/aboul3la/Sublist3r)
    * [https://github.com/TheRook/subbrute](https://github.com/TheRook/subbrute)
    * [https://github.com/OWASP/Amass/](https://github.com/OWASP/Amass/)
    * [https://github.com/OJ/gobuster](https://github.com/OJ/gobuster)
  * service enumeration
    * nmap
    * shodan
  * directory brute-forcing
    * [https://github.com/maurosoria/dirsearch](https://github.com/maurosoria/dirsearch)
    * [https://github.com/OJ/gobuster](https://github.com/OJ/gobuster)
  * spidering the site
    * [https://www.zaproxy.org/download/](https://www.zaproxy.org/download/)
  * third-party hosting
    * `site:s3.amazonaws.com`` `_`Company_name`_
    * amazonaws bucket _company\_name_
    * amazonaws _company\_name_
    * s3 _company\_name_
    * [https://buckets.grayhatwarfare.com/](https://buckets.grayhatwarfare.com)
  * github recon
    * search for the users profile
    * go through commits
    * [https://github.com/streaak/keyhacks](https://github.com/streaak/keyhacks)
    * [https://github.com/michenriksen/gitrob](https://github.com/michenriksen/gitrob)
    * [https://github.com/trufflesecurity/truffleHog](https://github.com/trufflesecurity/truffleHog)
    * [https://github.com/kevthehermit/PasteHunter](https://github.com/kevthehermit/PasteHunter)
  * recon platforms
    * [https://github.com/projectdiscovery/nuclei](https://github.com/projectdiscovery/nuclei)
    * [https://github.com/intrigueio/intrigue-core](https://github.com/intrigueio/intrigue-core)
