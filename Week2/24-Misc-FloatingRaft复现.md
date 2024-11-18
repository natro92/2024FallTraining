## Week 1
### CyberChef's Secret
一道签到题，根据题目提示应该可以用CyberChef一把梭

打开txt文档，根据题目提示把这串密码复制粘贴至CyberChef

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731726184641-e8e65431-4596-4767-b262-094321305b9e.png)

先按base32解码，再按base58解码，最后再base64解码，直接得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731726412987-1a66f1cb-7b03-4f80-8b2f-135942c42b0b.png)



> **<font style="color:#000000;">知识点总结：这道题考察了CyberChef的使用以及对几种Base类型编码的判断，然后就可以一把梭</font>**
>

**<font style="color:#000000;"></font>**

### <font style="color:#000000;">机密图片</font>
图片隐写题，先查看图片属性，没有信息

再查看图片本身，是一张QR码，识别后也没有任何信息

用010Editor打开，查看文件头以及数据块，也没有异常，也没有CRC值报错

那就猜测这题可能考察LSB隐写

用StegSlove打开，LSB常规操作得到flag如下

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731727184261-5d108341-872d-4fa6-9872-ddb237ffe16d.png)



> **知识点总结：这道题考察了LSB隐写的相关知识以及Stegsolve的使用**
>

****

### 流量！鲨鱼！
流量分析题，题目提示的很明确，用wireshark来解题

（但是笔者对这类题目不是很拿手）

用wireshark打开附件的流量包，观察得到只有TCP协议与HTTP协议流量

用过滤器过滤出HTTP流量，检索flag关键字，可以看到flag相关流量

导出所有HTTP对象文件

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731732942294-b153ce85-d969-4aa7-8173-d07b3684005a.png)

但是几个与flag相关的文件内容都只有NOT FOUND

再浏览了一遍所有文件，找到一个很可能是flag的文件

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731733054530-9c8bbd6b-94c3-4076-a1b7-5b7e0c989eb3.png)

打开后是一串base编码，按照文件名提示应该是Base64编码

复制粘贴至CyberChef，按Base64解码两次得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731733218606-2b114885-1f6d-4e75-8347-dbc8e37f9f78.png)

****

> **知识点总结：这道题考察了流量分析的基本操作、Wireshark的使用，以及Base类型的解码**
>



### 压缩包们
压缩包题，根据题目提示，先用010Editor看看

发现题目给的文件应该是zip文件，但是文件头和文件名后缀不对，我们都改回来

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731734619036-a365a8c7-dc56-4de3-b480-6225c324e48d.png)

改完后解压，得到一个flag.zip，直接解压提示文件损坏

修复后解压提示需要密码

观察了一下压缩包注释，应该是一段Base64编码，我们解码一下

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731834103315-d9267be2-d104-4ead-9b8f-c08e3ac2c29c.png)

压缩包密码是六位纯数字，直接用Archpr暴力破解得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731834271644-4a448d94-6b4d-4c0a-bf28-8fdc8269164b.png)



> **知识点总结：这道题考察了zip文件头、base64解码以及压缩包密码暴力破解**
>



### 空白格
打开附件，一个空白的文档

Ctrl+A全选，发现全是空格

第一反应是零宽度字符，复制粘贴到零宽度字符在线解密网站[https://www.mzy0.com/ctftools/zerowidth1/](https://www.mzy0.com/ctftools/zerowidth1/)

解密后什么都没有，说明思路是错误的

通过搜索得知了一种名叫WhiteSpace的编程语言，我们找到了一个运行WhiteSpace语言的在线网站[https://vii5ard.github.io/whitespace/](https://vii5ard.github.io/whitespace/)

复制粘贴后运行得到了flag

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731834995742-6c795a5b-9d13-4d38-98f5-d9c154558143.png)



> **知识点总结：这道题考察了一种名为WhiteSpace的编程语言**
>



### 隐秘的眼睛
附件是一张jpg图片，图片中只有一个眼睛，背景有很多规则分布的小灰点

根据题目名字以及图片提示，猜测是被SilentEye隐写了

直接用SilentEye解码得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731835540852-89a848c2-b872-4d26-b32e-94a60b84fc78.png)



> **知识点总结：这道题考察了被SilentEye隐写后的图片特征以及SilentEye工具的使用**
>



