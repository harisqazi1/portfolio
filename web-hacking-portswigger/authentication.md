# Authentication

{% hint style="info" %}
The "Web Hacking" section are notes that I have compiled while learning using the PortSwigger Web Security Academy and from the book _Bug Bounty Bootcamp: The Guide to Finding and Reporting Web Vulnerabilities_ by Vickie Li. A majority of the notes/codes are from them, however I will modify these according to my needs.
{% endhint %}

### Username enumeration considerations <a href="#username-enumeration" id="username-enumeration"></a>

Username enumeration is when an attacker is able to observe changes in the website's behavior in order to identify whether a given username is valid.While attempting to brute-force a login page, you should pay particular attention to any differences in:

* **Status codes**
* **Error messages**
* **Response times**

### Username enumeration via different responses

BurpSuite Intruder can be used to brute-force a login page in order to see which usernames and passwords work together. You start with finding out what usernames are on the system, by running the **Sniper** **Attack Type** (with a custom wordlist) on just the username portion:

`username=§apache§&password=admin`

Viewing the Intruder attack logs, if we see a difference in length of the response, then we have found a valid username. We then do the same for the password as well:

`username=apache&password=§admin§`

Since we know the username from the previous attack, we then only need the password for the full login. For this one, we view the HTTP response status codes in the Intruder window. If one is different from the rest, we have then found our password.

### Username enumeration via subtly different responses

We can also use responses themselves instead of the status codes in order to find out when a username and password are correct/incorrect. An example of this is viewing the response from a website:

When the username and password are wrong:

![](<../.gitbook/assets/image (340) (1) (1) (1).png>)

When the username is correct and the password is wrong (notice the last period missing):

![](<../.gitbook/assets/image (337).png>)

We can use these strings to then use the Burp Intruder to search for those exact strings using the **Grep - Match** feature (under Options):

![](<../.gitbook/assets/image (325) (1) (1).png>)

When we do not see that string in the response, we can assume we have a password:

![](<../.gitbook/assets/image (339) (1) (1).png>)

### Username enumeration via response timing

We can use responses to notice differences in output from a system to notice a potential found username. This is done by adding a

`X-Forwarded-For: xxxxxx`

header which then allows you to bypass an IP-address limit for requests. Using the **Pitchfork Attack Type** you can choose different lists to use for different sections. An example query:

`POST /login HTTP/1.1 Host: ace31fb61fba8c41c03d201f009d0022.web-security-academy.net`&#x20;

`.....`

`Connection: close`

`X-Forwarded-For: §2§`

`username=§mysql§&password=hgjgjhghjghjgjh`

You can use this to then bypass the IP-address limit and then look for responses that are different, indicating that you have potentially found a username:

![](<../.gitbook/assets/image (325) (1).png>)

You can do the same with the password, however for the password the **status** column lets you know if you got the correct password.

