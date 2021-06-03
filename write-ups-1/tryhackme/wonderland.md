# Wonderland \(Incomplete\)

I will first run an nmap scan on the IP address, in order to find what is on the web server. Here is the output from the scan:

```c
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 8e:ee:fb:96:ce:ad:70:dd:05:a9:3b:0d:b0:71:b8:63 (RSA)
|   256 7a:92:79:44:16:4f:20:43:50:a9:a8:47:e2:c2:be:84 (ECDSA)
|_  256 00:0b:80:44:e6:3d:4b:69:47:92:2c:55:14:7e:2a:c9 (ED25519)
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Follow the white rabbit.
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

I can see that port 80 \(most common for HTTP\) is being used. I then ran gobuster on the website. This can be done by the following command:

```c
gobuster dir -u http://10.10.213.33 -t 10 -w SecLists/Discovery/Web-Content/directory-list-2.3-small.txt
```

> -u = IP or URL to the website
>
> -t = threads to run at once
>
> -w = location of the wordlist

The word list that I am using is from [Github](https://github.com/danielmiessler/SecLists). You can clone it to your own system, and then use it. It has a lot of lists, so you can choose which one you think is a good idea. I got a couple results from the gobuster command:

```c
gobuster dir -u http://10.10.213.33 -q -t 10 -w SecLists/Discovery/Web-Content/directory-list-2.3-small.txt 
/img (Status: 301)
/r (Status: 301)
```

I checked out /img, and it did not have anything that interesting. I then checked out /r.

![](../../.gitbook/assets/image%20%2815%29.png)

In this file, it tells us to keep going. So I will run gobuster on it again, to see where I can go from here. this time, I will add "/r" at the end of my IP address. I did get another result:

![gobuster output](../../.gitbook/assets/image%20%2816%29.png)

I can see a directory called /a. In the directory, I did not see anything special other than the main page of it:

![](../../.gitbook/assets/image%20%2814%29.png)

My guess is that, it could be the word rabbit, bit a "/" between each letter. That worked! I ended up at a page called IP\_address/r/a/b/b/i/t. Looking at the source code for the website, I did see the a hint:

![](../../.gitbook/assets/screenshot-2021-03-09-162320.png)

I added this hint to my notes. This could be a way in through ssh. Fortunately, it worked and I got into the machine using the hint from above.

![](../../.gitbook/assets/image%20%2812%29.png)

I now have to find the user.txt file, as the room suggests I look for. I did find other users in the machine:

![](../../.gitbook/assets/image%20%2813%29.png)

At this point, I am trying to find anything that I can to help me find the user.txt. I found out by running "sudo -l" that alice only has one sudo command:

```c
User alice may run the following commands on wonderland:
    (rabbit) /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py
```

I realized that I might have to find a way to read another file using this. I did get lost at this point. When the hint did not make sense to me, I found [this website](https://0x00jeff.github.io/tryhackme-wonderland-writeup/) that guided me into the right direction. I realized that this meant that the user.txt flag was in the root directory.

![](../../.gitbook/assets/screenshot-2021-03-09-165204.png)

I had found the user.txt file. Now I had to find a way to upgrade my user privilege. Using the previous website as a reference, I then would have to use the rabbit user on the machine, as well as make a file called "random" that the python script would call. I then typed the following commands, which I found on [this website](https://0x00jeff.github.io/tryhackme-wonderland-writeup/) \(same as before\):

```c
echo 'import pty;pty.spawn("/bin/bash")' > random.py
sudo -u rabbit /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py
```

After this, I was the rabbit user on the machine. I now had to find a way to get higher. In the rabbit user's home directory, there is an executable called "teaParty". At this point, I have read the write up for it, and realized that there was way more work to be done. I will try to continue this later.

{% hint style="warning" %}
**Under maintenance**
{% endhint %}

