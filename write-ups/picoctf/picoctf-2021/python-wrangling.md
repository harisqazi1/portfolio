# Python Wrangling

![](<../../../.gitbook/assets/image (78).png>)

For this CTF, you start off with 3 different files, and we have to use that to get the flag. cat-ing the flag file showed that it was encrypted:

![](<../../../.gitbook/assets/image (56).png>)

I realized that I would have to use the python script and enter the password in the pw.txt file to fix the flag. Running the python script you get shown this message:

![](<../../../.gitbook/assets/image (73) (1).png>)

For me, this means that the password file would be the "(-e/-d)" and the "\[file]" would be the flag file. I got n error, and I realized that the (-e/-d) was a choice I had to make and not a file. I also realized that I would have to manually enter in the password. I did that, and then I got the flag:

![](<../../../.gitbook/assets/image (64).png>)
