# <font style="color:rgb(33, 37, 41);">NewStarCTF 2023 --Misc方向</font>
# week1
## <font style="color:rgb(33, 37, 41);">CyberChef's Secret </font>


1.解压压缩包，打开flag.txt发现了以下字符串，特征是大写字母+数字，后面跟一个等号。根据这个特征可以得出其为base32加密(base64加密一般由大小写字母+数字),使用随波逐流软件解密得到新的字符串。

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730681745358-bd5e87b1-abb2-4b25-8f97-c1a494013ace.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730681941091-7e038e47-0bf1-4c09-8675-178bf43d65e8.png)



2.该字符串特征没那么明显，选择梭哈，得知是base58编码，解密得到了base64编码字符串

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730682016588-6a70004e-9ba1-4604-8a0b-525b42b5c393.png)

3.然后在base64解码得到flag：flag{Base_15_S0_Easy_^_^}

整个过程为base32->base58->base64	



## <font style="color:rgb(33, 37, 41);">机密图片</font>
根据提示“<font style="color:rgb(33, 37, 41);">小宝最近学会了隐写术，并且在图片中藏了一些秘密，你能发现他的秘密吗？</font>”可知该题考察的是图片隐写术，可能藏在属性里面，也可能藏在图片的ascii码形式里面，也可能是lsb隐写。

1.打开图片发现是二维码，下意识扫描，发现没有用

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730682615191-ae375d3e-027e-4ac7-9ee5-f3878d81198c.png)

2.尝试查看属性和010打开文件，均未果。

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730682661347-c4869c4a-cb08-4902-a4d5-e70fa56bc982.png)![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730682708388-e6bf7756-91c8-4db8-9fdc-7185e016f8c8.png)

3.尝试lsb隐写，勾选r0 g0 b0其他默认，查看得到flag:flag{W3lc0m3_t0_N3wSt4RCTF_2023_7cda3ece}

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730682850670-77f8000a-a0d2-4cce-81ae-48aa62e0558f.png)



## <font style="color:rgb(33, 37, 41);">流量！鲨鱼！</font>
1.下载附件，得到流量包，使用wireshark打开，首先ctrl+f，搜索分组字节流的字符串中的flag,经过筛查，发现没有什么有用的东西

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730683417091-91733c61-f0b3-4fc8-985e-0f1206e5a3e1.png)

2.尝试导出对象为http，查看文件名称，发现base64相关信息，flag应该是被加密成base64了

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730683545011-c9095e90-7672-4447-84de-f33cf3765f32.png)![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730683557206-526f5af7-6a47-428a-b140-7b9b57b0af30.png)

3.尝试保存flag文件发现前面的都是无用信息，直到保存打开ffffllll....|base64文件得到base64编码字符串

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730683716984-8a77b076-642b-4bfa-b7e2-8a555881728d.png)

4.两次base64解码得到：flag{Wri35h4rk_1s_u53ful_b72a609537e6}



## <font style="color:rgb(33, 37, 41);">压缩包们</font>
1.下载附件，010打开发现是zip，于是把文件名后缀改为zip然后进行解压，但是显示压缩文件损坏

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730814970945-4852a0b4-9eb5-4768-876d-961925fe321f.png)



2.使用zip repair工具对压缩包进行修复，得到了正常的zip，但解压需要密码。于此同时发现010打开文件得到了base64编码，解码得到了提示：I like six-digit numbers because they are very concise and easy to remember.

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730815096288-8bbaf888-5cd6-4fcc-9ef3-09b8a8f42a39.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730815351374-231c84ec-6dca-4f81-9306-13bd54ce044f.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730815236460-208c5b33-78ed-4566-8cbd-a0af2bea6682.png)

3.使用ziperello软件进行暴力破解得到密码，然后解压压缩包得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730815493788-f77e8f8f-b996-4104-968d-d25aebfc81c5.png)![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730815522064-e6166e8e-2a9c-46f9-8d40-0eaaab23c0aa.png)

## 空白格
1.打开附件，得到一个名为 white的文件，里面有很多空白的东西，根据提示“<font style="color:rgb(33, 37, 41);">我们之间留了太多空白格"可猜想是有关空白字符的隐写。用notepad++打开，选择视图->显示符号->显示所有符号看见该空白字符是由空格，Tab,换行符构成。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730815780259-7fe878da-c6f6-444a-b50e-9596997b5405.png)

