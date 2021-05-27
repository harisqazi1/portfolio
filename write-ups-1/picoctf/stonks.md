# Stonks

![](../../.gitbook/assets/image%20%2877%29.png)

The first thing I did, was to download the code and then try to compile it and run it on my own local machine. When that was unsuccessful, I then tried to play around with the online version of it. After using **ghidra**, I was still unable to find the flag. I then references [this site](https://dmfrsecurity.com/2021/04/07/picoctf-2021-stonks-writeup/), and then I got a hint about what to do. I also did view [this site](https://github.com/Dvd848/CTFs/blob/master/2021_picoCTF/Stonks.md) in order to get a better understanding of the program. I then ran the script from the second website, using pwntools. 

![](../../.gitbook/assets/image%20%2879%29.png)

This led me to get the flag:

![](../../.gitbook/assets/image%20%2861%29.png)

However, that flag was incorrect. I had to do further research and found [this site](https://github.com/vivian-dai/PicoCTF2021-Writeup/blob/main/Binary%20Exploitation/Stonks/Stonks.md), where the guy reveals how he solved it and that answer worked for me:

![](../../.gitbook/assets/image%20%2858%29.png)



