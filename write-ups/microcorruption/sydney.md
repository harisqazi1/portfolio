# Sydney

This is my write-up for the level Sydney located at: [https://microcorruption.com/debugger/Sydney](https://microcorruption.com/debugger/Sydney).

![](<../../.gitbook/assets/image (27) (1) (2) (1).png>)

I looked in the main function to see what was going on:

![](<../../.gitbook/assets/image (12) (1) (2).png>)

There were two functions that interested me: `get_password` and `check_password:`

**get\_password**:

![](<../../.gitbook/assets/image (19) (4) (1).png>)

**check\_password**:

![](<../../.gitbook/assets/image (5) (4).png>)

The `get_password` seems to be getting the password from what the user inputs. The `check_password` function, from my understanding, seems to be running multiple times, which would mean that there are multiple passwords in order to pass this exercise. I will set a break-point at the first line of the check\_password function and see what is being compared. I tried this out and learned that maybe `#0x303a` might be a number, and that is being compared with the user input. I entered the hex value AND its decimal equivalent to the program, and it was rejected both times. I then took another look at the code. At this point, I was stumped. I did research online into the right side of the "equation" and found this: [https://stackoverflow.com/questions/21207778/what-does-0x0r15-mean](https://stackoverflow.com/questions/21207778/what-does-0x0r15-mean). It just reaffirmed my position, that the value being compared to `#0x303a` is either one password or part of the password. After tinkering with the code, I realized that instead of entering a number, I should try entering a string, or taking the preset value and converting it into a string. `0x303a` is `0:` in ASCII. I entered that, and the password was incorrect. I then entered `:0`, because it could have been a little endian issue, and I got to the next step:

![](<../../.gitbook/assets/image (7) (4).png>)

Now I have to convert the next 3 values in order to get the correct password. It seems that all of the compares are doing 2 characters at a time from an offset of 2 from r15. We know the first 2 characters are `:0`, now we just have to continue with the same way we got the first 2 characters.

| Hex (little endian) | ASCII |
| ------------------- | ----- |
| 3a 30               | :0    |
| 38 70               | 8p    |
| 64 26               | d&    |
| 3c 5f               | <\_   |

The whole password ends up being: `:08pd&<_` . Now we enter it in the program and have access:

![](<../../.gitbook/assets/image (2) (1) (1) (2) (1).png>)
