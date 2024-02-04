# babygame01

<figure><img src="../../../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption></figcaption></figure>

I started off by checking out the game they mention.&#x20;

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

Seems like a "snake game"-esque type situation, where our goal is "X". I was playing around with the code and didn't get anywhere. I then popped this binary open in Ghidra:

<figure><img src="../../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

I modified the initial variable to be called "player\_location". We notice that the do-while loop breaks if the variable **equals** "0x1d". I type that into the enter location and my character moved right one step. My current thoughts are that in order to move right, I would have to keep typing "1d" and to move down it would be "59". Unfortunately, it did not work like that. While browsing through the `move_player` function, I saw the following:

<figure><img src="../../../.gitbook/assets/image (15) (5).png" alt=""><figcaption></figcaption></figure>

This seems to be a classic WASD game. I noticed that we can run `p` as well. We get the output of "You win!", but no actual flag. I was playing around with directions I was able to go and thought, maybe you are not supposed to go to the X at all. I then went backward, and noticed that the `Player position` variable and its line disappear when my character goes off the map. When I went back a couple positions beyond that, I saw the value of `Player has flag` to go from 0 to 46. This could have been because I was spamming `a` in my code. I then hit `p` after I saw the value of 46, and then I got the flag. Here is the locally tested output:

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

Then I got the flag actually:

<figure><img src="../../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

Here is the code I used:

```python
from pwn import *
context(arch = 'i386', os = 'linux')

#p = remote('saturn.picoctf.net', 57100)
# EXPLOIT CODE GOES HERE
#r.send(asm(shellcraft.sh()))
#r.interactive()

#modified from https://ir0nstone.gitbook.io/notes/other/pwntools/elf
elf = ELF('./game')
p = elf.process()

str = ""
for i in range(368):
	str += "a"

p.sendline(str)
p.sendline("p")
p.interactive()
```
