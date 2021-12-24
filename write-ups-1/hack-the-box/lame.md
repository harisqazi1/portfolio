# Lame

This is my write-up for the machine Lame on Hack The Box located at: [https://app.hackthebox.com/machines/Lame](https://app.hackthebox.com/machines/Lame).

{% hint style="info" %}
I am a beginner at penetration testing, so I will be referencing the Official Hack The Box Walk-through for this machine.
{% endhint %}

rustscan:  `rustscan -a 10.10.10.3`

![](<../../.gitbook/assets/image (327) (1).png>)

I saw that port 21 was open. I was able to login to the anonymous user:

![](<../../.gitbook/assets/image (343) (1).png>)

There was nothing in the folder. My guess is that I will have to upload a reverse shell on the system and then trigger it in order to get a connection to the system. I then ran `enum4linux -a 10.10.10.3`, which showed me this:

![](<../../.gitbook/assets/image (336) (1).png>)

Running `smbclient -L <IP_ADDRESS>` shows the same information:

![](<../../.gitbook/assets/image (341) (1) (1).png>)

Looking in the **tmp** directory did not give anything useful:

![](<../../.gitbook/assets/image (349) (1).png>)

From playing around in the **SMB** system, I noticed that I have access to the **tmp** and **IPC$** user. Neither of those had gotten me information that I can work off of. I looked at the write-up to see what I missed. There was a exploit on Metasploit that got you root:

![](<../../.gitbook/assets/image (351) (1).png>)

Now I have to look for the flags:

![](<../../.gitbook/assets/image (339).png>)