## Week 2
### 新建Word文档
题目附件给了一个后缀名为docx的文件

打开文件，是空白的

用010Editor打开查看，发现是PK头且文件中包含了其他文件

联想到了压缩包以及docx文件的本质就是压缩包，于是修改后缀名为zip并解压，得到了一个文件夹

用记事本打开word目录下的document.xml文件，发现了其中一段文字

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731836326051-7e48748c-fc7d-42aa-89d6-b1128027d0f2.png)

联想到了与佛论禅编码，但是与佛论禅编码一般由“佛曰”或“佛又曰”开头而非“新佛曰”

尝试用与佛论禅解码，也确实解不出来

但是又了解到一种和与佛论禅编码类似的编码，叫“新约佛论禅”

[新约佛论禅/佛曰加密 - 萌研社 - PcMoe!](http://hi.pcmoe.net/buddha.html?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJleHAiOjE2OTczNzU3NTAsImZpbGVHVUlEIjoiRHk1ZWtISmhLbzBhcDV2MyIsImlhdCI6MTY5NzM3NTQ1MCwiaXNzIjoidXBsb2FkZXJfYWNjZXNzX3Jlc291cmNlIiwidXNlcklkIjotODMxMTE0NzQxNX0.mTWIluoUq65XDAhk6XrX9TnWYTlf03r-E2CS43TFFdY)

用新约佛论禅解码得到了flag

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731836948450-511e7e8d-d29d-4886-afec-03e0b365d44d.png)



> **知识点总结：这道题考察了docx文档的本质是压缩文件以及新约佛论禅密码的解密**
>



### 永不消逝的电波
题目给了一个wav文件，打开播放是一段摩斯密码

导入到摩斯密码在线听译网站[摩尔斯电码音频解码器 – Morse Code Magic](https://morsecodemagic.com/zh/%E6%91%A9%E5%B0%94%E6%96%AF%E7%94%B5%E7%A0%81%E9%9F%B3%E9%A2%91%E8%A7%A3%E7%A0%81%E5%99%A8/)进行解码得到flag内容

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731837333403-11ed0ae8-9acd-4dce-ba99-4b679119238d.png)

删去flag后包上flag{}提交



> **知识点总结：这道题考察了摩斯密码的解码**
>

****

****

### 1-序章
根据题目提示这应该是一道SQL盲注日志分析题

先了解了一下SQL盲注是什么

