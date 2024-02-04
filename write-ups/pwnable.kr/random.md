# random

I used FileZilla to move the files to my local PC.

#### Code:

```c
#include <stdio.h>

int main(){
	unsigned int random;
	random = rand();	// random value!

	unsigned int key=0;
	scanf("%d", &key);

	if( (key ^ random) == 0xdeadbeef ){
		printf("Good!\n");
		system("/bin/cat flag");
		return 0;
	}

	printf("Wrong, maybe you should try 2^32 cases.\n");
	return 0;
}
```

My current target is the if-statement. That is what will lead me to getting the flag. I was not sure what the `^` symbol was called. I then learned it is called a `caret`. I learned from [https://www.cprogramming.com/tutorial/bitwise\_operators.html](https://www.cprogramming.com/tutorial/bitwise\_operators.html) that the process currently going on in the if-statement is a "Bitwise Exclusive-Or (XOR)". In XOR, we get a 1 if either of the inputs is a 1. If not, we get a 0. If both are a 1 or a 0, then your result will be a 0. The previous mentioned site had a great example:

```c
01110010 ^
10101010
--------
11011000
```

Back to the code, I converted `deadbeef` to binary to get: `11011110101011011011111011101111`. Basically, if the key I produce and the random value XOR to get me `11011110101011011011111011101111`, I am then able to get the flag. My first thought process if to brute-force it. Since I have 32 characters already from the conversion, I would just have my own random 32-character string and then brute-force the code to get the flag. I then went to [https://github.com/Gallopsled/pwntools](https://github.com/Gallopsled/pwntools) to see how I could leverage the library in my own code and brute-forcing. Before I did that, I created a flag file and filled with some text that will display, if my code is correct. I downloaded pwntools by running `pip install pwn`. I realized the brute-force would take long after testing. I checked out [https://jaimelightfoot.com/blog/pwnable-kr-random-walkthrough/](https://jaimelightfoot.com/blog/pwnable-kr-random-walkthrough/) for a nudge in the right direction, and I realized that my initial hunch for printing out random was right and I should have followed it. I edited the code to be the following:

```c
#include <stdio.h>

int main(){
	unsigned int random;
	random = rand();	// random value!
	printf("random is %x\n",random);
	return 0;
}
```

I ran the code, and I keep getting the same output:

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

I guess it was not random after all. I took this value and got the binary value of it, since this is hexadecimal. I ended up getting: `01101011100010110100010101100111`. Now all I have to do is "unXOR" these values and I can get the right value. Here is the mini cheat-sheet I used:

```
1 + 0 = 1
0 + 1 = 1
0 + 0 = 0
1 + 1 = 0
```

| Name       | Binary Value                     |
| ---------- | -------------------------------- |
| random     | 01101011100010110100010101100111 |
| Key        | 10110101001001101111101110001000 |
| 0xdeadbeef | 11011110101011011011111011101111 |

I eventually got `10110101001001101111101110001000`. I converted this to decimal (3039230856) and got the flag:

<figure><img src="../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

### Code:

The calculation portion can be automated in Python.&#x20;

```python
#!/usr/bin/python3

random = '01101011100010110100010101100111'        
output = '11011110101011011011111011101111'
#turning the strings into lists
lr = list((random))
lo = list((output))

string = ''
len = 32 #length of bits

for x in range(0,len):
	# top is 0  / bottom is 1
	if int(lr[x]) == 0 and int(lo[x]) == 1:
		string += '1'
	# top is 0 / bottom is 0
	elif int(lr[x]) == 0 and int(lo[x]) == 0:
		string += '0'
	#top is 1 / bottom is 1
	elif int(lr[x]) == 1 and int(lo[x]) == 1:
		string += '0'
	# top is 1 / bottom is 0
	else:
		string += '1'


print("Binary:" + string)
print("Decimal:")
print(int(string,2))
```

#### Lessons Learned:

* Print all variables: If I had printed the random variable, I would have not needed to reference the write-up
* Keep track of variable types: At the end, I had entered the bits and got it wrong. I then learned you had to convert the string to decimal and get the flag.
