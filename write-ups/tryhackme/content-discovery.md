# Content Discovery

### Task 1: What Is Content Discovery?

The answers for these questions were in the reading:

![](<../../.gitbook/assets/image (711).png>)

### Task 2: Manual Discovery - Robots.txt

I see the following at `IP_Address/robots.txt`:

![](<../../.gitbook/assets/image (646).png>)

This is what the disallowed endpoint showed:

![](<../../.gitbook/assets/image (534).png>)

### Task 3: Manual Discovery - Favicon

The THM written part mentioned the command to run: `curl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico | md5sum`

I run the command on my local machine:

![](<../../.gitbook/assets/image (352) (1) (1).png>)

I then ran a Control-F on [https://wiki.owasp.org/index.php/OWASP\_favicon\_database](https://wiki.owasp.org/index.php/OWASP\_favicon\_database), and I found this:

![](<../../.gitbook/assets/image (324) (1).png>)

The favicon was from **cgiirc**. Another way to find this was to open the favicon:

![](<../../.gitbook/assets/image (533).png>)

If you can read the image, it spells out **cgiirc**.

### Task 4: Manual Discovery - Sitemap.xml

We can look through the sitemap to see if there is anything interesting. I found the following:

![](<../../.gitbook/assets/image (506).png>)

This led me to this site:

![](<../../.gitbook/assets/image (565).png>)

### Task 5: Manual Discovery - HTTP Headers

Running the command mentioned in the Task, I got the following:

![](<../../.gitbook/assets/image (411) (1).png>)

### Task 6: Manual Discovery - Framework Stack

I accessed the link mentioned in the Task, and got here:

![](<../../.gitbook/assets/image (580) (1).png>)

I then went under **documentation**, and saw the following:

![](<../../.gitbook/assets/image (402) (2).png>)

Using those credentials (on that endpoint) I was then able to get the flag:

![](<../../.gitbook/assets/image (574) (1).png>)

### Task 7: OSINT - Google Hacking / Dorking

You can answer the question by looking at the information provided on the page:

![](<../../.gitbook/assets/image (524).png>)

The answer was **site:**

### Task 8: OSINT - Wappalyzer

The answer to this question was based on the reading in the Task: **wappalyzer**

### **Task 9: OSINT - Wayback Machine**

Similar to the last question, the answer to this question was in the reading as well: **https://archive.org/web/**

### Task 10: OSINT - GitHub

The answer was in the reading:

![](<../../.gitbook/assets/image (485).png>)

### Task 11: OSINT - S3 Buckets

The answer was once again in the reading:

![](<../../.gitbook/assets/image (532) (1).png>)

### Task 12: Automated Discovery

I ended up using `ffuf`, since it seemed to be the fastest for me in terms of response:

![](<../../.gitbook/assets/image (510) (2).png>)
