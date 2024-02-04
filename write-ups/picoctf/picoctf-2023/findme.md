# findme

<figure><img src="../../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

After entering `test` as the username and `test!` as the password, we see the following page:

<figure><img src="../../../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>

I tried accessing /flag after the domain, but I had no luck. I then noticed something. Between the login and the page with the screenshot mentioned above, we are being redirected. If I can edit those redirects or see what encoding they are using, I can try to get a flag that way. I then used Intruder in Burpsuite to see what the situation was.

After logging in, I saw the the `id` being mentioned in the URL:

<figure><img src="../../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

If we scroll over the test, we can see that it is encoded with Base64, and that we might get the flag in multiple redirects.

<figure><img src="../../../.gitbook/assets/image (147).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

That was the flag.&#x20;
