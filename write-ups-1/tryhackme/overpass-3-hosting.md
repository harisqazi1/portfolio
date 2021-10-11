# Overpass 3 - Hosting

This is my write-up for the TryHackMe machine at: [https://tryhackme.com/room/overpass3hosting](https://tryhackme.com/room/overpass3hosting)

Nmap Scan command: (9:40-

```
nmap -T4 -A 10.10.168.65 -oN nmapscan
```

Nmap output:

```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-10-10 22:39 EDT
Nmap scan report for 10.10.168.65
Host is up (0.62s latency).
Not shown: 997 filtered ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.0 (protocol 2.0)
| ssh-hostkey: 
|   3072 de:5b:0e:b5:40:aa:43:4d:2a:83:31:14:20:77:9c:a1 (RSA)
|   256 f4:b5:a6:60:f4:d1:bf:e2:85:2e:2e:7e:5f:4c:ce:38 (ECDSA)
|_  256 29:e6:61:09:ed:8a:88:2b:55:74:f2:b7:33:ae:df:c8 (ED25519)
80/tcp open  http    Apache httpd 2.4.37 ((centos))
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.37 (centos)
|_http-title: Overpass Hosting
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 50.91 seconds
```

I noticed that they have 3 ports open. I went to the web (port 80) first to check it out:

![](<../../.gitbook/assets/image (325).png>)

There was no `/robots.txt` file. I then ran **gobuster** on the IP address:

```
gobuster dir --url http://10.10.168.65/  -w directory-list-lowercase-2.3-big.txt -t 40
```

About 4% of the gobuster search, I got the following output:

![](<../../.gitbook/assets/image (323).png>)

Going to the backups website, I saw this:

![](<../../.gitbook/assets/image (326).png>)

I then downloaded the file. There were two files in the zip file:

![](<../../.gitbook/assets/image (324).png>)

Using [this write-up](https://musyokaian.medium.com/overpass-3-hosting-tryhackme-walkthrough-d77703a72495), I realized that I can decrupt the file with the private key I have:

```
gpg --import priv.key //import the key
gpg --decrypt CustomerDetails.xlsx.gpg > CustomerDetails.xlsx //output to new file
```

