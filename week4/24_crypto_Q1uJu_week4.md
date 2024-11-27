<meta name="referrer" content="no-referrer">

# Week4 WriteUp

## L1

### 题目源代码

```python
from Crypto.Util.number import *
from gmpy2 import *

flag = b'******'
flag = bytes_to_long(flag)

p = getPrime(1024)
r = getPrime(175)
a = inverse(r, p)
a = (a*flag) % p

print(f'a = {a}')
print(f'p = {p}')
'''
a = 79047880584807269054505204752966875903807058486141783766561521134845058071995038638934174701175782152417081883728635655442964823110171015637136681101856684888576194849310180873104729087883030291173114003115983405311162152717385429179852150760696213217464522070759438318396222163013306629318041233934326478247
p = 90596199661954314748094754376367411728681431234103196427120607507149461190520498120433570647077910673128371876546100672985278698226714483847201363857703757534255187784953078548908192496602029047268538065300238964884068500561488409356401505220814317044301436585177722826939067622852763442884505234084274439591
'''
```

### WriteUp

#### 解题思路

最近刚好新学了格密码，题名L1的L也是格(Lattices)的缩写，题目形式也很像，用格密码解题来解决本题。

已知a和q，且p为一个1024bit的素数，r是一个175bit的未知素数。

由第九行和第十行代码，可以得到如下等式
$$
a\equiv{r^{-1}*flag}\ (mod\ p) \\
flag\equiv{a*r}\ (mod\ p) \\
flag=a*r+k*p,\ k\in{Z}
$$
a和p已知，构造格
$$
\begin{pmatrix}
k & r
\end{pmatrix}
*
\begin{pmatrix}
p & 0\\
a & 1
\end{pmatrix}
=
\begin{pmatrix}
flag & r
\end{pmatrix}
$$

#### EXP

```python
# env: SageMath 9.3
from Crypto.Util.number import long_to_bytes

# 已知参数
a = 79047880584807269054505204752966875903807058486141783766561521134845058071995038638934174701175782152417081883728635655442964823110171015637136681101856684888576194849310180873104729087883030291173114003115983405311162152717385429179852150760696213217464522070759438318396222163013306629318041233934326478247
p = 90596199661954314748094754376367411728681431234103196427120607507149461190520498120433570647077910673128371876546100672985278698226714483847201363857703757534255187784953078548908192496602029047268538065300238964884068500561488409356401505220814317044301436585177722826939067622852763442884505234084274439591
# flag = a * r + k * p
# 构造格矩阵
Lattice = Matrix(ZZ, [[p, 0],
                      [a, 1]])
# print(Lattice.LLL())# 使用 LLL 算法进行格基约简
flag = abs(Lattice.LLL()[0][0])# 提取短向量 abs()返回数字的绝对值
print(long_to_bytes(flag))
# b'NSSCTF{e572546b-abb5-4358-8970-471abc12b7ef}'
```

#### flag

NSSCTF{e572546b-abb5-4358-8970-471abc12b7ef}

## L2

### 题目源代码

```python
from Crypto.Util.number import *
from gmpy2 import *

flag = b'******'
m = bytes_to_long(flag)

assert m.bit_length() == 351
p = getPrime(1024)
b = getPrime(1024)
c = getPrime(400)

a = (b*m + c) % p

print(f'a = {a}')
print(f'b = {b}')
print(f'p = {p}')

'''
a = 92716521851427599147343828266552451834533034815416003395170301819889384044273026852184291232938197215198124164263722270347104189412921224361134013717269051168246275213624264313794650441268405062046423740836145678559969020294978939553573428334198212792931759368218132978344815862506799287082760307048309578592
b = 155530728639099361922541063573602659584927544589739208888076194504495146661257751801481540924821292656785953391450218803112838556107960071792826902126414012831375547340056667753587086997958522683688746248661290255381342148052513971774612583235459904652002495564523557637169529882928308821019659377248151898663
p = 100910862834849216140965884888425432690937357792742349763319405418823395997406883138893618605587754336982681610768197845792843123785451070312818388494074168909379627989079148880913190854232917854414913847526564520719350308494462584771237445179797367179905414074344416047541423116739621805238556845903951985783
'''
```

### WriteUp

#### 解题思路

同L1，用格密码解。

已知a,b和p，p和b为1024bit的素数，c为400bit的素数，m长度为351位。

