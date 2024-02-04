# Aircrack

## TLDR Commands

```bash
sudo apt-get install aircrack-ng #installs the aircrack suite
sudo airmon-ng start wlan0 #assuming wlan0 is the wlan of the wireless card
sudo airodump-ng wlan0mon #see local BSSIDs/ESSIDs
#run following on one terminal
sudo airodump-ng -c <channel_number_of_network> --bssid <BSSID of network> -w <output location of captured files> wlan0mon
#run following on another terminal and wait for handshake capture
sudo aireplay-ng --deauth <count of deauthentication packets> -a <BSSID of network> wlan0mon
#after capture
sudo aircrack-ng <location of .cap file> -w <dictionary or wordlist>
```

## Setup

When it comes to password cracking, there are two main components that can potentially prevent you from cracking the password. This is not being able to capture the 4-way handshake, as well as not having the actual password in your wordlist. This guide will walk you though how to crack WPA/WPA2 passwords, as long as you have the two aforementioned items. I will be doing these cracks using the Kali Linux VMware OS. In order to intercept packets and crack passwords, we will need a network adapter. I have bought [this one](https://www.amazon.com/Panda-300Mbps-Wireless-USB-Adapter/dp/B00EQT0YK2/) from Amazon:

![](<../../../.gitbook/assets/image (42) (1).png>)

Next you will have to install the software for cracking the password. We will be using the aircrack-ng suite for this. In order to download this, run the following command:

```c
sudo apt-get install aircrack-ng
```

This will download aircrack-ng, as well as the dependencies for air-crack-ng.

## Intercepting Passwords

In order to crack passwords, we will first have to capture the 4-way handshake. This allows us to then crack the capture (.cap) file in order to find out the password. In order to do this, we have to run the following command:

```bash
sudo airmon-ng start wlan0 #assuming wlan0 is the wlan of the wireless card
```

This command will initialize our wireless card to be in monitor mode. We can then use it to intercept traffic. The next command we will use will allow us to see the nearby networks and see their BSSIDs.

```bash
sudo airodump-ng wlan0mon #same assumption as above
```

We can now see the local networks, alongside their BSSIDs. I usually let the command run for 10-30 seconds, which gives it enough time to get together a good list of local networks. We then have to capture the handshake. For this to happen, we will have to have two terminal windows open simultaneously. On the first terminal we will run the following command:

```bash
sudo airodump-ng -c <channel_number_of_network> --bssid <BSSID of network> -w <output location of captured files> wlan0mon
```

On the other terminal, we will run:

```bash
sudo aireplay --deauth <count of deauthentication packets> -a <BSSID of network> wlan0mon
```

This part gets a bit complicated. We have to run the "aireplay" command with a count that will allow us to get a handshake capture. For me, 20 works. The capture will be seen in the terminal running "airodump-ng". If nothing has changed in the "airodump-ng" terminal, then the handshake has not been captured yet. When the handshake is captured, you can go to the "Cracking Passwords" section of this tutorial.

## Cracking Passwords

In order to crack passwords, there are 2 main methods to do it: using aircrack-ng to crack the password file, or using a password cracking software such as John the ripper or hashcat. I will be using the aircrack-ng command to crack the passwords. As I mentioned earlier, the only way you will be able to crack the password are based on two conditions: you have a 4-way handshake captured in a .cap file, and you have the password to the wifi in the dictionary you are using. I would reccomend building your own wordlist. This can help you have a dictionary which has 8+ characters (which is the minimum password size). I would look at the following links to make your own list. I use a (somewhat) combination of all of the following links:

* [https://github.com/kennyn510/wpa2-wordlists](https://github.com/kennyn510/wpa2-wordlists)
* [https://github.com/danielmiessler/SecLists/tree/master/Passwords/WiFi-WPA](https://github.com/danielmiessler/SecLists/tree/master/Passwords/WiFi-WPA)
* [https://github.com/berzerk0/Probable-Wordlists/blob/master/Real-Passwords/WPA-Length/Real-Password-WPA-MegaLinks.md](https://github.com/berzerk0/Probable-Wordlists/blob/master/Real-Passwords/WPA-Length/Real-Password-WPA-MegaLinks.md)

You could also make your own password list using the "crunch" command on Kali or Parrot OS.

To crack the password, using the aircrack-ng command, we will run the following command:

```bash
sudo aircrack-ng <location of .cap file> -w <dictionary or wordlist>
```

### Resources

* [https://book.hacktricks.xyz/pentesting/pentesting-network/wifi-attacks#wifi-basic-commands](https://book.hacktricks.xyz/pentesting/pentesting-network/wifi-attacks#wifi-basic-commands)
* [https://medium.com/@brannondorsey/crack-wpa-wpa2-wi-fi-routers-with-aircrack-ng-and-hashcat-a5a5d3ffea46](https://medium.com/@brannondorsey/crack-wpa-wpa2-wi-fi-routers-with-aircrack-ng-and-hashcat-a5a5d3ffea46)
* [https://medium.com/@TheEyeOfCyberBuckeyeSecurity/how-to-crack-wpa-wpa2-wi-fi-passwords-using-aircrack-ng-8cb7161abcf9](https://medium.com/@TheEyeOfCyberBuckeyeSecurity/how-to-crack-wpa-wpa2-wi-fi-passwords-using-aircrack-ng-8cb7161abcf9)
* (MacOS aircrack setup) [https://louisabraham.github.io/articles/WPA-wifi-cracking-MBP.html](https://louisabraham.github.io/articles/WPA-wifi-cracking-MBP.html)
