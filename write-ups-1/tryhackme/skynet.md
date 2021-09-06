---
description: '2:34-2:51 -> 3:03-'
---

# Skynet

This is my write-up for the machine on TryHackMe known as **Skynet**: [https://tryhackme.com/room/vulnnetroasted](https://tryhackme.com/room/skynet)

I start off by a running an nmap scan:

```c
sudo nmap -T4 -A -vv -sS -p- 10.10.224.214 -oN nmap_skynet.txt 
```

The results I get are the following:

```c
# Nmap 7.91 scan initiated Mon Sep  6 15:37:11 2021 as: nmap -T4 -A -vv -sS -p- -oN nmap_skynet.txt 10.10.224.214
Increasing send delay for 10.10.224.214 from 5 to 10 due to 11 out of 21 dropped probes since last increase.
Warning: 10.10.224.214 giving up on port because retransmission cap hit (6).
Nmap scan report for 10.10.224.214
Host is up, received reset ttl 61 (0.098s latency).
Scanned at 2021-09-06 15:37:11 EDT for 927s
Not shown: 65525 closed ports
Reason: 65525 resets
PORT      STATE    SERVICE     REASON         VERSION
22/tcp    open     ssh         syn-ack ttl 61 OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 99:23:31:bb:b1:e9:43:b7:56:94:4c:b9:e8:21:46:c5 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDKeTyrvAfbRB4onlz23fmgH5DPnSz07voOYaVMKPx5bT62zn7eZzecIVvfp5LBCetcOyiw2Yhocs0oO1/RZSqXlwTVzRNKzznG4WTPtkvD7ws/4tv2cAGy1lzRy9b+361HHIXT8GNteq2mU+boz3kdZiiZHIml4oSGhI+/+IuSMl5clB5/FzKJ+mfmu4MRS8iahHlTciFlCpmQvoQFTA5s2PyzDHM6XjDYH1N3Euhk4xz44Xpo1hUZnu+P975/GadIkhr/Y0N5Sev+Kgso241/v0GQ2lKrYz3RPgmNv93AIQ4t3i3P6qDnta/06bfYDSEEJXaON+A9SCpk2YSrj4A7
|   256 57:c0:75:02:71:2d:19:31:83:db:e4:fe:67:96:68:cf (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBI0UWS0x1ZsOGo510tgfVbNVhdE5LkzA4SWDW/5UjDumVQ7zIyWdstNAm+lkpZ23Iz3t8joaLcfs8nYCpMGa/xk=
|   256 46:fa:4e:fc:10:a5:4f:57:57:d0:6d:54:f6:c3:4d:fe (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICHVctcvlD2YZ4mLdmUlSwY8Ro0hCDMKGqZ2+DuI0KFQ
80/tcp    open     http        syn-ack ttl 61 Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Skynet
110/tcp   open     pop3        syn-ack ttl 61 Dovecot pop3d
|_pop3-capabilities: TOP SASL UIDL CAPA AUTH-RESP-CODE PIPELINING RESP-CODES
139/tcp   open     netbios-ssn syn-ack ttl 61 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp   open     imap        syn-ack ttl 61 Dovecot imapd
|_imap-capabilities: capabilities have IMAP4rev1 post-login SASL-IR listed more Pre-login LOGIN-REFERRALS LOGINDISABLEDA0001 ENABLE OK ID LITERAL+ IDLE
445/tcp   open     netbios-ssn syn-ack ttl 61 Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
8851/tcp  filtered unknown     no-response
24495/tcp filtered unknown     no-response
31760/tcp filtered unknown     no-response
43412/tcp filtered unknown     no-response
<Redacted extra info>
```

Going to the website, I see:

![](../../.gitbook/assets/image%20%28167%29.png)

 I then ran feroxbuster on the IP address:



