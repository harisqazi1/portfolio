# Reversing ELF

This is my write-up for Reversing ELF on TryHackMe located at: [https://tryhackme.com/room/reverselfiles](https://tryhackme.com/room/reverselfiles).

### Crackme1

![](<../../.gitbook/assets/image (12) (2) (1).png>)

I just ran the file and it gave me the flag:

![](<../../.gitbook/assets/image (18) (2).png>)

### Crackme2

![](<../../.gitbook/assets/image (10) (2) (1).png>)

Running the code lets you know that there is a password you need to get access to the flag:

![](<../../.gitbook/assets/image (20) (2).png>)

Running strings on the code shows me that there might be the password in plain text:

![](<../../.gitbook/assets/image (21) (2).png>)

Using that password, you are able to see the flag:

![](<../../.gitbook/assets/image (486).png>)

### Crackme3

![](<../../.gitbook/assets/image (15) (1) (2).png>)

![](<../../.gitbook/assets/image (16) (2).png>)

I ran strings on the file to see if there was something I was able to get from it. I was able to find what looked like a base64 encoded string:

![](<../../.gitbook/assets/image (17) (2).png>)

I then decoded that string using the terminal, and got the flag:

![](<../../.gitbook/assets/image (13) (2) (1).png>)

### Crackme4

![](<../../.gitbook/assets/image (9) (2) (1) (1).png>)

![](<../../.gitbook/assets/image (19) (2).png>)

I think I had seen somewhere that strcmp is vulnerable. In addition, strings did not provide me with any hints or passwords. I pulled up the code in Ghidra, and noticed that there is a function called **compare\_pwd**:

![](<../../.gitbook/assets/image (10) (2).png>)

I then patched the code to make the password seem correct. I did this by patching the instruction for where _iVar == 0_, thus leading to if iVar ==0, IF the password is incorrect. However, this did not end up giving me the flag:

![](<../../.gitbook/assets/image (11) (3) (1).png>)

I then opened the file in Cutter, and put a break-point where the strcmp occurs:

![](<../../.gitbook/assets/image (18) (3).png>)

After I get to the break-point, I held my mouse over the rax variable, and that led me to the password. The following screenshot is from the Register References window in Cutter:

![](<../../.gitbook/assets/image (22) (1).png>)

I was able to replicate this in gdb-peda as well:

![](<../../.gitbook/assets/image (13) (2).png>)

The main thing here is that strcmp will compare two strings and output an integer value based on if they are equal or not. If you are able to print out the values at the registers prior to the comparison, you can get the flag.

### Crackme5

![](<../../.gitbook/assets/image (341).png>)

Running the command doesn't really give that much information:

![](<../../.gitbook/assets/image (19) (1).png>)

I looked at the code in Cutter:

![](<../../.gitbook/assets/image (17) (3).png>)

In the image, I have highlighted the registers that interested me the most. It seemed that those registers were being compared later on. Since they will be compared, my assumption was that one was the correct password and one variable was the input from the user. With that being said, I hopped into gdb (with the peda modifications) and then put a break-point right at _rax = $s1_. The reason for this was that when I put the break-point there, rdx AND rax would be set with the values and it would be easier to read both there. That ended up getting me the flag:

![](<../../.gitbook/assets/image (9) (2) (1).png>)

In the image, I did end up accidentally using the flag as my input, thus rax AND rdx being hidden.

### Crackme6

![](<../../.gitbook/assets/image (12) (2) (2).png>)

![](<../../.gitbook/assets/image (574).png>)

Running the program by itself does not give off too much data. I opened the code using Cutter:

![](<../../.gitbook/assets/image (17) (1) (2).png>)

Here we can see the if statement checking if there is a command line argument while the code is being run or not. If there is not, then it prints the string. If there is, then it goes into the _compare\_pwd_ function:

![](<../../.gitbook/assets/image (12) (2) (3).png>)

This is a simple function to check if the password is correct or not. It does rely on another function in order to make this decision: _my\_secure\_test._

![](<../../.gitbook/assets/image (433).png>)

This function seems to be taking in the users input and then checking each character of the user input. There are 8 labels in total, so my assumption is then that this is an 8 character password. I see that **al** (a register?) is being compared in each label to a hex value. I got all of the hex values from each of the labels and used [https://www.rapidtables.com/convert/number/hex-to-ascii.html](https://www.rapidtables.com/convert/number/hex-to-ascii.html) to decode it for me:

![](<../../.gitbook/assets/image (718).png>)

It ended up outputting a password for me. That was the flag:

![](<../../.gitbook/assets/image (510).png>)

### Crackme7

![](<../../.gitbook/assets/image (483).png>)

![](<../../.gitbook/assets/image (620).png>)

It seems like I would have to bypass the checks in place for each of the functions to get to the flag. I opened the file up in Cutter and noticed a _giveFlag_ function:

![](<../../.gitbook/assets/image (428).png>)

I went back into the main function to see if there was anything more tangible for me to go off of. I noticed something interesting:

![](<../../.gitbook/assets/image (672).png>)

In order for you to even have access to the _giveFlag_ function, the register **eax** has to be set to the hex value of **0x7a69**. I tried multiple variations of this:

![](<../../.gitbook/assets/image (11) (3).png>)

None of them seemed to work. Then I opened the file in Ghidra, and held my mouse on the value:

![](<../../.gitbook/assets/image (424).png>)

It gave me the decimal value, which turned out to be the password:

![](<../../.gitbook/assets/image (660).png>)

### Crackme8

![](<../../.gitbook/assets/image (13) (3).png>)

![](<../../.gitbook/assets/image (16) (1) (2).png>)

I opened the file in Cutter to see what the main function had in it:

![](<../../.gitbook/assets/image (717).png>)

It seems that this should be pretty straight forward. If the register **eax** is equal to the value **0xcafef00d**, you get the flag.

![](<../../.gitbook/assets/image (14) (2).png>)

Trying that as is did not seem to work. I then looked into the _atoi_ function and found this website that explained a bit about it: [https://www.tutorialspoint.com/c\_standard\_library/c\_function\_atoi.htm](https://www.tutorialspoint.com/c\_standard\_library/c\_function\_atoi.htm).

![](<../../.gitbook/assets/image (385).png>)

This makes me think that I might have to change the hex value of **0xcafef00d** to some other type to get the flag. The return value of atoi is of type integer so that does give a little bit of a hint. I did end up getting the flag, but not the way it was intended. I will look at other people's solutions to how they ended up solving this without patching the code. What I have learned so far was that atoi and itoa are "opposite" functions. atoi would make a string an integer, while itoa would make an integer into a string. In the code, you are to input a value, which would get passed into atoi. Then that output from atoi gets compared to **0xcafef00d**. If they are the same, then you get the flag. I was not able to get the flag this way. The way I did it was that I patched the code using ghidra, and then got the flag since the patch made it that any password other than the correct password would get me the flag:

![](<../../.gitbook/assets/image (402).png>)

![](<../../.gitbook/assets/image (18) (1) (2) (1).png>)

After reading [https://tereresecurity.wordpress.com/2021/12/09/write-up-reversing-elf/](https://tereresecurity.wordpress.com/2021/12/09/write-up-reversing-elf/), I realized that I had overlooked the two's compliment number, which was a number I had seen multiple times while in Ghidra. That number was the actual password for this binary.
