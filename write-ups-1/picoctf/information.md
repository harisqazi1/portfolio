# information

![](../../.gitbook/assets/image%20%2868%29.png)

I downloaded the file and it was an image of a cat sitting on a laptop. I ran **strings** on it and got no usable information out of it. I ran **exiftool** on the file as well, and had no luck getting the flag. I even used **stegsolve.jar** and had no luck with it. I finally viewed this video: [https://www.youtube.com/watch?v=EwAv3p5C10A](https://www.youtube.com/watch?v=EwAv3p5C10A) where I found the solution. In order to find the solution, you have to take the License value in the metadata and then decode that in base 64.

![](../../.gitbook/assets/image%20%2865%29.png)

I then got the flag:

![](../../.gitbook/assets/image%20%2872%29.png)