由代码中已知等式信息可得到如下等式
$$
a\equiv b*m+c(mod\ p)\\
a=b*m+c+k*p\\
c=k*p-b*m+a
$$
a,b,q已知，构造格
$$
\begin{pmatrix}
k & m & 1
\end{pmatrix}
*
\begin{pmatrix}
p & 0 & 0\\
-b & 1 & 0\\
a & 0 & 1
\end{pmatrix}
=
\begin{pmatrix}
c & m & 1
\end{pmatrix}
$$
此时发现结果基不平衡，m和c的位数分别为351和400，通过$Hermite$定理验证配平

$Hermite$定理

每一个$n$维的lattice $L$，都包含一个非零向量$v\in{L}$满足
$$
||v||\le \sqrt{n}\ det(L)^{1/n}
$$
代码实现如下

```python
import gmpy2
from Crypto.Util.number import *

bit_try = 2 ** 180   # 这是不断调整配平用的
f = flag = getPrime(351)
b = 155530728639099361922541063573602659584927544589739208888076194504495146661257751801481540924821292656785953391450218803112838556107960071792826902126414012831375547340056667753587086997958522683688746248661290255381342148052513971774612583235459904652002495564523557637169529882928308821019659377248151898663
p = 100910862834849216140965884888425432690937357792742349763319405418823395997406883138893618605587754336982681610768197845792843123785451070312818388494074168909379627989079148880913190854232917854414913847526564520719350308494462584771237445179797367179905414074344416047541423116739621805238556845903951985783
c = getPrime(400)

# 行列式维数
n = 3
det = p * bit_try

# 行列式的值 大于等于下面的即可
temp = gmpy2.iroot(n, 2)[0] * gmpy2.iroot(det, n)[0]
print(temp.bit_length())

# 最短向量的值
temp2 = gmpy2.iroot(f**2+c**2+bit_try**2, 2)[0]
print(temp2.bit_length())

if temp.bit_length() >= temp2.bit_length():
    print("true")
else:
    print("false")
'''
402
400
True
'''
```

调整格子
$$
\begin{pmatrix}
k & m & 1
\end{pmatrix}
*
\begin{pmatrix}
p & 0 & 0\\
-b & 1 & 0\\
a & 0 & 2^{180}
\end{pmatrix}
=
\begin{pmatrix}
c & m & 2^{180}
\end{pmatrix}
$$


#### EXP

```python
# env: SageMath 9.3
from Crypto.Util.number import *

# 已知参数
a = 92716521851427599147343828266552451834533034815416003395170301819889384044273026852184291232938197215198124164263722270347104189412921224361134013717269051168246275213624264313794650441268405062046423740836145678559969020294978939553573428334198212792931759368218132978344815862506799287082760307048309578592
b = 155530728639099361922541063573602659584927544589739208888076194504495146661257751801481540924821292656785953391450218803112838556107960071792826902126414012831375547340056667753587086997958522683688746248661290255381342148052513971774612583235459904652002495564523557637169529882928308821019659377248151898663
p = 100910862834849216140965884888425432690937357792742349763319405418823395997406883138893618605587754336982681610768197845792843123785451070312818388494074168909379627989079148880913190854232917854414913847526564520719350308494462584771237445179797367179905414074344416047541423116739621805238556845903951985783
# c = k * p - b * m + a
# 构造格矩阵
Lattice = Matrix(ZZ, [[p, 0, 0],
                      [-b, 1, 0],
                      [a, 0, 2**180]])
# print(Lattice.LLL())# 使用 LLL 算法进行格基约简
m = abs(Lattice.LLL()[0][1])   # 提取短向量 abs()返回数字的绝对值
print(long_to_bytes(m))
# b'NSSCTF{ee5cb1a5-257a-48b0-9d62-9ef56ff0651a}'
```

#### flag

NSSCTF{ee5cb1a5-257a-48b0-9d62-9ef56ff0651a}

## E1

### 题目源代码

