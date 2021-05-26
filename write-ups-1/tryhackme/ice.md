# Ice

### This is a room by TryHackMe located at: [https://tryhackme.com/room/ice](https://tryhackme.com/room/ice)

## Recon \(30 min\):

I started this room my running an nmap scan \(this was from the hint from the room\):

```c
sudo nmap -sS -p- 10.10.158.64 -oN nmap_syn
```

I then got this output for it after 14 minutes:

```c
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-26 00:10 EDT
Nmap scan report for 10.10.158.64
Host is up (0.19s latency).
Not shown: 65523 closed ports
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
5357/tcp  open  wsdapi
8000/tcp  open  http-alt
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49158/tcp open  unknown
49159/tcp open  unknown
49160/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 871.23 seconds

```

For the nmap, I would recommend using something like:

```c
sudo nmap -T4 -A -sS -p- <ip_address> -oN output
```

The reason for this is that with this, you would get more information about the software running than just the names of the services.

**QUESTION: What port is this open on?**

> **3389**

**QUESTION: What service did nmap identify as running on port 8000? \(First word of this service\)**

> **Icecast**

**QUESTION: What does Nmap identify as the hostname of the machine? \(All caps for the answer\)**

> DARK-PC

## **Gain Access \(5 min\)**

I noticed that there was an image of the icecast system on the Gain Access tab.

![](../../.gitbook/assets/image%20%2846%29.png)

I assumed that this would be the vulnerable service I had to get into. I ran another nmap scan, this time only catered towards the port the icecast service was running on:

```c
nmap -T4 -A <IP_address> -p 3389
```

I then got the following result:

```c
PORT     STATE SERVICE            VERSION
3389/tcp open  ssl/ms-wbt-server?
| ssl-cert: Subject: commonName=Dark-PC
| Not valid before: 2021-05-25T17:57:35
|_Not valid after:  2021-11-24T17:57:35
|_ssl-date: 2021-05-26T18:02:17+00:00; -2s from scanner time.

Host script results:
|_clock-skew: -2s
```

I thought for some reason that I would get a different result, however I was wrong. I decided then to visit the site itself to see if there was anything on there for me to see. I ran into an error of not being able to connect, even while my VPN connection was working correctly:



