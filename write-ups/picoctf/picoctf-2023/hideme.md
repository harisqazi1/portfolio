# hideme

<figure><img src="../../../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

This the the file we got:

<figure><img src="../../../.gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>

I do believe this is a steganograhy challenge. Running `strings` on the challenge, I saw something interesting at the end:

<figure><img src="../../../.gitbook/assets/image (142).png" alt=""><figcaption></figcaption></figure>

There might be a file hidden inside this file. I ran `foremost` on this file and extracted two files. The one that had the flag in it was the zip file. After unzipping that file, I was able to display the flag:

<figure><img src="../../../.gitbook/assets/image (143).png" alt=""><figcaption></figcaption></figure>