2.上浏览器搜索有关空白字符的隐写，在这篇blog发现了white_space编程隐写（[https://blog.csdn.net/qq_51999772/article/details/122418926](https://blog.csdn.net/qq_51999772/article/details/122418926)）

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730815968069-6868c3cb-3d06-4b39-9174-583875ba9900.png)

3.使用该解码工具进行解码得到flag([Whitelips the Whitespace IDE (vii5ard.github.io)](https://vii5ard.github.io/whitespace/))![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730816086831-62b1f659-9f60-43c3-8ac0-461e8fd3797a.png)



## <font style="color:rgb(33, 37, 41);">隐秘的眼睛</font>
1.下载附件，得到了一张图片，查看属性，010查看是否藏有flag都没什么有用的发现。

2.认真分析提示“<font style="color:rgb(33, 37, 41);">有一双隐秘的眼睛在盯着你...</font>”和题目名字“隐秘的眼睛”，可能在提示该图片用什么软件加密的，于是在百度搜索，发现了Silenteye加密软件

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730816749397-5d841102-3a92-4d76-9c13-fd1e3c0372ef.png)![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730816846864-3c6755ee-b238-451e-8ab4-4d7ecea68658.png)

3.使用 silenteye软件进行解密，所有设置均为默认，得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730816929195-847500bd-77bd-415f-ad2d-3dedfc8b8901.png)



# Week2
## <font style="color:rgb(33, 37, 41);">新建Word文档</font>
1.下载文件得到word文档，猜想是word文档隐写，首先全选发现没有内容被选中

2.然后点击文件->选项，勾选全部，就可看见一串文字

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730817480383-1576932f-d766-48b0-9c75-cbf31bf948c2.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730817502711-e982fffe-3d5a-4ecc-93b0-ac954dde9200.png)

3.搜 新佛曰解密 得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730817615531-b3da23ac-21eb-4892-9d5a-893518e2ffe2.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730817587515-c55e2716-a2a5-4c18-a10b-980c260ae5c3.png)



## <font style="color:rgb(33, 37, 41);">永不消逝的电波</font>
1.下载附件得到wav文件，打开听一下，发现是莫斯密码，于是使用Audacity打开，把细的转为0，粗的转为1

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730817882702-39643cf5-6f4b-41ae-a79b-6df649e7a988.png)2.转化后得到：0010 0100 01 110 1 0000 0 1000 0 000 1 1010 1 0010 0 010 00 000 1011 111 001,0代表点，1代表短线，解码得到：FLAGTHEBESTCTFERISYOU，转小写得到：flagthebestctferisyou![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730818251648-494110d8-4bdf-47b5-be08-6ccd4eb2a846.png)

3.提交 flag{flagthebestctferisyou} 错误，把flag去掉提交 flag{thebestctferisyou 成功





## <font style="color:rgb(33, 37, 41);">1-序章</font>
1.根据题目描述，我们得到的日志文件是sql注入日志，拿出一个注入进行URL解码：if(ascii(substr((select group_concat(username,password) from user),1,1))=121,sleep(1),1)，查询资料知道：该代码的意思是查询数据库的用户名和密码，并判断该结果的第一个字符是否为121，若是，则休眠一秒，否则立刻返回结果



![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730818600059-4015bd9d-98c0-4eb8-bbb5-caacecb9ebfe.png)

2.根据上面分析可知，该攻击在爆破数据库的用户和密码，我们只要编写脚本获取爆破每个字符判断的最后一个ASCII值，然后拼接即可。脚本如下(构造模式串p时注意普通的"("和")"前面需要加上转义字符\):

```plain
import re
if __name__ == '__main__':
    with open(r"access.log") as file:
         str0=file.read();
         # print(str0)
         flag=""
         for i in range(1,63):
            p=rf'pid%5B0%5D=-1%20or%20if\(ascii\(substr\(\(select%20group_concat\(username,password\)%20from%20user\),{i},1\)\)=(\w+),sleep\(1\),1\)'
            number=re.findall(p,str0)#返回匹配到的数字
            print(number[-1])
            flag+=chr(int(number[-1]))
    print(flag)
```

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730819487580-7891f9f1-2900-41dd-811e-6364096a72fe.png)

3.最后得到flag: flag{just_w4rm_up_s0_you_n3ed_h4rder_6026cd32}

## base!
1.打开文件，发现里面很多行的base64密文，解密得到没有用的文字

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730820766556-e0c5ae63-f890-4494-9c4f-34c840555105.png)

