# Virtual Machines

## Hacking (CTFs)

I use Kali Linux. Sometimes I install meta-packages (see [https://www.kali.org/docs/general-use/metapackages/](https://www.kali.org/docs/general-use/metapackages/)), however, usually I just install the applications I need to it. Here is the following script that works for me.

```bash
#/bin/bash
# Update Repository and upgrade packages
sudo apt update && sudo apt upgrade -y 
#Download REMnux
cd ~/Downloads
wget https://REMnux.org/remnux-cli
mv remnux-cli remnux
chmod +x remnux
sudo mv remnux /usr/local/bin
# Download other apps as needed
```

## Breach Data

After some deliberation between different platforms, I settled on [https://www.kicksecure.com/](https://www.kicksecure.com/). This is a hardened Debian system that uses TOR to route NTP, APT package installs, etc. The following is my configuration (**not a script**):

```bash
#Remove apt install being routed through TOR (decreases security, but makes download much faster))
sudo nano /etc/apt/sources.list.d/debian.list
#Remove "tor+" before the non-tor links
sudo apt update && sudo apt upgrade -y #Update & Upgrade
sudo apt install keepassxc #Password manager 
sudo apt install qbittorrent transmission #Torrent software
#In qbittorrent enable Anonymous Mode in Settings: 
#https://github.com/qbittorrent/qBittorrent/wiki/Anonymous-Mode
#Download and setup arkenfox for firefox: https://github.com/arkenfox/user.js/
#Firefox ------
#Change default search to DuckDuckGo
#Change DNS to quad9 in browser
#--------------
sudo apt install git wget sqlitebrowser #General Tools
# Get a new temp email address for creating accounts only meant for the VM
```

## Testing Environment

Since I use Pop!\_OS ([https://pop.system76.com/](https://pop.system76.com/)) daily, I also use it as my testing environment. This allows me to test code or configurations before applying it to my own system. Other than `sudo apt update && sudo apt upgrade -y`, I don't have a specific configuration for it. I download things as needed. One thing to mention here is that `ntpdate` comes clutch when your date/time is not matching the actual date/time. You have to force an NTP update with something like: `ntpdate pool.ntp.org`.
