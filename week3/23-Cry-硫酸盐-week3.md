![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732329876704-60a80715-e417-4376-baa9-e8c7e0034ab1.png)



<h2 id="D8FAa">Point Negation</h2>
题目：

E:Y² =X³ +497X+1768 mod 9739  
P(8045,6936), find the point Q(x,y) such that P + Q = 0 



要使得 P + Q = 0，说明 <font style="color:rgb(6, 6, 7);">Q 是 P 关于 x 轴的反射，对于椭圆曲线 E:Y²=X³+aX+b mod p，如果 P=(x1,y1)，那么 Q=(x1,−y1 mod p)</font>

所以 Q = (8045,-6936 mod 9739)，即 Q = (8045,2803)

---

先了解sage上实现椭圆曲线的运算

构造椭圆曲线E：先定义 p , a , b，令 <font style="color:rgb(77, 77, 77);">E = EllipticCurve(GF(p),[a,b])</font>

<font style="color:rgb(77, 77, 77);">定义曲线上的点：</font>如 P = E([10,20]) ， Q = E（[40,50])

加法运算：P + Q

数乘运算：10 * P

<h2 id="E5QPs">Point Addition</h2>
题目：

E:Y² =X³ +497X+1768 mod 9739，P = ( 493 , 5564 ) , Q = ( 1539 , 4742 ) , R = ( 4403 , 5202 ), find the point S ( x , y ) = P + P + Q + R 



![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732065324459-2c48b2fd-5cce-479d-bceb-907ae1eb0f44.png?x-oss-process=image%2Fformat%2Cwebp)

所以 S(4215,2162)

<h2 id="rgllf">Scalar Multiplication</h2>
题目：

E:Y² =X³ +497X+1768 mod 9739，P=(2339,2213), find the point Q(x,y) = [7863] P 



![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732065420708-e8b8438f-c10e-4f81-89d3-96b853dc7d08.png?x-oss-process=image%2Fformat%2Cwebp)

所以 Q(9467,2742)

<h2 id="vPP11">Curves and Logs</h2>
题目：

E:Y² =X³ +497X+1768 mod 9739，G(1804,5368)，Calculate the shared secret after Alice sends you QA=(815,3190), with your secret integer nB=1829 ，Generate a key by calculating the SHA1 hash of the x coordinate (take the integer representation of the coordinate and cast it to a string). The flag is the hexdigest you find.

  
由![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732111754348-e3351517-f076-4383-b35f-0a20a7d5a133.png?x-oss-process=image%2Fformat%2Cwebp)

S= [nA]QB=[nB]QA

和上一题一样，算乘法

![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1732332635413-38820ae5-2841-4f4e-8a89-f6789afc6873.png)

所以 S(7929,707)



求 S 的x坐标的SHA1值的十六进制，脚本如下

```plain
import hashlib

x = "7929"
x_bytes = x.encode()
hash = hashlib.sha1(x_bytes)
print(hash.hexdigest())

# 80e5212754a824d3a4aed185ace4f9cac0f908bf
```

<h2 id="SAaRr">Efficient Exchange</h2>
题目：

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

<h4 id="qtTHC"></h4>
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

<font style="color:rgb(28, 31, 35);"></font>

解题脚本如下：

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

