# Sunday

This is my write-up for the machine on Hack The Box called **Sunday** located at: [https://app.hackthebox.com/machines/Sunday](https://app.hackthebox.com/machines/Sunday).

I started off with a basic nmap scan:

![](<../../.gitbook/assets/image (364) (1) (1) (1).png>)

This seems to be a printer exploit maybe? I saw the **finger** process being shown. I then searched on **Metasploit** to see any modules already there to see if any can help me. I was able to find one:

![](<../../.gitbook/assets/image (352) (1).png>)

At this point, I was lost, so I read [this write-up](https://0xdf.gitlab.io/2018/09/29/htb-sunday.html) to find out what I had missed. The first thing I had missed was that I had used the default word-list from Metasploit, which I should not have. When I changed it to the word-list from [SecLists](https://github.com/danielmiessler/SecLists/blob/master/Usernames/Names/names.txt), I was then able to find out a couple more users:

![](<../../.gitbook/assets/image (336) (1).png>)

Going back to the nmap scan I had done earlier, I had realized that my nmap scan was incorrect, since there were some ports that had been missed. According to the same write-up mentioned previously, there were two ports that I over looked:

![](<../../.gitbook/assets/image (373).png>)

Since SSH is available on port 22022, I will use **hydra** to brute force the login. I tried hydra with multiple variations, but I keep getting the same error:

![](<../../.gitbook/assets/image (351) (1) (1).png>)

I looked at the official write-up as well as others, and people just happen to guess **sunday** to be the password (since it is the name of the machine) and it works for them. The official write-up does not give a hint into what software to use to brute-force the password, it just says to do so. This could just be my lack of pen-testing knowledge, so I'll just take the password to be **sunday** from the write-ups and move on:

![](<../../.gitbook/assets/image (354) (1) (1).png>)

I was then able to find the **user.txt** flag:

![](<../../.gitbook/assets/image (337) (1).png>)

I found out I have access to one command as the root user:

![](<../../.gitbook/assets/image (339) (1) (1).png>)

Looking on GTFOBins, I was not able to find a binary with this name:

![](<../../.gitbook/assets/image (368).png>)

To me, this means that this program is custom made. It seems that **/root/troll** was an actual troll, as I was not able to run it:

![](<../../.gitbook/assets/image (353) (1).png>)

I then went to the root of the machine and tried to find what I can. I did find the following in the **backups** folder:

![](<../../.gitbook/assets/image (341) (1).png>)

Running a **diff** on both of the files, it came up empty. This means that the files are the same. I first trimmed the hashes file to only have the essentials needed to crack the hashes:

![](<../../.gitbook/assets/image (331) (1).png>)

Using [hashcat's example hashes page](https://hashcat.net/wiki/doku.php?id=example\_hashes), we can find out what hash we are looking at:

![](<../../.gitbook/assets/image (350) (1).png>)

I can then build by command:

`hashcat -a 0 -m 7400 hashes rockyou.txt`

After a bit of time, I was able to find the password for the other user:

![](<../../.gitbook/assets/image (372).png>)

Running the **su** command, I was then able to change my user to be the **sammy** user:

![](<../../.gitbook/assets/image (367).png>)

Running `sudo -l` as this user, reveals the following:

![](<../../.gitbook/assets/image (357) (1).png>)

I am able to run **wget** as root. I had kept on trying the **sudo** command on **GTFOBins** for the **wget** command. When I tried the **File read** commands, it worked!

![](<../../.gitbook/assets/image (330) (1).png>)

![](<../../.gitbook/assets/image (371).png>)
