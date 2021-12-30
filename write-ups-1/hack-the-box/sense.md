# Sense

This is my write-up for the machine **Sense** on Hack The Box located at: [https://app.hackthebox.com/machines/Sense](https://app.hackthebox.com/machines/Sense).

nmap scan: `nmap -T4 -A -v -Pn 10.10.10.60 -oN sense.nmap`

```
PORT    STATE SERVICE  VERSION
80/tcp  open  http     lighttpd 1.4.35
|_http-server-header: lighttpd/1.4.35
|_http-title: Did not follow redirect to https://10.10.10.60/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
443/tcp open  ssl/http lighttpd 1.4.35
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: 082559A7867CF27ACAB7E9867A8B320F
|_ssl-date: TLS randomness does not represent time
|_http-title: Login
| ssl-cert: Subject: commonName=Common Name (eg, YOUR name)/organizationName=CompanyName/stateOrProvinceName=Somewhere/countryName=US
| Issuer: commonName=Common Name (eg, YOUR name)/organizationName=CompanyName/stateOrProvinceName=Somewhere/countryName=US
| Public Key type: rsa
| Public Key bits: 1024
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2017-10-14T19:21:35
| Not valid after:  2023-04-06T19:21:35
| MD5:   65f8 b00f 57d2 3468 2c52 0f44 8110 c622
|_SHA-1: 4f7c 9a75 cb7f 70d3 8087 08cb 8c27 20dc 05f1 bb02
|_http-server-header: lighttpd/1.4.35
```

There are two ports open, on port 80, we see the following:

![](<../../.gitbook/assets/image (342) (1).png>)

It seems that pfSense is running on the system. I tried the default credentials of **admin**:**pfsense**, but that did not work out. I ran a **dirsearch** command (`dirsearch -u https://10.10.10.60/`) to see what I was able to get access to and found this:

![](<../../.gitbook/assets/image (341) (1).png>)

There still seems to be one vulnerability in place. I ran different searches in the directory to try to find something else, but came up short. When I viewed the official write-up to see what I missed, apparently running the lowercase medium word-list in **dirbusters** word-list directory would have shown me an \<IP>/system-users.txt file:

![](<../../.gitbook/assets/image (362) (1).png>)

The company defaults for pfSense was **pfsense**. The credentials **rohit**:**pfsense** got me into the system:

![](<../../.gitbook/assets/image (363) (1).png>)

Searching for "pfsense" on **metasploit** showed me a couple of results for exploits. I chose one and then fixed the options to work for this box:

![](<../../.gitbook/assets/image (335).png>)

I then had a reverse shell:

![](<../../.gitbook/assets/image (348) (1).png>)

I was then able to find the user.txt flag in **rohit**'s home directory:

![](<../../.gitbook/assets/image (360) (1).png>)

I was then able to get the root flag as well:

![](<../../.gitbook/assets/image (340) (1).png>)