2. 分析提示“<font style="color:rgb(33, 37, 41);">base! 贝斯！贝斯也有自己的秘密“，可能是base64相关隐写。搜索相关资料，得知是利用base64后面=来藏信息，于是直接上脚本解出：</font>

```plain
# base64隐写
import base64
def get_diff(s1, s2):
    base64chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/'
    res = 0
    for i in range(len(s2)):
        if s1[i] != s2[i]:
            return abs(base64chars.index(s1[i]) - base64chars.index(s2[i]))
    return res
def b64_stego_decode(path):
    file = open(path, "rb")
    x = ''  # x即bin_str
    lines = file.readlines()
    for line in lines:
        l = str(line, encoding="utf-8")
        stego = l.replace('\n', '')
        # print(stego)
        realtext = base64.b64decode(l)
        # print(realtext)
        realtext_encode = str(base64.b64encode(realtext), encoding="utf-8")
        # print(realtext)
        diff = get_diff(stego, realtext_encode)  # diff为隐写字串与实际字串的二进制差值
        n = stego.count('=')
        if diff:
            x += bin(diff)[2:].zfill(n * 2)
        else:
            x += '0' * n * 2
    i = 0
    flag = ''
    while i < len(x):
        if int(x[i:i + 8], 2):
            flag += chr(int(x[i:i + 8], 2))
        i += 8
    print(flag)
if __name__ == '__main__':
    filename="1.txt"
    b64_stego_decode(filename)
```

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730820887509-3cd2f3b6-1456-4861-9180-48e74fd2d7bd.png)

3.解出来的字符串也是base相关的密文，梭哈解密得:

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730821003309-2733c0ba-fac0-4b14-a7ed-ee78c21f9ac7.png)

## <font style="color:rgb(33, 37, 41);">WebShell的利用</font>
1.查看文件，发现里面是各种加密的密文，编写php代码尝试解密发现是套娃，于是编写脚本解密，然后得到了相关提示。

```plain
<?php
$shell = "eval(str_rot13(convert_uudecode(str_rot13(base64_decode('此处省略题目文件中的编码内容')))));";
for($i=0; $i<50; $i++){
    if(preg_match("/base64/",$shell)){
        $tmp = preg_replace("/eval/","return ",$shell);
        $shell = eval($tmp);
    }else{
        break;
    }
}
echo $shell;
```

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730822605565-0ed002fd-1e08-4559-b537-66667dee659d.png)

2. <font style="color:rgb(56, 56, 56);">由代码可知这是一个 PHP 中函数的链式调用，</font>GET里面传函数，POST传参数。于是我们利用火狐浏览器中的hackbar插件进行pyload,构造system(ls / ) 和system(cat /flag)命令进行执行，从而得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730822735493-d8ceacf3-8d62-4b80-963e-e0612875addb.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730822978686-07ec3bb5-7d2f-4c44-a0e8-196a16b9df03.png)

## <font style="color:rgb(33, 37, 41);">Jvav</font>
1.下载附件得到一张图片，根据提示 "<font style="color:rgb(33, 37, 41);">给阿姨来一杯卡布奇诺，多看看题目名哦，秘密就隐藏在图片中</font>" 猜想是图片相关隐写，并且和java有关？

2.使用盲水印工具进行解密，但只能看到一部分，不管怎么调参都看不到全部...

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730825734665-7198a2be-7512-4556-974f-da3dd5ae37e1.png)

3.搜索发现了相关wp，然后发现就是用java盲水印软件解密即可得到如下东西（<font style="color:rgb(77, 77, 77);">可以利用这个项目来提取Java盲水印内容：</font>[<font style="color:rgb(77, 77, 77);">https://github.com/ww23/BlindWaterMark</font>](https://github.com/ww23/BlindWaterMark?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJleHAiOjE2OTczNzU3NTAsImZpbGVHVUlEIjoiRHk1ZWtISmhLbzBhcDV2MyIsImlhdCI6MTY5NzM3NTQ1MCwiaXNzIjoidXBsb2FkZXJfYWNjZXNzX3Jlc291cmNlIiwidXNlcklkIjotODMxMTE0NzQxNX0.mTWIluoUq65XDAhk6XrX9TnWYTlf03r-E2CS43TFFdY)<font style="color:rgb(77, 77, 77);"> </font>）：<font style="color:rgb(51, 51, 51);">flag{3bb3c3a628a94c}</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730826493027-18157981-fbeb-4596-82ec-9a6e983405b3.png)



# Week3
## <font style="color:rgb(33, 37, 41);">阳光开朗大男孩</font>
1.下载附件，发现secret.txt文件中是社会主义核心价值观的加密密文，使用随波逐流的解密功能解密得到password

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730894947009-6fa098b5-8720-48ac-88cc-3a5ddad8173a.png)![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730895021535-2adea410-4664-418e-98a5-5ec1216f8d33.png)

