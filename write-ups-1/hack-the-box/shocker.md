# Shocker

This is my write-up for the machine called **Shocker** on the Hack The Box website located at: [https://app.hackthebox.com/machines/Shocker](https://app.hackthebox.com/machines/Shocker).

{% hint style="info" %}
I am a beginner at penetration testing, so I will be referencing the Official Hack The Box Walk-through for this machine.
{% endhint %}

nmap scan (basic):

![](<../../.gitbook/assets/image (330) (1) (1) (1) (1).png>)

From the basic scan, we can see that http and another service are running. Running a deeper nmap scan (`sudo nmap -T4 -A -v -sS 10.10.10.56 -oN shocker.nmap`) shows the same ports being open:

![](<../../.gitbook/assets/image (338) (1) (1) (1) (1).png>)

On the main page of the website, we see the following:

![](<../../.gitbook/assets/image (340) (1) (1) (1) (1) (1).png>)

The source code doesn't seem to give away much:

![](<../../.gitbook/assets/image (341) (1) (1) (1) (1) (1) (1) (1).png>)

At this point, after multiple **gobuster** and **feroxbuster** runs, I had to see what I missed in the write-up. What I had missed was running **DirBuster** with the wordlist of **directory-list-lowercase-2.3-medium.txt** and with the extensions of **cgi​, sh, pl​, py**. This got me the following file:

![](<../../.gitbook/assets/image (327) (1) (1).png>)

Downloading that file, and reading its contents provides us with the following:

![](<../../.gitbook/assets/image (332) (1) (1) (1) (1).png>)

Going back to the write-up, we are able to see that this is a **shellshock** exploit, and there is a module on Metasploit. I was able to find it on Metasploit:

![](<../../.gitbook/assets/image (346) (1) (1) (1) (1) (1) (1).png>)

I was able to get a shell after filling in the information:

![](<../../.gitbook/assets/image (329) (1) (1) (1).png>)

Looking in the user directory, I was able to get the user.txt flag:

![](<../../.gitbook/assets/image (347) (1) (1) (1) (1) (1) (1).png>)

With the shell in meterpreter, I was unable to run the commands that I wanted to run. I then found [https://www.exploit-db.com/exploits/34900](https://www.exploit-db.com/exploits/34900) with the recommendation from the write-up. I was then able to get a reverse shell on the system:

![](<../../.gitbook/assets/image (331) (1) (1) (1).png>)

Now when I run `sudo -l`, I can see what commands my user is able to run as root:

![](<../../.gitbook/assets/image (342) (1) (1) (1).png>)

Going to **GTFOBins**, we can see the following for perl:

![](<../../.gitbook/assets/image (343) (1) (1) (1).png>)

Running that command, I was able to get root:

![](<../../.gitbook/assets/image (350) (1) (1) (1) (1) (1) (1) (1) (1).png>)

I was then able to get the flag for root as well (the connection was a bit laggy):

![](<../../.gitbook/assets/image (336) (1) (1) (1) (1) (1) (1).png>)