```python
from Crypto.Util.number import getPrime
from libnum import s2n
from secret import flag

p = getPrime(256)
a = getPrime(256)
b = getPrime(256)
E = EllipticCurve(GF(p), [a, b])
m = E.random_point()
G = E.random_point()
k = getPrime(256)
K = k * G
r = getPrime(256)
c1 = m + r * K
c2 = r * G
cipher_left = s2n(flag[:len(flag)//2]) * m[0]
cipher_right = s2n(flag[len(flag)//2:]) * m[1]

print(f"p = {p}")
print(f"a = {a}")
print(f"b = {b}")
print(f"k = {k}")
print(f"E = {E}")
print(f"c1 = {c1}")
print(f"c2 = {c2}")
print(f"cipher_left = {cipher_left}")
print(f"cipher_right = {cipher_right}")
'''
p = 74997021559434065975272431626618720725838473091721936616560359000648651891507
a = 61739043730332859978236469007948666997510544212362386629062032094925353519657
b = 87821782818477817609882526316479721490919815013668096771992360002467657827319
k = 93653874272176107584459982058527081604083871182797816204772644509623271061231
E = Elliptic Curve defined by y^2 = x^3 + 61739043730332859978236469007948666997510544212362386629062032094925353519657*x + 12824761259043751634610094689861000765081341921946160155432001001819005935812 over Finite Field of size 74997021559434065975272431626618720725838473091721936616560359000648651891507
c1 = (14455613666211899576018835165132438102011988264607146511938249744871964946084 : 25506582570581289714612640493258299813803157561796247330693768146763035791942 : 1)
c2 = (37554871162619456709183509122673929636457622251880199235054734523782483869931 : 71392055540616736539267960989304287083629288530398474590782366384873814477806 : 1)
cipher_left = 68208062402162616009217039034331142786282678107650228761709584478779998734710
cipher_right = 27453988545002384546706933590432585006240439443312571008791835203660152890619

'''
```

### WriteUp

#### 解题思路

代码中很明显可以看出来这是一道椭圆曲线加密的题目，其中m和G是两个E上生成的随机点，k和r是两个随机生成的256bit的素数，已知p，a，b，可知椭圆曲线方程
$$
E:Y^2=X^3+aX+b\ mod p
$$
由已知等式
$$
K=k*G\\
c1=m+r*K\\
c2=r*G
$$
k，c1，c2均已知，可如下推导出m的解
$$
c1=m+r*K=m+r*k*G\\
r*k*G=c2*k\\
m=c1-k*c2
$$
得到m后，由代码

```python
cipher_left = s2n(flag[:len(flag)//2]) * m[0]
cipher_right = s2n(flag[len(flag)//2:]) * m[1]
```

可得flag的前半部分和后半部分分别为`cipher_left//m[0]`和`cipher_right//m[1]`，将其拼在一起得到flag。

#### EXP

```python
# env: SageMath 9.3
from Crypto.Util.number import *

cipher_left = 68208062402162616009217039034331142786282678107650228761709584478779998734710
cipher_right = 27453988545002384546706933590432585006240439443312571008791835203660152890619
p = 74997021559434065975272431626618720725838473091721936616560359000648651891507
a = 61739043730332859978236469007948666997510544212362386629062032094925353519657
b = 87821782818477817609882526316479721490919815013668096771992360002467657827319
E = EllipticCurve(GF(p), [a, b])
k = 93653874272176107584459982058527081604083871182797816204772644509623271061231
c1 = E(14455613666211899576018835165132438102011988264607146511938249744871964946084, 25506582570581289714612640493258299813803157561796247330693768146763035791942)
c2 = E(37554871162619456709183509122673929636457622251880199235054734523782483869931, 71392055540616736539267960989304287083629288530398474590782366384873814477806)
m = c1 - c2 * k
flag0 = cipher_left // m[0]
flag1 = cipher_right // m[1]
print(flag0, flag1)
print(long_to_bytes(int(flag0)) + long_to_bytes(int(flag1)))
# b'hgame{Ecc$is!sO@HaRd}'
```

#### flag

hgame{Ecc$is!sO@HaRd}

## E2

### 题目源代码

