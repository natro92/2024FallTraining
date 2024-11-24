# 椭圆曲线
因为加了个笔记文件夹，所有以下只进行crypto hack上题目的分析，相关知识我放那个笔记文件夹里，下面放一个完成情况

![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1732155655521-f51fc1e8-9e51-48fb-a9d8-d6e3a88b1997.png)

## Background Reading
**题目**：

`The flag is the name we give groups with a commutative operation. `



**考点**：群论，阿贝尔群

题目简单介绍了椭圆曲线以及其上的点加法运算（没有放上来，就放一个最后问的的题目），然后问我们以何来命名可以交换的群。

需要我们对群论有一定的了解，我们就可以知道可交换群，我们把它叫做阿贝尔群（椭圆曲线上的点加法就构成一个阿贝尔群），所以答案呼之欲出，就是阿贝尔群。（不知道英文名字可以回去题目翻翻）

**flag**：crypto{Abelian}



## Point Negation
**题目**：

`E: Y2=X3 +497X+1768 mod 9739`

`Using the above curve, and the point P(8045,6936), find the point Q(x,y) such that P+Q=O.`



**考点**：椭圆曲线逆元计算

我们知道椭圆曲线上的点加法就是将两点（A，B两点）连成直线，找到该直线与曲线的交点（R'点），这个点（R'点）关于x轴的对称点（R点）就是点加法的结果，那么R'点就是R的逆元（因为这两个点的连线与曲线交在无穷远处，也就是我们规定的单位元）

所有求逆元很简单，只要把y坐标取负就行（因为关于x轴对称）

即R（x，y）		R'（x，-y）

但是我们是在有限域上去研究椭圆曲线，所有结果和上面多了一步取模

因为-y = p - y mod p，所以在有限域上的逆元表示成

R'（x，p - y）

回到题目就很简单了，P的逆元就可以求出

Q = （8045，9739 - 6936）= （8045，2803）

**flag**：crypto{8045,2803}



## Point Addition
**题目**：

`E: Y2=X3 +497X+1768 mod 9739`

`You can test your algorithm by asserting: X+Y=(1024,4440) and X+X=(7284,2107) for X=(5274,2841) and Y=(8669,740).`

`Using the above curve, and the points P=(493,5564),Q=(1539,4742),R=(4403,5202), find the point S(x,y)=P+P+Q+R by implementing the above algorithm.`

`After calculating S, substitute the coordinates into the curve. Assert that the point S is in E(Fp)`



**考点**：椭圆曲线上的点加法，点加法的算法实现

这里题目应该是期望我们自己写一个算法代码来实现椭圆曲线的点加法（给了简单的算法过程，也给了asserting来给我们验证算法的正确性）

虽然但是，我是直接用sagemath直接算的（流泪破忒头），所以只能放一个遇到题目的代码，试了一下，是可行的（人家函数定义什么的比我好多了，比手指）

```python
from Crypto.Util.number import inverse
from collections import namedtuple

# 定义点
Point = namedtuple("Point", "x y")

# 单位元
O = 'Origin'

# 确保点在椭圆曲线上
def check_point(P: tuple):
    if P == O:
        return True
    else:
        return (P.y**2 - (P.x**3 + a*P.x + b)) % p == 0 and 0 <= P.x < p and 0 <= P.y < p

# 求逆元
def point_inverse(P: tuple):
    if P == O:
        return P
    return Point(P.x, -P.y % p)

# 点加法
def point_addition(P: tuple, Q: tuple):
    # based of algo. in ICM
    if P == O:
        return Q
    elif Q == O:
        return P
    elif Q == point_inverse(P):
        return O
    else:
        if P == Q:
            lam = (3*P.x**2 + a)*inverse(2*P.y, p)
            lam %= p
        else:
            lam = (Q.y - P.y) * inverse((Q.x - P.x), p)
            lam %= p
    Rx = (lam**2 - P.x - Q.x) % p
    Ry = (lam*(P.x - Rx) - P.y) % p
    R = Point(Rx, Ry)
    assert check_point(R)
    return R

# 定义椭圆曲线
p = 9739
a = 497
b = 1768

P = Point(493,5564)
Q = Point(1539,4742)
R = Point(4403,5202)
A = point_addition(P,P)
B = point_addition(A,Q)
S = point_addition(B,R)
print(S)

# Point(x=4215, y=2162)
```

下面也放一个sagemath上的代码（看，多简单嘛，比手指）

```python
# sagemath
E = EllipticCurve(GF(9739),[497,1768])
P = E([493,5564])
Q = E([1539,4742])
R = E([4403,5202])
print(P+P+Q+R)

# (4215 : 2162 : 1)
```

flag：crypto{4215,2162}



## Scalar Multiplication
**题目**：

`E: Y2=X3 +497X+1768 mod 9739`

`You can test your algorithm by asserting: [1337]X=(1089,6931) for X=(5323,5438).`

`Using the above curve, and the points P=(2339,2213), find the point Q(x,y)=[7863]P by implementing the above algorithm.`

