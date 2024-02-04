# New Orleans

This is my write-up for the Microcorruption level New Orleans located at: [https://microcorruption.com/debugger/New%20Orleans](https://microcorruption.com/debugger/New%20Orleans).

![](<../../.gitbook/assets/image (1) (2).png>)

I started off by looking through the code. I noticed a line that stood out to me:

![](<../../.gitbook/assets/image (721).png>)

There is a two strings being compared. My assumption is that one string is our input and one string is the password. When they get compared, the output will be a number, and that leads to the jump being taken or the jump **not** being taken based on the output. I the set a break-point at 44c2, to see what those values are:

![](<../../.gitbook/assets/image (10) (1) (2).png>)

When I read r14, there was not any data of use there. I then tried 2400, which is the same thing as 0x2400 in this case. I saw a string of random characters, and submitted it as the password `` `*[6hCQ ``:

![](<../../.gitbook/assets/image (1) (3) (1).png>)

There was a way to see what the password is without waiting for the compare. In the create\_password function, you can see the following:

![](<../../.gitbook/assets/image (4) (1) (2).png>)

There are bytes being moved, which can be converted from hex to ascii:

![](<../../.gitbook/assets/image (3) (2) (2).png>)

This was another way to get the password.
