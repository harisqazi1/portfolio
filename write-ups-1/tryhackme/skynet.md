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

![](../../.gitbook/assets/image%20%28177%29.png)

 I then ran feroxbuster on the IP address:

```c
feroxbuster --url http://10.10.224.214 -w directory-list-2.3-big.txt
```

This resulted in:

```c
[>-------------------] - 7m    599288/25476360 4h      found:31      errors:58333  
[>-------------------] - 7m     48073/1273818 111/s   http://10.10.224.214
[>-------------------] - 7m     44397/1273818 103/s   http://10.10.224.214/admin
[>-------------------] - 7m     46814/1273818 109/s   http://10.10.224.214/css
[>-------------------] - 7m     41748/1273818 98/s    http://10.10.224.214/js
[>-------------------] - 7m     41566/1273818 98/s    http://10.10.224.214/config
[>-------------------] - 6m     39220/1273818 93/s    http://10.10.224.214/ai
[>-------------------] - 6m     38660/1273818 96/s    http://10.10.224.214/ai/notes
[>-------------------] - 5m     25328/1273818 73/s    http://10.10.224.214/squirrelmail
[>-------------------] - 5m     23502/1273818 69/s    http://10.10.224.214/squirrelmail/themes
[>-------------------] - 5m     27861/1273818 83/s    http://10.10.224.214/squirrelmail/plugins
[>-------------------] - 5m     24115/1273818 72/s    http://10.10.224.214/squirrelmail/images
[>-------------------] - 5m     25772/1273818 77/s    http://10.10.224.214/squirrelmail/src
[>-------------------] - 5m     21173/1273818 64/s    http://10.10.224.214/squirrelmail/config
[>-------------------] - 5m     23467/1273818 71/s    http://10.10.224.214/squirrelmail/plugins/calendar
[>-------------------] - 5m     23831/1273818 76/s    http://10.10.224.214/squirrelmail/plugins/demo
[>-------------------] - 5m     23040/1273818 74/s    http://10.10.224.214/squirrelmail/plugins/test
[>-------------------] - 5m     21175/1273818 69/s    http://10.10.224.214/squirrelmail/themes/css
[>-------------------] - 4m     20956/1273818 73/s    http://10.10.224.214/squirrelmail/plugins/filters
[>-------------------] - 4m     20306/1273818 78/s    http://10.10.224.214/squirrelmail/plugins/fortune
[>-------------------] - 4m     18264/1273818 74/s    http://10.10.224.214/squirrelmail/plugins/administrator
```

One of the links led me to a site:

![](../../.gitbook/assets/image%20%28168%29.png)

I then viewed the hint for the first question:

![](../../.gitbook/assets/image%20%28169%29.png)

I then realized I will have to go with this route first. Looking back at the nmap scan, we see that port **445** seems to be for Samba. Using **enum4linux**, I saw the following shares:

```bash
//10.10.224.214/print$  Mapping: DENIED, Listing: N/A
//10.10.224.214/anonymous       Mapping: OK, Listing: OK
//10.10.224.214/milesdyson      Mapping: DENIED, Listing: N/A
//10.10.224.214/IPC$    [E] Can't understand response:
```

I also got the following users using enum4linux:

```bash
S-1-22-1-1001 Unix User\milesdyson (Local User)
S-1-5-21-2393614426-3774336851-1116533619-501 SKYNET\nobody (Local User)
S-1-5-21-2393614426-3774336851-1116533619-513 SKYNET\None (Domain Group)
```

The enum4linux information is verified by **smbmap**:

![](../../.gitbook/assets/image%20%28174%29.png)

There seems to be read access to they anonymous disk. Using **smbclient** I was able to see a couple files:

![](../../.gitbook/assets/image%20%28176%29.png)

There was also a directory called **logs**:

![](../../.gitbook/assets/image%20%28175%29.png)

I downloaded all of those files to my local machine using "**mget \*"**. I viewed all of the downloaded files. 

![](../../.gitbook/assets/image%20%28172%29.png)

The **log** files seemed to contain passwords. I went back to the mail site and entered the username **milesdyson** and password **cyborg007haloterminator** and I got in:

> What is Miles password for his emails? **cyborg007haloterminator**

![](../../.gitbook/assets/image%20%28178%29.png)

There does seem to be another user here **serenakogan**. I kept that in my notes just for future reference. The email from **skynet@skynet** seemed to have some interesting information in it:

![](../../.gitbook/assets/image%20%28171%29.png)

Using that password, I was able to log into miles' samba share:

![](../../.gitbook/assets/image%20%28170%29.png)

In the **notes** directory, I found a file called **important.txt**. In it it contained the following information:

```bash
1. Add features to beta CMS /45kra24zxs28v3yd
2. Work on T-800 Model 101 blueprints
3. Spend more time with my wife
```

This points us to the answer for our next question.

> What is the hidden directory? **/45kra24zxs28v3yd**
>
> What is the vulnerability called when you can include a remote file for malicious purposes? **remote file inclusion**

Visiting that directory online leads to a new page:

![](../../.gitbook/assets/image%20%28173%29.png)

I then ran **feroxbuster** again on this directory leading to the following results:

```bash
[>-------------------] - 1m     20462/1185239 217/s   http://10.10.224.214/45kra24zxs28v3yd/
[>-------------------] - 1m     11588/1185239 146/s   http://10.10.224.214/45kra24zxs28v3yd/administrator
[>-------------------] - 1m     10241/1185239 134/s   http://10.10.224.214/45kra24zxs28v3yd/administrator/alerts
[>-------------------] - 1m     13158/1185239 174/s   http://10.10.224.214/45kra24zxs28v3yd/administrator/media
[>-------------------] - 1m     12527/1185239 165/s   http://10.10.224.214/45kra24zxs28v3yd/administrator/templates
[>-------------------] - 1m     12172/1185239 162/s   http://10.10.224.214/45kra24zxs28v3yd/administrator/js
[>-------------------] - 1m      7290/1185239 97/s    http://10.10.224.214/45kra24zxs28v3yd/administrator/components
[>-------------------] - 1m     13761/1185239 186/s   http://10.10.224.214/45kra24zxs28v3yd/administrator/classes
[>-------------------] - 1m      6704/1185239 91/s    http://10.10.224.214/45kra24zxs28v3yd/administrator/templates/default
```

I then found this site using the link of http://10.10.224.214/45kra24zxs28v3yd/administrator/:

![](../../.gitbook/assets/image%20%28167%29.png)

