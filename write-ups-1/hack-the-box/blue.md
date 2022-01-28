# Blue

This is my write-up for the machine **Blue** on Hack The Box located at [https://app.hackthebox.com/machines/Blue](https://app.hackthebox.com/machines/Blue).

I started off with a basic nmap scan:

![](<../../.gitbook/assets/image (352).png>)

I noticed that the ports 135 and 445 were open. To me this meant that SMB could potentially be running on the system. I ran `smbmap -H 10.10.10.40` as well as `enum4linux -a 10.10.10.40`, and ended up getting no solid leads from there. Something hit me while I was working on this machine. I had a recollection of hearing the term "eternalblue" being mentioned in a security context. This combined with the idea that Hack The Box machines are names after some aspect of the machine itself, gave me the idea that maybe it could be "eternalblue". I searched **Metasploit** for "blue" and found this:

![](<../../.gitbook/assets/image (364) (1).png>)

I used this module, and after a couple minutes I had a meterpreter session:

![](<../../.gitbook/assets/image (365) (1).png>)

I was able to then find the **user.txt** shell:

![](<../../.gitbook/assets/image (359).png>)

I was then able to get the **root.txt** file as well:

![](<../../.gitbook/assets/image (366) (1).png>)