`After calculating Q, substitute the coordinates into the curve. Assert that the point Q is in E(Fp)`



**考点**：椭圆曲线的标量乘法，标量乘法的算法实现

同样是期望我们自己写一个算法代码来实现椭圆曲线的点加法，就是强调一点，标量乘法是得看成点加法的不断进行（因为是群，群内只有一种二元运算），属于是理解上的问题。

下面是python代码，同样是遇到的题目所给的代码

```python
from Crypto.Util.number import inverse
from collections import namedtuple

# 定义点
Point = namedtuple("Point", "x y")

# 单位元
O = 'Origin'

# 确保点在椭圆曲线上
def check_point(P: tuple):
    if P == O:
        return True
    else:
        return (P.y**2 - (P.x**3 + a*P.x + b)) % p == 0 and 0 <= P.x < p and 0 <= P.y < p

# 求逆元
def point_inverse(P: tuple):
    if P == O:
        return P
    return Point(P.x, -P.y % p)

# 点加法
def point_addition(P: tuple, Q: tuple):
    # based of algo. in ICM
    if P == O:
        return Q
    elif Q == O:
        return P
    elif Q == point_inverse(P):
        return O
    else:
        if P == Q:
            lam = (3*P.x**2 + a)*inverse(2*P.y, p)
            lam %= p
        else:
            lam = (Q.y - P.y) * inverse((Q.x - P.x), p)
            lam %= p
    Rx = (lam**2 - P.x - Q.x) % p
    Ry = (lam*(P.x - Rx) - P.y) % p
    R = Point(Rx, Ry)
    assert check_point(R)
    return R

# 标量乘法
def double_and_add(P: tuple, n: int):
    # based of algo. in ICM
    Q = P
    R = O
    while n > 0:
        if n % 2 == 1:
            R = point_addition(R, Q)
        Q = point_addition(Q, Q)
        n = n // 2
    assert check_point(R)
    return R
    
# 定义椭圆曲线
p = 9739
a = 497
b = 1768

P = Point(2339,2213)
Q = double_and_add(P,7863)
print(Q)
# Point(x=9467, y=2742)
```

同样放一个sagemath上的代码，会简单很多

```python
E = EllipticCurve(GF(9739),[497,1768])
P = E([2339,2213])
print(7863*P)
# (9467 : 2742 : 1)
```

**flag**：crypto{9467,2742}



## Curves and Logs
**题目**：

`E: Y2=X3 +497X+1768 mod 9739`

`Calculate the shared secret after Alice sends you QA=(815,3190), with your secret integer nB=1829.`

`Generate a key by calculating the SHA1 hash of the x coordinate (take the integer representation of the coordinate and cast it to a string). The flag is the hexdigest you find.`



**考点**：椭圆曲线离散对数问题，基于椭圆曲线的DH密钥交换，SHA1哈希值计算

这道题主要考察对基于椭圆曲线的DH密钥交换（ECDH）的理解。

Alice和Bob协商在椭圆曲线上找一个点G（公开），并用分别用自己随机生成的整数nA和nB与G做标量乘法得到QA（公开）和QB（公开），并把得到的值分别传给对方。

Alice得到QB并用自己一开始的那个随机数nA与QB做标量乘法，得到S，Bob同理。两人把得到的S作为共享密钥。

对于Alice：S = nA * （nB * G）

对于Bob：  S =  nB *（nA * G）

因为椭圆曲线是一个阿贝尔群，所以运算次序对结果不影响，所以两个人得到的S相同，这确保了交换的可行性

这个过程里面，外人只可能得到G、QA和QB（这三个是公开的），但是仅仅通过这三个是算不出nA和nB的（即算不出S），因为这相当于是一个离散对数问题，这确保了这个交换过程的安全性。

理解以上之后我们来看题目要求，题目告诉我们Alice给我们送来了QA，作为Bob的我们需要计算一下和Alice的共享密钥，所以很简单，把得到的QA和我们自己的随机整数做个标量乘法。falg是我们得到的S点的x轴坐标的SHA1哈希值的十六进制表示（这个我其实一开始也不懂，问了一下AI才知道怎么弄的，后面会去仔细了解的，流泪破忒头）。

下面是代码

