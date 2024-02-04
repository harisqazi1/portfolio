# Cusco

This is my write-up for the level Cusco located at: [https://microcorruption.com/debugger/Cusco](https://microcorruption.com/debugger/Cusco).

![](<../../.gitbook/assets/image (20) (1) (2).png>)

From reading the document, it seems that they may have put some checks in place to make sure the password is not too long. Using a long password was something we used for the Hanoi level in order to pass the password check. I started off by checking out the `main` function:

![](<../../.gitbook/assets/image (2) (1) (1) (1) (2).png>)

The main was calling another function called `login`:

![](<../../.gitbook/assets/image (11) (5).png>)

Playing around with the code, I notice that our input is stored in r15:

![](<../../.gitbook/assets/image (3) (4).png>)

I also learned at there are bytes being moved in the `test_password_valid` function:

![](<../../.gitbook/assets/image (9) (3) (1).png>)

My current attempt is to try to see how I can mess with the bytes to override the location and thus override the value at r15. I noticed a push of `0x7d` in line 4468, which is `}` in ASCII. When I brute forced that character, I noticed the following:

![](<../../.gitbook/assets/image (15) (1) (1) (1).png>)

It did accept my input, but the password was in correct. Maybe I can override the value this way? I think I was on the right track. I looked at the write-up at [https://jaimelightfoot.com/blog/microcorruption-embedded-security-ctf-cusco/](https://jaimelightfoot.com/blog/microcorruption-embedded-security-ctf-cusco/), and was on the right track. The insn address was unaligned, which after reading the write-up meant that it was some sort of return address for the program. I have to try to get this to return pass the check and go directly to unlocking the door for me. Lets see if I can try to push it here:

![](<../../.gitbook/assets/image (10) (4).png>)

I converted the location to hexadecimal:

![](<../../.gitbook/assets/image (12) (2).png>)

That unfortunately did not work out. I got stuck here and went to the write-up I linked to earlier, and I got reminded that little-endian is a thing. Little-Endian is a type of addressing, which is used in C ([Source](https://www.techopedia.com/definition/12892/little-endian) paraphrased). I was trying to enter in the correct input, but since it was not little-endian, it was not working at all. The author from the write-up ended up using the `unlock_door` function to get back to, while I was able to get it to work using the address for when it is called in the `login` function. It is just a line difference, so from what I understand about this, should not be too big of a difference. Here is the step by step:

Take the address for the `unlock_door` function or the call from the `login` function. I will use the one from the `login` function, so that address is `0x4528` . The important part now, is to make this in little-endian format so `0x2845`. Now we can spam this in our password block (make sure to check the "Check here if entering hex encoded input." box (since addresses are in hexadecimal) and hit submit. For me, repeating `0x2845` 15 times worked out for me:

![](<../../.gitbook/assets/image (7) (5).png>)

Now we can actually submit the value using the `solve` command:

![](<../../.gitbook/assets/image (1) (4).png>)

In both cases, we did not actually get the password. I think this exercise was more about getting the door open by manipulating return addresses, than figuring out the password for the level.
