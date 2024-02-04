# Legacy

This is my write-up for the Hack The Box machine called **Legacy** located at: [https://app.hackthebox.com/machines/Legacy](https://app.hackthebox.com/machines/Legacy).

I ran a basic nmap scan to start off:

![](<../../.gitbook/assets/image (503).png>)

The basic nmap scan was backed up by a more deeper scan:

![](<../../.gitbook/assets/image (504).png>)

From my understanding, it seems that **SMB** is open. Running `enum4linux -a 10.10.10.4` gets me the following information:

![](<../../.gitbook/assets/image (576).png>)

Running a **Metasploit** module on the IP Address we can see the SMB version:

![](<../../.gitbook/assets/image (697).png>)

Doing a Google search for "smb Windows XP SP3" got me to [this site](https://www.rapid7.com/db/modules/exploit/windows/smb/ms08\_067\_netapi/). I then ran this exploit and filled in the information for this machine. I was then able to get a meterpreter shell:

![](<../../.gitbook/assets/image (582).png>)

After searching for a while, I was able to find the **user.txt** flag:

![](<../../.gitbook/assets/image (661).png>)

{% hint style="info" %}
To change directory into "Documents and Settings", you have to run: **`cd Documents\ and\ Settings`**. Note the "\ ".
{% endhint %}

I was then able to read the root flag as well:

![](<../../.gitbook/assets/image (404).png>)