```python
# coding: utf-8
#!/usr/bin/env python2

import gmpy2
import random
import binascii
from hashlib import sha256
from sympy import nextprime
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
from Crypto.Util.number import long_to_bytes
from FLAG import flag
#flag = 'wdflag{123}'


def victory_encrypt(plaintext, key):
    key = key.upper()
    key_length = len(key)
    plaintext = plaintext.upper()
    ciphertext = ''

    for i, char in enumerate(plaintext):
        if char.isalpha():
            shift = ord(key[i % key_length]) - ord('A')
            encrypted_char = chr(
                (ord(char) - ord('A') + shift) % 26 + ord('A'))
            ciphertext += encrypted_char
        else:
            ciphertext += char

    return ciphertext


victory_key = "WANGDINGCUP"
victory_encrypted_flag = victory_encrypt(flag, victory_key)

p = 0xfffffffffffffffffffffffffffffffffffffffffffffffffffffffefffffc2f
a = 0
b = 7
xG = 0x79be667ef9dcbbac55a06295ce870b07029bfcdb2dce28d959f2815b16f81798
yG = 0x483ada7726a3c4655da4fbfc0e1108a8fd17b448a68554199c47d08ffb10d4b8
G = (xG, yG)
n = 0xfffffffffffffffffffffffffffffffebaaedce6af48a03bbfd25e8cd0364141
h = 1
zero = (0, 0)

dA = nextprime(random.randint(0, n))

if dA > n:
    print("warning!!")


def addition(t1, t2):
    if t1 == zero:
        return t2
    if t2 == zero:
        return t2
    (m1, n1) = t1
    (m2, n2) = t2
    if m1 == m2:
        if n1 == 0 or n1 != n2:
            return zero
        else:
            k = (3 * m1 * m1 + a) % p * gmpy2.invert(2 * n1, p) % p
    else:
        k = (n2 - n1 + p) % p * gmpy2.invert((m2 - m1 + p) % p, p) % p
    m3 = (k * k % p - m1 - m2 + p * 2) % p
    n3 = (k * (m1 - m3) % p - n1 + p) % p
    return (int(m3), int(n3))


def multiplication(x, k):
    ans = zero
    t = 1
    while(t <= k):
        if (k & t) > 0:
            ans = addition(ans, x)
        x = addition(x, x)
        t <<= 1
    return ans


def getrs(z, k):
    (xp, yp) = P
    r = xp
    s = (z + r * dA % n) % n * gmpy2.invert(k, n) % n
    return r, s


z1 = random.randint(0, p)
z2 = random.randint(0, p)
k = random.randint(0, n)
P = multiplication(G, k)
hA = multiplication(G, dA)
r1, s1 = getrs(z1, k)
r2, s2 = getrs(z2, k)

print("r1 = {}".format(r1))
print("r2 = {}".format(r2))
print("s1 = {}".format(s1))
print("s2 = {}".format(s2))
print("z1 = {}".format(z1))
print("z2 = {}".format(z2))

key = sha256(long_to_bytes(dA)).digest()
cipher = AES.new(key, AES.MODE_CBC)
iv = cipher.iv
encrypted_flag = cipher.encrypt(
    pad(victory_encrypted_flag.encode(), AES.block_size))
encrypted_flag_hex = binascii.hexlify(iv + encrypted_flag).decode('utf-8')

print("Encrypted flag (AES in CBC mode, hex):", encrypted_flag_hex)

# output
# r1 = 111817653331957669294460466848850458804857945556928458406600106150268654577388
# r2 = 111817653331957669294460466848850458804857945556928458406600106150268654577388
# s1 = 86614391420642776223990568523561232627667766343605236785504627521619587526774
# s2 = 99777373725561160499828739472284705447694429465579067222876023876942075279416
# z1 = 96525870193778873849147733081234547336150390817999790407096946391065286856874
# z2 = 80138688082399628724400273131729065525373481983222188646486307533062536927379
# ('Encrypted flag (AES in CBC mode, hex):', u'6c201c3c4e8b0a2cdd0eca11e7101d45d7b33147d27ad1b9d57e3d1e20c7b3c2e36b8da3142dfd5abe335a604ce7018878b9f157099211a7bbda56ef5285ec0b')
```

### WriteUp

首先感谢Eleven师傅出的网鼎杯青龙组真题，考点很多，受教了~

#### 解题思路

这道题考点一共有下面三个：

1. 维吉尼亚加密
   - victory_encrypt实现了一个简单的类维吉尼亚加密，它以密钥`WANGDINGCUP`对flag逐个字母进行了移位加密
2. 椭圆曲线数字签名
   - 定义了椭圆曲线参数（`p`,`a`,`b`,`G`,`n`等）
   - 随机生成了一个私钥`dA`，并使用它计算了曲线上的点相乘的结果`hA`
   - 利用椭圆曲线的点加法和标量乘法实现了签名过程，生成了签名参数`r1`,`s1`,`r2`,`s2`
3. AES-CBC加密
   - 生成了一个对称加密的密钥（从私钥`dA`的SHA256哈希值派生）
   - 使用AES-CBC模式加密了维吉尼亚加密后的密文`victory_encrypted_flag`

解密过程就是先进行AES-CBC解密，再对解密得到的信息进行维吉尼亚解密。

**过程分析**

可以观察到两次椭圆曲线数字签名生成的`r1`和`r2`相同，即使用的随机数`k`一致，这是一种常见的ECDSA实现漏洞，可以通过已知的`z1`,`z2`,`r1`,`s1`,`s2`来恢复私钥`dA`。

