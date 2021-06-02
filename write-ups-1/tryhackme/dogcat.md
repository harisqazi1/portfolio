# dogcat

### This is a room on TryHackMe which can be found at: [https://tryhackme.com/room/dogcat](https://tryhackme.com/room/dogcat)

I started off by running my **nmap** scan on the IP address:

```c
nmap -T4 -A -p 1-10000 10.10.11.191 -oN nmap_scan.txt
```

I then got this output:

```c
Nmap scan report for 10.10.11.191
Host is up (0.19s latency).
Not shown: 9997 closed ports
PORT     STATE    SERVICE  VERSION
22/tcp   open     ssh      OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 24:31:19:2a:b1:97:1a:04:4e:2c:36:ac:84:0a:75:87 (RSA)
|   256 21:3d:46:18:93:aa:f9:e7:c9:b5:4c:0f:16:0b:71:e1 (ECDSA)
|_  256 c1:fb:7d:73:2b:57:4a:8b:dc:d7:6f:49:bb:3b:d0:20 (ED25519)
80/tcp   open     http     Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: dogcat
2884/tcp filtered flashmsg
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

I browsed to port 80, usually used for HTTP, and came across this:

![](../../.gitbook/assets/image%20%2885%29.png)

I wanted to see what was on the other ports as well. I went to **port 2884** to see what was there and was not able to see anything. I believe this was a mistake on nmaps part. Back on the website \(port 80\), I noticed that there is a GET request being done for the images:

![](../../.gitbook/assets/image%20%2828%29.png)

![](../../.gitbook/assets/image%20%2841%29.png)

I was wondering if there was a way to exploit this system. After I was unsuccessful to do so, I used a hint by using [this site](https://blog.szymex.pw/thm/dogcat.html) to see how they solved this problem. This website mentioned something about **Directory Traversal**. I then realized that instead of using what they had used to solve this problem, I would search online to learn about it to see if I can play around it myself. I came across this site [https://www.acunetix.com/websitesecurity/php-security-2/](https://www.acunetix.com/websitesecurity/php-security-2/) and it gave me examples of how to do a directory traversal using the search bar. I then ran a test search:

![](../../.gitbook/assets/image%20%284%29.png)

This made me realize that the idea was right, but I just had to find the right directory/file. I did get stuck at this point. I had seen a bunch of write-ups talk about using a **bse64 encode** in order to get the source code. I was not sure why they did it until I found this: [https://owasp.org/www-project-web-security-testing-guide/latest/4-Web\_Application\_Security\_Testing/07-Input\_Validation\_Testing/11.1-Testing\_for\_Local\_File\_Inclusion](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion). This OWASP site has a **PHP Filter** section:

![](../../.gitbook/assets/image%20%2886%29.png)

I then tried this on the website I am attacking. I seemed to have a flag:

![](../../.gitbook/assets/image%20%283%29.png)

The "flag" was just**:**

![](../../.gitbook/assets/image%20%2825%29.png)

I found out it wasn't actually a flag. I believed that I had to go deeper in this, but I assumed the mentality was correct. After a while, I was not able to get anywhere. I then read [this write-up](https://noxtal.com/writeups/2020/07/03/tryhackme-dogcat/) and got the idea to use **php://filter/convert.base64-encode/cat/resource=index** instead of **php://filter/convert.base64-encode/resource=cat**. I then got this result:

![](../../.gitbook/assets/image%20%2884%29.png)

Converting the long string to Ascii using a base64 decoder shows us this:

![](../../.gitbook/assets/image%20%2883%29.png)

I am a beginner to Local File Inclusions, so again, I got stuck. I then viewed [the same write-up](https://noxtal.com/writeups/2020/07/03/tryhackme-dogcat/) in order to see the next move. 

