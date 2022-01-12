# Beep

This is my write-up for the machine **Beep** on Hack The Box located at: [https://app.hackthebox.com/machines/Beep](https://app.hackthebox.com/machines/Beep).

nmap scan: `nmap 10.10.10.7`

![](<../../.gitbook/assets/image (366) (1) (1).png>)

There are a lot of ports open, so I will start with port 80 (HTTP) first. We see a login page there:

![](<../../.gitbook/assets/image (339) (1) (1).png>)

I went to port 10000 out of curiosity and found a Webmin login page there:

![](<../../.gitbook/assets/image (350) (1) (1) (1).png>)

I found this exploit on **exploitdb**:

![](<../../.gitbook/assets/image (348) (1) (1) (1) (1).png>)

I had to go read the official Hack The Box write-up for this machine to find out what I overlooked. Turns out the page I was looking at had the exploit all along:

![](<../../.gitbook/assets/image (337) (1).png>)

Changing the URL to be **/etc/passwd** shows you the following:

![](<../../.gitbook/assets/image (336) (1) (1).png>)

Now we know there is a user called **fanis** on the system. Looking in their directory, we are able to find the flag:

![](<../../.gitbook/assets/image (362) (1) (1) (1).png>)

Going back to the LFI page provided in the LFI exploit, we can see a password:

![](<../../.gitbook/assets/image (331).png>)

From this [write-up](https://dalemazza.github.io/htb/2020/07/04/HTB-Beep-OSCP-Walkthrough.html), I understood if we use this password for the root user, we will be able to log in as root:

![](<../../.gitbook/assets/image (340) (1) (1).png>)

