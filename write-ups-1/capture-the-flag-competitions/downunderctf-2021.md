# DownUnderCTF 2021

### The Introduction

![](../../.gitbook/assets/image%20%28219%29.png)

For this one, I ran the netcat command, and then waited for the output to finish, and then I got the flag:

![](../../.gitbook/assets/image%20%28217%29.png)

### Retro!

I downloaded the file, and ran `strings` on the file, and found the flag:

![](../../.gitbook/assets/image%20%28220%29.png)

### Who goes there?

![](../../.gitbook/assets/image%20%28222%29.png)

For this one, I was looking for the domain registration for the website to get as much information as I can. I found the following on a WHOIS Lookup Domain website:

![](../../.gitbook/assets/image%20%28225%29.png)

The flag was the Phone number in DUCTF{Phone\_Number} format.

### Get Over it!

![](../../.gitbook/assets/image%20%28216%29.png)

We get an image for this one:

![](../../.gitbook/assets/image%20%28224%29.png)

I ran it in **tineye** and got no result. I then ran it on **Yandex**, and I saw a picture that looks like the one I had gotten from the CTF:

![](../../.gitbook/assets/image%20%28227%29.png)

The bridge seems to match this image. I then used [https://inflact.com/profiles/instagram-viewer/](https://inflact.com/profiles/instagram-viewer/) to view the image from the poster. I needed to view the tags:

![](../../.gitbook/assets/image%20%28226%29.png)

The tag that helped the most was the **saintluciabrisbane**. I Google-ed "saint lucia brisbane bridge" and got the following:

![](../../.gitbook/assets/image%20%28221%29.png)

I went on the Wikipedia page for the first page and got the following:

![](../../.gitbook/assets/image%20%28223%29.png)

The bridge does look the same as the image. I then tried the flag multiple times, but was not able to get it with the size of 390. I then googled "Eleanor Schonell Bridge size", and got the following:

![](../../.gitbook/assets/image%20%28218%29.png)

The flag was `DUCTF{Eleanor_Schonell_Bridge-185m}`

