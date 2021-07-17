# Anthem

This is my write-up for the TryHackMe box known as Anthem located at: [https://tryhackme.com/room/anthem](https://www.tryhackme.com/room/anthem). 

## Website Analysis

The firs thing I did was ran nmap onto the IP address. I did this with the following command:

```c
sudo nmap -Pn -T4 -A -sS -p- <IP Address> -oN output
```

I then got the following as the result:

```c
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-10 22:28 EDT
Nmap scan report for 10.10.183.242
Host is up (0.12s latency).
Not shown: 65533 filtered ports
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: WIN-LU09299160F
|   NetBIOS_Domain_Name: WIN-LU09299160F
|   NetBIOS_Computer_Name: WIN-LU09299160F
|   DNS_Domain_Name: WIN-LU09299160F
|   DNS_Computer_Name: WIN-LU09299160F
|   Product_Version: 10.0.17763
|_  System_Time: 2021-07-11T02:34:48+00:00
| ssl-cert: Subject: commonName=WIN-LU09299160F
| Not valid before: 2021-07-10T02:27:57
|_Not valid after:  2022-01-09T02:27:57
|_ssl-date: 2021-07-11T02:35:47+00:00; 0s from scanner time.
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows XP (85%)
OS CPE: cpe:/o:microsoft:windows_xp::sp3
Aggressive OS guesses: Microsoft Windows XP SP3 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 4 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   48.48 ms  10.6.0.1
2   ... 3
4   124.12 ms 10.10.183.242

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 442.82 seconds
```

> What port is for the web server? **80**
>
> What port is for remote desktop service? **3389**

From this point, I went to browsing the webpage to see if there is anything suspicious or standing out for me to use. After browsing around and not finding anything, I decided to run DirBuster on the website:

![](../../.gitbook/assets/image%20%28113%29.png)

Although I was getting a lot of information from the DirBuster search, it was the hint for the question which got me further:

![](../../.gitbook/assets/image%20%28116%29.png)

My first thought after looking at this was **robots.txt**, and sure enough when I went to that webpage, I found this:

![](../../.gitbook/assets/image%20%28114%29.png)

> What is a possible password in one of the pages web crawlers check for? **UmbracoIsTheBest!**
>
> What CMS is the website using? **Umbraco**

For the previous question, I took a look at [this writeup](https://apjone.uk/anthem-tryhackme-write-up/), which made me realize that **Umbraco** was a CMS all along. As for the next question, a quick glance at the main page can show you what the domain name is:

![](../../.gitbook/assets/image%20%28115%29.png)

> What is the domain of the website? **Anthem.com**

I was having a hard time trying to find the admin of the website. I looked around all around the website looking though the JavaScript and HTML, and was not able to find anything. I then [viewed this write-up](https://swafox.com/anthem/) to see where I went wrong. I had to look more closely at the "poem" on one of the pages:

![](../../.gitbook/assets/image%20%28145%29.png)

Instead of searching about this online, I realized that I might have heard this before in reference to Solomon Grundy \(maybe in regards to Comics?\). I tried it and it worked as the answer!

> What's the name of the Administrator? Solomon Grundy

Using the hint from TryHackMe, I realized that I had to find the naming scheme for the email. I then found this:

![](../../.gitbook/assets/image%20%28141%29.png)

> Can we find the email address of the administrator? SG@anthem.com

## Spot the flags

I had to use the hint for the first flag. The hint was "**Have we inspected the pages yet?**", and then I tried to go off of that, and got nowhere. I then viewed [the same write-up](https://swafox.com/anthem/) and noticed that the author used burpsuite. I realized that I will try to do the same. I then found the flag:

![](../../.gitbook/assets/image%20%28138%29.png)

For Flag 2, I just found it on source of the main page:

![](../../.gitbook/assets/image%20%28147%29.png)

For Flag 3, I was looking around on different pages, and ended up finding it on the site: **&lt;IP\_adress&gt;/authors/jane-doe**:

![](../../.gitbook/assets/image%20%28136%29.png)

For Flag 4, I then found it following the same steps as Flag 1:

![](../../.gitbook/assets/image%20%28142%29.png)

## Final Stage

We have to figure out the username and password for the box. My first thought was to make a name list based on the names I saw on the website. I looked at the other port, other than 80, and it was 3389. Port 3389 was used for RDP, also knowing as Remote Desktop Protocol. I then had to find a software to get access to the desktop of the machine. I find out about [**rdesktop**](http://www.rdesktop.org/), and used it. As for the username, I viewed [this write up](https://pencer.io/ctf/ctf-thm-anthem/#task-3---final-stage) and realized that the username would be **SG**. It took me a while to get the password, which was from the first section of the machine: **UmbracoIsTheBest!** I then used that information to log in using the command:

```c
rdesktop -u SG 10.10.152.51
```

After I entered the password, I then saw the following:

![](../../.gitbook/assets/image%20%28140%29.png)

I opened the **user.txt** file, and got the following:

![](../../.gitbook/assets/image%20%28146%29.png)

> Gain initial access to the machine, what is the contents of user.txt? **THM{N00T\_NO0T}**

After I was lost for a while trying to find an answer to the next question, I viewed [this write-up](https://pencer.io/ctf/ctf-thm-anthem/), and noticed that a folder was hidden in the **C:\** drive. In order to view it, you had to change the settings to view the hidden folder:

![](../../.gitbook/assets/image%20%28148%29.png)

I was then able to see the hidden folder:

![](../../.gitbook/assets/image%20%28139%29.png)

I was unable to view the file, because I did not have permissions. I then changed the permissions to be for **SG**:

![](../../.gitbook/assets/image%20%28144%29.png)

I was then able to view the file:

![](../../.gitbook/assets/image%20%28143%29.png)

> Can we spot the admin password? **ChangeMeBaby1MoreTime**

I did not have any idea about how to get root on a Windows machine. I then viewed the same previous write-up and noticed that we have the admin password. So we can use PowerShell at an admin level. I then got into the Desktop folder of the root and got the flag:

![](../../.gitbook/assets/image%20%28149%29.png)

## Lessons Learned

This is one of my first 5 Windows machines that I have learned to work on. I did learn a lot about, such as making sure to looking for hidden folders to how to spot flags using BurpSuite. It was a great box. It took me about 3 hours to complete. A lot of that had to do with my lack of knowledge in Windows exploitation. In addition, enumerating for me took a while, since some searches were taking a long time so I had to go find an alternative.



