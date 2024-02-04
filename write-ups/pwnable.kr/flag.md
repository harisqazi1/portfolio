# flag

![](<../../.gitbook/assets/image (11) (1) (2).png>)

There is no source code this time. I downloaded the code and then ran it:

![](<../../.gitbook/assets/image (12) (3) (1).png>)

They mention that they use strcpy, which means that the string will be copied from one location to another. This means we might be able to find the string during transit from one register to another. I ran strings on the file to see if there was anything interesting, and I found this:

![](<../../.gitbook/assets/image (14) (1) (2) (1).png>)

I unpacked the flag file:

![](<../../.gitbook/assets/image (10) (3) (1).png>)

I then ran the code with gdb, and made a breakpoint at main. Then I just kept going line by line (by entering "next") until I saw something interesting in the registers:

![](<../../.gitbook/assets/image (13) (4).png>)

This had a pwnable.kr flag feel to it, so I entered it, and this was the flag. I want to see if I can get the flag using Cutter as well. You could technically find this by looking through all the strings in the file in Cutter:

![](<../../.gitbook/assets/image (7) (1) (2) (1).png>)
