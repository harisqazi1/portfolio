# Mind your Ps and Qs

![](<../../../.gitbook/assets/image (82).png>)

In the file, I can see 3 values:

```c
Decrypt my super sick RSA:
c: 62324783949134119159408816513334912534343517300880137691662780895409992760262021
n: 1280678415822214057864524798453297819181910621573945477544758171055968245116423923
e: 65537
```

My assumption is that I will have to decode it using the RSA algorithm. After multiple online decoders, I realized that the value of **e** given was too small. I then used the python code from [this site](https://github.com/Dvd848/CTFs/blob/master/2021\_picoCTF/Mind\_your\_Ps\_and\_Qs.md) in order to solve the problem:

{% hint style="warning" %}
Under maintenance
{% endhint %}
