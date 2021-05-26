# Ice

### This is a room by TryHackMe located at: [https://tryhackme.com/room/ice](https://tryhackme.com/room/ice)

## Recon \(30 min\):

I started this room my running an nmap scan \(this was from the hint from the room\):

```c
sudo nmap -sS -p- 10.10.158.64 -oN nmap_syn
```

I then got this output for it after 14 minutes:

```c
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-26 00:10 EDT
Nmap scan report for 10.10.158.64
Host is up (0.19s latency).
Not shown: 65523 closed ports
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
5357/tcp  open  wsdapi
8000/tcp  open  http-alt
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49158/tcp open  unknown
49159/tcp open  unknown
49160/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 871.23 seconds

```

For the nmap, I would recommend using something like:

```c
sudo nmap -T4 -A -sS -p- <ip_address> -oN output
```

The reason for this is that with this, you would get more information about the software running than just the names of the services.

**QUESTION: What port is this open on?**

> **3389**

**QUESTION: What service did nmap identify as running on port 8000? \(First word of this service\)**

> **Icecast**

**QUESTION: What does Nmap identify as the hostname of the machine? \(All caps for the answer\)**

> DARK-PC

## **Gain Access \(**2**0 min\)**

I noticed that there was an image of the icecast system on the Gain Access tab.

![](../../.gitbook/assets/image%20%2846%29.png)

I assumed that this would be the vulnerable service I had to get into. I ran another nmap scan, this time only catered towards the port the icecast service was running on:

```c
nmap -T4 -A <IP_address> -p 3389
```

I then got the following result:

```c
PORT     STATE SERVICE            VERSION
3389/tcp open  ssl/ms-wbt-server?
| ssl-cert: Subject: commonName=Dark-PC
| Not valid before: 2021-05-25T17:57:35
|_Not valid after:  2021-11-24T17:57:35
|_ssl-date: 2021-05-26T18:02:17+00:00; -2s from scanner time.

Host script results:
|_clock-skew: -2s
```

I thought for some reason that I would get a different result, however I was wrong. I decided then to visit the site itself to see if there was anything on there for me to see. I was not able to access the website, maybe they have no webserver for the port. I then searched up CVEs related to Icecast, the service running on port 3389. The hint states that it would have a score of 7.5 or 7.4.

![cvedetails.com for Icecast](../../.gitbook/assets/image%20%2855%29.png)

I then entered in all of the Vulnerability types that I assumed were correct. After I was unsuccessful, I then looked into the hint, and then I was still unable to solve it. I then realized that the "Exec" in "Exec Code Overflow" stood for "Execute" and no "Execution". That is how I solved the first question for this section.

**What type of vulnerability is it?**

> Execute Code Overflow

**What is the CVE number for this vulnerability?**

> CVE-2004-1561

**What is the full path \(starting with exploit\) for the exploitation module?**

> exploit/windows/http/icecast\_header

**What is the only required setting which currently is blank?**

> RHOSTS

I then changed the LHOST to be my own IP given to me by TryHackMe. This was my options for the exploit:

![](../../.gitbook/assets/image%20%2853%29.png)

After I ran "**exploit**", I then got a meterpreter shell:

![](../../.gitbook/assets/image%20%2848%29.png)

## Escalate \(**3**0\)

For the following, it is straight forward questions, so I did not go super deep into how to answer the question.

**Woohoo! We've gained a foothold into our victim machine! What's the name of the shell we have now?**

> meterpreter

**What user was running that Icecast process ?**

> Dark

**What build of Windows is the system?**

> 7601

**First, what is the architecture of the process we're running?**

> **x64**

**What is the full path \(starting with exploit/\) for the first returned exploit?**

> exploit/windows/local/bypassuac\_eventvwr

After this, I then changed my payload to be for bypassing uac:

![](../../.gitbook/assets/image%20%2850%29.png)

**We'll have to set one more as our listener IP isn't correct. What is the name of this option?**

> LHOST

I then ran run and the exploit was successful:

![](../../.gitbook/assets/image%20%2852%29.png)

Running "getprivs" on the second session got me:

![](../../.gitbook/assets/image%20%2849%29.png)

This helped me answer the question.

**We can now verify that we have expanded permissions using the command `getprivs`. What permission listed allows us to take ownership of files?**

> SeTakeOwnershipPrivilege

## Looting \(10 min\)

I then ran 'ps' to see the current processes on the room. This was my result:

![](../../.gitbook/assets/image%20%2854%29.png)

I then had to find a printer program for the next question. I was able to find it using the hint.

**The printer spool service happens to meet our needs perfectly for this and it'll restart if we crash it! What's the name of the printer service?**

> spoolsv.exe

As suggested by the room, I then ran the following command:

```c
migrate -N spoolsv.exe
```

This was my result from that:

![](../../.gitbook/assets/image%20%2851%29.png)

**Let's check what user we are now with the command `getuid`. What user is listed?**

> NT AUTHORITY\SYSTEM

I then installed Mimikatz on the machine using the following command:

```c
load kiwi
```

**Which command allows up to retrieve all credentials?**

> creds\_all

I then ran this command, and got the following information:

![](../../.gitbook/assets/image%20%2847%29.png)

Here we can see the answer to the next question.

**Run this command now. What is Dark's password?**

> Password01!

## Post-Exploitation

**What command allows us to dump all of the password hashes stored on the system?**

> hashdump

**While more useful when interacting with a machine being used, what command allows us to watch the remote user's desktop in real time?**

> screenshare

**How about if we wanted to record from a microphone attached to the system?**

> record\_mic

**To complicate forensics efforts we can modify timestamps of files on the system. What command allows us to do this?**

> Timestomp

**Mimikatz allows us to create what's called a `golden ticket`, allowing us to authenticate anywhere with ease. What command allows us to do this?**

> golden\_ticket\_create



