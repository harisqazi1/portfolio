# Hanoi

This is my write-up for Honoi located at: [https://microcorruption.com/debugger/Hanoi](https://microcorruption.com/debugger/Hanoi).

![](<../../.gitbook/assets/image (722).png>)

I will now look through the code to see what is going on:

![](<../../.gitbook/assets/image (723).png>)

The main seems to be calling one function only: login. Login is a bit of a bigger function than we are used to seeing for these challenges:

![](<../../.gitbook/assets/image (6) (1) (3).png>)

In line 4544, we can see that another function called `test_password_valid` gets called into place, to check if our password is correct or not.

![](<../../.gitbook/assets/image (5) (1) (1).png>)

I will try to play around with the code and see what I can find out. It seems that 2400 is where our input is being sent to:

![](<../../.gitbook/assets/image (7) (3).png>)

I started printing out registers in the `test_password_valid` function and noticed something that stood out to me:

![](<../../.gitbook/assets/image (13) (1) (2).png>)

I am thinking that maybe that could be the password? It does seem between 8 to 16 characters, so does not hurt to try it out. The first line is 16 characters, so I will try that out:

```
HE<D1@.D.B\.u.5.
```

I will start from 8 characters and keep adding one each time to see if I can get the password that way. I did not end up getting the password this way. I then went back to look at the function where the password was being tested. After spending some time working on the code, I had gotten nowhere. I then read this [write-up](https://jaimelightfoot.com/blog/microcorruption-embedded-security-ctf-hanoi/), where the author made it more clear to me that maybe that function might be a false flag for finding the actual password. I then took a step back and looked what happens in the code AFTER the function gets called:

![](<../../.gitbook/assets/image (2) (1) (1) (2) (2) (1) (1).png>)

`0x7c` is `|` in text. I will try to see if I can just spam that and get the flag. I did:

![](<../../.gitbook/assets/image (15) (1) (1) (2).png>)

Here is why that worked. Although the program said 8 to 16 characters, there was nothing really limiting us from adding more of less characters. I did not see any length check here. In addition, in order to pass the check for the password, one line really gives it away:

![](<../../.gitbook/assets/image (1) (3).png>)

The cmp.b instruction is comparing a preset value of `0x7c` or `|` (in ASCII) with the data at the location of 0x2410. If you had stuck to 16 characters for the password, you would not have reached the address where the checking is actually being done.

Same answer but with the actual level solver:

![](<../../.gitbook/assets/image (2) (3).png>)

![](<../../.gitbook/assets/image (4) (1) (1).png>)
