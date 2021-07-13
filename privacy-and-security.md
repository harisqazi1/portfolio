# Privacy & Security

## Introduction

I take privacy and security a bit seriously. With that being said, this page is to show my setup in terms of privacy. It definitely can be better, but this is what I use. Most of these are based of the recommendation of Michael Bazzell \(his book on Extreme Privacy and podcast\)

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

### VPN

A VPN is a software that allows you to mask your true IP address from the websites you are trying to reach. A lot of poeple have this assumption of VPNs as being used for hacking and malicious content. While that may be true, we can use it as a way to protect our traffic and prevent it from being seen without our permission. I currently use [NordVPN](https://nordvpn.com/). However, I am planning on switching over to better solutions such as [Mullvad](https://mullvad.net/en/) or [ProtonVPN](https://protonvpn.com/). For VPN providers, the main thing I look for is their logging policy, and where the company is hosted from. The logging policy is important because you want to know what is going on with your data while it is in their custody. If they have an anonymous logging policy, then you should be in a good place. The best policy in my opinion, would be to have a no logging policy. That way you know that your data is not being kept by then for any amount of time. As for the company location, this is super important. If a company is part of the [14-eyes](https://restoreprivacy.com/5-eyes-9-eyes-14-eyes/) or even part of the other "eyes", the company can be mandated to share your VPN information with your country, if it is part of the "eyes". That is why it is important to check this prior to purchasing a VPN.

### Firefox 

Firefox is the more "privacy-centric" browsers from the bunch \(Edge, Chrome, Firefox, Opera, etc.\). With that being said, it still can be improved to prove your privacy. These are

## Windows

I recently had an issue with my machine where I was unable to boot back into my Windows machine due to something being corrupted. I then realized it would be a mission for me to remember all of the software that I had installed prior to the Windows being corrupted. This is why suggest having a **text file \(.txt or .md\) or any other form of documentation to have inventory of all of your software that you downloaded.** That is what I do now and have it on a backup drive alongside folders that are important to me \(school, work, etc.\)

### SSD or Hard Drive Encryption \(or USB\)

Encryption is the idea of "scrambling data so that only authorized parties can understand the information. In technical terms, it is the process of converting human-readable plaintext to incomprehensible text, also known as ciphertext" \([link](https://www.cloudflare.com/learning/ssl/what-is-encryption/)\). I have this setup on my Windows SSD that way **HAVE TO** enter a password into my PC before I can get access to my Windows Login page. I use [**VeraCrypt**](https://www.veracrypt.fr/en/Home.html) for this. I followed the tutorial [here](https://www.howtogeek.com/howto/6169/use-truecrypt-to-secure-your-data/) for my own setup. Depends on what Wipe mode you choose the longer this can take. I do not plan on having this for much longer, as I am planning on switching over to Linux. You can use this for USB as well, which is great. The only bad part about this is that every time you would have to get a file off of your encrypted drive, you will have to wait for 30 seconds to 1 minute \(depending on your resources\) for Veracrypt to decrypt the drive.











