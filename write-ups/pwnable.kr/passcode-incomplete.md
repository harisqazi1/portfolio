# passcode (incomplete)

![](<../../.gitbook/assets/image (3) (3).png>)

**passcode.c**:

```c
#include <stdio.h>
#include <stdlib.h>

void login(){
	int passcode1;
	int passcode2;

	printf("enter passcode1 : ");
	scanf("%d", passcode1);
	fflush(stdin);

	// ha! mommy told me that 32bit is vulnerable to bruteforcing :)
	printf("enter passcode2 : ");
        scanf("%d", passcode2);

	printf("checking...\n");
	if(passcode1==338150 && passcode2==13371337){
                printf("Login OK!\n");
                system("/bin/cat flag");
        }
        else{
                printf("Login Failed!\n");
		exit(0);
        }
}

void welcome(){
	char name[100];
	printf("enter you name : ");
	scanf("%100s", name);
	printf("Welcome %s!\n", name);
}

int main(){
	printf("Toddler's Secure Login System 1.0 beta.\n");

	welcome();
	login();

	// something after login...
	printf("Now I can safely trust you that you have credential :)\n");
	return 0;	
}
```

We can see the passwords in plain text in the code: `338150` and `13371337`. I tried entering those and ended up getting a segmentation fault:

![](<../../.gitbook/assets/image (15) (2).png>)

I ran **checksec** on the program just to see what was there:

![](<../../.gitbook/assets/image (16) (3) (1) (1).png>)

There was nothing too out of the ordinary for me here. I went back at the prompt for the question and noticed they mentioned something about a compiler warning. I compiled the code again to focus on the compiler warnings:

![](<../../.gitbook/assets/image (17) (4) (1).png>)

I learned online that scanf takes in a pointer to an input instead of the input itself. I think we might able to manipulate this by writing our passcodes in the name string. We can then point scanf to look for the pointer where that passcode is located. I fired up gdb with the pwndebug add-on to it. I made the first break in main. This was just to get the program going to see where the updated memory locations were of the functions and variables. I then disassembled the welcome function because that is where our input goes to:

![](<../../.gitbook/assets/image (7) (2) (2).png>)

I will break after scanf just to see what register is holding our data in it:

![](<../../.gitbook/assets/image (18) (4) (1).png>)

There seems to be 3 registers that are currently holding our information: rax, r9, and rsp. I tried multiple things after this but was unable to get anywhere. I then found this write-up of the same challenge which made me understand this a bit more: [https://jaimelightfoot.com/blog/pwnable-kr-passcode-walkthrough/](https://jaimelightfoot.com/blog/pwnable-kr-passcode-walkthrough/). The first thing I had learned from this write-up was that the lea instruction and registers were a way to figure out what the variable is in a given context. In our case, we have the following:

![](<../../.gitbook/assets/image (4) (3).png>)

After the data gets scanned, we can make a good guess that the value that just got scanned in will be moved to another register.

![](<../../.gitbook/assets/image (720).png>)

Now we have verified that this is the location of our variable's data. However, it is only temporary, since it gets moved due to the code still being run:

![](<../../.gitbook/assets/image (1) (2) (2).png>)

{% hint style="info" %}
This write-up is incomplete.
{% endhint %}
