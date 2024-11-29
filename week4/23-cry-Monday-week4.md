## E1
### 题目
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

### 分析
**考察对ECC的理解**

可以看到题目把flag分成了两部分，并用椭圆曲线上的m点对这两部分进行加密（简单的乘法），我们只要把得到的密文除以m的x、y值就可以了，所以现在目的变成了求m

m怎么求呢？我们发现m是椭圆曲线上随机一点，和m有关的有一个式子

![image](https://cdn.nlark.com/yuque/__latex/00e183b45d63b584a73c8abc9879e2e8.svg)

那么![image](https://cdn.nlark.com/yuque/__latex/342c0223db6394d2e456ba55a40f4998.svg)

虽然我们不知道K和r，但是知道k和c2，那么就要依赖椭圆曲线的知识

![image](https://cdn.nlark.com/yuque/__latex/bd34e953101e26e73a7dfdfc47a4073a.svg)

![image](https://cdn.nlark.com/yuque/__latex/df6a3626dc139bb27b7ff7c3ba82348f.svg)

![image](https://cdn.nlark.com/yuque/__latex/3ad02220b60cba08488359342eea86be.svg)

所以

![image](https://cdn.nlark.com/yuque/__latex/53d55ed2286d94409ad636c3c03978db.svg)

求出m后就没什么难度了，直接解密然后连起来就是密文了（记得newstar有一题一模一样的）下面是解密代码

```python
from Crypto.Util.number import *
p = 74997021559434065975272431626618720725838473091721936616560359000648651891507
a = 61739043730332859978236469007948666997510544212362386629062032094925353519657
b = 87821782818477817609882526316479721490919815013668096771992360002467657827319
k = 93653874272176107584459982058527081604083871182797816204772644509623271061231

E = EllipticCurve(GF(p),[a,b])

c1 = E(14455613666211899576018835165132438102011988264607146511938249744871964946084,25506582570581289714612640493258299813803157561796247330693768146763035791942)
c2 = E(37554871162619456709183509122673929636457622251880199235054734523782483869931,71392055540616736539267960989304287083629288530398474590782366384873814477806)
cipher_left = 68208062402162616009217039034331142786282678107650228761709584478779998734710
cipher_right = 27453988545002384546706933590432585006240439443312571008791835203660152890619

m = c1 - k * c2
left = cipher_left//m[0]
right = cipher_right//m[1] 
print(long_to_bytes(int(left))+long_to_bytes(int(right)))

```

flag：hgame{Ecc$is!sO@HaRd}





## E2
### 题目
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

### 分析
**考察椭圆曲线数字签名算法，AES加密，古典密码**

考的东西是挺多的，不过慢慢分析下来就会发现这题不算太难

首先我们来看最后加密的地方，是CBC模式的AES加密，最后把iv和加密后的密文的十六进制编码给我们了，所以相当于iv和密文我们就已知了（转回字节序列，根据块加密性质，前十六个字节就是iv）

而密钥key是dA的SHA256哈希值，所以现在我们缺的的只有dA

那么dA在哪里有出现呢，我们发现在`dA = nextprime(random.randint(0, n))`这里第一次出现定义，dA是一个介于0到n之间的随机素数

在定义dA后，我们只有在getrs()这个函数里面有用到dA，所以重点来看一下这个函数

```python
def getrs(z, k):
    (xp, yp) = P
    r = xp
    s = (z + r * dA % n) % n * gmpy2.invert(k, n) % n
    return r, s
```

这个函数首先获取P的x坐标记为r，再把输入值z、k和r、dA进行了一系列的运算，最后返回r和s。知道这个函数的功能后我们看看哪里用到了它，就找到下面这两个式子

`r1, s1 = getrs(z1, k)`

`r2, s2 = getrs(z2, k)`

然后我们根据getrs()函数把上面两个式子具体表示出来

![image](https://cdn.nlark.com/yuque/__latex/145d1e423a2c45a987dd107f5342ca0e.svg)

![image](https://cdn.nlark.com/yuque/__latex/f7a742108f32c5ff0f54fe3a1b0851cc.svg)

我们再根据数论的知识进行简单化简（两式相减），得到下面这个式子

![image](https://cdn.nlark.com/yuque/__latex/a6a26ce9a8e4cc38167dda6017148441.svg)

由于s2、s1、z2、z1、r2、r1题目都给了我们，且r1 = r2，即![image](https://cdn.nlark.com/yuque/__latex/c6b24dda8d5c83c4e9133b5195ad2508.svg)这一项为0，所以我们很容易就可以求出k

![image](https://cdn.nlark.com/yuque/__latex/24c6860663a480537c6ede0ac54d101e.svg)

知道k以后我们再代回![image](https://cdn.nlark.com/yuque/__latex/145d1e423a2c45a987dd107f5342ca0e.svg)这个式子，就可以得到dA

![image](https://cdn.nlark.com/yuque/__latex/63faf5bc0832cb13c067852a9e8c828e.svg)

所有值都已知，很容易就求出dA啦

求出dA后解AES就可以得到victory_encrypted_flag，但是这个还不是flag，是经过victory_encrypt()函数加密后的结果，那么我们来分析一下这个加密函数

```python
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
```

简单来讲就是把明文的每一个字母大写，再加上密钥的字母的ASCII值，那写解密脚本也十分简单，减去加上的值就行（代码上也就是加号改减号，很简单），如下（解完记得转小写，题目告诉我们flag是小写）

```python
def victory_decrypt(plaintext, key):
    key = key.upper()
    key_length = len(key)
    plaintext = plaintext.upper()
    ciphertext = ''

    for i, char in enumerate(plaintext):
        if char.isalpha():
            shift = ord(key[i % key_length]) - ord('A')
            encrypted_char = chr(
                (ord(char) - ord('A') - shift) % 26 + ord('A'))
            ciphertext += encrypted_char
        else:
            ciphertext += char

    return ciphertext
```

到这里就会发现解完了，但是好像没有用到椭圆曲线的知识对不对，但其实题目定义addition()和multiplication()这两个函数，应该就是为了实现椭圆曲线数字签名算法。但是不了解也能做题就是了，但是还是可以趁这个机会去了解一下椭圆曲线数字签名算法，这里不展开，放个链接，[学习链接](https://blog.csdn.net/qq_64585761/article/details/138250846?ops_request_misc=%257B%2522request%255Fid%2522%253A%25226317643b646d5d7aa1728d0554a2f1b5%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=6317643b646d5d7aa1728d0554a2f1b5&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-138250846-null-null.142^v100^pc_search_result_base7&utm_term=%E6%A4%AD%E5%9C%86%E6%9B%B2%E7%BA%BF%E7%AD%BE%E5%90%8D%E7%AE%97%E6%B3%95&spm=1018.2226.3001.4187)

下面是解密代码

```python
import binascii
from hashlib import sha256
from Crypto.Cipher import AES
from Crypto.Util.number import long_to_bytes
from Crypto.Util.Padding import unpad
import gmpy2

n = 0xfffffffffffffffffffffffffffffffebaaedce6af48a03bbfd25e8cd0364141
r1 = 111817653331957669294460466848850458804857945556928458406600106150268654577388
r2 = 111817653331957669294460466848850458804857945556928458406600106150268654577388
s1 = 86614391420642776223990568523561232627667766343605236785504627521619587526774
s2 = 99777373725561160499828739472284705447694429465579067222876023876942075279416
z1 = 96525870193778873849147733081234547336150390817999790407096946391065286856874
z2 = 80138688082399628724400273131729065525373481983222188646486307533062536927379
k = (z1 - z2) * gmpy2.invert(s1 - s2, n) % n
dA = (s1 * k - z1) * gmpy2.invert(r1, n) % n

encrypted_flag_hex = u'6c201c3c4e8b0a2cdd0eca11e7101d45d7b33147d27ad1b9d57e3d1e20c7b3c2e36b8da3142dfd5abe335a604ce7018878b9f157099211a7bbda56ef5285ec0b'
encrypted_flag_bytes = binascii.unhexlify(encrypted_flag_hex)
iv = encrypted_flag_bytes[:16]
encrypted_flag = encrypted_flag_bytes[16:]

key = sha256(long_to_bytes(dA)).digest()
cipher = AES.new(key, AES.MODE_CBC, iv)

victory_encrypted_flag = unpad(cipher.decrypt(encrypted_flag), AES.block_size).decode('utf-8')

def victory_decrypt(ciphertext, key):
    key = key. upper()
    key_length = len(key)
    ciphertext = ciphertext. upper()
    plaintext = ''
    
    for i, char in enumerate(ciphertext):
        if char.isalpha():
            shift = ord(key[i % key_length]) - ord('A')
            decrypted_char = chr((ord(char) - ord('A') - shift)% 26 + ord('A'))
            plaintext += decrypted_char
        else:
            plaintext += char
    
    return plaintext
print(victory_encrypted_flag)
victory_key = "WANGDINGCUP"
flag = victory_decrypt(victory_encrypted_flag, victory_key)

print(flag.lower())
```

flag：wdflag{58ae00432d8228c9e3a927bbcd8d67d2}





## L1
### 题目
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

### 分析
**考察格攻击求解小未知数**

我们看到`a = (a*flag) % p`这个式子，把它表示为

![image](https://cdn.nlark.com/yuque/__latex/8df16131d9581a1cd4362c802e3bae8d.svg)

再转换一下，可以得到

![image](https://cdn.nlark.com/yuque/__latex/eaf556b3d3addc4b425ca5fc2b7764b1.svg)

构造格

![image](https://cdn.nlark.com/yuque/__latex/e3374ad3915c2742be7935bb61bdd32b.svg)

到这里应该验证一下向量的大小和行列式的值，但是题目没给我们flag的长度，我们不好判断，那没办法喽，直接LLL规约走起（比手指），代码如下

```python
# sagemath

from gmpy2 import *
from Crypto.Util.number import *
from Crypto.Util.number import long_to_bytes
 
a = 79047880584807269054505204752966875903807058486141783766561521134845058071995038638934174701175782152417081883728635655442964823110171015637136681101856684888576194849310180873104729087883030291173114003115983405311162152717385429179852150760696213217464522070759438318396222163013306629318041233934326478247
p = 90596199661954314748094754376367411728681431234103196427120607507149461190520498120433570647077910673128371876546100672985278698226714483847201363857703757534255187784953078548908192496602029047268538065300238964884068500561488409356401505220814317044301436585177722826939067622852763442884505234084274439591

mat = [[p,0],[a,1]]
M = Matrix(ZZ,mat)
flag,r= M.LLL()[0]  # 
 
print(long_to_bytes(flag))
# b'NSSCTF{e572546b-abb5-4358-8970-471abc12b7ef}'
```

这里可以直接求解，说明flag长度确实不大，但是有一些题目就会需要我们手动去配平，以达到Hermite定理的要求，只能具体题目具体看，不过可以写一个下面的脚本，来判断向量和矩阵大小（原理就是Hermite定理，具体改一下式子和数据）

```python
import gmpy2
from Crypto.Util.number import *

a = 79047880584807269054505204752966875903807058486141783766561521134845058071995038638934174701175782152417081883728635655442964823110171015637136681101856684888576194849310180873104729087883030291173114003115983405311162152717385429179852150760696213217464522070759438318396222163013306629318041233934326478247
p = 90596199661954314748094754376367411728681431234103196427120607507149461190520498120433570647077910673128371876546100672985278698226714483847201363857703757534255187784953078548908192496602029047268538065300238964884068500561488409356401505220814317044301436585177722826939067622852763442884505234084274439591

n=2**0
# 行列式的值
temp = gmpy2.iroot(2*p*n,2)[0]
print(temp.bit_length())

# 向量大小
temp1 = gmpy2.iroot(2**(335*2)+2**(175*2),2)[0]		# 一般flag有335位，估计一下
print(temp1.bit_length())
```

flag：NSSCTF{e572546b-abb5-4358-8970-471abc12b7ef}





## L2
### 题目
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

### 分析
**考察格攻击求解小未知数（三维格）**

格的题目就是找式子构造格，那么就看到`a = (b*m + c) % p`这个式子，转化为

![image](https://cdn.nlark.com/yuque/__latex/0d5ef35327e19d4e8fb58590148cc8d0.svg)

这里面

> p	1024bit
>
> b	1024bit
>
> c	400bit
>
> m	351bit
>
> a	小于1024bit
>

所有我们可以看到c和m算是小未知数，要尽量让向量里包含这两个未知数

但是在这里我卡了好久，实在构造不出来格，主要还是卡在想在二维格里去构造。下面是错误思路

我把![image](https://cdn.nlark.com/yuque/__latex/0d5ef35327e19d4e8fb58590148cc8d0.svg)化为了![image](https://cdn.nlark.com/yuque/__latex/21dc52c28da9d0d093a079e78857fe10.svg)构造出了下面这个格

![image](https://cdn.nlark.com/yuque/__latex/e297a785265d4b29a86c516b294253a8.svg)

但是我们根据下面这个脚本算一下大小

```python
import gmpy2
from Crypto.Util.number import *

a = 92716521851427599147343828266552451834533034815416003395170301819889384044273026852184291232938197215198124164263722270347104189412921224361134013717269051168246275213624264313794650441268405062046423740836145678559969020294978939553573428334198212792931759368218132978344815862506799287082760307048309578592
b = 155530728639099361922541063573602659584927544589739208888076194504495146661257751801481540924821292656785953391450218803112838556107960071792826902126414012831375547340056667753587086997958522683688746248661290255381342148052513971774612583235459904652002495564523557637169529882928308821019659377248151898663
p = 100910862834849216140965884888425432690937357792742349763319405418823395997406883138893618605587754336982681610768197845792843123785451070312818388494074168909379627989079148880913190854232917854414913847526564520719350308494462584771237445179797367179905414074344416047541423116739621805238556845903951985783

n=2**0
# 行列式的值
temp = gmpy2.iroot(2*p*n,2)[0]
print(temp.bit_length())

# 向量大小
temp1 = gmpy2.iroot((2**1024-2**400)**2+(2**351)**2,2)[0]		# 一般flag有335位，估计一下
print(temp1.bit_length())

# 513
# 1024
```

向量明显大于行列式大小，用不了LLL规约，原因在于这个a太大了，所以是求不出来的

这个时候就需要扩展一下思维（~~比如上csdn找找类似题~~）简单瞥了一下才知道要构造三维格，于是我就试着开始构造三维格（也是打破了我只构造二维格的偏见了）

对于![image](https://cdn.nlark.com/yuque/__latex/0d5ef35327e19d4e8fb58590148cc8d0.svg)，我们把c这个小未知数放在一边，得到![image](https://cdn.nlark.com/yuque/__latex/bd1d081f1472a17325a72db800dd042f.svg)因为q、m、a都已知，所以可以拿这三个构造矩阵，所以系数向量（我自己取的名字）应该为（k，m，1），但是我们现在还差两个式子才能构造矩阵，我们期望m这个小未知数也出现在右边向量里，所以加上式子![image](https://cdn.nlark.com/yuque/__latex/4bd924ad93660b173ccd50cb71c0910b.svg)

现在还差一个式子实在找不出来，但是没关系，我们先试着构造成下面这个样子（相当于加了一个![image](https://cdn.nlark.com/yuque/__latex/fb3ff48282153f46f7b9e86745853444.svg)，就是为了凑成一个矩阵，不一定正确）

![image](https://cdn.nlark.com/yuque/__latex/98db8078f67a6a1b49e637e9ee321bd3.svg)

这个时候我们再验证一下

```python
import gmpy2
from Crypto.Util.number import *

p = 100910862834849216140965884888425432690937357792742349763319405418823395997406883138893618605587754336982681610768197845792843123785451070312818388494074168909379627989079148880913190854232917854414913847526564520719350308494462584771237445179797367179905414074344416047541423116739621805238556845903951985783
b = 155530728639099361922541063573602659584927544589739208888076194504495146661257751801481540924821292656785953391450218803112838556107960071792826902126414012831375547340056667753587086997958522683688746248661290255381342148052513971774612583235459904652002495564523557637169529882928308821019659377248151898663
a = 92716521851427599147343828266552451834533034815416003395170301819889384044273026852184291232938197215198124164263722270347104189412921224361134013717269051168246275213624264313794650441268405062046423740836145678559969020294978939553573428334198212792931759368218132978344815862506799287082760307048309578592

n=2**0
# 行列式的值
temp = gmpy2.iroot(3*p*n,3)[0]
print(temp.bit_length())

# 向量大小
temp1 = gmpy2.iroot(2**800+2**702+n**2,2)[0]
print(temp1.bit_length())

# 342
# 401
```

看起来不太行，因为向量又比矩阵大了，那么我们希望扩大矩阵，并且尽量少影响矩阵，那么我们就可以修改最后一个式子，因为我们看到最后一个式子是我们一开始随便假设的（相当于假设了矩阵的最后一个参数是1，我们现在要修改），我们可以改一下矩阵1那个位置的参数，可以继续用上面那个脚本，试着修改一下，找到合适的范围，原理在于我们扩大n（用来替换我们一开始假设的1）的值，对于矩阵是实打实的扩大，因为是乘法，而对于向量而言，是加法，扩大不明显（相当于抓大头），所以我们只要扩大n的值就可以实现对矩阵的扩大（之后如果遇到不能直接用格的题目，也要考虑一下能不能配平一下再用，整体思路就是找到那个对矩阵影响大，但是对向量影响小的地方）

简单试一下就可以知道n的范围大概在2的300次到500次之间（试之前也可以简单预测一下），都可以，随便选一个范围内的数就行，之后就可以用格了，就很简单了，下面是解密代码

```python
from gmpy2 import *
from Crypto.Util.number import *
from Crypto.Util.number import long_to_bytes
 
p = 100910862834849216140965884888425432690937357792742349763319405418823395997406883138893618605587754336982681610768197845792843123785451070312818388494074168909379627989079148880913190854232917854414913847526564520719350308494462584771237445179797367179905414074344416047541423116739621805238556845903951985783
b = 155530728639099361922541063573602659584927544589739208888076194504495146661257751801481540924821292656785953391450218803112838556107960071792826902126414012831375547340056667753587086997958522683688746248661290255381342148052513971774612583235459904652002495564523557637169529882928308821019659377248151898663
a = 92716521851427599147343828266552451834533034815416003395170301819889384044273026852184291232938197215198124164263722270347104189412921224361134013717269051168246275213624264313794650441268405062046423740836145678559969020294978939553573428334198212792931759368218132978344815862506799287082760307048309578592

mat = [[p,0,0],[-b,1,0],[a,0,2**500]]
M = Matrix(ZZ,mat)
c,m,k= M.LLL()[0]  # pp=p-r,qq=x-q
 
print(long_to_bytes(m))
```

flag：NSSCTF{ee5cb1a5-257a-48b0-9d62-9ef56ff0651a}

