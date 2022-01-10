# SolidState

This is my write-up for the Hack The Box machine called **SolidState** located at: [https://app.hackthebox.com/machines/SolidState](https://app.hackthebox.com/machines/SolidState).

nmap scan:

![](<../../.gitbook/assets/image (357).png>)

The basic nmap scan shows 4 ports open. However, in the machine tags, we see the following:

![](<../../.gitbook/assets/image (336) (1).png>)

It seems that our basic nmap scan did not catch any web ports (80 or 443). I then ran a deeper nmap scan (`nmap -T4 -A -v -Pn 10.10.10.51 -oN solidstate.nmap`)which led me to find out port 80 is open as well:

![](<../../.gitbook/assets/image (361).png>)

Going to the the website, we see a message submission box:

![](<../../.gitbook/assets/image (350) (1).png>)

Maybe this might be used for command execution or a reverse shell process? I then ran **dirsearch** (`dirsearch -e php,html,js,cgi,bak,txt -u http://10.10.10.51 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt`) on the IP Address to see if there are items we have access to:

![](<../../.gitbook/assets/image (358).png>)

Looking at the files in those directories led me to a dead end. I then wanted to enumerate the smtp users to see that maybe there is a user whose mailbox I can get access to. I ran the command `smtp-user-enum -M VRFY -U rockyou.txt -t 10.10.10.51` for this. After the program ran for a while, I turned it off, since I had not gotten any result from this. I then found an exploit on Metasploit that had an exploit exactly for this version:

![](<../../.gitbook/assets/image (344) (1).png>)

I tried various settings to get it to work, however I was not able to do so. While browsing this exploit on Metasploit, I realized the default credentials loaded into the exploit were **root**:**root**. I had a hunch that I should try this out, but I did not follow it. Looking at the official Hack The Box write-up for this machine, I realized that I was right. Also, I had found out that my nmap scan had missed port 4555. I was able to login into the port using those credentials:

![](<../../.gitbook/assets/image (354).png>)

When we run **listusers** we see the following:

![](<../../.gitbook/assets/image (362).png>)

After I was stuck for a while, I found out from the official write-up that I was looking at the wrong exploit, and the correct one was: [https://www.exploit-db.com/exploits/35513](https://www.exploit-db.com/exploits/35513). I then also learned that we have to modify this exploit to make it to work. If we got to [this GitHub page (swisskyrepo)](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#bash-tcp) we can see commands we can use for reverse shells. I then added one of the Bash TCP payloads and edited the python file:

![](<../../.gitbook/assets/image (346) (1).png>)

On another terminal, I ran a netcat listener to wait for the reverse shell:

![](<../../.gitbook/assets/image (342) (1).png>)

After you run the python script (on another terminal), you get the following message:

![](<../../.gitbook/assets/image (338).png>)

The payload was submitted, but I was not able to get a shell. I realized the netcat listener was not going to come in handy for this, so I closed it. The official write-up stated that I should change the password for the user **mindy** and then login to her account:

![](<../../.gitbook/assets/image (340).png>)

![](<../../.gitbook/assets/image (343) (1).png>)

Reading the second email shows us the following:

![](<../../.gitbook/assets/image (347) (1).png>)

We can then use these credentials to login to SSH and get the user flag:

![](<../../.gitbook/assets/image (341) (1).png>)

Running commands like **wget**, was showing me an error:

![](<../../.gitbook/assets/image (337).png>)

I then found [this website](https://www.hacknos.com/rbash-escape-rbash-restricted-shell-escape/) that showed me the way to get out of rbash restricted shells:

![](<../../.gitbook/assets/image (364).png>)

I then uploaded **linpeas.sh** to the machine using **python3**:

![](<../../.gitbook/assets/image (365).png>)

![](<../../.gitbook/assets/image (332).png>)

For some reason, unknown to me, the **linpeas.sh** script would not run all the way through. I then tried the **LinEnum.sh** script, and that was able to go through. However, it did not show me files that I was able to read/write to on the system. I then viewed the official write-up and [this write-up](https://0xdf.gitlab.io/2020/04/30/htb-solidstate.html) to then learn that there was a python file in the **/opt/** directory:

![](<../../.gitbook/assets/image (334) (1).png>)

I tried to overwrite the file with my own, but I did not have permissions to do so:

![](<../../.gitbook/assets/image (363) (1).png>)

I found out I can **echo** strings into the file:

![](<../../.gitbook/assets/image (359).png>)

I had a netcat listener setup on another terminal. Then, one line at a time, I **echo**-ed commands into the file until I had this:

![](<../../.gitbook/assets/image (360).png>)

After a minute, I had the root shell:

![](<../../.gitbook/assets/image (348) (1).png>)
