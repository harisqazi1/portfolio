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

1hr:20

For finding the admin of the site, I had a bit of trouble. 

