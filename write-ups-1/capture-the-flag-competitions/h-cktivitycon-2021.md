# H@cktivityCon 2021

### Tsunami

![](../../.gitbook/assets/image%20%28235%29.png)

For this one we get an audio file, I opened the file in **audacity** and viewed the audio in the **Spectogram** view:

![](../../.gitbook/assets/image%20%28234%29.png)

### Bass64

![](../../.gitbook/assets/image%20%28251%29.png)

The file leads to this:

![](../../.gitbook/assets/image%20%28228%29.png)

Only when you zoom out, you then see actual string:

![](../../.gitbook/assets/image%20%28222%29.png)

I then entered this on the website \(base64decode.org\) and ended up finding out the following:

![](../../.gitbook/assets/image%20%28242%29.png)

### Pimple

![](../../.gitbook/assets/image%20%28221%29.png)

For this challenge, we get a file format meant for GIMP. Running **file Pimple** gives us the following: `pimple: GIMP XCF image data, version 011, 1024 x 1024, RGB Color`. I then opened the file in gimp, and after just browsing around, I found this:

![](../../.gitbook/assets/image%20%28255%29.png)

### Six Four Over Two

![](../../.gitbook/assets/image%20%28252%29.png)

For this problem, we are given a file with the following information:

![](../../.gitbook/assets/image%20%28229%29.png)

My mentality was that 64/2 or "six four **over** two" is 32. I ran the command `echo "EBTGYYLHPNQTINLEGRSTOMDCMZRTIMBXGY2DKMJYGVSGIOJRGE2GMOLDGBSWM7IK" | base32 -d` and this led me to the flag: `flag{a45d4e70bfc407645185dd9114f9c0ef}`

