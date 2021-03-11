# CTF collection Vol.1

#### This is my write-up for the TryHackMe room located at: [https://tryhackme.com/room/ctfcollectionvol1](https://tryhackme.com/room/ctfcollectionvol1)

### Task 2: What does the base said?

The question asks us to decode a code: VEhNe2p1NTdfZDNjMGQzXzdoM19iNDUzfQ==. The command I ran was: 

```text
echo "VEhNe2p1NTdfZDNjMGQzXzdoM19iNDUzfQ==" | base64 -d
```

This decodes the string in the quotes using base64 encoding. After the command, I got the flag:

![](../../.gitbook/assets/screenshot-2021-03-10-170140.png)

### Task 3: Meta Meta

For this problem, it wants us to find a flag using its metadata. For this, I used a metadata reading tool on ParrotOS and Kali Linux called "exiftool". Running that command on the file that the problem presented led me to this:

![](../../.gitbook/assets/image%20%2817%29.png)

### Task 4: Mon, are we going to be okay?

For this one I had to look at the hint in order to figure out a way to crack the file. The hint pointed out that I should use the steganography tool known as "steghide. The command I used for this was:

```text
steghide --extract -sf Extinction.jpg
```

 I did not use a password for this, and it output the password in a new file for me:

```text
cat Final_message.txt 
It going to be over soon. Sleep my child.

THM{------------------------}
```



