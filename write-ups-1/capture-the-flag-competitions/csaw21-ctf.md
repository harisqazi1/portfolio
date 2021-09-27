# CSAW'21 CTF

### Welcome

![](../../.gitbook/assets/image%20%28217%29.png)

For this one, I joined the Discord and found the flag:

![](../../.gitbook/assets/image%20%28253%29.png)

### poem-collection

![](../../.gitbook/assets/image%20%28270%29.png)

For this one, I went on the website and saw the following:

![](../../.gitbook/assets/image%20%28269%29.png)

This led to another webpage:

![](../../.gitbook/assets/image%20%28227%29.png)

Clicking on the links led to urls such as [http://web.chal.csaw.io:5003/poems/?poem=poem1.txt](http://web.chal.csaw.io:5003/poems/?poem=poem1.txt) and [http://web.chal.csaw.io:5003/poems/?poem=poem2.txt](http://web.chal.csaw.io:5003/poems/?poem=poem2.txt). I then though about just replacing the poem text file with flag.txt. That did not work. However when I added "../" before the filename, I then got to a webpage. This led me to the flag:

![](../../.gitbook/assets/image%20%28233%29.png)

### Weak Password

![](../../.gitbook/assets/image%20%28254%29.png)

For this problem, they gave us the name and the format for the wordlist. I then created a wordlist using those ideas: `crunch 13 13 -t Aaron%%%%%%%% >> Aaron.txt`. I then used **hash-identifier** to find out what kind of hash this is:

![](../../.gitbook/assets/image%20%28255%29.png)

I then ran the hashcat command \(`hashcat -m 0 hash Aaron.txt`\) and got the flag:

![](../../.gitbook/assets/image%20%28220%29.png)

### Lazy Leaks

![](../../.gitbook/assets/image%20%28245%29.png)

For this problem, we get a pcapng file. I ran strings on the file, and found the flag in the file:

![](../../.gitbook/assets/image%20%28219%29.png)

