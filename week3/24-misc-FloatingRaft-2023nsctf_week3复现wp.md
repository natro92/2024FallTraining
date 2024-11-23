### 阳光开朗大男孩
附件是两个txt文件，名为flag和secret

打开flag.txt，文本内容全部由emoji表情组成，先猜测是emoji编码

> emoji编码又叫base100 编码，base100是一种加密，加密后全是emoji表情
>

用emoji编码解码器尝试解码，无果[Emoji表情符号编码/解码 - 一个工具箱 - 好用的在线工具都在这里！](http://www.atoolbox.net/Tool.php?Id=937)

经过查询，发现还有一种叫emoji aes的编码类型，这种编码类型与emoji编码不同的是，这种编码将原文转换为密文时需要一个私钥key，而解码也需要这个私钥key才能将密文转换为原文

所以另一个secret文件中所包含的应当是这个私钥

打开secret.txt，文本内容全部由社会主义核心价值观组成，猜测是核心价值观编码，用核心价值观编码解码器解码试试[核心价值观编码 - Bugku CTF平台](https://ctf.bugku.com/tool/cvecode)

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732016425721-0b2f370d-d2ff-450c-bf99-d63f1c9c6a69.png)

把's000_h4rd_p4sssw0rdddd'作为私钥，用emoji aes解码器解码得到flag

[https://aghorler.github.io/emoji-aes/](https://aghorler.github.io/emoji-aes/)

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732016566666-631d857f-efd5-4581-bce5-5d2cd711dc2c.png)



后面又去了解了一下AES。

> <font style="color:rgb(51, 51, 51);">参考资料：</font>
>
> [https://baike.baidu.com/item/%E9%AB%98%E7%BA%A7%E5%8A%A0%E5%AF%86%E6%A0%87%E5%87%86/468774](https://baike.baidu.com/item/%E9%AB%98%E7%BA%A7%E5%8A%A0%E5%AF%86%E6%A0%87%E5%87%86/468774)
>
> [https://blog.csdn.net/qq_28205153/article/details/55798628](https://blog.csdn.net/qq_28205153/article/details/55798628)
>

AES，中文名叫高级加密标准<font style="color:rgb(51, 51, 51);">（Advanced Encryption Standard，AES），这是最常见的对称加密算法。加密者设置一个密钥K，以密钥K与明文P为参数代入AES加密函数，最后输出密文C。而解密者需要知道密文C与密钥K，以密文C与密钥K为参数代入AES解密函数，从而得到明文P</font>



> **知识点总结：这道题考察了emoji aes编码解码与核心价值观编码解码，这几种编码与前面的与佛论禅、新约佛论禅都算比较抽象的几种编码类型**
>

### 大怨种
一张gif图片，有一帧里有一张二维码，我们用StegSolve打开逐帧查看并导出那一帧

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732111689064-565b6f28-7f5a-4e96-a02f-5b1fe2c1978d.png)

通过查询，得知这种二维码是汉信码，用汉信码在线识别网站[在线汉信码识别,汉信码解码 - 兔子二维码](https://tuzim.net/hxdecode/)识别得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732112104535-4f7a5511-7bdb-426b-a4b0-59912ebefd45.png)



通过了解，常见的二维码有<font style="color:rgb(36, 41, 47);">QRCode，PDF417，DataMatrix，汉信码、GridmMatrix、Aztec等，我们最经常使用的是QRCode，而AztecCode在2024 NewStarCTF Week4的扫码领取flag一题中也有考到过</font>

<font style="color:rgb(36, 41, 47);"></font>

> **<font style="color:rgb(36, 41, 47);">知识点总结：这道题考察了stegsolve如何导出gif文件其中一帧以及常见二维码中的汉信码的识别</font>**
>

### <font style="color:rgb(36, 41, 47);">2-分析</font>
一道流量分析题，这次需要找到的是攻击者登录使用的用户名、存在漏洞的文件名以及攻击者用来执行恶意代码的文件名

打开流量包，粗看一眼，只有HTTP与TCP协议，直接过滤出HTPP协议流量

这里我们先搞懂一下HTTP协议与TCP协议之间的区别与联系

> **TCP协议****<font style="color:rgb(77, 77, 77);">是传输层协议，主要解决数据如何在网络中传输，而HTTP协议是应用层协议，主要是解决如何包装数据。通俗的来说HTTP的任务是与服务器交换信息，它不管怎么连到服务器和保证数据正确的事情。而TCP的任务是保证连接的可靠，它只管连接，它不管连接后要传什么数据。HTTP协议是建立在TCP协议之上的一种应用，但是HTTP不一定要建立在TCP协议之上，</font>****<font style="color:rgb(17, 17, 17);">HTTP 协议中并没有规定必须使用 TCP/IP 或其支持的层。 事实上，HTTP 可以在任何互联网协议上，或其他网络上实现。 HTTP 假定其下层协议提供可靠的传输。 因此，任何能够提供这种保证的协议都可以被其使用</font>**
>
> **<font style="color:rgb(17, 17, 17);"></font>**
>
> **<font style="color:rgb(17, 17, 17);">详细内容可查看这篇文章 </font>**[**<font style="color:rgb(17, 17, 17);">面试：HTTP协议与TCP协议的区别和联系-CSDN博客</font>**](https://blog.csdn.net/weixin_44422604/article/details/108435009)
>

攻击者登录网站应该属于POST请求，所以我们查找POST请求，找到了一条登录的流量，查看这条流量，找到了攻击者登录的用户名即为username的值

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732267420498-8ffa87d5-13d1-4ac3-8b77-799fb2f382bc.png)

从登录这条流量往后继续查询，发现了一条可疑的流量，我们查看这条流量，找到了存在漏洞的文件名以及攻击者用来执行恶意代码的文件名

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732268611367-d2884256-538a-4ee3-a454-56cf77df45bf.png)

用md5加密网站[MD5在线加密/解密/破解—MD5在线](https://www.sojson.com/md5/)将这些内容按照格式进行md532位小写，包上flag{}提交

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732268694041-43cefc9b-8569-470e-b2cf-a3b89b9cc97d.png)



> **知识点总结：这道题考察了流量分析中的一些基础操作**
>

### 键盘侠
这是一道USB流量分析中的键盘流量分析题，先用tshark提取HID Data

在流量包所在目录下打开cmd输入指令

```plain
tshark -r draobyek.pcapng -T fields -e usbhid.data > hiddata.txt
```

这样我们就得到了一份键盘报告数据，键盘流量的每条流量包都是8个字节，且击键信息都在第三个字节

接下来只要把每条流量包中的第三个字节，也就是第五个和第六个字符与表进行对照，就可以得到击键信息了

我们用一段Python代码来进行这个操作

```python
normalKeys = {"04":"a", "05":"b", "06":"c", "07":"d", "08":"e", "09":"f", "0a":"g", "0b":"h", "0c":"i", "0d":"j", "0e":"k", "0f":"l", "10":"m", "11":"n", "12":"o", "13":"p", "14":"q", "15":"r", "16":"s", "17":"t", "18":"u", "19":"v", "1a":"w", "1b":"x", "1c":"y", "1d":"z","1e":"1", "1f":"2", "20":"3", "21":"4", "22":"5", "23":"6","24":"7","25":"8","26":"9","27":"0","28":"<RET>","29":"<ESC>","2a":"<DEL>", "2b":"\t","2c":"<SPACE>","2d":"-","2e":"=","2f":"[","30":"]","31":"\\","32":"<NON>","33":";","34":"'","35":"<GA>","36":",","37":".","38":"/","39":"<CAP>","3a":"<F1>","3b":"<F2>", "3c":"<F3>","3d":"<F4>","3e":"<F5>","3f":"<F6>","40":"<F7>","41":"<F8>","42":"<F9>","43":"<F10>","44":"<F11>","45":"<F12>"}

flag=[]
with open("hiddata.txt")as file:
    data=file.readlines();
    for i in data:
        if i =="\n":
            continue
        else:
            data1=i.strip()
            data_2=str(data1[4:6])
            if data_2!="00":
                try:
                    flag.append(normalKeys[data_2])              
                except:
                    print("",end="")
flag1="".join(flag)
print(flag1)

```

结果如下

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732362667651-5bd3faa8-27e6-439d-8652-5054fcb280c2.png)

但这不是最终结果，我们还要把<DEL><CAP>等字符串转换成对应的操作，完整代码如下

```python
import re

normalKeys = {"04":"a", "05":"b", "06":"c", "07":"d", "08":"e", "09":"f", "0a":"g", "0b":"h", "0c":"i", "0d":"j", "0e":"k", "0f":"l", "10":"m", "11":"n", "12":"o", "13":"p", "14":"q", "15":"r", "16":"s", "17":"t", "18":"u", "19":"v", "1a":"w", "1b":"x", "1c":"y", "1d":"z","1e":"1", "1f":"2", "20":"3", "21":"4", "22":"5", "23":"6","24":"7","25":"8","26":"9","27":"0","28":"<RET>","29":"<ESC>","2a":"<DEL>", "2b":"\t","2c":"<SPACE>","2d":"-","2e":"=","2f":"[","30":"]","31":"\\","32":"<NON>","33":";","34":"'","35":"<GA>","36":",","37":".","38":"/","39":"<CAP>","3a":"<F1>","3b":"<F2>", "3c":"<F3>","3d":"<F4>","3e":"<F5>","3f":"<F6>","40":"<F7>","41":"<F8>","42":"<F9>","43":"<F10>","44":"<F11>","45":"<F12>"}
shiftKeys = {"04":"A", "05":"B", "06":"C", "07":"D", "08":"E", "09":"F", "0a":"G", "0b":"H", "0c":"I", "0d":"J", "0e":"K", "0f":"L", "10":"M", "11":"N", "12":"O", "13":"P", "14":"Q", "15":"R", "16":"S", "17":"T", "18":"U", "19":"V", "1a":"W", "1b":"X", "1c":"Y", "1d":"Z","1e":"!", "1f":"@", "20":"#", "21":"$", "22":"%", "23":"^","24":"&","25":"*","26":"(","27":")","28":"<RET>","29":"<ESC>","2a":"<DEL>", "2b":"\t","2c":"<SPACE>","2d":"_","2e":"+","2f":"{","30":"}","31":"|","32":"<NON>","33":"\"","34":":","35":"<GA>","36":"<","37":">","38":"?","39":"<CAP>","3a":"<F1>","3b":"<F2>", "3c":"<F3>","3d":"<F4>","3e":"<F5>","3f":"<F6>","40":"<F7>","41":"<F8>","42":"<F9>","43":"<F10>","44":"<F11>","45":"<F12>"}

flag=[]
with open("hiddata.txt")as file:
    data=file.readlines();
    for i in data:
        if i =="\n":
            continue
        else:
         data1=i.strip()
         data_2=str(data1[4:6])
         if data_2!="00":
            try:
                flag.append(normalKeys[data_2])               
            except:
                print("",end="")
flag1="".join(flag)
flag1=flag1.replace("<SPACE>"," ").replace("<CAP>","").replace("<ESC>","")

pattern = r"<DEL>"

for i in range(100):
    match = re.search(pattern, flag1)
    if match == None:
        break;
    start_index=match.start()
    end_index=start_index+5
    flag1 = flag1[:start_index-1] + flag1[end_index:]
print(flag1)
```

最终结果如下

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732363525660-f151ab08-75e0-4244-b6b2-5ad599f327ea.png)



> **知识点总结：这道题考察了USB流量分析中的键盘流量分析**
>

### 滴滴滴
附件中一个jpg图片，一个wav音频，jpg图片属性中没有任何信息，用010Editor打开也没有CRC报错，文件格式没问题，也没有从16进制中看到隐藏文件

猜测应该是用steghide进行了隐写，用steghide尝试提取

在jpg图片目录下打开cmd输入指令

```plain
steghide extract -sf secret.jpg
```

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732370027906-dc304ab7-0c37-411c-b5eb-b6bbd64d06ce.png)

但是提取需要密码，猜测密码应该藏在另一个wav文件中

打开wav文件，是一段拨号音

用dtmf2num分析一下，还是在cmd中输入指令

```plain
dtmf2num 奇怪的音频.wav
```

得到了一串数字，猜测这串数字就是密码

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732370208461-435c5058-5ddc-4bd9-aa34-d9cf7a37e113.png)

用这串数字作为密码再试着提取，得到了flag

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732370255895-68241615-c79b-4b00-b09b-d6d8e8d87f54.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732370279998-591f0cc4-6166-4e48-b56d-e45daf47f4b6.png)



> **知识点总结：这道题考察了steghide隐写文件以及拨号音的分析**
>