2.查看flag.txt，发现是表情包的密文，用emoji表情符号解码，发现没反应，然后想起来base100也是emoji加密，尝试过后发现也不是...

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730895065255-69597f69-6b1e-4bbf-b551-4271e686f6d2.png)

3.最后使用密码学工具ToolsFx进行解密，得到了U2F开头的密文，查资料知道了可能是AES等加密。

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730895361004-3a0cd3f6-e166-42f5-a871-7340adbbf043.png)

4.使用在线解密得到flag(网站：<u><font style="color:rgb(91, 128, 141);background-color:rgb(243, 242, 238);">https://tool.okcode.vip/dev/aes </font></u>)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730895778931-2b4fc86b-2903-4b73-9bae-d279c838f027.png)

## <font style="color:rgb(33, 37, 41);">大怨种</font>
1.下载附件得到一张gif图片，使用随波逐流的gif分帧功能进行分解得到3张图片，其中一个是类似二维码的图片，但不是二维码

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730896062082-9855ec9f-0d47-4af3-ab34-4727903af885.png)

2.百度搜寻知道了是汉信码，在线解码( [在线汉信码识别,汉信码解码 - 兔子二维码](https://tuzim.net/hxdecode/) )得到flag:

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730896376698-16f07c94-6495-41cd-96bf-3d0acf0b0a0d.png)



## 2-分析
1.根据题目意思是一个攻击者在攻击某个网站，然后利用得到的用户名和密码黑入该网站。得到的是一个流量文件，应该就是在流量中分析出一些信息。首先我们要获取登录的用户名，所以我们可以先过滤出http协议并且优先考虑post请求（<font style="color:rgb(51, 51, 51);">http.request.method == "POST" </font>重要信息往往会藏着post里面）。然后我们就发现了login.php，里面应该藏着用户名之类的信息，于是我们追踪http流发现了用户名：best_admin

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731392813284-ea1f8261-93f6-4b86-9bef-864d1975e0e5.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731392914815-758c520e-c3ea-431d-97ae-8554635555e0.png)

2.查找存在漏洞文件，发现一个很可疑的流量，前面的index.php为存在漏洞文件，后面这个可能是木马文件：wh1t3g0d.php

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731393243448-c58d3760-a4a5-4942-a4d2-d897ca60da15.png)

3.组合得到的信息提交：flag{best_admin_index.php_wh1t3g0d.php}

  flag{4069afd7089f7363198d899385ad688b}

## 键盘侠
1.下载附件，发现是pcapng文件，然后用wireshark打开，快速浏览了一下，是usb流量，根据题目又知道了是键盘的流量。于是使用tshark命令提取hiddata。

 命令： tshark -r  draobyek.pcapng  -T fields -e usbhid.data  > out1.txt

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730897553689-ef0c28e5-c4ef-4cc7-ba43-e368a17b816a.png)

2.得到了键盘流量，其中第五位和第六位是对应按键的代码。

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730897854627-e0e63647-b9ed-4d4e-93bb-1947fe9d67ee.png)

3.找到按键对应代码的映射表，编写代码进行转换解码得到flag

