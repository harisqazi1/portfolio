# H@cktivityCon 2021

### Tsunami

![](<../../.gitbook/assets/image (251).png>)

For this one we get an audio file, I opened the file in **audacity** and viewed the audio in the **Spectogram** view:

![](<../../.gitbook/assets/image (252).png>)

### Bass64

![](<../../.gitbook/assets/image (253).png>)

The file leads to this:

![](<../../.gitbook/assets/image (254).png>)

Only when you zoom out, you then see actual string:

![](<../../.gitbook/assets/image (255).png>)

I then entered this on the website (base64decode.org) and ended up finding out the following:

![](<../../.gitbook/assets/image (256).png>)

### Pimple

![](<../../.gitbook/assets/image (257).png>)

For this challenge, we get a file format meant for GIMP. Running **file Pimple** gives us the following: `pimple: GIMP XCF image data, version 011, 1024 x 1024, RGB Color`. I then opened the file in gimp, and after just browsing around, I found this:

![](<../../.gitbook/assets/image (258).png>)

### Six Four Over Two

![](<../../.gitbook/assets/image (259).png>)

For this problem, we are given a file with the following information:

![](<../../.gitbook/assets/image (260).png>)

My mentality was that 64/2 or "six four **over** two" is 32. I ran the command `echo "EBTGYYLHPNQTINLEGRSTOMDCMZRTIMBXGY2DKMJYGVSGIOJRGE2GMOLDGBSWM7IK" | base32 -d` and this led me to the flag: `flag{a45d4e70bfc407645185dd9114f9c0ef}`

### Confidentiality

![](<../../.gitbook/assets/image (261).png>)

For this challenge, we are sent to a webpage:

![](<../../.gitbook/assets/image (262).png>)

It seems that you can view any file and see it it exists:

![](<../../.gitbook/assets/image (263).png>)

My assumption was that in the backend `ls` was running. So I had to find a way to use one command to get the flag. I first tried just a `;` and this is what it showed me:

![](<../../.gitbook/assets/image (264).png>)

All I had to do now, was to print the contents of the file. I ran `; cat flag.txt` and got the flag:

![](<../../.gitbook/assets/image (265).png>)

### 2EZ

![](<../../.gitbook/assets/image (266).png>)

For this question we get a file with a incorrect header:

![](<../../.gitbook/assets/image (267).png>)

I changed the header to be `ff d8 ff e0`. I also changed the file from **2ez** to **2ez.jfif**. I then got the flag:

![](<../../.gitbook/assets/image (268).png>)
