# Nibbles

This is my write-up for the machine **Nibbles** on Hack The Box located at: [https://app.hackthebox.com/machines/Nibbles](https://app.hackthebox.com/machines/Nibbles).

nmap: `nmap 10.10.10.75`

![](<../../.gitbook/assets/image (537).png>)

We see that 2 ports are open **SSH** and **HTTP**. Going to port 80, we see the following:

![](<../../.gitbook/assets/image (435).png>)

In the source code, I saw something interesting:

![](<../../.gitbook/assets/image (567).png>)

At this time, I had run a deeper nmap scan to see if anything was different on that scan. Fortunately, nothing was different in the deeper scan (`nmap -T4 -A -v 10.10.10.75 -oN nibbles.nmap`):

![](<../../.gitbook/assets/image (670) (1).png>)

Going to the directory from above, we see the following:

![](<../../.gitbook/assets/image (337).png>)

I then ran **feroxbuster** on the system (`feroxbuster -u http://10.10.10.75/nibbleblog/ -w directory-list-lowercase-2.3-medium.txt`) to see what other directories were there. I found a lot:

![](<../../.gitbook/assets/image (539) (1).png>)

Looking through the website I found the following:

![](<../../.gitbook/assets/image (460).png>)

I then looked up the name of the author, and this seems to be a CMS application:

![](<../../.gitbook/assets/image (551).png>)

I then ran `git clone https://github.com/dignajar/nibbleblog.git` to download the file, but I also looked on metasploit to see if there was an exploit. Turns out there was:

![](<../../.gitbook/assets/image (509).png>)

I then found this [video write-up](https://www.youtube.com/watch?v=iXyKLm1nQac) that made me realized my search had overlooked a crucial website: the admin login page. I looked at what software the person in the video was using, and they were using **diresearch**. I did the same and was able to find the page:

![](<../../.gitbook/assets/image (592).png>)

![](<../../.gitbook/assets/image (451).png>)

I now have to brute-force the username and password. I followed [this website](https://infinitelogins.com/2020/02/22/how-to-brute-force-websites-using-hydra/) to see what parameters I needed for my **Hydra** command. I then ran the `hydra -l admin -P rockyou.txt 10.10.10.75 http-post-form "/nibbleblog/admin.php:username=admin&password=^PASS^:Incorrect username or password."` command, and got the following:

![](<../../.gitbook/assets/image (391).png>)

I tried all of those passwords and came back empty. I looked at the official Hack The Box write-up and saw this:

![](<../../.gitbook/assets/image (568) (1) (1).png>)

I then had to find a way to find the password on my own by guessing. Even viewing Ippsec's attempt, we can see he randomly guesses the password to be **nibbles** and gets in. I then used the **admin**/**nibbles** credentials to log in:

![](<../../.gitbook/assets/image (394).png>)

Back to the **Metasploit** exploit previously mentioned, I entered the password to be **nibbles** and was able to get a meterpreter shell on the system:

![](<../../.gitbook/assets/image (511).png>)

I uploaded the **php-reverse-shell** from pentestmonkey online by running `upload php-reverse-shell.php`. I then ran netcat on another terminal and then accessed the page on a new tab:

![](<../../.gitbook/assets/image (563).png>)

![](<../../.gitbook/assets/image (636).png>)

I was logged in as **nibbler**. I was able to get a TTY shell by typing in **bash**:

![](<../../.gitbook/assets/image (371).png>)

There were two files:

![](<../../.gitbook/assets/image (604).png>)

I got the user flag:

![](<../../.gitbook/assets/image (415).png>)

In the nibbler account, I setup a Python http server in the home directory of nibbler. I then downloaded the **personal.zip** file:

![](<../../.gitbook/assets/image (401).png>)

![](<../../.gitbook/assets/image (564).png>)

When I pressed **CNTRL-C** to stop the http server, I had gotten out of the shell. I then had to get back in using the same php-reverse-shell as before:

![](<../../.gitbook/assets/image (486) (1).png>)

In the **personal.zip** folder, there was a bash script. My assumption was that I was going to run the bash script on the machine. I found out I was unable to unzip the file on the machine:

![](<../../.gitbook/assets/image (323).png>)

In order to upload the file from my machine, I used the python3 http module to upload it:

![](<../../.gitbook/assets/image (396).png>)

![](<../../.gitbook/assets/image (688) (1).png>)

I then ran the bash script:

![](<../../.gitbook/assets/image (518).png>)

I ran `sudo -l` to see what I have access to as the sudo user:

![](<../../.gitbook/assets/image (538) (1).png>)

My guess is that I will have to edit the bash script to get to be root. After I had the TTY python shell, I was then able to unzip:

![](<../../.gitbook/assets/image (555).png>)

I thought about the privilege escalation to root a bit too much enough to overthink it. The solution was super simple, which the official write-up had made me realize:

![](<../../.gitbook/assets/image (600).png>)