```plain
import re
normalKeys = {"04":"a", "05":"b", "06":"c", "07":"d", "08":"e", "09":"f", "0a":"g", "0b":"h", "0c":"i", "0d":"j", "0e":"k", "0f":"l", "10":"m", "11":"n", "12":"o", "13":"p", "14":"q", "15":"r", "16":"s", "17":"t", "18":"u", "19":"v", "1a":"w", "1b":"x", "1c":"y", "1d":"z","1e":"1", "1f":"2", "20":"3", "21":"4", "22":"5", "23":"6","24":"7","25":"8","26":"9","27":"0","28":"<RET>","29":"<ESC>","2a":"<DEL>", "2b":"\t","2c":"<SPACE>","2d":"-","2e":"=","2f":"[","30":"]","31":"\\","32":"<NON>","33":";","34":"'","35":"<GA>","36":",","37":".","38":"/","39":"<CAP>","3a":"<F1>","3b":"<F2>", "3c":"<F3>","3d":"<F4>","3e":"<F5>","3f":"<F6>","40":"<F7>","41":"<F8>","42":"<F9>","43":"<F10>","44":"<F11>","45":"<F12>"}
shiftKeys = {"04":"A", "05":"B", "06":"C", "07":"D", "08":"E", "09":"F", "0a":"G", "0b":"H", "0c":"I", "0d":"J", "0e":"K", "0f":"L", "10":"M", "11":"N", "12":"O", "13":"P", "14":"Q", "15":"R", "16":"S", "17":"T", "18":"U", "19":"V", "1a":"W", "1b":"X", "1c":"Y", "1d":"Z","1e":"!", "1f":"@", "20":"#", "21":"$", "22":"%", "23":"^","24":"&","25":"*","26":"(","27":")","28":"<RET>","29":"<ESC>","2a":"<DEL>", "2b":"\t","2c":"<SPACE>","2d":"_","2e":"+","2f":"{","30":"}","31":"|","32":"<NON>","33":"\"","34":":","35":"<GA>","36":"<","37":">","38":"?","39":"<CAP>","3a":"<F1>","3b":"<F2>", "3c":"<F3>","3d":"<F4>","3e":"<F5>","3f":"<F6>","40":"<F7>","41":"<F8>","42":"<F9>","43":"<F10>","44":"<F11>","45":"<F12>"}

flag=[]
with open("2.txt")as file:
    data=file.readlines();
    for i in data:
        if i =="\n":
            continue
        else:
         data1=i.strip()
         data_2=str(data1[4:6])
         if data_2!="00":
            try:
                # print(normalKeys[data_2])
                flag.append(normalKeys[data_2])
            except:
                print("",end="")
print(flag)
flag1="".join(flag)
print(flag1)
flag1=flag1.replace("<SPACE>"," ").replace("<CAP>","").replace("<ESC>","")
# 定义正则表达式模式，匹配 <DEL>
pattern = r"<DEL>"
# 使用 re.search 函数查找第一个匹配 <DEL> 的子串
for i in range(100):
    match = re.search(pattern, flag1)
    if match == None:
        print("审查完毕")
        break;
    start_index=match.start()
    end_index=start_index+5
    flag1 = flag1[:start_index-1] + flag1[end_index:]
print(flag1)
```

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1730898043252-79c4c84f-0eda-4641-8a34-e984ba84ec2c.png)

## <font style="color:rgb(33, 37, 41);">滴滴滴</font>
1.得到一张图片和一个音频，然后听音频，发现是拨号声，于是使用dtmf2num进行解码得到密码：

 52563319066

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731394146752-8de0a1c7-0e2f-46b4-9649-ea0af147a6f0.png)



2.图像为jpg图像，然后上面又给了密码，猜想可能是 <font style="color:rgb(31, 9, 9);background-color:rgb(243, 242, 238);">steghide、outguess、jphs、F5-Steg等加密，经过尝试发现是steghide 加密，得到flag:flag{1nf0rm4t10n_s3cur1ty_1s_a_g00d_j0b_94e0308b}</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731394836148-641ecd4f-16a9-41a9-b067-659466c3249b.png)![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731394851742-0b2769dd-1f47-44ab-9cf6-a9f73b557b51.png)

# week4
## R通大残
得到一张png图片，先常规属性 010看一下，没什么发现。然后根据提示和R有关，那应该是RGB中的R,于是尝试stegsolve,勾选R的all得到flag:flag{a96d2cc1-6edd-47fb-8e84-bd953205c9f5}

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731395634825-b8eb16f3-0c7f-403c-913a-befa9c6ff41a.png)



## Nmap
1.参考这个blog（[nmap流量分析_nmap流量特征-CSDN博客](https://blog.csdn.net/m0_43406494/article/details/109091389)）得知根据wireshark流量包的特征如何区分哪个端口是开放的



<font style="color:rgb(77, 77, 77);">（1）-sS参数启动TCP SYN Scan，向目标各个端口发送设置了SYN位的TCP包。</font>

<font style="color:rgb(77, 77, 77);">（2）若是目标回复RST包，则说明目标端口是关闭的。</font>

**<font style="color:rgb(77, 77, 77);">（3）若是回复SYN/ACK包，则说明目标端口开放。</font>**

<font style="color:rgb(77, 77, 77);">（4）若是回复ICMP不可达报文或者没有回复则说明目标端口被防火墙屏蔽了。</font>

<font style="color:rgb(77, 77, 77);"></font>

<font style="color:rgb(77, 77, 77);">2.搜索 [SYN, ACK]字符串，然后得到SYN,ACK的回复流量，其中源端口为开放端口。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731397132349-4b603700-afe8-42c0-a0ed-dad53dd9205f.png)



