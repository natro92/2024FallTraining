# CRYPTOHACK-ELLIPTIC CURVES
## BACKGROUND
椭圆曲线在公钥密码学中的应用最早于1985年提出，在抵御了数十年的攻击后，从2005年左右开始被广泛使用。椭圆曲线密码学（ECC）是一种非对称密码协议，与RSA和Diffie-Hellman（DH）一样，安全性依赖于单向陷门函数的难度。但是ECC有一个优势就是其操作流程并不像RSA和DH的乘法、取数字幂一样在平时生活中经常出现。

### 椭圆曲线基本介绍
椭圆曲线的**基本形式**如下

![](https://cdn.nlark.com/yuque/__latex/6915579a96ee79cc9ea3b6509eeb4ab0.svg)

它有一个很重要的特性是“点加”，这种运算在某条曲线上取两个点，并在原曲线上产生第三个点。取椭圆曲线上的点集后，点加法可以定义一个阿贝尔（Abelian）群运算。

从几何上讲，点加法就是取一个椭圆曲线并沿曲线标记两点![](https://cdn.nlark.com/yuque/__latex/ffd1905f6d4d60accedfa6b91be93ea9.svg)，![](https://cdn.nlark.com/yuque/__latex/4ef7132d0df72d9e3db76f6391960a3d.svg)及其![](https://cdn.nlark.com/yuque/__latex/712ecf7894348e92d8779c3ee87eeeb0.svg)，![](https://cdn.nlark.com/yuque/__latex/bf98c0ddcbe9c1e535f767c78c3aa813.svg)坐标，画一条穿过两个点的直线。现在延长这条线使其与椭圆曲线第三次相交，标记这个新的交点![](https://cdn.nlark.com/yuque/__latex/dd1caa3f2e1582dab2cf9bfdb21b7556.svg)。最后取![](https://cdn.nlark.com/yuque/__latex/dd1caa3f2e1582dab2cf9bfdb21b7556.svg)关于y轴对称，得到点![](https://cdn.nlark.com/yuque/__latex/218e4e95aac30adc09caaf9a08b70d50.svg)

![](https://cdn.nlark.com/yuque/__latex/5da8fae3e28265a22130d557580db556.svg)

如果没有第三个交点时，我们认为这条线与一个位于无穷远处的垂直线末端的一个点![](https://cdn.nlark.com/yuque/__latex/a946bfe2d39be198589a4de189e0f670.svg)相交，所以椭圆曲线的点加法是在2D空间中定义的，其中一个附加点位于无穷远处。

下面展示一个理解这些不同情况的视觉辅助图

![](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241118164305997.png)

点![](https://cdn.nlark.com/yuque/__latex/a946bfe2d39be198589a4de189e0f670.svg)充当组中的恒等算子

![](https://cdn.nlark.com/yuque/__latex/42c448cfd43e644c4d37be0f08193cb1.svg)

**定义**：椭圆曲线E是Weierstrass方程的解集

![](https://cdn.nlark.com/yuque/__latex/6915579a96ee79cc9ea3b6509eeb4ab0.svg)

与无穷远处的一个点![](https://cdn.nlark.com/yuque/__latex/a946bfe2d39be198589a4de189e0f670.svg)。参数![](https://cdn.nlark.com/yuque/__latex/26fdbf8e53cb0e48da5f4ddd4aaf5a5c.svg),![](https://cdn.nlark.com/yuque/__latex/d29c2e5f4926e5b0e9a95305650f6e54.svg)必须满足下述关系

![](https://cdn.nlark.com/yuque/__latex/266b4b4d8fbaffb46235d541b29bf493.svg)

以确保曲线上没有奇点。

形式上，设![](https://cdn.nlark.com/yuque/__latex/321138a59e6eab0c97c21f05282a80a6.svg)为椭圆曲线，点加法具有以下性质

> (a) P+O=O+P=P
>
> (b) P+(-P)=O
>
> (c) (P+Q)+R=P+(Q+R)
>
> (d) P+Q=Q+P
>

在ECC中，我们研究有限域![](https://cdn.nlark.com/yuque/__latex/2289da1c86fb8362509135e96803bc02.svg)上的椭圆曲线。这意味着我们观察曲线对特征![](https://cdn.nlark.com/yuque/__latex/d4cd21d60552e207f237e82def9029b6.svg)的模，椭圆曲线将不再是曲线，而是一组![](https://cdn.nlark.com/yuque/__latex/5963837d99a8fcf1d9c5f7807a096f47.svg)坐标为![](https://cdn.nlark.com/yuque/__latex/2289da1c86fb8362509135e96803bc02.svg)中整数的点。

### 椭圆曲线的离散对数问题
给定一个椭圆曲线![](https://cdn.nlark.com/yuque/__latex/321138a59e6eab0c97c21f05282a80a6.svg)，![](https://cdn.nlark.com/yuque/__latex/321138a59e6eab0c97c21f05282a80a6.svg)上的一点![](https://cdn.nlark.com/yuque/__latex/ffd1905f6d4d60accedfa6b91be93ea9.svg)经过了![](https://cdn.nlark.com/yuque/__latex/56c1b0cb7a48ccf9520b0adb3c8cb2e8.svg)次点加法后得到![image](https://cdn.nlark.com/yuque/__latex/1553dae3cc5c15cddb4f5b5a367b0aba.svg)，现已知![](https://cdn.nlark.com/yuque/__latex/a9d8e7493ba3c1770ad1d78b01b8c21c.svg)，求出![image](https://cdn.nlark.com/yuque/__latex/56c1b0cb7a48ccf9520b0adb3c8cb2e8.svg)

这是椭圆曲线密码学主要解决的一个困难数学问题。

> ps: 这段是目前个人的理解
>
> 其中在实际应用场景中，![image](https://cdn.nlark.com/yuque/__latex/1553dae3cc5c15cddb4f5b5a367b0aba.svg)为公钥，![image](https://cdn.nlark.com/yuque/__latex/ffd1905f6d4d60accedfa6b91be93ea9.svg)为一个随机生成点（应该是？），![image](https://cdn.nlark.com/yuque/__latex/56c1b0cb7a48ccf9520b0adb3c8cb2e8.svg)为所需求的私钥
>

### Background Reading
#### 题目
性质![image](https://cdn.nlark.com/yuque/__latex/c3775e4800cb1a5b160985845315c7b3.svg)表明点加法是可交换的。flag是给具有交换运算的群起的名字。

#### flag
crypto{Abelian}

## STARTER
### Point Negation
在密码学环境中应用椭圆曲线，需要研究在有限域![image](https://cdn.nlark.com/yuque/__latex/2289da1c86fb8362509135e96803bc02.svg)中具有坐标的椭圆曲线。

仍然可以认为椭圆曲线的形式为![image](https://cdn.nlark.com/yuque/__latex/d13d84a2d52ce2a3ad72d2d9cdd9f156.svg)，并满足以下条件：![image](https://cdn.nlark.com/yuque/__latex/d0e8a63d95be149921385891bbfeee83.svg)

但不再将椭圆曲线看作一个几何对象，而是一组被如下定义的点

![image](https://cdn.nlark.com/yuque/__latex/5c6d5d58296750f2a8fcc4eb0ccbc6bf.svg)

当把![image](https://cdn.nlark.com/yuque/__latex/321138a59e6eab0c97c21f05282a80a6.svg)放入有限域后，椭圆曲线就不再是一条光滑曲线，而是一些不连续的点。

![](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241118181201117.png)

图来自[一个可绘制椭圆曲线的网站](https://andrea.corbellini.name/ecc/interactive/modk-add.html)

#### 题目
使用椭圆曲线

![image](https://cdn.nlark.com/yuque/__latex/82b46db47da5d259a75f96ed120acc9b.svg)

使用该曲线和点![image](https://cdn.nlark.com/yuque/__latex/0c77dd51656ad3521c3ef9b114fc65a2.svg)，找到点![image](https://cdn.nlark.com/yuque/__latex/9291c98793027f924d519191814fb23d.svg)使得![image](https://cdn.nlark.com/yuque/__latex/841f0d6ad341591d5fa39d5dca3cf354.svg)

hint：在有限域中工作，需要正确处理负数。

#### WriteUp
![](https://cdn.nlark.com/yuque/__latex/841f0d6ad341591d5fa39d5dca3cf354.svg)，可知![](https://cdn.nlark.com/yuque/__latex/4ef7132d0df72d9e3db76f6391960a3d.svg)的![](https://cdn.nlark.com/yuque/__latex/712ecf7894348e92d8779c3ee87eeeb0.svg)为8045，![](https://cdn.nlark.com/yuque/__latex/bf98c0ddcbe9c1e535f767c78c3aa813.svg)为-6939，由提示处理负数，将-6939对9739取模得到2803。

#### flag
crypto{8045,2803}

### Point Addition
在椭圆曲线密码学的使用过程中，经常需要用到点加法，本题考查的就是点加法。

题目给出了如下实现算法

> **两点的点加法算法：**![](https://cdn.nlark.com/yuque/__latex/1a6e58161705e6317b8cd54d96e13a40.svg)
>
> (a) 如果![](https://cdn.nlark.com/yuque/__latex/9fc50fef19e65d3668107d5b4d70003d.svg)，那么![](https://cdn.nlark.com/yuque/__latex/033e0cb0e624116b8ac16a69e9fc6232.svg)
>
> (b) 否则，如果![](https://cdn.nlark.com/yuque/__latex/877bf505150ff3e3be26d1be5a3e0201.svg)，那么![](https://cdn.nlark.com/yuque/__latex/70f52501a6c486f9ca9303202601f5e7.svg)
>
> (c) 否则，记![](https://cdn.nlark.com/yuque/__latex/592c4815258d677657b61c64bcaa756f.svg)
>
> (d) 如果![](https://cdn.nlark.com/yuque/__latex/24ac71930b9317f2ac76dc6bdd4ff5a9.svg)，那么![](https://cdn.nlark.com/yuque/__latex/841f0d6ad341591d5fa39d5dca3cf354.svg)
>
> (e) 否则：
>
> (e1) 如果![](https://cdn.nlark.com/yuque/__latex/7db3d59aac539aae8654949cf6bc617e.svg)
>
> (e2) 如果![](https://cdn.nlark.com/yuque/__latex/06821c39e39b03fbf726815a43206b2f.svg)
>
> (f) ![](https://cdn.nlark.com/yuque/__latex/1751d943a4b4191ecbfb732aceaabbdf.svg)
>
> (g) ![](https://cdn.nlark.com/yuque/__latex/95e8081ae4b77420a011aa7187569a6b.svg)
>
> (h) ![](https://cdn.nlark.com/yuque/__latex/1423457c509f211613a5bceedcb05ae7.svg)
>

#### 题目
使用以下椭圆曲线和素数：

![](https://cdn.nlark.com/yuque/__latex/82b46db47da5d259a75f96ed120acc9b.svg)

> 可以通过以下等式来测试算法正确性：![](https://cdn.nlark.com/yuque/__latex/d7a8b3928f640ce760dbb5b2540ab279.svg)
>

使用该曲线和点![](https://cdn.nlark.com/yuque/__latex/b6be852ba958d386d4107f7e39619c5f.svg)，通过实现上述算法找到点![](https://cdn.nlark.com/yuque/__latex/517575dac15081d19d2a7ff5a913a0e3.svg)

> 在计算完![](https://cdn.nlark.com/yuque/__latex/55fc237afbe535f7d8434985b848a6a7.svg)后，将坐标代入曲线以确保点![](https://cdn.nlark.com/yuque/__latex/55fc237afbe535f7d8434985b848a6a7.svg)在![](https://cdn.nlark.com/yuque/__latex/59496f18e8d7960318929ac8ac17abfa.svg)上
>

#### WriteUp
根据所给出的算法流程构建`addition(p1: Point, p2: Point, a, b)`函数，并测试算法正确性之后计算点加和，最后确定![](https://cdn.nlark.com/yuque/__latex/55fc237afbe535f7d8434985b848a6a7.svg)在![](https://cdn.nlark.com/yuque/__latex/59496f18e8d7960318929ac8ac17abfa.svg)上。

#### EXP&flag
```python
from sage.all import GF

class Point:
    def __init__(self, x, y, p):
        F = GF(p) # 定义有限域
        # 让x,y在有限域内运算
        self.x = F(x)
        self.y = F(y)
        self.modulus = p # 模数
        
def addition(p1: Point, p2: Point, a, b):
    x1 = p1.x
    y1 = p1.y
    x2 = p2.x
    y2 = p2.y
    if x1 == x2 and y1 == y2: # 如果P,Q重合
        d = (3*x1**2 + a) / (2*y1)
    else: # P,Q为不同两点
        d = (y2 - y1) / (x2 - x1)
    x = d**2 - x1 - x2
    y = d*(x1 - x) - y1
    return Point(x, y, p1.modulus)

X = Point(5274, 2841, 9739)
Y = Point(8669, 740, 9739)
XY = Point(1024, 4440, 9739)
XX = Point(7284, 2107, 9739)
# 确定addition函数正确性
assert addition(X, Y, 497, 1768).x == XY.x and addition(X, Y, 497, 1768).y == XY.y
assert addition(X, X, 497, 1768).x == XX.x and addition(X, X, 497, 1768).y == XX.y
P = Point(493, 5564, 9739)
Q = Point(1539, 4742, 9739)
R = Point(4403, 5202, 9739)
x1 = addition(P, P, 497, 1768)
x2 = addition(x1, Q, 497, 1768)
S = addition(x2, R, 497, 1768)
# 确定所求点S在椭圆曲线E上
assert S.y**2 == x3.x**3 + 497*S.x + 1768
print('crypto{'+str(S.x)+','+str(S.y)+'}')
# crypto{4215,2162}
```

### Scalar Multiplication
两点的标量乘法由重复加法定义：![](https://cdn.nlark.com/yuque/__latex/21ef8cf78c81eaffc73a65e792f16a93.svg)

本题也给出了如下实现算法

> **标量乘法的二重加法算法**
>
> 输入：![](https://cdn.nlark.com/yuque/__latex/269a6b7fcf9e0065815017d4fdb581ff.svg)和一个整数![](https://cdn.nlark.com/yuque/__latex/1e08bbdd41f0ec72b310f7b8d0673b25.svg)
>
> 输出：![](https://cdn.nlark.com/yuque/__latex/535926ca544d11a3c7d4b8cfc828539e.svg)
>
> 1. 设![](https://cdn.nlark.com/yuque/__latex/9faa6f10971e95359ec2b660899d422d.svg)
> 2. while循环 n > 0
> 3. 如果![](https://cdn.nlark.com/yuque/__latex/b5d81293ce1841cdcb4ab58b38717308.svg)，设![](https://cdn.nlark.com/yuque/__latex/1d29473f7b51df4e7f2a61ffdda2ea88.svg)
> 4. 设![](https://cdn.nlark.com/yuque/__latex/5446ed96669137534506553976fb937a.svg)
> 5. 如果![](https://cdn.nlark.com/yuque/__latex/1e08bbdd41f0ec72b310f7b8d0673b25.svg)，继续第二步循环
> 6. 返回点![](https://cdn.nlark.com/yuque/__latex/58f174354f8b28abc98969927d94c7f1.svg)
>

#### 题目
使用以下椭圆曲线和素数：

![](https://cdn.nlark.com/yuque/__latex/82b46db47da5d259a75f96ed120acc9b.svg)

> 可以通过以下等式来测试算法准确性：
>
> ![](https://cdn.nlark.com/yuque/__latex/5f0058648f9ab852425fdad4b22ea056.svg)
>

使用该曲线和点![](https://cdn.nlark.com/yuque/__latex/957862bd685f87bafe8b2cc349d28643.svg)，通过实现上述算法找到点![](https://cdn.nlark.com/yuque/__latex/698e80cebb23a3e3acc33562f4111751.svg)

> 在计算完![](https://cdn.nlark.com/yuque/__latex/4ef7132d0df72d9e3db76f6391960a3d.svg)后，将坐标代入曲线以确保点![](https://cdn.nlark.com/yuque/__latex/4ef7132d0df72d9e3db76f6391960a3d.svg)在![](https://cdn.nlark.com/yuque/__latex/59496f18e8d7960318929ac8ac17abfa.svg)上
>

#### WriteUp
同上题，根据所给出的算法流程构建`Scalar_Multiplication(p: Point, n, a, b)`函数，并测试算法正确性之后计算点![](https://cdn.nlark.com/yuque/__latex/4ef7132d0df72d9e3db76f6391960a3d.svg)，最后确定![](https://cdn.nlark.com/yuque/__latex/4ef7132d0df72d9e3db76f6391960a3d.svg)在![](https://cdn.nlark.com/yuque/__latex/59496f18e8d7960318929ac8ac17abfa.svg)上。

#### EXP&flag
```python
from sage.all import GF

class Point:
    def __init__(self, x, y, p):
        F = GF(p) # 定义有限域
        # 让x,y在有限域内运算
        self.x = F(x)
        self.y = F(y)
        self.modulus = p # 模数
        
def addition(p1: Point, p2: Point, a, b):
    x1 = p1.x
    y1 = p1.y
    x2 = p2.x
    y2 = p2.y
    if x1 == x2 and y1 == y2: # 如果P,Q重合
        d = (3*x1**2 + a) / (2*y1)
    else: # P,Q为不同两点
        d = (y2 - y1) / (x2 - x1)
    x = d**2 - x1 - x2
    y = d*(x1 - x) - y1
    return Point(x, y, p1.modulus)

def Scalar_Multiplication(p: Point, n, a, b):
    q = p
    r = 0
    while n > 0:
        if n % 2 == 1:
            try:
                r = addition(r, q, a, b)
            except:
                r = q
        q = addition(q, q, a, b)
        n = n // 2
    return r

# 确定Scalar_Multiplication函数正确性
X = Point(5323, 5438, 9739)
assert Scalar_Multiplication(X, 1337, 497, 1768).x == 1089
assert Scalar_Multiplication(X, 1337, 497, 1768).y == 6931
p = Point(2339, 2213, 9739)
Q = Scalar_Multiplication(p, 7863, 497, 1768)
# 确定点S在曲线上
assert Q.y**2 == Q.x**3 + 497*Q.x + 1768
print('crypto{' + str(Q.x) + ',' + str(Q.y) + '}')
# crypto{9467,2742}
```

### Curves and Logs
椭圆曲线离散对数问题（ECDLP）是找到一个整数![](https://cdn.nlark.com/yuque/__latex/df378375e7693bdcf9535661c023c02e.svg)，使得![](https://cdn.nlark.com/yuque/__latex/c93d4c9fb8f43b5f75e185d89f1aa7eb.svg)的问题，也是椭圆曲线密码学中一个核心问题。

Alice和Bob正在交谈，他们想创建一个共享密钥，这样他们就可以开始用一些对称的加密协议加密他们的消息。Alice和Bob不信任他们的连接，所以他们需要一种方法来创建其他人无法复制的密钥。

首先，Alice和Bob就曲线![](https://cdn.nlark.com/yuque/__latex/321138a59e6eab0c97c21f05282a80a6.svg)、素数![](https://cdn.nlark.com/yuque/__latex/d4cd21d60552e207f237e82def9029b6.svg)和生成点![](https://cdn.nlark.com/yuque/__latex/f8df64a4bfdeb9bdcbc357668b6fb123.svg)达成一致，生成素数阶![](https://cdn.nlark.com/yuque/__latex/34c7b563b30bde3c748139530686798e.svg)的子群![](https://cdn.nlark.com/yuque/__latex/3d8022f4e225d2804673d5988ea369bd.svg)



> 在椭圆曲线密码学中，![](https://cdn.nlark.com/yuque/__latex/f8df64a4bfdeb9bdcbc357668b6fb123.svg)的阶是素数是很重要的。构建安全曲线很复杂，建议使用预先构建的曲线，其中客户端会得到要使用的曲线、素数和生成器。
>

椭圆曲线Diffie-Hellman密钥交换过程如下：

+ Alice生成一个随机秘密整数![](https://cdn.nlark.com/yuque/__latex/6f5011d8226f21ce9f00804d8bb18efd.svg)并计算![](https://cdn.nlark.com/yuque/__latex/aceeb7446739114b43252070ced979ff.svg)
+ Bob生成一个随机秘密整数![](https://cdn.nlark.com/yuque/__latex/55edb6f83635d53d5475e93b605120d9.svg)并计算![](https://cdn.nlark.com/yuque/__latex/d082a3e0de2cae75b987db7cee8f56ec.svg)
+ Alice将![](https://cdn.nlark.com/yuque/__latex/1d6b573c5cee427fa483de482fce0e09.svg)发送给Bob，Bob将![](https://cdn.nlark.com/yuque/__latex/b59676b3246575fe91046496a22e2f44.svg)发送给Alice。由于ECDLP问题的难度，旁观者Eve无法在有效时间内计算![](https://cdn.nlark.com/yuque/__latex/ef9618d6fda249e412a84f8dcd19090a.svg)
+ Alice计算![](https://cdn.nlark.com/yuque/__latex/017e5dbe94db3fd589fc0ca860bf2bdd.svg)，Bob计算![](https://cdn.nlark.com/yuque/__latex/fcea90a36d329b36d2088e2f16aa0698.svg)
+ 由标量乘法的可结合性，![](https://cdn.nlark.com/yuque/__latex/27001ad7c2b2618a1c40c88560666d28.svg)
+ Alice和Bob能够使用![](https://cdn.nlark.com/yuque/__latex/55fc237afbe535f7d8434985b848a6a7.svg)作为他们的共享秘密

#### 题目
使用曲线、素数和生成器：

![](https://cdn.nlark.com/yuque/__latex/632e6dc50784914133d53141515e8844.svg)

当Alice发送给你![](https://cdn.nlark.com/yuque/__latex/75b5571be2530a18f9347b09c48c3abd.svg)后，用你的秘密整数![](https://cdn.nlark.com/yuque/__latex/2d76340644346e2ba3b6196be0879882.svg)计算共享秘密。

通过计算![](https://cdn.nlark.com/yuque/__latex/712ecf7894348e92d8779c3ee87eeeb0.svg)坐标的SHA1哈希生成密钥（取坐标的整数表示并将其转换为字符串）。flag就是你找到的十六进制摘要。

> 这条曲线并非不是加密安全的！！本题选择了一个小素数，以便在学习时保持一切快速。加密安全曲线使用比特大小≈256的素数
>

#### WriteUp
和DH类似，椭圆曲线DH密钥交换过程如下公式

![](https://cdn.nlark.com/yuque/__latex/07001b9c28705e306c0717b18c021f9d.svg)

题目已经给出![](https://cdn.nlark.com/yuque/__latex/1d6b573c5cee427fa483de482fce0e09.svg)和![](https://cdn.nlark.com/yuque/__latex/55edb6f83635d53d5475e93b605120d9.svg)，直接用前面的标量乘法函数求解![](https://cdn.nlark.com/yuque/__latex/1d7af5368c17853932a09a6ba891b9e4.svg)即可，解得![](https://cdn.nlark.com/yuque/__latex/55fc237afbe535f7d8434985b848a6a7.svg)后将![](https://cdn.nlark.com/yuque/__latex/ff19ce66179d022e787690c98ab7fd63.svg)转换为字符串并用SHA1生成十六进制摘要得到flag。

#### EXP&flag
```python
from sage.all import GF
import hashlib

class Point:
    def __init__(self, x, y, p):
        F = GF(p) # 定义有限域
        # 让x,y在有限域内运算
        self.x = F(x)
        self.y = F(y)
        self.modulus = p # 模数
        
def addition(p1: Point, p2: Point, a, b):
    x1 = p1.x
    y1 = p1.y
    x2 = p2.x
    y2 = p2.y
    if x1 == x2 and y1 == y2: # 如果P,Q重合
        d = (3*x1**2 + a) / (2*y1)
    else: # P,Q为不同两点
        d = (y2 - y1) / (x2 - x1)
    x = d**2 - x1 - x2
    y = d*(x1 - x) - y1
    return Point(x, y, p1.modulus)

def Scalar_Multiplication(p: Point, n, a, b):
    q = p
    r = 0
    while n > 0:
        if n % 2 == 1:
            try:
                r = addition(r, q, a, b)
            except:
                r = q
        q = addition(q, q, a, b)
        n = n // 2
    return r

qa = Point(815, 3190, 9739)
s = Scalar_Multiplication(qa, 1829, 497, 1768)
x_str = str(s.x)
hash_object = hashlib.sha1(x_str.encode())
sha1_hash = hash_object.hexdigest()
print('crypto{' + sha1_hash + '}')
# crypto{80e5212754a824d3a4aed185ace4f9cac0f908bf}
```

### Efficient Exchange
Alice和Bob正在研究椭圆曲线离散对数问题，并思考他们发送的数据。

他们希望尽可能保持数据传输的效率，并意识到不需要同时发送公钥的![](https://cdn.nlark.com/yuque/__latex/712ecf7894348e92d8779c3ee87eeeb0.svg)和![](https://cdn.nlark.com/yuque/__latex/bf98c0ddcbe9c1e535f767c78c3aa813.svg)坐标。

只要Alice和Bob在曲线参数上达成一致，对于给定的![](https://cdn.nlark.com/yuque/__latex/712ecf7894348e92d8779c3ee87eeeb0.svg)，![](https://cdn.nlark.com/yuque/__latex/bf98c0ddcbe9c1e535f767c78c3aa813.svg)只有两个可能的值。

事实上，给定他们接收到的![](https://cdn.nlark.com/yuque/__latex/712ecf7894348e92d8779c3ee87eeeb0.svg)值中允许的![](https://cdn.nlark.com/yuque/__latex/bf98c0ddcbe9c1e535f767c78c3aa813.svg)值，他们共享秘密的![](https://cdn.nlark.com/yuque/__latex/712ecf7894348e92d8779c3ee87eeeb0.svg)坐标将是相同的。

#### 题目
使用曲线、素数和生成器：

![](https://cdn.nlark.com/yuque/__latex/632e6dc50784914133d53141515e8844.svg)

在Alice发送给你![](https://cdn.nlark.com/yuque/__latex/ffe2239235f632fadf3665cdea09060e.svg)后用你的秘密整数![](https://cdn.nlark.com/yuque/__latex/99402bef5a74e0f9624ac90041b07adb.svg)计算出共享秘密值。

使用`decrypt.py`文件来解码flag

> {'iv': 'cd9da9f1c60925922377ea952afc212c', 'encrypted_flag': 'febcbe3a3414a730b125931dccf912d2239f3e969c4334d95ed0ec86f6449ad8'}
>

> 您可以通过只发送一个位来指定公共![](https://cdn.nlark.com/yuque/__latex/bf98c0ddcbe9c1e535f767c78c3aa813.svg)坐标取了两个可能值中的哪一个。试着想想如何做到这一点。这两个![](https://cdn.nlark.com/yuque/__latex/bf98c0ddcbe9c1e535f767c78c3aa813.svg)值是如何相互关联的？
>

#### WriteUp
由题前背景介绍中可以知道，在曲线参数一致，即在同一条椭圆曲线上，给定![](https://cdn.nlark.com/yuque/__latex/712ecf7894348e92d8779c3ee87eeeb0.svg)，![](https://cdn.nlark.com/yuque/__latex/bf98c0ddcbe9c1e535f767c78c3aa813.svg)值只有两个可能；题目中已经给出![](https://cdn.nlark.com/yuque/__latex/732d87b610d5fcf5b6f93afcb6564dc4.svg)，可以借助`sqrt_mod`函数来求解两个![](https://cdn.nlark.com/yuque/__latex/bf98c0ddcbe9c1e535f767c78c3aa813.svg)值，并可测试两个![](https://cdn.nlark.com/yuque/__latex/bf98c0ddcbe9c1e535f767c78c3aa813.svg)值取的不同点标量乘法求得的点的![](https://cdn.nlark.com/yuque/__latex/712ecf7894348e92d8779c3ee87eeeb0.svg)相同。求得![](https://cdn.nlark.com/yuque/__latex/712ecf7894348e92d8779c3ee87eeeb0.svg)值后用题目给出的`decrypt`函数求解得到flag。

#### EXP&flag
```python
from sage.all import GF
from sympy.ntheory import sqrt_mod
import hashlib

class Point:
    def __init__(self, x, y, p):
        F = GF(p) # 定义有限域
        # 让x,y在有限域内运算
        self.x = F(x)
        self.y = F(y)
        self.modulus = p # 模数
        
def addition(p1: Point, p2: Point, a, b):
    x1 = p1.x
    y1 = p1.y
    x2 = p2.x
    y2 = p2.y
    if x1 == x2 and y1 == y2: # 如果P,Q重合
        d = (3*x1**2 + a) / (2*y1)
    else: # P,Q为不同两点
        d = (y2 - y1) / (x2 - x1)
    x = d**2 - x1 - x2
    y = d*(x1 - x) - y1
    return Point(x, y, p1.modulus)

def Scalar_Multiplication(p: Point, n, a, b):
    q = p
    r = 0
    while n > 0:
        if n % 2 == 1:
            try:
                r = addition(r, q, a, b)
            except:
                r = q
        q = addition(q, q, a, b)
        n = n // 2
    return r

x = 4726
n_b = 6534

[y1,y2] = sqrt_mod(x**3 + 497*x + 1768, 9739, True)
p1 = Point(x, y1, 9739)
p2 = Point(x, y2, 9739)
a1 = Scalar_Multiplication(p1, n_b, 497, 1768)
a2 = Scalar_Multiplication(p2, n_b, 497, 1768)
print(a1.x, a1.y, a2.x, a2.y)
# 1791 7558 1791 2181
```

```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
import hashlib


def is_pkcs7_padded(message):
    padding = message[-message[-1]:]
    return all(padding[i] == len(padding) for i in range(0, len(padding)))


def decrypt_flag(shared_secret: int, iv: str, ciphertext: str):
    # Derive AES key from shared secret
    sha1 = hashlib.sha1()
    sha1.update(str(shared_secret).encode('ascii'))
    key = sha1.digest()[:16]
    # Decrypt flag
    ciphertext = bytes.fromhex(ciphertext)
    iv = bytes.fromhex(iv)
    cipher = AES.new(key, AES.MODE_CBC, iv)
    plaintext = cipher.decrypt(ciphertext)

    if is_pkcs7_padded(plaintext):
        return unpad(plaintext, 16).decode('ascii')
    else:
        return plaintext.decode('ascii')


shared_secret = 1791
iv = 'cd9da9f1c60925922377ea952afc212c'
ciphertext = 'febcbe3a3414a730b125931dccf912d2239f3e969c4334d95ed0ec86f6449ad8'

print(decrypt_flag(shared_secret, iv, ciphertext))
# crypto{3ff1c1ent_k3y_3xch4ng3}
```

## 附 解题截图
![](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241118173231996.png)