从`getrs`函数中可以得到签名的关系式
$$
r=P_x=(k\cdot G)_x\\
s=(z+r\cdot dA)\cdot k^{-1}\ (mod\ n)
$$
其中`z`和`k`是有限域内的随机整数，`dA`是签名者的私钥，`n`是椭圆曲线的阶，确保结果在有限域内。

由于`k`固定，`r1`和`r2`相等，两次签名的关系如下：
$$
s_1\cdot{k}\equiv z_1+r_1\cdot d_A\ (mod\ n)\\
s_2\cdot{k}\equiv z_2+r_2\cdot d_A\ (mod\ n)\equiv z_2+r_1\cdot d_A\ (mod\ n)
$$

由上下式做差可以解出`k`，再借助`k`可以求出私钥`dA`
$$
k=(z_2-z_1)*(s_2-s_1)^{-1}\ mod\ n\\
dA=(s_1\cdot k-z_1)*r_1^{-1}\ mod\ n=(s_2\cdot k-z_2)*r_1^{-1}\ mod\ n
$$
有了`dA`后，将`dA`转为字节并通过hash算法sha256得到AES的`key`

常规AES解密流程即可得到AES的明文段，最后是一个类维吉尼亚加密，密钥`WANGDINGBEI`也已给出，将其解密即可得到flag

#### EXP

```python
# env: python 3.12.4
from gmpy2 import invert
from hashlib import sha256
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad
import binascii


# 已知值
z1 = 96525870193778873849147733081234547336150390817999790407096946391065286856874
z2 = 80138688082399628724400273131729065525373481983222188646486307533062536927379
s1 = 86614391420642776223990568523561232627667766343605236785504627521619587526774
s2 = 99777373725561160499828739472284705447694429465579067222876023876942075279416
r1 = 111817653331957669294460466848850458804857945556928458406600106150268654577388
n = 0xfffffffffffffffffffffffffffffffebaaedce6af48a03bbfd25e8cd0364141

# 计算 k
k = ((z1 - z2) * invert(s1 - s2, n)) % n
print(f"k = {k}")

# 计算 dA
dA = ((s1 * k - z1) * invert(r1, n)) % n
print(f"dA = {dA}")

# 将私钥转换为对称密钥
key = sha256(long_to_bytes(dA)).digest()
print(f"Key = {binascii.hexlify(key).decode()}")

# 已知加密的 flag 数据
encrypted_flag_hex = "6c201c3c4e8b0a2cdd0eca11e7101d45d7b33147d27ad1b9d57e3d1e20c7b3c2e36b8da3142dfd5abe335a604ce7018878b9f157099211a7bbda56ef5285ec0b"
encrypted_flag = binascii.unhexlify(encrypted_flag_hex)

# 拆分 IV 和加密数据
iv = encrypted_flag[:AES.block_size]
ciphertext = encrypted_flag[AES.block_size:]

# AES解密
cipher = AES.new(key, AES.MODE_CBC, iv)
decrypted_flag = unpad(cipher.decrypt(ciphertext), AES.block_size)

print(f"AES Decrypted flag = {decrypted_flag.decode()}")

# Victory Decrypt
def victory_decrypt(ciphertext, key):
    key = key.upper()
    key_length = len(key)
    ciphertext = ciphertext.upper()
    plaintext = ''

    for i, char in enumerate(ciphertext):
        if char.isalpha():
            shift = ord(key[i % key_length]) - ord('A')
            decrypted_char = chr(
                (ord(char) - ord('A') - shift) % 26 + ord('A'))
            plaintext += decrypted_char
        else:
            plaintext += char

    return plaintext

victory_key = "WANGDINGCUP"
print(f"Victory Decrypted flag = {victory_decrypt(decrypted_flag.decode(), victory_key)}")
'''
k = 41166710682323407984109541250081400981796577119230772899908337474502175902010
dA = 69405496326001412913236615261048994463471549259512015521599373390407938295479
Key = 2705e1d0d384bae39910b30594594f10c8823c92a0904cd634457cec346c3d67
AES Decrypted flag = SDSRDO{58UT00432L8228R9E3G927HDWS8D67G2}
Victory Decrypted flag = WDFLAG{58AE00432D8228C9E3A927BBCD8D67D2}
'''
```

#### flag

WDFLAG{58AE00432D8228C9E3A927BBCD8D67D2}