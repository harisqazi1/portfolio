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

![](../../.gitbook/assets/image%20%28119%29.png)

This seemed to be some sort of selling website so I was not sure if this is what I am looking for. Port 443 and 8080 lead me to the same result:

![](../../.gitbook/assets/image%20%28117%29.png)

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

![](../../.gitbook/assets/image%20%28118%29.png)



