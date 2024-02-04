# Wonderland

This is my write-up for the TryHackMe machine called Wonderland located at: [https://tryhackme.com/room/wonderland](https://tryhackme.com/room/wonderland).

I started off by running an nmap scan:

`sudo nmap -sV -sC -O -n -oA nmapscan 10.10.91.174`

![](<../../.gitbook/assets/image (408).png>)

There seems to be 2 ports open. Following port 80 leads me to the following:

![](<../../.gitbook/assets/image (428) (2).png>)

There does not seem to be a _robots.txt_ file on the system:

![](<../../.gitbook/assets/image (541).png>)

I then ran **feroxbuster** on the IP address:

`feroxbuster -u http://10.10.91.174 -x pdf,js,html,php,txt,json,docx -w /usr/share/wordlists/dirb/common.txt -t 30`

I noticed that there was a pattern here:

![](<../../.gitbook/assets/image (352) (1).png>)

I looked at the man page for feroxbuster and found that a recursion depth of 0 is infinite. Although it seems like the word from each directory would form rabbit, I still wanted to make sure. This was my updated command:

`feroxbuster -u http://10.10.91.174 -x pdf,js,html,php,txt,json,docx -w /usr/share/wordlists/dirb/common.txt -t 30 -d 0`

![](<../../.gitbook/assets/image (602).png>)

This led to a page:

![](<../../.gitbook/assets/image (375).png>)

I viewed the source code of this page and found the following:

![](<../../.gitbook/assets/image (328).png>)

This seems to be maybe an SSH account. It was:

![](<../../.gitbook/assets/image (341) (2).png>)

There were two files, and both were owned by root:

![](<../../.gitbook/assets/image (538).png>)

Running `sudo -l`, I saw the following:

![](<../../.gitbook/assets/image (559).png>)

It seems that I can run sudo, but as rabbit:

![](<../../.gitbook/assets/image (550).png>)

I have to find a way to switch over to rabbit, using this access. I was thinking I can maybe over-write the file and change the content of the file itself. I then found this write-up [https://github.com/Slowdeb/Tryhackme/blob/main/Wonderland.md](https://github.com/Slowdeb/Tryhackme/blob/main/Wonderland.md) that mentioned that I had missed the user.txt in the root directory. Sure enough, I had:

![](<../../.gitbook/assets/image (713).png>)

However, I was still stuck as to how I would escalate my privileges. I had tried modifying the python script and modifying the python3.6 file on the system, and was not able to do either. The same previous walkthrough led me to this post: [https://medium.com/analytics-vidhya/python-library-hijacking-on-linux-with-examples-a31e6a9860c8](https://medium.com/analytics-vidhya/python-library-hijacking-on-linux-with-examples-a31e6a9860c8). This was about Python library hijacking. In the python code, there is only one import:

![](<../../.gitbook/assets/image (385) (2).png>)

![](<../../.gitbook/assets/image (679).png>)

The same write-up ended up making a local file called random.py and entered the following into it:

```
#!/usr/bin/python3.6

import pty
pty.spawn("/bin/bash")
```

I then realized that the original python code might be referencing locally AND THEN referencing the alternate location (/usr/lib/python3.6/random.py).

![](<../../.gitbook/assets/image (540).png>)

We are now the user rabbit. I look in the home directory of rabbit and find an executable called **teaParty**. I ran the code, and it seems to have the sleep command incorporated in it:

![](<../../.gitbook/assets/image (609).png>)

I wanted to know what the code did, so I copied the code from rabbit's directory into the /tmp directory. I then used FileZilla with alice's login and downloaded the file to my local system. I then used strings to see what was in the file:

![](<../../.gitbook/assets/image (470).png>)

I had to go back to the previous write-up again. I learned that the date command is not using the absolute path. As the author did, I entered the following into a file called date:

```
#!/bin/bash

/bin/bash
```

I made and edited the file in alice's home directory. I then copied it over to the /tmp folder, where I then used my access to rabbits account to grab it from there. I also had to update the PATH to then make the SUID binary read from the date file we had made. Here are the commands I had run after I moved the date file to rabbits home folder:

`chmod +x date #to make date executable`

`export PATH=/home/rabbit/:$PATH #to make the system recognize the path we have access to`

`./teaParty #run the binary`

I then had access to hatter:

![](<../../.gitbook/assets/image (370).png>)

There is a password file in hatter's home directory:

![](<../../.gitbook/assets/image (580).png>)

The password was for the hatter user. I then ran `find / -type f -perm -04000 -ls 2>/dev/null` to see what executables I had access to. I saw the following, where one had stuck out to me:

![](<../../.gitbook/assets/image (660) (2).png>)

There was a major vulnerability in pkexec that allows you to get root. I used the code from [https://github.com/ly4k/PwnKit/blob/main/PwnKit.sh](https://github.com/ly4k/PwnKit/blob/main/PwnKit.sh) to download the exploit on my own machine. I then used FileZilla to transfer that exploit to the hatter home directory. From there, I was able to make it executable and get root:

![](<../../.gitbook/assets/image (339).png>)

I was then able to get the root.txt file as well:

![](<../../.gitbook/assets/image (666).png>)

**Lessons Learned:** I learned a couple of main things while working on the box. The first error I made was not understanding that if root.txt was in a users home directory, that user.txt might have been in the root directory. The first thing I had learned was Python imports and how they check locally for the import before looking in the library folder. Another item I learned was about SUID binary exploiting by manipulating the PATH variable.
