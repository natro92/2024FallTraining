<h1 id="EVawP">Cryptohack-LATTICES</h1>

<h2 id="zJbsy">Find the Lattice</h2>

正如我们所看到的，格包含一些难题，这些难题可以形成密码系统的陷门函数。我们还发现，在密码分析中，格可以破坏最初似乎与格无关的密码协议。

这个挑战使用模块化算法来加密标志，但协议中隐藏着一个二维格。我们强烈建议花时间应对这一挑战，并找到如何用格子打破它。这是一个著名的例子，有很多可用的资源，但知道如何在系统中发现格通常是打破它的关键。

作为提示，您将能够使用前一个挑战的高斯缩减来完成这个挑战。

题目代码如下：

```python
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
```

题目输出：

> Public key: (7638232120454925879231554234011842347641017888219021175304217358715878636183252433454896490677496516149889316745664606749499241420160898019203925115292257, 2163268902194560093843693572170199707501787797497998463462129592239973581462651622978282637513865274199374452805292639586264791317439029535926401109074800)  
Encrypted Flag: 5605696495253720664142881956908624307570671858477482119657436163663663844731169035682344974286379049123733356009125671924280312532755241162267269123486523
>

已知q, h, e，其中

![image](https://cdn.nlark.com/yuque/__latex/6e1dffa9ebd2b4a7d31cad3cee9583ae.svg)

> q 512位
>
> f,g 低于256位
>
> h,e 512位
>
> r 256位
>
> m为flag，大小约为231bit
>

要求得m，需要获得`decrypt`的参数，还剩![image](https://cdn.nlark.com/yuque/__latex/18f3c2855f0e85a1ac2257f64d917144.svg)![image](https://cdn.nlark.com/yuque/__latex/55594b170f29da9dac13eb7cafd6dff1.svg)![image](https://cdn.nlark.com/yuque/__latex/7a1e6a754b7a8e45cb731688765c5e85.svg)是未知的，由(6)式中可知

![image](https://cdn.nlark.com/yuque/__latex/969542d79b5766373046047d46b8deeb.svg)

用上一题格的思想，构造一个由下面这个矩阵![image](https://cdn.nlark.com/yuque/__latex/6f5dde593f0bc27956e14b5eaec2ed17.svg)![image](https://cdn.nlark.com/yuque/__latex/bde8b8c66cca086df793129eb2e70246.svg)![image](https://cdn.nlark.com/yuque/__latex/0b5ea6aff0d3159cf2cd8e886e37bc5e.svg)所张成的Lattice：

![image](https://cdn.nlark.com/yuque/__latex/d173f3b3fe16d4ab341d6c40d3e776eb.svg)

向量![image](https://cdn.nlark.com/yuque/__latex/6348f6830dbfbb654db0b335a9b2d010.svg)![image](https://cdn.nlark.com/yuque/__latex/f8f3c54a01d4d6488ec7e5324ec4b8cb.svg)![image](https://cdn.nlark.com/yuque/__latex/8a92b607130b5be247860cc37d34eca7.svg)![image](https://cdn.nlark.com/yuque/__latex/94362838ca2410472d7364c61a4229ce.svg)![image](https://cdn.nlark.com/yuque/__latex/d33e86ee07ddb8d97d89f08e0361648e.svg)

![image](https://cdn.nlark.com/yuque/__latex/d2b4f725f2d2c9edfd8805cde763811e.svg)

即![image](https://cdn.nlark.com/yuque/__latex/c81a489eb0d655ceaf033a1d326b533b.svg)满足条件

向量![image](https://cdn.nlark.com/yuque/__latex/6348f6830dbfbb654db0b335a9b2d010.svg)![image](https://cdn.nlark.com/yuque/__latex/c584a5da9bf585fd5be912f13acb98bf.svg)![image](https://cdn.nlark.com/yuque/__latex/6f5dde593f0bc27956e14b5eaec2ed17.svg)![image](https://cdn.nlark.com/yuque/__latex/358c9412ab28558be2e3d4d1eef84309.svg)![image](https://cdn.nlark.com/yuque/__latex/33604ee711dc2490851f73504ebce189.svg)![image](https://cdn.nlark.com/yuque/__latex/4c2af1329d04951aa6933a2123f48aa0.svg)![image](https://cdn.nlark.com/yuque/__latex/6348f6830dbfbb654db0b335a9b2d010.svg)就在这个Lattice上。

对于两个基底向量，(1,h):512位,(0,q):512位

而向量![image](https://cdn.nlark.com/yuque/__latex/6348f6830dbfbb654db0b335a9b2d010.svg)的长度约为256位，相比于基底向量极小，很大概率为此Lattice的最短向量。

EXP(SageMath):

```python
from Crypto.Util.number import *

# Gauss Lattice Reduction
def Gauss(v1, v2):
    while True:
        if norm(v2) < norm(v1):
            v1, v2 = v2, v1
        m = round( v1*v2 / norm(v1)^2 )
        if m == 0:
            return (v1, v2)
        v2 = v2 - m*v1

def decrypt(q, h, f, g, e):
    a = (f*e) % q
    m = (a*inverse(f, g)) % g
    return m
        
q, h = (7638232120454925879231554234011842347641017888219021175304217358715878636183252433454896490677496516149889316745664606749499241420160898019203925115292257, 2163268902194560093843693572170199707501787797497998463462129592239973581462651622978282637513865274199374452805292639586264791317439029535926401109074800)
e = 5605696495253720664142881956908624307570671858477482119657436163663663844731169035682344974286379049123733356009125671924280312532755241162267269123486523
v = vector([1, h])
u = vector([0, q])
Gauss(v, u)
# ((47251817614431369468151088301948722761694622606220578981561236563325808178756, 43997957885147078115851147456370880089696256470389782348293341937915504254589),
# (-67269010250212717075432182693043963184097648880165008621907831284647116025901, 99012763459529858809608436133564630524350106000242070336818304053467942269178))
f, g = (47251817614431369468151088301948722761694622606220578981561236563325808178756, 43997957885147078115851147456370880089696256470389782348293341937915504254589)
m = decrypt(q, h, f, g, e)
print(long_to_bytes(m))
# b'crypto{Gauss_lattice_attack!}'
```

