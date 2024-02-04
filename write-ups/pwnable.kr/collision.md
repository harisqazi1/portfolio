# collision

![](<../../.gitbook/assets/image (8) (2) (1).png>)

This was the source code for this exercise:

```c
#include <stdio.h>
#include <string.h>
unsigned long hashcode = 0x21DD09EC;
unsigned long check_password(const char* p){
	int* ip = (int*)p;
	int i;
	int res=0;
	for(i=0; i<5; i++){
		res += ip[i];
	}
	return res;
}

int main(int argc, char* argv[]){
	if(argc<2){
		printf("usage : %s [passcode]\n", argv[0]);
		return 0;
	}
	if(strlen(argv[1]) != 20){
		printf("passcode length should be 20 bytes\n");
		return 0;
	}

	if(hashcode == check_password( argv[1] )){
		system("/bin/cat flag");
		return 0;
	}
	else
		printf("wrong passcode.\n");
	return 0;
}
```

I will move this code to my local machine so I can play around with the code with a bit of ease.

![](<../../.gitbook/assets/image (7) (2) (1).png>)

We need a passcode in order to get the flag. From the code, I can see that the password needs to have a length of 20 bytes. In addition, if the password is equal to the hashcode variable, then the flag gets released. The value of hashcode decoded from hash would be: 568134124 in decimal. It seems that, even converted to decimal, that string is not as long as 20 bytes. I modified the code in order to see what the number we are looking for is:

![](<../../.gitbook/assets/image (9) (1) (2) (1).png>)

![](<../../.gitbook/assets/image (5) (1) (2) (1).png>)

The game now is to play around with the numbers until I have reached the same decimal value as the hashcode decimal value. I spent about 4 hours trying to solve this one, and was not able to get anywhere close to the flag. I read up on the write-up here to get a better idea: [https://0xrick.github.io/pwn/collision/](https://0xrick.github.io/pwn/collision/). I then learned that the input passcode gets broken down into five pieces. The five pieces have to add up to 568134124, which is what the hashcode equals to in decimal. The issue comes to be that that number is not divisible by 5:

![](<../../.gitbook/assets/image (2) (2).png>)

If we remove the decimal place from the output, and multiply that number by 5, we end up with:

![](<../../.gitbook/assets/image (3) (1) (1) (2).png>)

If we subtract the decimal version of hashcode from this number, this is what the output is:

![](<../../.gitbook/assets/image (6) (2).png>)

What this ends up meaning is that we will have 4 parts of 113626824, and one part of 113626828. Adding up these numbers gets us the initial hashcode value.

![](<../../.gitbook/assets/image (5) (2) (1) (1).png>)

Now we have to convert those values into hex and pass it onto the program. 113626824 equals to 6C5CEC8 in hex. The second integer 113626828 is equals to 6C5CECC in hex. After putting those values together, it seemed that it still did not work:

![](<../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png>)

I went back to the aforementioned write-up, and the author ends up using python in order to print those characters (after they have been translated to little-endian). I did copy the code from there:

`./col python -c 'print "\xc8\xce\xc5\x06" * 4 + "\xcc\xce\xc5\x06"'`

This is what ends up getting the flag.

### **What I learned**

I ended up learning about using python as a way to print characters for a binary. In addition, I learned about the actual reverse engineering process for this exercise, since I was stuck on it for a long time. The last thing I learned was that C code usually displays values in the little-endian format.
