# Mind your Ps and Qs

![](../../.gitbook/assets/image%20%2882%29.png)

In the file, I can see 3 values:

```c
Decrypt my super sick RSA:
c: 62324783949134119159408816513334912534343517300880137691662780895409992760262021
n: 1280678415822214057864524798453297819181910621573945477544758171055968245116423923
e: 65537   
```

My assumption is that I will have to decode it using the RSA algorithm. After multiple online decoders, I realized that the value of **e** given was too small. I then used the python code from [this site](https://github.com/Dvd848/CTFs/blob/master/2021_picoCTF/Mind_your_Ps_and_Qs.md) in order to solve the problem:

```c
from factordb.factordb import FactorDB
import gmpy2

c = 240986837130071017759137533082982207147971245672412893755780400885108149004760496
n = 831416828080417866340504968188990032810316193533653516022175784399720141076262857
e = 65537

f = FactorDB(n)
f.connect()
p, q = f.get_factor_list()
ph = (p-1)*(q-1)
d = gmpy2.invert(e, ph)
plaintext = pow(c, d, n)
print("Flag: {}".format(bytearray.fromhex(format(plaintext, 'x')).decode()))
```



