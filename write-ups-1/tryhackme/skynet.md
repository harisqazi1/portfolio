---
description: '2:34'
---

# Skynet

This is my write-up for the machine on TryHackMe known as **Skynet**: [https://tryhackme.com/room/vulnnetroasted](https://tryhackme.com/room/skynet)

I start off by a running an nmap scan:

```c
sudo nmap -T4 -A -vv -sS -p- 10.10.224.214 -oN nmap_skynet.txt 
```

The results I get are the following:



