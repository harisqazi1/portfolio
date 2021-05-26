# Ice

### This is a room by TryHackMe located at: [https://tryhackme.com/room/ice](https://tryhackme.com/room/ice)

### Recon \(30 min\):

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

### **Gain Access**

\*\*\*\*

\*\*\*\*



