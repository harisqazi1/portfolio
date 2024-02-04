# \[fd]

![](<../../.gitbook/assets/image (11) (4).png>)

After you SSH into the machine you see 3 files: the executable, source code, and the flag file. I took a look into the source code first:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
char buf[32];
int main(int argc, char* argv[], char* envp[]){
	if(argc<2){
		printf("pass argv[1] a number\n");
		return 0;
	}
	int fd = atoi( argv[1] ) - 0x1234;
	int len = 0;
	len = read(fd, buf, 32);
	if(!strcmp("LETMEWIN\n", buf)){
		printf("good job :)\n");
		system("/bin/cat flag");
		exit(0);
	}
	printf("learn about Linux file IO\n");
	return 0;

}
```

It seems that string compare is being used here. I will run the code locally so that way I am able to manipulate it as I want:

![](<../../.gitbook/assets/image (9) (2).png>)

I edited the source code on my local machine:

![](<../../.gitbook/assets/image (1) (1) (2) (1).png>)

The lines highlighted in red are my additions.

I looked over the source code, and noticed that if I entered all 0s, then this is the output I got:

![](<../../.gitbook/assets/image (3) (1) (1) (1).png>)

I was curious to see what 4660 was. It turns out that is what 0x1234 is in hex:

![](<../../.gitbook/assets/image (4) (2) (1).png>)

I then ran the code (on the pwnable.kr site) with this number:

![](<../../.gitbook/assets/image (4) (1) (1) (1).png>)

It seemed that the code was waiting for a response. I entered "LETMEWIN", since I saw that in the source code, and I then got the flag:

![](<../../.gitbook/assets/image (2) (1) (2) (1).png>)
