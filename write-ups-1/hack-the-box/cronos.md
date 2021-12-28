# Cronos

This is my write-up for the machine **Cronos** on Hack The Box located at: [https://app.hackthebox.com/machines/Cronos](https://app.hackthebox.com/machines/Cronos).

nmap scan: `nmap 10.10.10.13`

![](<../../.gitbook/assets/image (341).png>)

We see three ports open. On port 80, we see the following:&#x20;

![](<../../.gitbook/assets/image (332).png>)

I am not too familiar with DNS enumeration methods. For this reason, after I searched online for a while (and got nowhere), I then read up what I had missed from the official Hack The Box write-up for this machine. In in the author says to add **cronos.htb** to the **/etc/hosts** file. Then run the following command:

`dig axfr @10.10.10.13 cronos.htb`

This will show us the following:

![](<../../.gitbook/assets/image (327).png>)

We see an **admin.cronos.htb**, so we will add that to the **/etc/hosts** file as well (step was mentioned in the official write-up):

![](<../../.gitbook/assets/image (337).png>)

Now if we go to **admin.cronos.htb**, we see the following:

![](<../../.gitbook/assets/image (346).png>)

Looking at the tags for this machine, I believe that we have to do a SQL Injection for this authentication portal:

![](<../../.gitbook/assets/image (363).png>)

I then opened this web page in Burp Suite to see if I can manipulate the outgoing HTTP requests:

