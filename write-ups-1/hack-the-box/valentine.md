# Valentine

This is my write-up for the machine on Hack The Box called **Valentine** located at: [https://app.hackthebox.com/machines/Valentine](https://app.hackthebox.com/machines/Valentine).

I started off with an nmap scan:

![](<../../.gitbook/assets/image (363).png>)

This was a preliminary scan. I then ran a deeper scan (`nmap -T4 -A -v -Pn -p- 10.10.10.79 -oN valentine.nmap_full`) to see if the basic scan missed anything:

![](<../../.gitbook/assets/image (327).png>)

On port 80, we get greeted by the following image:

![](<../../.gitbook/assets/image (367) (1).png>)

I then ran the following **dirsearch** command to see what directories are available for me to access:

`dirsearch -e php,html,js,cgi,bak,txt -u http://10.10.10.79 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -t 50`

I then saw from the results that **/dev** was available for me to access:

![](<../../.gitbook/assets/image (364) (1).png>)

notes.txt:

![](<../../.gitbook/assets/image (370).png>)

hype\_key:

![](<../../.gitbook/assets/image (353) (1).png>)

The **hype\_key** file looked like hexadecimal to me, so I used an online converter to see if I can get the ASCII of it:

![](<../../.gitbook/assets/image (366).png>)

It seemed to be a private RSA key. At this time, my **dirsearch** command had completed from running:

![](<../../.gitbook/assets/image (348).png>)

The **/encode** and **/decode** both look similar and both use base64 in order to decode and encode:

![](<../../.gitbook/assets/image (361).png>)

![](<../../.gitbook/assets/image (329).png>)

**/omg** turned out to be the picture on the home screen we saw earlier:

![](<../../.gitbook/assets/image (351) (1).png>)

In the notes, this stuck out to me:

![](<../../.gitbook/assets/image (362).png>)

It seems the encoding and decoding is done server-side as well. I tried to run the following, in order to get a reverse shell, but it did not work:

![](<../../.gitbook/assets/image (352) (1).png>)

I was not sure how to bypass this, so I had to view the official Hack The Box write-up for this machine. Before I got any answers, I noticed this:

![](<../../.gitbook/assets/image (347).png>)

It seems that I have to exploit Heartbleed. Running a **Metasploit** module on it reveals that it is exploitable:

![](<../../.gitbook/assets/image (368) (1).png>)

This did not get me anywhere, so I looked at the same write-up again and noticed that I didn't connect a couple of dots together. When I tried to use the private key that I had found earlier in the **hype\_key** file, I was asked for a password, which I did not know. In addition, while I was running the  **Metaploit** module, I kept on getting the following:

![](<../../.gitbook/assets/image (371) (1).png>)

Decoding this gives us the following:

![](<../../.gitbook/assets/image (344).png>)

The dots that I was not able to connect was that this was the password for the private key file. The username for the server, **hype**, I should have assumed since the name of the file was **hype\_key**. Using this information, I was able to get a shell:

![](<../../.gitbook/assets/image (346).png>)

I then had gotten the **user.txt** flag:

![](<../../.gitbook/assets/image (339) (1).png>)

I then imported **linpeas.sh** to the server using a **python3** module:

![](<../../.gitbook/assets/image (334).png>)

![](<../../.gitbook/assets/image (369).png>)

Running **linpeas.sh**, I noticed something interesting:

![](<../../.gitbook/assets/image (350) (1).png>)

I had access to the **tmux** program, but I did not know where to go from there. I then read the official write-up and found out that I overlooked the **ps aux** command that shows what processes are being run currently. This would have showed me a root command of **tmux** being ran. If I ran that myself, I then get root:

![](<../../.gitbook/assets/image (372) (1).png>)