[SQL盲注(原理概述、分类)-CSDN博客](https://blog.csdn.net/weixin_49150931/article/details/111829828)

SQL注入这方面可以说是完全不懂，所以就找了几篇师傅的博客和去年比赛的wp来跟着做了

参考文档：

[NewStarCTF 2023 公开赛道 Week2_[newstarctf 2023 公开赛道]upload again!-CSDN博客](https://blog.csdn.net/weixin_64422989/article/details/133849967?spm=1001.2014.3001.5502)

[安全事件分析之SQL盲注溯源 - FreeBuf网络安全行业门户](https://www.freebuf.com/articles/web/264144.html)

根据参考文档中提到的，攻击者逐一测试从'username'和'password'字段中提取字符的Ascii码，以确定其中的某些字符，如果条件为真，'sleep(1)'函数执行，导致服务器休眠一秒钟，属于时间盲注

解题思路大概有了，接下来解题需要一个脚本检测时间变化（当然不用脚本一次次请求自己看过去也行）

不太会写代码，所以只是构思了一下脚本的思路大致如下：

:::info
由于每条请求的内容差别只在时间与检测的Ascii字符上，所以先将所有请求的时间与Ascii码对应存储到两个列表中，再新建一个空列表_result

然后对时间列表中第i个元素与第i+1个元素做差，如果差值为1则记录Ascii码列表中的第i项并存放在_result列表中

最后将_result列表中的的每一项转换成对应的Ascii字符并输出，输出结果就是攻击者获取到的数据

:::



找到了官方放出的脚本，思路与官方思路一致，但是官方脚本适用于python2.x版本，而我用的是python3.7.9，所以官方脚本编译报错，我们对其稍微进行了修改，代码如下

```python
import re
import urllib
 
f = open('access.log','r')
lines = f.readlines()
datas = []
for line in lines:
    t = urllib.parse.unquote(line)
    datas.append(t)
 
flag_ascii = {} 
for data in datas:
    matchObj = re.search( r'user\),(.*?),1\)\)=(.*?),sleep', data)
    if matchObj:
        key = int(matchObj.group(1))
        value = int(matchObj.group(2))
        flag_ascii[key] = value
       
flag = ''
for value in flag_ascii.values():
    flag += chr(value)
   
print(flag)
```

跑一遍脚本就得到了flag

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731852610838-d97dfd6b-a8eb-48cf-ad94-c01c8cb79160.png)



> **知识点总结：这道题考察了SQL盲注中的时间盲注，攻击者通过对系统时间的变化来判断注入是否成功，通过这些请求我们就可以用攻击者同样的方法来获取攻击者获取到的信息**
>

### base!
先把整段文字按base64解码了一遍，大部分可读但是与flag毫不相关，并且文字中穿插着部分乱码，说明这应该是被破坏后的一段base64编码

猜测应该是要将破坏的部分提取出来然后对这部分再进行其它类型的base解码

接下来又要写脚本，还是借鉴了其他人的wp中的脚本，代码如下

```python
import base64

def get_base64_diff_value(s1, s2):
    base64chars = b'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/'
    res = 0
    for i in range(len(s2)):
        if s1[i] != s2[i]:
            return abs(base64chars.index(s1[i]) - base64chars.index(s2[i]))
    return res

def solve_stego():
    with open('base!.txt', 'rb') as f: #文件放这里
        file_lines = f.readlines()
        bin_str = b''  # 使用字节串
        for line in file_lines:
            steg_line = line.replace(b'\n', b'')
            norm_line = base64.b64encode(base64.b64decode(steg_line)).replace(b'\n', b'')
            diff = get_base64_diff_value(steg_line, norm_line)
            print(diff)
            pads_num = steg_line.count(b'=')
            if diff:
                bin_str += bin(diff)[2:].zfill(pads_num * 2).encode()  # 将二进制字符串转换为字节串
            else:
                bin_str += b'0' * pads_num * 2
            print(goflag(bin_str))

def goflag(bin_str):
    res_str = b''
    for i in range(0, len(bin_str), 8):
        res_str += bytes([int(bin_str[i:i + 8], 2)])
    return res_str.decode()  # 将字节串转换为字符串

if __name__ == '__main__':
    solve_stego()
```

跑一遍脚本，得到了一串字符，应该也是用base编码的

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731854269866-daab5498-2eca-43a2-ae05-7b4aa2913f6b.png)

复制粘贴到CyberChef中解码，按base58解码后得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731854327791-d3cc480a-261b-4e5b-859f-01a692c1ec46.png)



> **知识点总结：这道题考察了base类型编码，但不是整段编码都是一种类型的base编码，而是有其他种类的base编码混淆其中，把base64编码的内容筛去就能得到剩下的base58编码的内容了**
>



### WebShell的利用
一个php文件，打开后观察，猜测应该是php混淆加密的内容

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731855235280-701c3b01-0fb2-4cb0-a9d9-6d2ccd36cadc.png)

用php混淆破解网站破解这个文件

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731855277135-4c30586e-7c42-42d6-82df-a735665782e2.png)

进入靶机，传参得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1731859810768-efaa8e6c-f119-416b-99cb-e1a7a67bfc10.png)



> **知识点总结：这道题考查了php混淆加密、php语言的基本知识以及hackbar传参**
>



### Jvav
附件是一张照片，用binwalk、010Editor、StegSolve都没有看出端倪

和题目结合在一起进行联想，猜测是Java盲水印隐写

[盲水印提取工具 java_mob649e8169ec5f的技术博客_51CTO博客](https://blog.51cto.com/u_16175522/7195549)

[【CTF-misc】java盲水印BlindWatermark工具使用_ctf 盲水印-CSDN博客](https://blog.csdn.net/qq_39972370/article/details/134145890)

参考这两篇文章，得知Java盲水印隐写要用到BlindWatermark工具

但是github上下载的BlindWatermark无法直接使用，需要将其编译成jar文件

但是编译这步没学会……

后面的步骤应该就是用java运行jar文件从图片中提取出水印，水印就是flag



> **知识点总结：这道题考察了java盲水印的提取**
>

