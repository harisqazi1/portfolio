# Walking An Application

### Walking An Application

The first two Tasks were just setting up the VPN and reading web information.

![](<../../.gitbook/assets/image (614) (1).png>)

### Exploring The Website

![](<../../.gitbook/assets/image (643).png>)

### Viewing The Page Source

I checked out the main page:

![](<../../.gitbook/assets/image (574) (1) (1).png>)

I then checked out the source code for the page, and found this:

![](<../../.gitbook/assets/image (328) (1) (1) (1).png>)

Clicking that link leads me to the following page:

![](<../../.gitbook/assets/image (370) (1) (1) (1) (1).png>)

I went after this question next:

![](<../../.gitbook/assets/image (324) (1) (1).png>)

I noticed this in the comments:

![](<../../.gitbook/assets/image (476).png>)

Following this link, got me the flag:

![](<../../.gitbook/assets/image (352) (1) (1) (1) (1).png>)

This was the next question:

![](<../../.gitbook/assets/image (411) (1) (1) (1).png>)

I ran **feroxbuster** on the URL:

![](<../../.gitbook/assets/image (444).png>)

I was running through the different links, and got one that the question was about:

![](<../../.gitbook/assets/image (328) (1) (1).png>)

I then got the flag:

![](<../../.gitbook/assets/image (621) (1).png>)

This was the last question:

![](<../../.gitbook/assets/image (630).png>)

I went back into the main website page, and I saw this comment:

![](<../../.gitbook/assets/image (688).png>)

This led me to the following:

![](<../../.gitbook/assets/image (352) (1) (1) (1).png>)

I found something interesting in the Change Log:

![](<../../.gitbook/assets/image (539).png>)

Going to `<Website.com>/tmp.zip` got me a file, which had the following information:

![](<../../.gitbook/assets/image (411) (1) (1).png>)

### Developer Tools - Inspector

For this Task, we go to `<Website.com>/news/article?id=3`.&#x20;

![](<../../.gitbook/assets/image (568) (1).png>)

We then have to remove the paywall. I found the following in the Inspector code:

![](<../../.gitbook/assets/image (552).png>)

If you delete the node (right-click on the line and hit "Delete Node"), you can then see the flag:

![](<../../.gitbook/assets/image (454).png>)

### Developer Tools - Debugger

For this task, the THM Author walks us through what to do. Essentially, you have to setup a breakpoint and wait for the flag to show up. For this, I went to the **contact** page. Then I opened the **Debugger** tab, and went to the **flash.min.js** file. After clicking on the **flash\['remove']\();**, which set up the breakpoint on the line, and I got the flag:

![](<../../.gitbook/assets/image (492).png>)

### Developer Tools - Network

For this one, I followed what the task said, and eventually found the flag. I typed random things in the fields, and then found the flag in the Response section:

![](<../../.gitbook/assets/image (370) (1) (1) (1).png>)
