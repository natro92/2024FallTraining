<h1 id="EVawP">Cryptohack-LATTICES</h1>
<h2 id="ltk0a">LATTICES</h2>
<h3 id="NSYXj">1 Vectors</h3>
在某个域![image](https://cdn.nlark.com/yuque/__latex/7aaf2781990aa336d909f7ebd32e2f69.svg)![image](https://cdn.nlark.com/yuque/__latex/c27929eb227b5bf6b840f886f1608415.svg)![image](https://cdn.nlark.com/yuque/__latex/9f493997c33913987175caf4a4849955.svg)是用两个二元运算符定义的集合。

对于一个向量![image](https://cdn.nlark.com/yuque/__latex/bd76aa89a34eec3ef918d32fff708027.svg)![image](https://cdn.nlark.com/yuque/__latex/1a4411060cb2885ca41fa1edef866dd4.svg)![image](https://cdn.nlark.com/yuque/__latex/fe1485b32149a85d8d86dfe19e63719c.svg)![image](https://cdn.nlark.com/yuque/__latex/87bbc36e907e26e551823e0e4e009535.svg)![image](https://cdn.nlark.com/yuque/__latex/2a5d790aebc64df0acc7b00fb2e6b2f5.svg)![image](https://cdn.nlark.com/yuque/__latex/92cb65553872c9e62201ffa767f470ae.svg)![image](https://cdn.nlark.com/yuque/__latex/2773f8bf16fe98eb742ba158e525bbcd.svg)![image](https://cdn.nlark.com/yuque/__latex/af036c12b33890c86d0a839c28946292.svg)![image](https://cdn.nlark.com/yuque/__latex/741fdd5f2064ebfcbadc233c02bee9c4.svg)![image](https://cdn.nlark.com/yuque/__latex/92cb65553872c9e62201ffa767f470ae.svg)![image](https://cdn.nlark.com/yuque/__latex/7e76c30e588807f1c188eeeedcb0988a.svg).

考虑实数域上的二维向量空间，一个向量![image](https://cdn.nlark.com/yuque/__latex/bd76aa89a34eec3ef918d32fff708027.svg)![image](https://cdn.nlark.com/yuque/__latex/ff521cc6678e313a1262814245134f86.svg)![image](https://cdn.nlark.com/yuque/__latex/7a003fd2c4931e3753e3126efe410bfc.svg)![image](https://cdn.nlark.com/yuque/__latex/92cb65553872c9e62201ffa767f470ae.svg)![image](https://cdn.nlark.com/yuque/__latex/353a8628dc9de7c1e6aa9d24430e687f.svg)![image](https://cdn.nlark.com/yuque/__latex/d543f05f962fd80146a8f49fc1e194f6.svg)![image](https://cdn.nlark.com/yuque/__latex/4e200a31e64d3f194191f3fa2026778f.svg)![image](https://cdn.nlark.com/yuque/__latex/63c67fd2aea3d99821b6865a185f061a.svg)![image](https://cdn.nlark.com/yuque/__latex/d9bd49c35f326caeac5787e7610ef7c7.svg).

还可以定义内积（也称为点积），它取两个向量并返回一个标量。从形式上讲，我们认为：对于![image](https://cdn.nlark.com/yuque/__latex/741fdd5f2064ebfcbadc233c02bee9c4.svg)![image](https://cdn.nlark.com/yuque/__latex/92cb65553872c9e62201ffa767f470ae.svg)![image](https://cdn.nlark.com/yuque/__latex/6ed2f1f3ccd531d9dbfbc31321bb8698.svg)![image](https://cdn.nlark.com/yuque/__latex/2dde94714f92b8a04e7ef08c2483e572.svg)![image](https://cdn.nlark.com/yuque/__latex/4c467da85a250769e4623b8cc41c681a.svg).

题目：

给出以下三个向量`v = (2,6,3)`，`w = (1,0,0)`，`u = (7,7,2)`，计算`3*(2*v - w) · 2*u`。

考察：基本的向量与标量之间的运算

直接计算或使用SageMath计算：

```plain
v = vector([2,6,3])
w = vector([1,0,0])
u = vector([7,7,2])
3*(2*v - w)*2*u
# 702
```

<h3 id="OTERW">2 Size and Basis</h3>
一组向量![image](https://cdn.nlark.com/yuque/__latex/50d688421c9a2f8ad2e38e65e59d2012.svg)是线性无关的，当且仅当

![image](https://cdn.nlark.com/yuque/__latex/8b7df9d6185eda078aebd644d29ac927.svg)

仅在![image](https://cdn.nlark.com/yuque/__latex/51b8ad6a62fe4a44eccc50b7f3c93597.svg)时成立

基是一组线性独立的向量![image](https://cdn.nlark.com/yuque/__latex/34ab041e12ce8876aee60d913474c5d4.svg)![image](https://cdn.nlark.com/yuque/__latex/bad26a890068f4821dae78adb7e1281b.svg)![image](https://cdn.nlark.com/yuque/__latex/26f57414235c49c1f60fd1a52e50c330.svg)可以写成：

![image](https://cdn.nlark.com/yuque/__latex/723ac08c7132d8f3604449dd074891a0.svg)

`基中的元素数量`是`向量空间的维数`

向量的大小，定义为`||v||`，向量自己和自己做内积：![image](https://cdn.nlark.com/yuque/__latex/4299ed8dc0b338d3c39a4d69c532d852.svg)

一组`正交基(orthogonal)`是指一组向量基![image](https://cdn.nlark.com/yuque/__latex/34ab041e12ce8876aee60d913474c5d4.svg)![image](https://cdn.nlark.com/yuque/__latex/62ce71a5194b595acb1388a587569381.svg)![image](https://cdn.nlark.com/yuque/__latex/7c002cad49fddf55ae041b2f25600977.svg).

一组`标准正交基(orthonormal)`是指一组正交基，对于所有![image](https://cdn.nlark.com/yuque/__latex/2443fbcfeb7e85e1d62b6f5e4f27207e.svg)![image](https://cdn.nlark.com/yuque/__latex/ad6f0ca8b0f9e5f77feb85b115ff2da7.svg)![image](https://cdn.nlark.com/yuque/__latex/f38232e74283833fbdac50d2e0fbf6c5.svg).

题目：

给出一个向量`v=(4,6,2,5)`，计算它的大小。

考察：求向量的大小或者说向量的模

直接计算或使用SageMath计算：

```plain
v = vector([4,6,2,5])
norm(v)
# 9
```

<h3 id="Y2khY">3 Gram Schmidt</h3>
给出一组向量空间的基![image](https://cdn.nlark.com/yuque/__latex/34ab041e12ce8876aee60d913474c5d4.svg)![image](https://cdn.nlark.com/yuque/__latex/bec09c54b3ce1bc3e776c3d6f81854ea.svg)![image](https://cdn.nlark.com/yuque/__latex/f35fcc40180e4eae25a7831d83f36c94.svg).

在Jeffrey Hoffstein、Jill Pipher、Joseph H.Silverman所著的《数学密码学导论》中，格拉姆-施密特算法如下：

> 格拉姆-施密特算法
>
> ![image](https://cdn.nlark.com/yuque/__latex/88064748c77660277046b00136c3e5a9.svg)
>
> Loop ![image](https://cdn.nlark.com/yuque/__latex/2443fbcfeb7e85e1d62b6f5e4f27207e.svg) = 2,3...,n
>
>     Compute ![image](https://cdn.nlark.com/yuque/__latex/deb2ba02ec861b30c4b3e09545ca2650.svg).
>
>     Set ![image](https://cdn.nlark.com/yuque/__latex/09d59ea56fd5c0e0ab91c8ef8edaa893.svg)![image](https://cdn.nlark.com/yuque/__latex/0f1231fc681b70b58eecc86414a9b617.svg)![image](https://cdn.nlark.com/yuque/__latex/036441a335dd85c838f76d63a3db2363.svg)![image](https://cdn.nlark.com/yuque/__latex/cf080bb827fec523d15c45647f2c3992.svg)![image](https://cdn.nlark.com/yuque/__latex/c8523dac018ed8882ef4b62d42f1cd01.svg))
>
> End Loop
>

题目：

给出以下一组基向量：

![image](https://cdn.nlark.com/yuque/__latex/6d546452ade9ccb49f316df2444204cb.svg)

使用格拉姆-施密特算法计算正交基。flag是![image](https://cdn.nlark.com/yuque/__latex/f10654576eabd74eeacf4f484708e776.svg)的第二个成员的浮点数值，保留小数点后五位。

考察：格拉姆-施密特算法计算正交基

使用SageMath计算：

```plain
v0 = vector([4,1,3,-1])
v1 = vector([2,1,-3,4])
v2 = vector([1,0,-2,7])
v3 = vector([6,2,9,-5])
M = Matrix([v0,v1,v2,v3])
M_GS = M.gram_schmidt()
round(M_GS[0][3][1],5)
# 0.91611
```

贴一个Python的Solution：

```python
import numpy as np
v = [
    np.array([4,1,3,-1]),
    np.array([2,1,-3,4]),
    np.array([1,0,-2,7]),
    np.array([6,2,9,-5])
]

"""
u1 = v1
Loop i = 2,3...,n
   Compute μij = vi ∙ uj / ||uj||2, 1 ≤ j < i.
   Set ui = vi - μij * uj (Sum over j for 1 ≤ j < i)
End Loop
"""

u = [v[0]]
for vi in v[1:]:
    mi = [np.dot(vi, uj) / np.dot(uj,uj) for uj in u]
    u += [vi - sum([mij * uj for (mij, uj) in zip(mi,u)])]

print(round(u[3][1], 5))
```

<h3 id="Bm7Nw">4 What's a Lattice?</h3>
给出一组线性无关的向量![image](https://cdn.nlark.com/yuque/__latex/f13df0dd0919a5d2320ecf81d9bad285.svg)![image](https://cdn.nlark.com/yuque/__latex/4349d803db10acbaba1277adf93b14b8.svg)![image](https://cdn.nlark.com/yuque/__latex/357fd1b324f53cd83804458f8e87b7fc.svg)![image](https://cdn.nlark.com/yuque/__latex/7730bc0b879e5c33abe31514a71c6bbe.svg)![image](https://cdn.nlark.com/yuque/__latex/c895173d3be4872abf206be4268a58cb.svg)![image](https://cdn.nlark.com/yuque/__latex/385152937a9a710a5eca1b12c1e07246.svg)![image](https://cdn.nlark.com/yuque/__latex/357fd1b324f53cd83804458f8e87b7fc.svg)和对应整系数的组合。

![image](https://cdn.nlark.com/yuque/__latex/3b6443fabc6fb776d415211732329c56.svg)

格![image](https://cdn.nlark.com/yuque/__latex/c895173d3be4872abf206be4268a58cb.svg)![image](https://cdn.nlark.com/yuque/__latex/6b6e7e94f2cab303f1489eb08edd4f12.svg)![image](https://cdn.nlark.com/yuque/__latex/393c9d8c00912297293bd58bae17ac79.svg)![image](https://cdn.nlark.com/yuque/__latex/55594b170f29da9dac13eb7cafd6dff1.svg)![image](https://cdn.nlark.com/yuque/__latex/2a6278724b813cac1c188d8aa2fb1e67.svg)生成的二维格。

![](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241104210111547.png)

使用基向量，我们可以用基向量的整数倍乘来到达格内的任意一点。基向量还定义了基本域：

![image](https://cdn.nlark.com/yuque/__latex/78d6efb5a49cb0198b0490eb141ffe46.svg)

举一个二维的例子，基本域是由边![image](https://cdn.nlark.com/yuque/__latex/91a8139c06c08372c8948b7db768f53a.svg)![image](https://cdn.nlark.com/yuque/__latex/55594b170f29da9dac13eb7cafd6dff1.svg)![image](https://cdn.nlark.com/yuque/__latex/308df52f26bb292e4c093fa6f85bc137.svg)构成的平行四边形。

我们可以通过基向量计算基本域的体积。例如，取一个以![image](https://cdn.nlark.com/yuque/__latex/29adee2a969920cd22db6c334cfc5f49.svg)![image](https://cdn.nlark.com/yuque/__latex/55bfe62c381719d8efeab147e92b6114.svg)![image](https://cdn.nlark.com/yuque/__latex/de951302f41d4707b9d80ca1af34dd0f.svg)![image](https://cdn.nlark.com/yuque/__latex/cba9165a90def72e70dd26a8e8a66c1f.svg)![image](https://cdn.nlark.com/yuque/__latex/77def391f9779dcf539c8e6438cf0cbf.svg)![image](https://cdn.nlark.com/yuque/__latex/dd4ce905dd73a1f9ece948514f4e5695.svg)![image](https://cdn.nlark.com/yuque/__latex/de951302f41d4707b9d80ca1af34dd0f.svg)![image](https://cdn.nlark.com/yuque/__latex/86a38a27f67bf8450a78c79395f46eee.svg)![image](https://cdn.nlark.com/yuque/__latex/3f7baf3275b74ed07e32d69f28458e08.svg).

题目：

计算由基向量![image](https://cdn.nlark.com/yuque/__latex/a4c8baf0453350ea290e9e980c5f84e2.svg)构成的基本域的体积。

考察：基向量构成的基本域的体积

```plain
v = vector
v1 = v([6,2,-3])
v2 = v([5,1,4])
v3 = v([2,7,1])
A = matrix([v1,v2,v3])
det(A)
```

<h3 id="UhAkM">5 Gaussian Reduction</h3>
如果你仔细观察，格密码开始在密码学中无处不在。有时，它们通过操纵加密系统出现，破坏了不够安全地生成的参数。最著名的例子是Coppersmith对RSA密码学的攻击。

格密码也可用于构建加密协议，其安全性基于两个基本的困难问题：

**最短向量问题（SVP）**：在格L中找到最短的非零向量。换句话说，在![image](https://cdn.nlark.com/yuque/__latex/1ded757481d92730b071dc05e5c34ff6.svg)![image](https://cdn.nlark.com/yuque/__latex/740b9b190c9c5551c5f37ab875aca298.svg)![image](https://cdn.nlark.com/yuque/__latex/293a3a2ac75ff6235bc12dff851b7292.svg)取最小值。

**最近向量问题（CVP）**：给定一个不在L中的向量![image](https://cdn.nlark.com/yuque/__latex/6bb26cea58333ccec9596c08e5cbc0f2.svg)![image](https://cdn.nlark.com/yuque/__latex/2979b8d34cdc5266d6ca527f61aa2d9b.svg)![image](https://cdn.nlark.com/yuque/__latex/c9b08ae6d9fed72562880f75720531bc.svg)![image](https://cdn.nlark.com/yuque/__latex/4e18d7e6f3c40c46dfa7be7b98ed7a14.svg)![image](https://cdn.nlark.com/yuque/__latex/1ded757481d92730b071dc05e5c34ff6.svg)![image](https://cdn.nlark.com/yuque/__latex/cf9ce3c2f45ef18bf99d0302fb534904.svg)![image](https://cdn.nlark.com/yuque/__latex/1ded757481d92730b071dc05e5c34ff6.svg)![image](https://cdn.nlark.com/yuque/__latex/566a16212c40fd7b9a389dca9b15c490.svg)![image](https://cdn.nlark.com/yuque/__latex/c42ca5af044b8db5f4a7c358e90d339e.svg)取最小值。

对于一般格来说，SVP很难，但对于足够简单的情况，有有效的算法来计算SVP的解或近似值。当格的维数为4或更小时，我们可以在多项式时间内精确计算；对于更高的维度，我们不得不接受一个近似值。

高斯开发了他的算法，为给定任意基的二维格找到最优基。此外，该算法的输出![image](https://cdn.nlark.com/yuque/__latex/d75b72964639f9b1f76c46198a46c47f.svg)![image](https://cdn.nlark.com/yuque/__latex/cacb240434eec866b72ab3f995aea68b.svg)![image](https://cdn.nlark.com/yuque/__latex/c895173d3be4872abf206be4268a58cb.svg)中最短的非零向量，因此求解了SVP。

> ps:对于更高维度，有一种称为LLL算法的格基约减算法，以Lenstra、Lenstra和Lovász的名字命名。如果你经常玩CTF，你就一定会知道。LLL算法在多项式时间内运行。不过，就目前而言，让我们停留在二维空间。
>

高斯的算法大致通过从另一个基向量中减去一个基向量的倍数来工作，直到不再可能使它们更小。由于这是在二维中工作的，因此很容易可视化。以下是Jeffrey Hoffstein、Jill Pipher和Joseph H.Silverman在《数学密码学导论》中对算法的描述：

> 高斯格约化算法
>
> Loop
>
>     (a) If ![image](https://cdn.nlark.com/yuque/__latex/78b3019f777ed765b8eab6f271cf6bbc.svg)![image](https://cdn.nlark.com/yuque/__latex/bbee8a19fe3ae0ef1ea2fbea80c7dae2.svg)![image](https://cdn.nlark.com/yuque/__latex/2a6278724b813cac1c188d8aa2fb1e67.svg)
>
>     (b) Compute ![image](https://cdn.nlark.com/yuque/__latex/c1f4edb3625a50fea593fe120f2f3e8e.svg)
>
>     (c) If ![image](https://cdn.nlark.com/yuque/__latex/9437f896b8b33348628614a568accefc.svg)![image](https://cdn.nlark.com/yuque/__latex/385b2e994620463087b58d66c1d3fae5.svg)![image](https://cdn.nlark.com/yuque/__latex/2a6278724b813cac1c188d8aa2fb1e67.svg)
>
>     (d) ![image](https://cdn.nlark.com/yuque/__latex/04be7eb35b3fbc29899b7040c01d40b4.svg)
>
> Continue Loop
>

请注意，与欧几里德的GCD算法在“交换”和“缩减”步骤上的相似性，我们必须对浮点数进行四舍五入，因为在格上，我们可能只使用整数作为基向量的系数。

题目：

取两个向量![image](https://cdn.nlark.com/yuque/__latex/571db36bfca6f5a5f06e3e30f8fac022.svg)，通过高斯算法，找到最优基，flag为新的基向量的内积。

考察：高斯格约化算法求最优基

```plain
v = vector([846835985, 9834798552])
u = vector([87502093, 123094980])
v1,v2 = u,v
m = 1
while m != 0:
    if norm(v2) < norm(v1):
        print('swap')
        v1,v2 = v2,v1
    m = int((v1*v2)/(v1*v1))
    v2 = v2 - m*v1
print(v1, v2)
print(v1*v2)
```

<h3 id="zJbsy">6 Find the Lattice</h3>
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

