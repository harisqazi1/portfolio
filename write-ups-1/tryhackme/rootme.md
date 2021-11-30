# RootMe

This is my write-up for the machine on TryHackMe called **RootMe**: [https://tryhackme.com/room/rrootme](https://tryhackme.com/room/rrootme)

I first ran an nmap scan on the IP address:

```c
sudo nmap -T4 -A 10.10.78.153
```

This is the output I got:

```c
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-20 15:45 EDT
Nmap scan report for 10.10.78.153
Host is up (0.10s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4a:b9:16:08:84:c2:54:48:ba:5c:fd:3f:22:5f:22:14 (RSA)
|   256 a9:a6:86:e8:ec:96:c3:f0:03:cd:16:d5:49:73:d0:82 (ECDSA)
|_  256 22:f6:b5:a6:54:d9:78:7c:26:03:5a:95:f3:f9:df:cd (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: HackIT - Home
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.91%E=4%D=9/20%OT=22%CT=1%CU=38132%PV=Y%DS=4%DC=T%G=Y%TM=6148E50
OS:A%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=10D%TI=Z%CI=Z%II=I%TS=A)SEQ
OS:(SP=103%GCD=1%ISR=10D%TI=Z%CI=Z%TS=A)OPS(O1=M506ST11NW7%O2=M506ST11NW7%O
OS:3=M506NNT11NW7%O4=M506ST11NW7%O5=M506ST11NW7%O6=M506ST11)WIN(W1=F4B3%W2=
OS:F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN(R=Y%DF=Y%T=40%W=F507%O=M506NNSN
OS:W7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%D
OS:F=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O
OS:=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W
OS:=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%R
OS:IPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 4 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 1025/tcp)
HOP RTT       ADDRESS
1   36.67 ms  10.6.0.1
2   ... 3
4   100.50 ms 10.10.78.153

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 27.63 seconds

```

My first step was to visit the website on port 80:

![](<../../.gitbook/assets/image (192).png>)

There seems to be nothing on the website other that the following text. I then ran **feroxbuster** with the big.txt file from the Seclists Github repository:

![](<../../.gitbook/assets/image (189).png>)

Two links stood out to me:

![](<../../.gitbook/assets/image (188).png>)

![](<../../.gitbook/assets/image (195).png>)

My assumption was to upload the **pentestmonkey php-reverse-shell** and then get a webshell using that. I first updated the script to have my information in it (IP and port):

![](<../../.gitbook/assets/image (185).png>)

Apparently PHP is not permitted:

![](<../../.gitbook/assets/image (198).png>)

I then though about using an alternative php version like phtml. That worked... a bit:

![](<../../.gitbook/assets/image (196).png>)

However, I was not able to get a shell on the system. I then tried the original pentestmonkey script, but then I changed the extension to be **.phtml**, and it worked:

![](<../../.gitbook/assets/image (191).png>)

At this point, I had realized that I had to answer questions on the TryHackMe site.&#x20;

> Scan the machine, how many ports are open? **2**
>
> What version of Apache is running? **2.4.29**
>
> What service is running on port 22? **ssh**
>
> What is the hidden directory? **/panel/**

Back to the shell, I was trying to find the user.txt file. It was not in the home directory folders. I then ran:

```c
find / -name user.txt
```

I then saw the following in the big output:

![](<../../.gitbook/assets/image (193).png>)

I then got the user.txt flag:

![](<../../.gitbook/assets/image (187).png>)

I then had to view the hint provided to see what command I should run to check for files with SUID permission. They recommended **find / -user root -perm /4000**. I ran that command and noticed a couple commands I could potentially use:

![](<../../.gitbook/assets/image (197).png>)

On TryHackMe, the format of the question seems to be in the following format:

![](<../../.gitbook/assets/image (186).png>)

This means that the executable has to be 6 letters in size. I tried **/usr/bin/python** and it worked. I went to **GTFOBins** and searched on it for python. I then came across the following:

![](<../../.gitbook/assets/image (190).png>)

I ran this code, but modified it to read the file from the root directory:

```c
python -c 'print(open("/root/root.txt").read())'
```

I then got the flag:

![](<../../.gitbook/assets/image (194).png>)
