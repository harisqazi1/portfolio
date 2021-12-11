# Authentication

### Username enumeration via different responses

BurpSuite Intruder can be used to brute-force a login page in order to see which usernames and passwords work together. You start with finding out what usernames are on the system, by running the **Sniper** **Attack Type** (with a custom wordlist) on just the username portion:

`username=§apache§&password=admin`

Viewing the Intruder attack logs, if we see a difference in length of the response, then we have found a valid username. We then do the same for the password as well:

`username=apache&password=§admin§`

Since we know the username from the previous attack, we then only need the password for the full login. For this one, we view the HTTP response status codes in the Intruder window. If one is different from the rest, we have then found our password.

### Username enumeration via subtly different responses

We can also use responses themselves instead of the status codes in order to find out when a username and password are correct/incorrect. An example of this is viewing the response from a website:

When the username and password are wrong:

![](<../.gitbook/assets/image (340).png>)

When the username is correct and the password is wrong (notice the last period missing):

![](<../.gitbook/assets/image (337).png>)

We can use these strings to then use the Burp Intruder to search for those exact strings using the **Grep - Match** feature (under Options):

![](<../.gitbook/assets/image (325).png>)

When we do not see that string in the response, we can assume we have a password:

![](<../.gitbook/assets/image (339).png>)

### Username enumeration via response timing

