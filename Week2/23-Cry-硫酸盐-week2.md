![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1731767422452-0b6cd3d8-8d8f-40c7-9f65-03d837fd3eb9.png)



Find the Lattice

题目如下  


```plain
from Crypto.Util.number import getPrime, inverse, bytes_to_long
import random
import math

FLAG = b'crypto{?????????????????????}'


def gen_key():
    q = getPrime(512)
    upper_bound = int(math.sqrt(q // 2))
    lower_bound = int(math.sqrt(q // 4))
    f = random.randint(2, upper_bound)
    while True:
        g = random.randint(lower_bound, upper_bound)
        if math.gcd(f, g) == 1:
            break
    h = (inverse(f, q)*g) % q
    return (q, h), (f, g)


def encrypt(q, h, m):
    assert m < int(math.sqrt(q // 2))
    r = random.randint(2, int(math.sqrt(q // 2)))
    e = (r*h + m) % q
    return e


def decrypt(q, h, f, g, e):
    a = (f*e) % q
    m = (a*inverse(f, g)) % g
    return m


public, private = gen_key()
q, h = public
f, g = private

m = bytes_to_long(FLAG)
e = encrypt(q, h, m)

print(f'Public key: {(q,h)}')
print(f'Encrypted Flag: {e}')

"""
Public key: (7638232120454925879231554234011842347641017888219021175304217358715878636183252433454896490677496516149889316745664606749499241420160898019203925115292257, 2163268902194560093843693572170199707501787797497998463462129592239973581462651622978282637513865274199374452805292639586264791317439029535926401109074800)
Encrypted Flag: 5605696495253720664142881956908624307570671858477482119657436163663663844731169035682344974286379049123733356009125671924280312532755241162267269123486523
"""
```



由题目，已知的量有q，h，e，在gen_key()中得知h = (inverse(f, q)*g) % q，即

h = ((f ⁻¹ mod q) * g) mod q

h = (f ⁻¹ * g) mod q

h * f = g mod q

h * f + k * q = g （k是任意整数)

h * f + k * q + f = g + f

构造格

[k  f] * [q  0]

                        [h  1] = [g  f]



解题脚本如下

```plain
from Crypto.Util.number import *

q = 7638232120454925879231554234011842347641017888219021175304217358715878636183252433454896490677496516149889316745664606749499241420160898019203925115292257
h = 2163268902194560093843693572170199707501787797497998463462129592239973581462651622978282637513865274199374452805292639586264791317439029535926401109074800
e = 5605696495253720664142881956908624307570671858477482119657436163663663844731169035682344974286379049123733356009125671924280312532755241162267269123486523

def decrypt(q, h, f, g, e):
    a = (f*e) % q
    m = (a*inverse(f, g)) % g
    return m

"""
# sage
mat = [[q,0],[h,1]]
M = matrix(ZZ,mat)
g,f = M.LLL()[0]
"""

g = 43997957885147078115851147456370880089696256470389782348293341937915504254589
f = 47251817614431369468151088301948722761694622606220578981561236563325808178756

m = decrypt(q, h, f, g, e)
print(long_to_bytes(m))
```



![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1731768935340-e16d0923-a36c-4487-bc6e-df4ffcdda409.png)