3.一个一个找得到 80,3306,5000,7000,8021,9000

## 依旧是空白
1.下载附件得到一张图片和一个空白文本，先把该图片放进随波逐流查看，然后修正长宽后得到了密码：

s00_b4by_f0r_y0u

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731397695188-61003a8e-95de-48cd-a4e8-445e6d00c20e.png)

2.文本空白，开始猜想是whitespace编程，但又用不到密码，于是想到了snow空白隐写，snow解码

  SNOW.EXE -p s00_b4by_f0r_y0u  -C ./White.txt output.txt

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731397896421-3989d5b4-4e79-41c9-bc3b-c312766455fd.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731397904078-3fb4d6f0-8356-47e4-9f57-a7ac5bc30313.png)



## <font style="color:rgb(33, 37, 41);">3-溯源</font>
1. 通过 [https://www.cnblogs.com/CoLo/p/13233359.html](https://www.cnblogs.com/CoLo/p/13233359.html) 该网址学习到冰蝎流量的一些基本知识点。冰蝎流量通信一般分为两个阶段：1.Client通过post或者get借助shell.php发送秘钥 2. Client把执行命令加密（AES或者xor加密）作为输入传给Server,然后Server解密密文并执行，然后把结果加密返回给Server。
2. 所以我们首先得找到shell.php文件。在wireshark中选择导出对象->http->搜索shell即可。然后把得到的字符串进行url解密和base64解密得到key: e45e329feb5d925b

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731659742976-db597c4b-73f6-439c-8635-e1291aef4445.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731659820273-7b219c61-8784-4829-9a11-d9fbefa090a8.png)

3.再选择导出对象->http->选择text/html类型，然后降序大小，保存1.php。对其先进行AES解码（[http://tools.bugscaner.com/cryptoaes/](http://tools.bugscaner.com/cryptoaes/)），然后对负载信息msg进行base64解码可以得到ip相关信息:  172.17.0.2

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731660354299-ccf50f4f-4d01-415b-a85a-4a5400e318bf.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731660508394-77e30dc3-d946-4b82-b362-28ff657ecb7a.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731660549792-0a86191c-07ca-4914-b8be-a8ea0732d148.png)![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731660631352-d7b4f100-5453-43ec-9dd3-db0012db1edd.png)

4.选择导出对象->http->选择text/html类型，然后升序大小，选择第一个即可，然后进行AES解密，得到了:www-data

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731661260689-a381edf2-0483-4e1b-83f7-379378764bb6.png)



![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731661193005-86134532-9d58-4b38-88d3-bbe684385178.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731661221279-8ab6fdf0-99db-4d78-90b0-0844173fd216.png)

最后提交flag: flag{www-data_172.17.0.2}

# week5
## 隐秘的图片
1.下载附件得到两张二维码，扫描第一张，没有用的信息，然后猜想是两张图片做运算。

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731398636424-f6392ff0-b125-4c70-bb7d-eea09af38a35.png)

2.使用stegsolve进行图片组合运算，发现xor运算的时候得到二维码，扫码得到flag

flag{x0r_1m4ge_w1ll_g0t_fl4ggg_3394e4ecbb53}

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731398765812-20a0ef44-d92f-43bf-9723-ed4577d5d05c.png)![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731398725280-313ccedf-4ba6-414c-a500-10e949a9a9c3.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731398809901-c48bf8a4-773d-44a1-9607-9e22bf1896eb.png)



## <font style="color:rgb(33, 37, 41);">新建Python文件</font>
1.根据提示对pyc进行反编译（ [https://tool.lu/pyc/](https://tool.lu/pyc/) ），但得到的python代码没有用处.

2.根据注释提示可能是python pyc文件隐写

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731662671781-7ebbb8fc-00fd-41df-9613-40cf283399d6.png)

3.上网查python pyc隐写，得到pyc隐写工具：[https://github.com/AngelKitty/stegosaurus](https://github.com/AngelKitty/stegosaurus)

4.下载工具进行解码得到:   ./stegosaurus flag.pyc -x  

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731662777029-84c8dfe5-7a9e-457a-a0b0-b891ec079bf8.png)

