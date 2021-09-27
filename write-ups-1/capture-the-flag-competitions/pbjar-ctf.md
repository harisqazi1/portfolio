# PBJar CTF

### Convert

![](../../.gitbook/assets/image%20%28236%29.png)

For this question, we get a text file with the following information: `666c61677b6469735f69735f615f666c346767675f68317d`. Converting that online gets you the flag:

![](../../.gitbook/assets/image%20%28262%29.png)

### Stegosaurus stenops

![](../../.gitbook/assets/image%20%28221%29.png)

For this one, we get a image:

![](../../.gitbook/assets/image%20%28238%29.png)

I ran **strings** on the file, and was not able to find a flag. But then when I ran `stegseek --crack -sf stegosaurus.jpg -wl rockyou.txt` I got the flag: `flag{ungulatus_better_than_stenops}`

