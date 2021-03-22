# Password Cracking

## Setup

When it comes to password cracking, there are two main components that can potentially prevent you from cracking the password. This is not being able to capture the 4-way handshake, as well as not having the actual password in your wordlist. This guide will walk you though how to crack WPA/WPA2 passwords, as long as you have the two aforementioned items. I will be doing these cracks using the Kali Linux VMware OS. In order to intercept packets and crack passwords, we will need a network adapter. I have bought [this one](https://www.amazon.com/Panda-300Mbps-Wireless-USB-Adapter/dp/B00EQT0YK2/) from Amazon:

![](../.gitbook/assets/image%20%2842%29.png)

Next you will have to install the software for cracking the password. We will be using the aircrack-ng suite for this. In order to download this, run the following command:

```c
sudo apt-get install aircrack-ng
```

This will download aircrack-ng, as well as the dependencies for air-crack-ng.

## Intercepting Passwords

In order to crack passwords, we will first have to capture the 4-way handshake. This allows us to then crack the capture \(.cap\) file in order to find out the password. In order to do this, we have to run the following command:

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



