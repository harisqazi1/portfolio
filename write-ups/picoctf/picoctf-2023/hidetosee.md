# HideToSee

<figure><img src="../../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

I ran `steghide --extract -sf atbash.jpg`. This then prompted me for a password. After I hit the endter key with no password, it output a string into a file:

<figure><img src="../../../.gitbook/assets/image (29) (2).png" alt=""><figcaption></figcaption></figure>

Since the file as showing an image of the atbash cipher, and the file is also names atbash, I believe this is the flag encoded by atbash. Using an online atbash decoder, we are able to decode the string:

<figure><img src="../../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>
