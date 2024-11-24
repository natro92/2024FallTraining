---

参考链接：

[【ECC加密算法】| ECC加密原理详解| 椭圆曲线加密| 密码学| 信息安全_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1v44y1b7Fd/?spm_id_from=333.337.search-card.all.click&vd_source=24312761fbf161db538ab5ea0eb572c6)

[sage之椭圆曲线-CSDN博客](https://blog.csdn.net/ckm1607011/article/details/106826116)

[CryptoHack – Elliptic Curves challenges](https://cryptohack.org/challenges/ecc/)

---

<h2 id="Lq77J">椭圆曲线方程</h2>
椭圆曲线是： y² = x³ + ax +b，其中还要满足（4a3 + 27b≠0）

---

<h2 id="rdl4Q">椭圆曲线的点加法</h2>
![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732002359679-87345dd8-b0a7-46dc-a98d-92db42671cc5.png)

![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732002393060-8b97dc9f-7cce-44be-8544-ff39eaddd47c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732002413547-fdcb7d45-505e-491d-bf6d-21fd800a57ff.png)

![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732002516915-a6a91e2a-7143-41ef-aa14-cfa37a0f24c0.png)

---

<h2 id="VbuXk">ECC加密过程</h2>
![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732176986077-aa60c078-aa86-4b52-ac2b-11f92403e002.png)

---

<h2 id="HP9oa">ECC密钥交换过程</h2>
![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732111754348-e3351517-f076-4383-b35f-0a20a7d5a133.png)



---

<h2 id="C8i3t">有限域上的椭圆曲线计算</h2>
![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732003210321-439f4c05-b21b-40ad-a870-f2af274bcaef.png)



_<font style="color:rgb(6, 6, 7);">P</font>_<font style="color:rgb(6, 6, 7);">=(x1,y1) 和 Q=(x2,y2)是椭圆曲线上的两个点，k是根据 P</font>_<font style="color:rgb(6, 6, 7);"> </font>_<font style="color:rgb(6, 6, 7);">和 Q 是否相等来计算的斜率，P+Q=(x3,y3)是点加法的结果。</font>



<h3 id="u5t82">举例说明</h3>
![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732003597454-81c8da45-c913-4e91-9eae-1e06a6490956.png)



计算2A

由图可知，a=b=1，x1=x2=0，y1=y2=1

k=(3*0² +1)/2*y1 (mod 23)=1/2 mod 23

令 n≡1/2 mod 23

    2n≡1 mod 23

    1≡2*n mod 23

解得 n=12=k

所以 x3=12²-0-0 (mod 23)= 6

        y3=12*(0-6)-1 (mod 23)=-73 mod 23=-73 - 23 * [-73/23]（向下取整）=-73 - 23 * -4=19

        负数求模：a<0，a mod b = a - b * [a/b]（向下取整）

 所以 2A（6 ,19）

---

<h2 id="QDDVf">sage上实现椭圆曲线的运算</h2>
<h3 id="Xan3h">构造椭圆曲线E</h3>
先定义 p , a , b

令 <font style="color:rgb(77, 77, 77);">E = EllipticCurve(GF(p),[a,b])</font>

<h3 id="kwPKF"><font style="color:rgb(77, 77, 77);">定义曲线上的点</font></h3>
如 P = E([10,20]) ， Q = E（[40,50])

<h3 id="Q8Czn">加法运算</h3>
P + Q

<h3 id="oV3c0">数乘运算</h3>
10 * P

<h3 id="CHhzE">举例说明</h3>
<h4 id="TVg6Z">题目1</h4>
![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732065214587-f19387ab-349c-4002-9007-80c02fe29141.png)

解题脚本如下

![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732065324459-2c48b2fd-5cce-479d-bceb-907ae1eb0f44.png)



<h4 id="N5ufK">题目2</h4>
![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732065377981-d23cc5ab-ed2f-42e0-8e95-3003b621aad7.png)

解题脚本如下

![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732065420708-e8b8438f-c10e-4f81-89d3-96b853dc7d08.png)

---

<h2 id="g4fcY">来两道题练手</h2>
<h3 id="HC8iL">NewStart CTF 2024的一道简单 ECC 题目</h3>
<h4 id="HCdLG">题目如下</h4>
```plain
from Crypto.Util.number import * # type: ignore
from secret import flag

p = 64408890408990977312449920805352688472706861581336743385477748208693864804529
a = 111430905433526442875199303277188510507615671079377406541731212384727808735043
b = 89198454229925288228295769729512965517404638795380570071386449796440992672131
E = EllipticCurve(GF(p),[a,b])
m = E.random_point()
G = E.random_point()
k = 86388708736702446338970388622357740462258632504448854088010402300997950626097
K = k * G
r = getPrime(256)
c1 = m + r * K
c2 = r * G
c_left =bytes_to_long(flag[:len(flag)//2]) * m[0]
c_right = bytes_to_long(flag[len(flag)//2:]) * m[1]


print(f"c1 = {c1}")
print(f"c2 = {c2}")
print(f"cipher_left = {c_left}")
print(f"cipher_right = {c_right}")


'''
c1 = (10968743933204598092696133780775439201414778610710138014434989682840359444219 : 50103014985350991132553587845849427708725164924911977563743169106436852927878 : 1)
c2 = (16867464324078683910705186791465451317548022113044260821414766837123655851895 : 35017929439600128416871870160299373917483006878637442291141472473285240957511 : 1)
c_left = 15994601655318787407246474983001154806876869424718464381078733967623659362582
c_right = 3289163848384516328785319206783144958342012136997423465408554351179699716569
'''
```

<h4 id="Uc48O">分析</h4>
1、E = EllipticCurve(GF(p),[a,b])<font style="color:rgb(6, 6, 7);">使用 a 和 b 以及有限域 GF(p) 创建了一个椭圆曲线 E。</font>

<font style="color:rgb(6, 6, 7);">2、m = E.random_point()</font>

<font style="color:rgb(6, 6, 7);">      G = E.random_point()</font>

<font style="color:rgb(6, 6, 7);">      随机选择了椭圆曲线上的两个点 m 和 G。</font>

<font style="color:rgb(6, 6, 7);">3、 k = 86388708736702446338970388622357740462258632504448854088010402300997950626097</font>

<font style="color:rgb(6, 6, 7);">      定义了一个私钥k</font>

<font style="color:rgb(6, 6, 7);"></font>

<font style="color:rgb(6, 6, 7);">因为</font>

<font style="color:rgb(6, 6, 7);">K = k * G</font>

<font style="color:rgb(6, 6, 7);">所以</font>

<font style="color:rgb(6, 6, 7);">c1 = m + r * K = m + r * k * G</font>

<font style="color:rgb(6, 6, 7);">m = c1 - k * c2</font>

<font style="color:rgb(60, 60, 60);">这样就得到了 m，m 是一个点，x 坐标和 y 坐标分别是 m[0] 和 m[1]，flag 前半和后半分别整除 x 和 y，然后拼接就能得到 flag</font>

<h4 id="y8Bcu">解题脚本如下</h4>
```plain
from Crypto.Util.number import *
"""
# sage
p = 64408890408990977312449920805352688472706861581336743385477748208693864804529
a = 111430905433526442875199303277188510507615671079377406541731212384727808735043
b = 89198454229925288228295769729512965517404638795380570071386449796440992672131
k = 86388708736702446338970388622357740462258632504448854088010402300997950626097
E = EllipticCurve(GF(p),[a,b])

c1 = E([10968743933204598092696133780775439201414778610710138014434989682840359444219, 50103014985350991132553587845849427708725164924911977563743169106436852927878 ])
c2 = E([16867464324078683910705186791465451317548022113044260821414766837123655851895, 35017929439600128416871870160299373917483006878637442291141472473285240957511 ])
cipher_left = 15994601655318787407246474983001154806876869424718464381078733967623659362582
cipher_right = 3289163848384516328785319206783144958342012136997423465408554351179699716569
m = c1 - k*c2

x=m[0]
y=m[1]

left = cipher_left // x
right = cipher_right // y
"""
(left,right) =(531812496965563174412251588431148136, 526357398425538015765092604513836925)

print(long_to_bytes(int(left))+long_to_bytes(int(right)))
```

![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732070076153-be96a6a7-2e5b-4180-a50b-920d5b85735a.png)



<h3 id="zS3QT">CryptoHack上的一道题目</h3>
题目位置：[https://cryptohack.org/challenges/ecc/](https://cryptohack.org/challenges/ecc/) 的Efficient Exchange



<h4 id="iqhYm">题目如下</h4>
```plain
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

"""
a = 497
b = 1768
p = 9739
Q_x = 4726
nB = 6534
iv = 'cd9da9f1c60925922377ea952afc212c'
encrypted_flag = 'febcbe3a3414a730b125931dccf912d2239f3e969c4334d95ed0ec86f6449ad8'
p ≡ 3 mod 4 (which will help you find y from y^2)
"""
```

<h4 id="qtTHC">分析</h4>
根据 y² = x³ + ax +b (mod p)，这道题给出了x的值，那么y的值就有两种可能 y1 和 y2 , Q点也有两种可能

求y的值，是一个**二次剩余**问题（简单了解可以看[【一口气学完】密码学的数学基础 3，《二次剩余》_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1xt4y1n7T5/?spm_id_from=333.337.search-card.all.click&vd_source=802632f153a1ecb77319c61fa70dccdf)）

求出 y1 和 y2 后，得到 Q1 和 Q2，然后就可以得到共享密钥 secrect1 和 secrect2

最后把 secrect[0] , iv , encrypted_flag 代进函数 decrypt_flag()中即可求出 flag



让我来解释一下函数 **sqrt，**和y1, y2 = sqrt((pow(Q_x, 3) + a * Q_x + b) % p, p)



1、因给定一个整数 a 和一个正整数 m，如果存在一个整数 x 使 x² ≡ a ( mod m ) 成立，就可以说 a 是模 m 的二次剩余（x 叫 a 在模 m 下的平方根）。

a，m 已知，求 x 的值

x² ≡ a ( mod m )

a ≡ x² ( mod m )

由我的总结（记就完了）：x的范围是[0,m-1]，a的范围是[0,m-1]，如果 a = 0，那么 x 才能取0，否则范围都是[1,m-1]

所以，设 i 遍历<font style="color:rgb(77, 77, 77);">1，2，3，……，m-1，逐一平方取模计算找模m的二次剩余，若等于 a，则 </font>**<font style="color:rgb(77, 77, 77);">x1 = i，x2 = m - i</font>**

<font style="color:rgb(77, 77, 77);">对 x2 = m - i 有疑问就再好好学一下二次剩余，或者看下面这张图找规律</font>

![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732173925177-e5a121f5-3a50-4801-996f-68b029436d1b.png)

<font style="color:rgb(77, 77, 77);"></font>

<font style="color:rgb(77, 77, 77);">2、按理来说应该是</font>y1, y2 = sqrt(pow(Q_x, 3) + a * Q_x + b, p)，但是因为 p 是有限域的大小，<font style="color:rgb(6, 6, 7);">当我们计算 y 的值时，我们需要确保所有的运算都在模 p</font><font style="color:rgb(6, 6, 7);"> 下进行，以保证结果是有限域中的一个有效元素。</font>

<font style="color:rgb(28, 31, 35);">( 1 )在有限域中，所有的算术运算（加法、减法、乘法和除法）都是在模 p 下执行的。这意味着当我们处理椭圆曲线上的点时，我们关心的是这些点在模 p 下的坐标。</font>

<font style="color:rgb(28, 31, 35);">( 2 )( pow(Q_x, 3) + a * Q_x + b ) 的结果大于 p ，这个结果在有限域 p 中是不合法的，因为有限域 p 仅包含从 0  到 p-1 的整数，所以要取模。</font>

<h4 id="u4VzE">解题脚本如下</h4>
```plain
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
import hashlib

def sqrt(x, q):
    for i in range(1, q):
        if pow(i, 2) % q == x:
            return (i, q - i)
    return None

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

a = 497
b = 1768
p = 9739
Q_x = 4726
nB = 6534
iv = 'cd9da9f1c60925922377ea952afc212c'
encrypted_flag = 'febcbe3a3414a730b125931dccf912d2239f3e969c4334d95ed0ec86f6449ad8'

y1, y2 = sqrt((pow(Q_x, 3) + a * Q_x + b) % p, p)
print(y1,y2)

Q1 = (Q_x, int(y1))
Q2 = (Q_x, int(y2))
print(Q1,Q2)


"""
# sage
a = 497
b = 1768
p = 9739
E = EllipticCurve(GF(p), [a, b])
Q1 = E([4726, 3452])
Q2 = E([4726, 6287])
secret1 = nB * Q1   # secret1 = (1791 : 7558 : 1) 
secret2 = nB * Q2   # secret2 = (1791 : 2181 : 1)
"""

secret1 = (1791,7558)
secret2 = (1791,2181)

if Q1[1] % 4 ==3:
    flag = decrypt_flag(secret1[0], iv, encrypted_flag)
    print(flag)
else:
    flag = decrypt_flag(secret2[0], iv, encrypted_flag)
    print(flag)
```

