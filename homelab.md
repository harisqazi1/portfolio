# Homelab

## Introduction

I wanted to build a Homelab for myself in order to get enterprise experience without having an enterprise-caliber occupation. I then started doing research and asking around until I found a setup that worked for me. This write-up is my journey through creating the Homelab.

## Materials

### Hardware

| Type | Name | Cost \(USD $\) |
| :--- | :--- | :--- |
| Firewall | Protectli Vault 4 Port​ | 319.00 |
| Power strip with surge protector​ | Tripp Lite 650VA UPS Battery Backup, LCD, 325W Eco Green, USB, RJ11, 8 Outlets ​ | 95.08​ |
| Server​ | Dell PowerEdge R710 2U Server X5650 2.66GHz 12-Cores / 64gb / 3x 1TB SAS / 2xPSU​ | 326.76​ |
| Cat 8 Ethernet Cable​ | Cat8 Ethernet Cable, Outdoor&Indoor, 6FT Heavy Duty High Speed 26AWG Cat8 LAN Network Cable 40Gbps, 2000Mhz with Gold Plated RJ45 Connector, Weatherproof S/FTP UV Resistant for Router/Gaming​ | 8.99 |
| Cat 7 Ethernet Cable​ | Cat7 Ethernet Cable 1.5 ft \(2 Pack\) RJ45 Connector - Double Shielded STP - 10 Gigabit 600MHz​ | 7.49 |
| Cat 7 Ethernet Cable​ | Amazon Basics RJ45 Cat 7 High-Speed Gigabit Ethernet Patch Internet Cable, 10Gbps, 600MHz - White, 5-Foot​ | 6.99​ |
| Unmanaged Switch​ | NETGEAR 5-Port Gigabit Ethernet Unmanaged Switch \(GS105NA\)​ | 28.99​ |
| Power Cord​ | Cable Matters 2-Pack 16 AWG Heavy Duty 3 Prong Computer Monitor Power Cord in 15 Feet, UL Listed \(NEMA 5-15P to IEC C13\)​ | 24.99​ |
| Total |  | 818.29 |

### Software/OS

| Type | Name |
| :--- | :--- |
| Firewall | pfSense |
| Virtualization \(Type I Hypervisor\) | VMware ESXi 6.5U3​ |
| Cloud​ | NextCloud​ |
| RSS Feed​ | FreshRSS​ |
| Password/Hash Cracking​ | Kali Linux​ |
| Media System | Jellyfin |
| Dashboard | Homer |

## Firewall Setup

| Hardware | Software |
| :--- | :--- |
| Protectli Vault 4 Port | pfSense |

I setup the firewall first, since all of the data was going to go through the Firewall. I used [Michael Bazzell's Book](https://www.amazon.com/Extreme-Privacy-What-Takes-Disappear/dp/B094LDWKGZ/) in order to setup pfSense on my Protectli Vault. The steps \(for me\) were as follows:​

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

