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

### Task 5: Erm......Magick

This problem mentions the word "Magick" in the title. This makes me think that I might need to run image. After 5 minutes of looking online to find the answer, I had to take a look at the hint. The hint told me that the flag could be hidden on the page somewhere. I then found he flag on the page:

![](../../.gitbook/assets/image%20%2819%29.png)

### Task 6: QRrrrr

For this problem, all a person has to do is to is scan the QR code on the picture you download. When you scan the QR code it shows you the flag on your phone \(or the device you use for the scan\).

### Task 7: Reverse it or read it?

For this problem, we are downloading a ELF file. This is a program that is an executable. My first thought was using ltrace \(a linux command to see a bit more of the backend of the executable\). When that did not work, I then ran "strings" on the file such as:

```c
strings hello.hello | grep THM
```

This gave the output:

![](../../.gitbook/assets/image%20%2821%29.png)

### Task 8: Another decoding stuff

We are asked to decode a string: 3agrSy1CewF9v8ukcSkPSYm3oKUoByUpKG4L. I looked at the hint for this one and noticed that it is in base58. I ran the following command:

```c
echo "3agrSy1CewF9v8ukcSkPSYm3oKUoByUpKG4L" | base58 -d
```

This got me the flag:

![](../../.gitbook/assets/image%20%2820%29.png)

### Task 9: Left or right

Reading about this, I realize that this might be some sort of shift. The text did mention ROT 13, so I played around with ROT and then got a ROT number to work for me:

![](../../.gitbook/assets/image%20%2822%29.png)

The website I used for this was [CyberChef](https://gchq.github.io/CyberChef/). 

### Task 10: Make a comment

I needed a hint to know what a foothold can be into solving this puzzle. It did not help me as much as I hoped. I found [this write-up](https://shafdo.github.io/pages/blog/ctf/ctf_collection_Vol_1/), which pointed me in the right direction. I then found the flag:

![](../../.gitbook/assets/image%20%2818%29.png)

### Task 11: Can you fix it?

For this problem, it states "I accidentally messed up with this PNG file. Can you help me fix it?". This tells me that I have to edit the file header. For this, I will use hexeditor. Looking online, I found the "magic numbers" to be "89 50 4E 47". I then used hexeditor to modify the file. 

![](../../.gitbook/assets/image%20%2825%29.png)

I then got the flag in the image.

![](../../.gitbook/assets/image%20%2823%29.png)