```python
from Crypto.Util.number import inverse
from collections import namedtuple
import hashlib

# 定义点
Point = namedtuple("Point", "x y")

# 单位元
O = 'Origin'

# 确保点在椭圆曲线上
def check_point(P: tuple):
    if P == O:
        return True
    else:
        return (P.y**2 - (P.x**3 + a*P.x + b)) % p == 0 and 0 <= P.x < p and 0 <= P.y < p

# 求逆元
def point_inverse(P: tuple):
    if P == O:
        return P
    return Point(P.x, -P.y % p)

# 点加法
def point_addition(P: tuple, Q: tuple):
    # based of algo. in ICM
    if P == O:
        return Q
    elif Q == O:
        return P
    elif Q == point_inverse(P):
        return O
    else:
        if P == Q:
            lam = (3*P.x**2 + a)*inverse(2*P.y, p)
            lam %= p
        else:
            lam = (Q.y - P.y) * inverse((Q.x - P.x), p)
            lam %= p
    Rx = (lam**2 - P.x - Q.x) % p
    Ry = (lam*(P.x - Rx) - P.y) % p
    R = Point(Rx, Ry)
    assert check_point(R)
    return R

# 标量乘法
def double_and_add(P: tuple, n: int):
    # based of algo. in ICM
    Q = P
    R = O
    while n > 0:
        if n % 2 == 1:
            R = point_addition(R, Q)
        Q = point_addition(Q, Q)
        n = n // 2
    assert check_point(R)
    return R
    
# 定义椭圆曲线
p = 9739
a = 497
b = 1768

Q = Point(815, 3190)
S = double_and_add(Q, 1829)
print(S)
# Point(x=7929, y=707)

shared_secret=S[0]
sha1 = hashlib.sha1(str(shared_secret).encode()).hexdigest()
print(sha1 )
# 80e5212754a824d3a4aed185ace4f9cac0f908bf
```

同样放一个sagemath上的（可以通过python代码来理解原理，但真正用来解题还是sagemath吧，更简单）

```python
# sagemath
import hashlib
E = EllipticCurve(GF(9739),[497,1768])
P = E([815, 3190])
S= 1829*P

shared_secret=S[0]
sha1 = hashlib.sha1(str(shared_secret).encode()).hexdigest()
print(sha1 )
# 80e5212754a824d3a4aed185ace4f9cac0f908bf
```

**falg**：crypto{80e5212754a824d3a4aed185ace4f9cac0f908bf}



## Efficient Exchange
**题目**：

`E: Y2=X3 +497X+1768 mod 9739`

`Calculate the shared secret after Alice sends you x(QA)=4726, with your secret integer nB=6534.`

`Use the file to decode the flag decrypt.py`

`{'iv': 'cd9da9f1c60925922377ea952afc212c', 'encrypted_flag': 'febcbe3a3414a730b125931dccf912d2239f3e969c4334d95ed0ec86f6449ad8'}`

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


shared_secret = ?
iv = ?
ciphertext = ?

print(decrypt_flag(shared_secret, iv, ciphertext))

```



**考点**：二次剩余，AES解密，ECDH的高效传递

这里主要考察对ECDH的高效传递的理解，Alice和Bob认为，椭圆曲线方程已知，那干嘛还要传递y呀？（欸！我有一个好主意）于是他们觉得只传递x就可以了。于是Alice给我发了一个x值，希望我能够以此知道共享密钥。并且Alice用共享密钥通过AES加密了flag

事实上只传递x也确实是可行的，我们考虑把已知的x值代入曲线方程就可以得到	y2 = k mod p	的形式

其中k是带入x后的算出值，这就是一个二次剩余问题（数论的知识点），这样就是一个比较简单解决的问题（课本上这么说的，虽然在我看来还是很难），很容易就可以求出y

但这里会有一个小问题，那就是如果数对（x，y）满足上面那个式子，那么显然的（x，p - y）也是满足的，所以实际生活中，我们在传递x坐标时，会附带传递一个指定y的奇偶性的标志位，以此来达到区分到底是哪个点（感兴趣可以查一下，挺复杂的，没看懂）

不过这道题会相对简单，因为我们最后只用x来做文章，所以y的值我们不关心，那么对于同一个x，即使两个人最后算出来的y不一样（一个是y，一个是p-y），经过运算后也不会影响x的值（可能画个图会好理解一点，两个对称的点，画斜率，与曲线的交点的x还是一样的），所以我们任意求出一个y来进行计算就行

现在来进行y的计算，题目给了提示p = 3 mod 4，这就可以运用二次剩余的知识点来解决，下面是原理图

![](https://cdn.nlark.com/yuque/0/2024/jpeg/42554774/1732085889491-3c78c1ef-668a-4c38-af7c-ba3b30ef22a4.jpeg)

具体分析原因就不再这里展开，感兴趣可以去看我的笔记，这里直接用

![image](https://cdn.nlark.com/yuque/__latex/c189b7875b415a1cb0af87f81632096b.svg)

当然你也可以直接遍历p（遍历完总能找到一个解），因为这题p确实不大，两个办法都行，都可以求p（我一开始用的第二个，觉得不是预期解法才去查的第一个），求出y了就没什么难度了，套那个解密文件就行。

```python
# sage
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

p = 9739
y = int(((4726**3+497*4726+1768)**((p+1)//4))%p)
print(y)
E = EllipticCurve(GF(9739),[497,1768])
P = E([4726,6287])
shared_secret = (6534*P)[0]
print(shared_secret)

iv = 'cd9da9f1c60925922377ea952afc212c'
ciphertext = 'febcbe3a3414a730b125931dccf912d2239f3e969c4334d95ed0ec86f6449ad8'

print(decrypt_flag(shared_secret, iv, ciphertext))
 
# crypto{3ff1c1ent_k3y_3xch4ng3}
```

**flag**：crypto{3ff1c1ent_k3y_3xch4ng3}

