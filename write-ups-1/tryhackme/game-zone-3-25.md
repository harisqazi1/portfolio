# Game Zone 3:25-

This is my write-up for the TryHackMe machine at: [https://tryhackme.com/room/gamezone](https://tryhackme.com/room/gamezone).

### Deploy the vulnerable machine

The hint for the first question is **Reverse Image Search**. I then posted the picture in `Yandex.com` and got the following:

![](../../.gitbook/assets/image%20%28279%29.png)

I then clicked on an image and it led me to a title: **Hitman Absolution**. I then Google-d the character name, and got the answer:

![](../../.gitbook/assets/image%20%28290%29.png)

> What is the name of the large cartoon avatar holding a sniper on the forum? Agent 47

### Obtain access via SQLi

 I then ran an **nmap** scan:

```c
nmap -T4 -A 10.10.72.202 -oN nmap_scan.txt
```

I got the following output:

```c
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-27 16:30 EDT
Nmap scan report for 10.10.72.202
Host is up (0.20s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 61:ea:89:f1:d4:a7:dc:a5:50:f7:6d:89:c3:af:0b:03 (RSA)
|   256 b3:7d:72:46:1e:d3:41:b6:6a:91:15:16:c9:4a:a5:fa (ECDSA)
|_  256 53:67:09:dc:ff:fb:3a:3e:fb:fe:cf:d8:6d:41:27:ab (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Game Zone
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.05 seconds

```

I noticed that there were only 2 ports open: 22 and 80. I went on port 80 to see what the website is:

![](../../.gitbook/assets/image%20%28278%29.png)

After entering in **' or 1=1 -- -** in the username portion and left the password field empty \(based on the recommendations of the THM room creator\). I was in then:

![](../../.gitbook/assets/image%20%28282%29.png)

> When you've logged in, what page do you get redirected to? **portal.php**

### **Using SQLMap**

For this one, we have to intercept the request to get the format for the SQLMap usage later. I turned on **Burpsuite**, and then got the request:

![](../../.gitbook/assets/image%20%28286%29.png)

I then saved this to a file:

![](../../.gitbook/assets/image%20%28277%29.png)

I then ran the command the room recommended:

```c
sqlmap -r sqlmap_output.txt --dbms=mysql --dump
```

I then got the following output:

![](../../.gitbook/assets/image%20%28291%29.png)

> In the users table, what is the hashed password?  **ab**\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
>
> What was the username associated with the hashed password? **agent47**
>
> What was the other table name? **post**

### Cracking a password with JohnTheRipper

For this one, I went with Hashcat, just because I am more comfortable with hashcat. I ran the following command:

```c
hashcat -m 1400 ab5db915fc9cea6c78df88106c6500c57f2b52901ca6c0c6218f04122c3efd14 rockyou.txt
```

The rockyou.txt file is default in Kali, and it is located at /usr/share/wordlist. I copied it to the local directory, ran **gunzip** on it, and got the file. As for the hashcat hash crack run, I got the following output:

![](../../.gitbook/assets/image%20%28276%29.png)

> What is the de-hashed password? **vi\*\*\*\*\*\*\*\*\*\***

I ran `ssh agent47@10.10.72.202` and used the password I had received before, and got into the machine:

![](../../.gitbook/assets/image%20%28281%29.png)

I then got the user.txt flag:

![](../../.gitbook/assets/image%20%28288%29.png)

> What is the user flag? **649\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\***

### **Exposing serviced with reverse SSH tunnels**

> How many TCP sockets are running? **5**

I then ran the following command, based on the recomendation from the machine:

```c
ssh -L 10000:localhost:10000 agent47@10.10.72.202
```

I then posted the password that I had gotten previously into the password for the SSH, and got in again:

![](../../.gitbook/assets/image%20%28285%29.png)

This time, we have the webapp running on our localhost:

![](../../.gitbook/assets/image%20%28287%29.png)

> What is the name of the exposed CMS? **Webmin**

The login credentials for the webapp were the same credentials from previously, this leads to this site:

![](../../.gitbook/assets/image%20%28280%29.png)

> What is the CMS version? **1.580**

### Privilege Escalation with Metasploit

I searched on msfconsole for an exploit:

![](../../.gitbook/assets/image%20%28292%29.png)

I then started filling in options, and ended up with this:

![](../../.gitbook/assets/image%20%28284%29.png)

I realized that the RHOST was supposed to be set to 127.0.0.1, based on [this writeup](https://www.aldeid.com/wiki/TryHackMe-Game-Zone#.5BTask_6.5D_Privilege_Escalation_with_Metasploit). When I changed that option in msfconsole, I was then able to get the exploit to work. I then got the flag:

![](../../.gitbook/assets/image%20%28275%29.png)

> What is the root flag? **a4\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\***





