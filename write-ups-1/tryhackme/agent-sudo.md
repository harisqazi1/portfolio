# Agent Sudo

This is my write-up for the machine on TryHackMe located at: [https://tryhackme.com/room/agentsudoctf](https://tryhackme.com/room/agentsudoctf).

nmap scan: `nmap -T4 -A 10.10.126.175`

```
Starting Nmap 7.92 ( https://nmap.org ) at 2021-12-17 19:59 EST
Nmap scan report for 10.10.126.175
Host is up (0.16s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ef:1f:5d:04:d4:77:95:06:60:72:ec:f0:58:f2:cc:07 (RSA)
|   256 5e:02:d1:9a:c4:e7:43:06:62:c1:9e:25:84:8a:e7:ea (ECDSA)
|_  256 2d:00:5c:b9:fd:a8:c8:d8:80:e3:92:4f:8b:4f:18:e2 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Annoucement
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.08 seconds
```

Port 80 was opened, so I checked it out:

![](<../../.gitbook/assets/image (323) (1) (1).png>)

I tried to use BurpSuite here to edit the User-Agent to be "Agent S", based off of my assumption that my codename was Agent S. This did not work. I then used **gobuster** alongside the **directory-list-2.3-small.txt** wordlist. I then was reading [this write-up](https://marcorei7.wordpress.com/2020/07/29/008-agent-sudo/) which led me to realize that I was not supposed to brute-force the website, but actually just change my user agent (I used **User-Agent Switcher and Manager** on Firefox):

![](<../../.gitbook/assets/image (331) (1) (1) (1) (1).png>)

We now know that user C is **chris** and his password is weak. I chose this time to answer the questions on THM:

![](<../../.gitbook/assets/image (346) (1) (1) (1) (1) (1) (1) (1).png>)

For the next task, it seemed that I needed to brute-force the password for FTP. Knowing that the username was **chris**, I then used hydra to try to brute-force the password: `hydra -l chris -P rockyou.txt ftp://10.10.126.175 -t 16`. I was able to get the password:

![](<../../.gitbook/assets/image (328) (1) (1) (1) (1).png>)

After logging into the ftp server with the credentials, we see the following:

![](<../../.gitbook/assets/image (344) (1) (1) (1) (1) (1) (1) (1) (1).png>)

I then used **mget \*** to download all of the files:

![](<../../.gitbook/assets/image (327) (1) (1) (1) (1) (1).png>)

![To\_agentJ.txt](<../../.gitbook/assets/image (341) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png>)

![cutie.png](<../../.gitbook/assets/image (332) (1) (1) (1) (1) (1) (1) (1).png>)

![cute-alien.jpg](<../../.gitbook/assets/image (347) (1) (1) (1) (1) (1) (1) (1).png>)

It seems from the **To\_agentJ.txt** file that we have to find the password in one of the images provided. Running **strings** on the **cutie.png** file, I saw this towards the end:

![](<../../.gitbook/assets/image (325) (1).png>)

This led me to assume that this was the correct file to brute-force. I ran `binwalk -e cutie.png` and this led me to a directory with the following:

![](<../../.gitbook/assets/image (342) (1) (1) (1) (1) (1) (1) (1) (1).png>)

There is a password for the zip file. I used **fcrackzip** in order to try to brute-force the password. I was trying for a bit to understand what was going on, but I was unable to crack the password. This is when I went back to the [website where I read the write-up from previously](https://marcorei7.wordpress.com/2020/07/29/008-agent-sudo/) and found out about **zip2john**. This program gets you a hash from a zip file which you can use with John The Ripper. I then ran the following to output the hash into a location that worked for me: `zip2john ../../8702.zip > ../../../zip_hash`. I then ran john on the hash, and got the password in a couple seconds:

![](<../../.gitbook/assets/image (338) (1) (1) (1) (1) (1) (1) (1).png>)

After entering the password for the zip by running `7z e 8702.zip`, I then was able to get the file hidden in it:

![](<../../.gitbook/assets/image (340) (1) (1) (1) (1) (1) (1) (1) (1) (1).png>)

In order to crack the password for the other image, I used **StegSeek**:

![](<../../.gitbook/assets/image (329) (1) (1) (1) (1) (1).png>)

This led me to this text file:

![](<../../.gitbook/assets/image (326) (1) (1).png>)

When I entered the password **hackerrules!** as the answer to the question _SSH password_, I got it correct. This means that this is the password for SSH. Logging in with those credentials, I was able to get into the system, and get the user flag:

![](<../../.gitbook/assets/image (348) (1) (1) (1) (1) (1) (1) (1).png>)

There was another jpeg on the system as well. I used **FileZilla** in order to download the file to my local machine:

![](<../../.gitbook/assets/image (345) (1) (1) (1) (1) (1) (1) (1) (1).png>)

![Alien\_autopsy.jpg](<../../.gitbook/assets/image (330) (1) (1) (1) (1) (1).png>)

On THM there was a question asking _What is the incident of the photo called?_. In my research I came to the conclusion it had something to do with Roswell and Aliens. I had to go back to the write-up, in order to realize the answer was **roswell alien autopsy**. I am still at the user (james) level, and I need to escalate my privileges. I ran `sudo -l` to see what sudo commands my user had:

![](<../../.gitbook/assets/image (333) (1) (1) (1) (1) (1).png>)

Seems that he can run /bin/bash with sudo permission. I Googled "exploit db (ALL, !root) /bin/bash" and found the following page: [https://web.archive.org/web/20210120002645/www.exploit-db.com/exploits/47502](https://web.archive.org/web/20210120002645/www.exploit-db.com/exploits/47502). This has an exploit in it for our system. I ran the exploit and got root:

![](<../../.gitbook/assets/image (343) (1) (1) (1) (1) (1) (1).png>)

I then also got the root flag:

![](<../../.gitbook/assets/image (339) (1) (1) (1) (1) (1) (1) (1).png>)
