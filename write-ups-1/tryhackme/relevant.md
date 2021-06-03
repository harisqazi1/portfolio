# Relevant

#### This is a write-up for the room on TryHackMe located at: [https://tryhackme.com/room/relevant](https://tryhackme.com/room/relevant)

### Nmap Scan:

I ran **nmap -T4 -A 10.10.161.100 -oN nmap\_output**, however this did not get me anywhere. It seemed to me that the host was blocking ping probes. I then ran **nmap -Pn 10.10.161.100**:

```c
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-03 15:25 EDT
Nmap scan report for 10.10.161.100
Host is up (0.23s latency).
Not shown: 997 closed ports
PORT     STATE    SERVICE
135/tcp  filtered msrpc
139/tcp  filtered netbios-ssn
3389/tcp filtered ms-wbt-server

Nmap done: 1 IP address (1 host up) scanned in 120.16 seconds

```

I later ran **sudo nmap -p- -Pn -sS -A 10.10.161.100 -oN fullscan.txt** which got me way more information**:**

```c
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-03 15:37 EDT
Nmap scan report for 10.10.161.100
Host is up (0.22s latency).
Not shown: 65527 filtered ports
PORT      STATE SERVICE            VERSION
80/tcp    open  http               Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Windows Server 2016 Standard Evaluation 14393 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
| rdp-ntlm-info: 
|   Target_Name: RELEVANT
|   NetBIOS_Domain_Name: RELEVANT
|   NetBIOS_Computer_Name: RELEVANT
|   DNS_Domain_Name: Relevant
|   DNS_Computer_Name: Relevant
|   Product_Version: 10.0.14393
|_  System_Time: 2021-06-03T19:48:50+00:00
| ssl-cert: Subject: commonName=Relevant
| Not valid before: 2021-06-02T19:27:43
|_Not valid after:  2021-12-02T19:27:43
|_ssl-date: 2021-06-03T19:49:26+00:00; 0s from scanner time.
49663/tcp open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
49667/tcp open  msrpc              Microsoft Windows RPC
49669/tcp open  msrpc              Microsoft Windows RPC
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2016|2012|2008 (91%)
OS CPE: cpe:/o:microsoft:windows_server_2016 cpe:/o:microsoft:windows_server_2012 cpe:/o:microsoft:windows_server_2008:r2
Aggressive OS guesses: Microsoft Windows Server 2016 (91%), Microsoft Windows Server 2012 (85%), Microsoft Windows Server 2012 or Windows Server 2012 R2 (85%), Microsoft Windows Server 2012 R2 (85%), Microsoft Windows Server 2008 R2 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 4 hops
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h24m00s, deviation: 3h07m51s, median: 0s
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: Relevant
|   NetBIOS computer name: RELEVANT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2021-06-03T12:48:49-07:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-06-03T19:48:51
|_  start_date: 2021-06-03T19:28:19

TRACEROUTE (using port 139/tcp)
HOP RTT       ADDRESS
1   59.97 ms  10.2.0.1
2   ... 3
4   200.41 ms 10.10.161.100

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 722.09 seconds

```

### Foothold:

I needed to find a way into the system, or a foothold. I browsed to the IP address:

![](../../.gitbook/assets/image%20%2890%29.png)

I wanted to see if SMB is being ran on this page, I then used metasploit to find out what version of SMB was running:

![](../../.gitbook/assets/image%20%2897%29.png)

I then ran a nmap script just for smb "**nmap --script smb-os-discovery 10.10.161.100**":

```c
Host script results:
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: Relevant
|   NetBIOS computer name: RELEVANT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2021-06-03T13:22:34-07:00
```

I believed that smb is my way in. I then ran **smbclient**:

![](../../.gitbook/assets/image%20%2896%29.png)

I then noticed a unique Sharename **nt4wrksv**. I wanted to see what's in here. So I ran smbclient to have access to the share:

![](../../.gitbook/assets/image%20%2887%29.png)

I then saw a **passwords.txt** file. 

![](../../.gitbook/assets/image%20%2888%29.png)

I then wanted to download it:

![](../../.gitbook/assets/image%20%2891%29.png)

I was able to then read the password file on my local system:

![](../../.gitbook/assets/image%20%28100%29.png)

The passwords were encoded, and it seemed to be base64 to me. Using [https://www.base64decode.org/](https://www.base64decode.org/) I was able to decode the text:

![](../../.gitbook/assets/image%20%2892%29.png)

To me, this seemed like a login for an smb user. Viewing [this writeup](https://medium.com/cybersecpadawan/relevant-walk-through-on-tryhackme-f7dedfcb00dc), I noticed that the decoded had made me miss another password. I then had 2 logins:

![](../../.gitbook/assets/image%20%2894%29.png)

I did not know what tool to use next to get further. I then used [the same write-up](https://medium.com/cybersecpadawan/relevant-walk-through-on-tryhackme-f7dedfcb00dc) and noticed that I would have to use **psexec** which is located in **/usr/share/doc/python3-impacket/examples**. I was then able to run it on the SMB:

![](../../.gitbook/assets/image%20%2898%29.png)

The user for Bill did not work:

![](../../.gitbook/assets/image%20%2899%29.png)

Now I wanted to move back and see another way inside. Both port 80 and 49663 both are running the same website:

![](../../.gitbook/assets/image%20%2889%29.png)

I then ran gobuster on the port 49663:



