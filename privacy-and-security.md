# Privacy & Security

## Introduction

I take privacy and security a bit seriously. With that being said, this page is to show my setup in terms of privacy. It definitely can be better, but this is what I use. Most of these are based of the recommendation of Michael Bazzell \(his book on [Extreme Privacy](https://inteltechniques.com/book7.html) and [podcast](https://inteltechniques.com/podcast.html)\). I will mention things that work for me and those that don't in the OS specific sections later on.

## General Privacy/Security

### Passwords

It is hard to have different passwords for every single website and still be able to remember all of them. For this reason I use [**Bitwarden**](https://bitwarden.com/). I actually use this along with [**KeePassXC**](https://keepassxc.org/). They way I have it setup is that all of my passwords \(VeraCrypt, Website logins, etc.\) are in Bitwarden. Most of these passwords contain 16+ characters with randomized strings, symbols, and numbers. The password to my Bitwarden account is a long password created in KeePassXC, which is also randomized as well. I want to clarify something here. Bitwarden is a cloud based Password Manager, which KeePassXC is locally hosted. For KeyPassXC, you will have a database file to make sure you keep a note of. This is where your password for Bitwarden is stored, if you use my method. I have my KeePassXC database backed up on an SSD, and on another device.

### Firewall

Firewalls are a way to control network traffic, that way certain items/sites can be blocked. A popular term in firewalls are blocklists. You have two main types: black list and a white list. The black list it to block, while the white list is not to block. I currently have [**pfSense**](https://www.pfsense.org/) ****running in my homelab. Since pfSense is a software firewall solution I have put it on a [**Protectli Vault**](https://protectli.com/vault-4-port/#buynow). If you are interested in setting up your own, I have done a write-up on the steps here:

* Installation
  * www.pfsense.org/download
  * Architecture: AMD64 \| Installer: USB Memstick Installer \| Console: VGA \| Mirror: New York​
  * Download the .gz file and decompress it​
    * You can use 7-zip for this​
    * You should end up with a "pfSense......-amd64.img" file​
  * Download Rufus or Etcher \(flashing programs\)​
  * Flash the pfSense file onto a USB device​
  * Power down Vault, if not powered down already​
  * Connect a USB keyboard and monitor to the Vault​
  * Insert USB into another USB port​
  * Power on the device​
  * Press Enter on all of the defaults​
  * After the installation is complete, disconnect the USB keyboard and Monitor​
  * Go to 192.168.1.1 -&gt; user:**admin** / password:**pfsense**​
  * Click Advanced to allow the page to load​
  * Accept all defaults by clicking "Next", "Close", and "Finish"​
* Activate Ports
  * Activate Ports \(only applicable for 4-port & 6-port\)​
  * Access the pfSense Web interface​
  * "Interfaces" -&gt; "Assignments"​
  * Click "Add" option next to each empty port​
  * Repeat until all ports have been added​
  * Save Changes​
  * Click through each new port \("Interfaces" &gt; "Opt1"/"Opt2"\)​
    * Enable each port by checking the first box​
    * Save change​
    * Do this for all ports​
      * "Interfaces" -&gt; "Assignments"	to continue for the next port​
  * When finished with all of them, apply changes in the upper right​
  *  "Interfaces" -&gt; "Assignments" -&gt; "Bridges"​
  * Click on "Add" to create a new bridge​
  * Select the LAN option and the other ports that was added with a CNTRL-CLICK or CMD-CLICK​
  * Provide a description, such as "bridge", and then hit "Save"​
  * "Firewall" -&gt; "Rules"​
  * Click each port \(Opt, Opt2, etc.\) and click the "Add" button \(up arrow\) for each​
  * Change "Protocol" to "Any"​
  * Click "Save" after each port is modified​
  * Apply changes in the upper right after all the ports have been added​
  * "Interfaces" -&gt; "Assignments"​
  * Click on "Add" next to "BRIDGE0" and click "Save"​
  * Click on a bridge, maybe called "Opt3" or "Opt5"​
  * Enable the Interface and change the description to "bridge"​
  * Click "Save", then "Apply Changes"​
  * "Firewall" -&gt; "Rules"​
  * Click on "Bridge", then the "Add" button \(up arrow\)​
  * Change the "Protocol" to "Any" and click "Save"​
  * Apply changes in the upper-right​
* Prevent DNS leakage
  * "System" -&gt; "General Setup"​
  * Add "1.1.1.1" as a DNS server, and choose the "WAN\_DHCP-wan" interface​
  * Click "Add DNS Server"​
  * Add "1.0.0.1" as a DNS server and choose the "WAN\_DHCP-wan" interface​
  * Disable "DNS server override"​
  * Click "Save"​
* Enable AES-NI CPU Crypto & PowerD​
  * "System" -&gt; "Advanced"​
  * Click on the "Miscellaneous" tab​
  * Locate the "Cryptographic & Thermal Hardware"​
  * Select "AES-NI CPU-based Acceleration" in the drop-down​
  * "System" -&gt; "Advanced" -&gt; "Miscellaneous" -&gt; "Power Savings" -&gt; Enable "PowerD"​
* Disable Notifications​
  * "System" -&gt; "Advanced" -&gt; "Notifications"​
  * Under the "E-mail" section, disable "SMTP Notifications"​
  * In the "Sounds" section, check the "Disable startup/shutdown beep"​
  * Click "Save"​

#### pfBlockerNG

These are the resources I used to setup pfBlockerNG on pfSense:

* [https://protectli.com/kb/how-to-setup-pfblockerng/](https://protectli.com/kb/how-to-setup-pfblockerng/)
* [https://openschoolsolutions.org/pfsense-web-filter-with-pfblockerng/](https://openschoolsolutions.org/pfsense-web-filter-with-pfblockerng/)
* [https://docs.netgate.com/pfsense/en/latest/recipes/dns-block-external.html](https://docs.netgate.com/pfsense/en/latest/recipes/dns-block-external.html)
* [https://docs.netgate.com/pfsense/en/latest/recipes/dns-redirect.html](https://docs.netgate.com/pfsense/en/latest/recipes/dns-redirect.html)

#### Blocklists

Blocklists are lists of domains or IP addresses that are listed that way you can block them using a Firewall. There are the blocklists that I have used in my pfSense pfBlockerNG setup \(some of them you can download by going to **Firewall -&gt; pfBlockerNG -&gt; Feeds**\):

* IP -&gt; IPv4
  * PRI1 - Collection of Feeds from the most reputable blocklist providers. \(Primary tier\)
  * PRI4 - Collection of Feeds from Fourth Tier providers.
  * BlocklistDE - Collection of specific fail2ban reporting service Feeds.
  * PRI2 - Collection of Feeds from Secondary Tier providers.
  * PRI3 - Collection of Feeds from Tertiary Tier providers.
  * Windows\_Spy_\__Blocker
    * [https://github.com/crazy-max/WindowsSpyBlocker/tree/master/data/firewall](https://github.com/crazy-max/WindowsSpyBlocker/tree/master/data/firewall)
* DNSBL -&gt; DNSBL Groups
  * ADs Basic - Collection of ADvertisement Domain Feeds.
  * EasyList Feeds - Utilizing only the domains which are listed to be blocked in full.
  * ADs - Collection of ADvertisement Domain Feeds.
  * Firebog\_Trackers - Places that track you.
  * Spotify\_Ad\_block
    * [https://raw.githubusercontent.com/x0uid/SpotifyAdBlock/master/hosts](https://raw.githubusercontent.com/x0uid/SpotifyAdBlock/master/hosts)
  * Tracking\_Telemetry
    * [https://www.github.developerdan.com/hosts/lists/ads-and-tracking-extended.txt](https://www.github.developerdan.com/hosts/lists/ads-and-tracking-extended.txt)

### VPN

A VPN is a software that allows you to mask your true IP address from the websites you are trying to reach. A lot of poeple have this assumption of VPNs as being used for hacking and malicious content. While that may be true, we can use it as a way to protect our traffic and prevent it from being seen without our permission. I currently use [Mullvad](https://mullvad.net/en/). For VPN providers, the main thing I look for is their logging policy, and where the company is hosted from. The logging policy is important because you want to know what is going on with your data while it is in their custody. If they have an anonymous logging policy, then you should be in a good place. The best policy in my opinion, would be to have a no logging policy. That way you know that your data is not being kept by then for any amount of time. As for the company location, this is super important. If a company is part of the [14-eyes](https://restoreprivacy.com/5-eyes-9-eyes-14-eyes/) or even part of the other "eyes", the company can be mandated to share your VPN information with your country, if it is part of the "eyes". That is why it is important to check this prior to purchasing a VPN.

### Firefox 

Firefox is the more "privacy-centric" browsers from the bunch \(Edge, Chrome, Firefox, Opera, etc.\). With that being said, it still can be improved to prove your privacy. These are some of the settings I use in Firefox Settings \(or just type **about:preferences** in the search bar\):

* General
  * Uncheck **Restore previous session**
    * If you use a VPN and your connection dies, then when you use Firefox again, it will not spawn the same website, which would give away your data to the website
* Home
  * Set **Homepage and new windows** to **Blank Page**
  * Set **New tabs** to **Blank Page**
    * These will prevent Firefox from spawning their own sites when you need a new page/tab
  * Uncheck everything under **Firefox Home Content** \(Shortcuts, Recommended by Pocket, Recent activity, Snippets\)
    * This will prevent Firefox from loading their own content
* Search
  * Change **Default Search Engine** to **DuckDuckGo**
  * Uncheck **Provide Suggestions**
    * Prevents the queries from going to the Google API
* Privacy & Security
  * Under **Cookies and Site Data**, check **Delete cookies and site data when Firefox is closed**
  * Under **Logins and Passwords**, uncheck the boxes \(with the inside list first\):
    * **Show alerts about passwords for breached websites**
    * **Suggest and generate strong passwords**
    * **Autofill logins and passwords**
    * and then, **Ask to save logins and passwords for websites**
  * Under **History,** in the dropdown choose Firefox will **Use custom settings for history**.
    * Also uncheck, **Remember search and form history**, and **Remember browsing and download history**
    * Check **Clear history when Firefox closes**
    * **DO NOT** check the box next to **Always use private browsing mode**
      * It will break Firefox containers
  * Under **Address Bar,** only have **Bookmarks** and **Open tabs** showing up
  * Under **Permissions**, click **Settings**... next to **Location, Camera, Microphone,** and **Notifications**. In the popup, check the **"Block new requests.."** box on the bottom.
  * Under **Firefox Data Collection and Use**, uncheck everything
  * Under **Security**, uncheck everything
  * Under **HTTPS-Only Mode**, best practice would be to use **Enable HTTPS-Only Mode in all windows**. However, I use **Enable HTTPS-Only in private windows only**

These are the settings I use when you type in **about:config** in the search bar:

* **geo.enabled** -&gt; false
  * Disables Firefox from sharing your location
* **browser.safebrowsing.malware.enabled** -&gt; false
  * Disables Google's ability to monitor your web traffic for malware
* **dom.battery.enabled** -&gt; false
  * Blocks sending battery level information
* **extensions.pocket.enabled** -&gt; false
  * Disables the Pocket feature
* **browser.newtabpage.activity-stream.section.highlights.includePocket** -&gt; false
* **browser.newtabpage.activity-stream.feeds.telemetry** -&gt; false
* **toolkit.telemetry.server** -&gt; \*DELETE URL\*
  * Removes telemetry
* **toolkit.telemetry.unified** -&gt; false
  * Removes telemetry
* **media.autoplay.default** -&gt; 5
  * Disables audio and video from playing automatically
* **dom.webnotifications.enabled** -&gt; false
  * Disables embedded notifications
* **privacy.resistFingerprinting** -&gt; true
  * Disables some fingerprinting
* **webgl.disabled** -&gt; true
  * Disables some fingerprinting
* **network.http.sendRefererHeader** -&gt; 0
  * Disables referring website notifications \(**BREAKS SOME SITES\)**
* **identity.fxaccounts.enabled** -&gt; false
  * Disables any embedded Firefox accounts
* **beacon.enabled** -&gt; false
  * Disables data being sent to servers when leaving pages
* **browser.cache.disk.enable** -&gt; false
  * Disk cache is not used by Firefox
*  **browser.cache.disk\_cache\_ssl** -&gt; false
  * Firefox will not cache https website contents.
* **geo.provider.network.url** -&gt; 127.0.0.1
  * The data provider used to power Firefox's geolocation feature
* **network.cookie.lifetimePolicy** -&gt; 2
  * Determines whether Firefox will accept so-called session cookies \(removed when browser exits\) automatically.
    * 0 = The cookie's lifetime is supplied by the server. \(Default\)
    * 1 = The user is prompted for the cookie's lifetime.
    * 2 = The cookie expires at the end of the session \(when the browser closes\).
    * 3 =  The cookie lasts for the number of days specified by network.cookie.lifetime.days.
* **pdfjs.disabled** -&gt; true
  * Disable JavaScript for PDF view

WebRTC settings:

* **media.peerconnection.enabled** -&gt; false
* **media.peerconnection.turn.disable** -&gt; true
* **media.peerconnection.use\_document\_iceservers** -&gt; false
* **media.peerconnection.video.enabled** -&gt; false

#### Firefox Extensions

* uBlock Origin - free and open source ad blocker
* LocalCDN - emulates CDNs to improve online privacy
* Popup Blocker \(strict\) - asks you for permission before you get redirected to another site
* CleanURLs - removes tracking elements from URLs
* CanvasBlocker - Modifies JavaScript to prevent fingerprinting

If you do not want to have the hassle of modifying all of these options, then check out [https://github.com/arkenfox/user.js](https://github.com/arkenfox/user.js). The person who made the repository has made the user profile with all the privacy settings, so all you have to do is download it and upload it as your own profile.

#### Resources

* [https://blog.increasinglyadequate.com/posts/hardening\_firefox.html](https://blog.increasinglyadequate.com/posts/hardening_firefox.html)
* [https://ebin.city/~werwolf/posts/firefox-hardening-guide/](https://ebin.city/~werwolf/posts/firefox-hardening-guide/)
* [https://www.ghacks.net/overview-firefox-aboutconfig-security-privacy-preferences/](https://www.ghacks.net/overview-firefox-aboutconfig-security-privacy-preferences/)
* [http://kb.mozillazine.org/Network.cookie.lifetimePolicy](http://kb.mozillazine.org/Network.cookie.lifetimePolicy)

## Windows

I recently had an issue with my machine where I was unable to boot back into my Windows machine due to something being corrupted. I then realized it would be a mission for me to remember all of the software that I had installed prior to the Windows being corrupted. This is why suggest having a **text file \(.txt or .md\) or any other form of documentation to have inventory of all of your software that you downloaded.** That is what I do now and have it on a backup drive alongside folders that are important to me \(school, work, etc.\)

### **VeraCrypt**

Encryption is the idea of "scrambling data so that only authorized parties can understand the information. In technical terms, it is the process of converting human-readable plaintext to incomprehensible text, also known as ciphertext" \([link](https://www.cloudflare.com/learning/ssl/what-is-encryption/)\). I have this setup on my Windows SSD that way **HAVE TO** enter a password into my PC before I can get access to my Windows Login page. I use [**VeraCrypt**](https://www.veracrypt.fr/en/Home.html) for this. I followed the tutorial [here](https://www.howtogeek.com/howto/6169/use-truecrypt-to-secure-your-data/) for my own setup. Depends on what Wipe mode you choose the longer this can take. I do not plan on having this for much longer, as I am planning on switching over to Linux. You can use this for USB as well, which is great. The only bad part about this is that every time you would have to get a file off of your encrypted drive, you will have to wait for 30 seconds to 1 minute \(depending on your resources\) for Veracrypt to decrypt the drive. There was an issue I ran into with VeraCrypt when I used it for encrypting my Windows boot partition: I had just updated Windows to a newer version. I did the "update and shutdown" option. I then go to sleep. The next day I turn on my computer, and prior to booting up and even prior to VeraCrypt even giving me a chance to put my password in, Windows starts running some program in the background to verify something related to the system update. That program and Veracrypt collided \("disagreed" with each other\) leading me to a "system corrupted" type screen on Windows, basically leading me to have no other option but to use my VeraCrypt backup USB and deleting VeraCrypt off of my PC \(I could've just bypassed it, but decided to take it down in the heat of the moment\). This took 20-30 minutes for me to do \(decryption process\). Since I use my PC quickly right after I turn it on, I then realized that Veracrypt was now in the way for me, and in the future this is not something I want to deal with. A good alternative is [Bitlocker](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-overview), but it is not included by default on some Windows OSs.

### **Glasswire**

This is straight from MBs book. I paid for a one year subscription that I am currently still a part of. I honestly like the software. It has a super-clean interface, and it shows me all out going connections, and it shows other devices on my network. It is kind of a "daily-driver" software for me, since I care about what information is going in and out. Here is my issue with Glasswire. I could be wrong on this, but I can not find a way to block an IP address on its own. For example, one "website" that I see contacted multiple times during the day is wns.trafficmanager.net \(its close to that spelling\). This is basically a Windows service being contacted through the browser. I would love to, in two-clicks, block that from going out, but I am not able to do that. The only way for me to block that would be to block the whole browser, which is not ideal. Also, Glasswire is a good software, but $30 is something I would pay for a one-time payment, not for every year. Example of Glasswire \(fire means it is blocked from contacting the internet\):

![](.gitbook/assets/image%20%28164%29.png)

### [Portmaster](https://safing.io/portmaster/)

So this is another host based firewall that MB mentioned. This one, however, is not something I would recommend. Not due to privacy reasons, but for usability in general. I just had a bad overall experience with it. First of all, the second right after you download it, it blocks all outgoing internet connections and starts updating itself \(or updating its internal list?\). This took me 30 minutes. Then the whole app itself is super overwhelming. There is a bit too much going on that what I would like. If you want another alternative to this, I would recommend [Netlimiter](https://www.netlimiter.com/). I have not looked into their privacy policy, but it works similar to Glasswire, and it is a one-time payment of $30.

### **Software-based** 

I have used [O&OShutup10](https://www.oo-software.com/en/shutup10) and [W10Privacy](https://www.w10privacy.de/english-home/). You can use either, I chose to test out both. However the issue I ran into with these is that you have to use these software after every update. So that is why I do not go deep into these. These are also pretty straight forward.

O&OShutup10 screenshot:

![](.gitbook/assets/image%20%28163%29.png)

W10Privacy screenshot:

![](.gitbook/assets/image%20%28165%29.png)

I think with the setup I have \(Firewall with the aforementioned software\) I am at about 95% privacy/tracking coverage \(but this is super biased since it is my personal setup\). Would live to hear your guys' opinion on this. I think the fight is definitely doable, with just a couple updates to the blocklists.















