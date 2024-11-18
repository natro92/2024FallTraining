# NewStarCTF 2023

## Misc



### CyberChef’s Secret

打开base加密，使用在线工具base32 58 64连续解码

![image-20241118112750506](C:\Users\33027\AppData\Roaming\Typora\typora-user-images\image-20241118112750506.png)

### 机密图片、

扫码，额没用

更改为jpg，额没用

试一下工具Stegsolve.jar，然后获得flag

找不到合适的版本打开的字都很小，眼睛疼

![image-20241118123835784](C:\Users\33027\AppData\Roaming\Typora\typora-user-images\image-20241118123835784.png)

![image-20241118123826742](C:\Users\33027\AppData\Roaming\Typora\typora-user-images\image-20241118123826742.png)

### 流量！鲨鱼！

使用WireShark打开它

然后导出HTTP对象文件

然后发现了这个有点像flag的文件名

打开之后发现是一串base加密后的密文，那么将它解密即可，是两层base64![image-20241118124049946](C:\Users\33027\AppData\Roaming\Typora\typora-user-images\image-20241118124049946.png)

### 压缩包们

先修改后缀名zip，打不开

winhex修改前八位为50 4B 03 04 14 00 00 00

解压，需要密码ziperello直接暴力破解得到![image-20241118172707527](C:\Users\33027\AppData\Roaming\Typora\typora-user-images\image-20241118172707527.png)

看了一下解析

发现原来还有一串base64编码，不过无伤大雅，破解速度快慢的问题而已

![image-20241118173504852](C:\Users\33027\AppData\Roaming\Typora\typora-user-images\image-20241118173504852.png)

SSBsaWtlIHNpeC1kaWdpdCBudW1iZXJzIGJlY2F1c2UgdGhleSBhcmUgdmVyeSBjb25jaXNlIGFuZCBlYXN5IHRvIHJlbWVtYmVyLg==

解码得到密码提示：6位纯数字

无法解压，测试后发现压缩包存在错误。

使用bandzip进行修复

flag{y0u_ar3_the_m4ter_of_z1111ppp_606a4adc}

### 空白格

打开什么都有，更换字体也没有

但是鼠标下滑有蓝色框框，说明是有内容的

搜索ctf空格加密

WhiteSpace，是一种只用空白字符（空格，TAB和回车）编程的语言，而其它可见字符统统为注释。https://byxs20.github.io/posts/7979.html在线解码

![img](file:///C:/Users/33027/Pictures/Screenshots/屏幕截图%202024-11-17%20203851.png)

### 隐秘的眼睛

题目翻译英文查了一下发现是SilentEye 