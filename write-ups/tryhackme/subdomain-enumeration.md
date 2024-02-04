# Subdomain Enumeration

### Task 1: Brief

The answers to the questions in this task, were all in the written portion:

![](<../../.gitbook/assets/image (620) (2).png>)

### Task 2: OSINT - SSL/TLS Certificates

I went to [https://crt.sh](https://crt.sh), and searched for "tryhackme.com". This got me a list:

![](<../../.gitbook/assets/image (411).png>)

I then ran a Control-F for the date they mentioned, and found this:

![](<../../.gitbook/assets/image (716).png>)

This was the answer to the question.

### Task 3: OSINT - Search Engines

If you type "**-site:www.tryhackme.com  site:\*.tryhackme.com**" on Google.com, then you will find this:

![](<../../.gitbook/assets/image (369).png>)

This is the answer for the question.

### Task 4: DNS Bruteforce

For this question, THM provides a link to **View Site**, which leads to a panel:

![](<../../.gitbook/assets/image (639) (1) (1).png>)

The answer was in the output.

### Task 5: OSINT - Sublist3r

Similar panel to the previous question:

![](<../../.gitbook/assets/image (353).png>)

The answer was in the output as well.

### Task 6: Virtual Hosts

I ran the command `ffuf -w namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.110.177 -fs 2395` and I got the solution eventually (after 13 minutes):

![](<../../.gitbook/assets/image (359) (1).png>)
