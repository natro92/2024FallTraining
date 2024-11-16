#笔记

参考文档：

[分类简介 | 狼组安全团队公开知识库](https://wiki.wgpsec.org/knowledge/ctf/)

[Kengwang 博客](https://blog.kengwang.com.cn/)

# 知识点：
## MD5绕过 
参考文档：[总结ctf中 MD5 绕过的一些思路_ctf md5-CSDN博客](https://blog.csdn.net/CSDNiamcoming/article/details/108837347)

题目：

basectf2024 week1 MD5绕过欸

newstar2024 week1 遗失的拉链



== ：**只比较值**  
=== ：**比较值和类型**

### 常规的0e绕过
原理：

PHP在处理哈希字符串时，它把每一个以“0E”开头的哈希值都解释为0，所以如果两个不同的密码经过哈希以后，其哈希值都是以“0E”开头的，PHP会当作科学计数法来处理，也就是0的n次方，得到的值比较的时候都相同。

注：这种方式只有在如下弱比较的时候才能使用。

if (md5($str1) == md5($str2))

**以下值在md5加密后以0E开头：**

+ QNKCDZO
+ 240610708
+ s878926199a
+ s155964671a
+ s214587387a
+ s214587387a

#### 特殊情况：
$a==md5($a)

0e21596201的 MD5 值也是由 **0e** 开头，在 PHP 弱类型比较中相等

### 数组绕过
原理：对于php<u>强比较和弱比较</u>：md5()，sha1()函数无法处理数组，如果传入的为数组，会返回NULL，所以两个数组经过加密后得到的都是NULL，也就是相等的。

 if($a!==$b && md5($a)===md5($b))

因此只需要构建如下所示数组计科==即可

?a[]=1&b[]=2

### MD5碰撞
if((string)$_POST['a']!==(string)$_POST['b'] )

a=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%00%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%55%5d%83%60%fb%5f%07%fe%a2

b=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%02%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%d5%5d%83%60%fb%5f%07%fe%a2

进行url解码后的MD5值相等

### 双MD5绕过
参考文档：

[[CTF]CTF中if (md5(md5($_GET[‘a’])) == md5($_GET[‘b’])) 的绕过 - 肖洋肖恩、 - 博客园](https://www.cnblogs.com/-mo-/p/11582424.html)



## 伪协议
参考文档：

[PHP伪协议详解-CSDN博客](https://blog.csdn.net/cosmoslin/article/details/120695429)

[PHP伪协议总结](https://segmentfault.com/a/1190000018991087)

题目：basectf week1 Aura 酱的礼物

#### 类型：
 file:// — 访问本地文件系统

http:// — 访问 HTTP(s) 网址

php:// — 访问各个输入/输出流（I/O streams）

data:// — 数据（RFC 2397）



#### data:// 
用途：允许直接在 URL 中嵌入小型数据，适合小型文本或图像。  

格式：data:[<media type>][;base64],<data>

media type（可选）：指定数据的 MIME 类型，如 text/plain、image/png 等。如果省略，

默认使用 text/plain。

base64（可选）：如果数据经过 Base64 编码，必须在媒体类型后加上 ;base64，

然后再跟随 Base64 编码的数据。

data：实际要传输的数据，可以是普通文本或经过 Base64 编码的二进制数据。

示例：

data://text/plain,Aura

data://text/plain;base64,QXVyYQ==

$data = file_get_contents('data:text/plain;base64,SGVsbG8sIFdvcmxkIQ==');  

####  file:// 
用途：用于访问本地文件系统，并且不受allow_url_fopen，allow_url_include影响  
语法：file://协议主要用于访问文件(绝对路径、相对路径以及网络路径)  
示例：

http://www.xx.com?file=file:///etc/passsword

#### http://
用途： 通过 HTTP(S) 协议请求远程资源，适合访问外部网站或 API。  

示例：

$content = file_get_contents('http://example.com');  

#### php:// 

用途：php:// 访问各个输入/输出流（I/O streams），在CTF中经常使用的是php://filter和php://input，php://filter用于读取源码，php://input用于执行php代码。

说明：  
PHP 提供了一些杂项输入/输出（IO）流，允许访问 PHP 的输入输出流、标准输入输出和错误描述符，  
内存中、磁盘备份的临时文件流以及可以操作其他读取写入文件资源的过滤器。

| **协议**    | **作用**         |
| :------------------------------------------------------ | :----------------------------------------------------------- |
| php://input | 可以访问请求的原始数据的只读流，在POST请求中访问POST的`data`部分，在`enctype="multipart/form-data"` 的时候`php://input `是无效的。 |


| php://filter | (>=5.0.0)一种元封装器，设计用于数据流打开时的筛选过滤应用。对于一体式`（all-in-one）`的文件函数非常有用，类似 `readfile()`、`file()`和 `file_get_contents()`，在数据流内容读取之前没有机会应用其他过滤器。 |
| :------------------------------------------------------- | :----------------------------------------------------------- |


php://filter参数详解

该协议的参数会在该协议路径上进行传递，多个参数都可以在一个路径上传递。具体参考如下：

| **php://filter 参数** | **描述** | |
| :--- | :--- | --- |
| resource=<要过滤的数据流> | 必须项。它指定了你要筛选过滤的数据流。 | |
| read=<读链的过滤器> | 可选项。可以设定一个或多个过滤器名称，以管道符（*\ | *）分隔。 |
| write=<写链的过滤器> | 可选项。可以设定一个或多个过滤器名称，以管道符（ | ）分隔。 |
| <; 两个链的过滤器> | 任何没有以 _read=_ 或 _write=_ 作前缀的筛选器列表会视情况应用于读或写链。 | |


语法

php://filter/read=convert.base64-encode/resource=[文件名]读取文件源码（针对php文件需要base64编码）

## PHP
###  执行命令  
exec() 和 shell_exec(): 需要通过 echo 手动输出，返回结果不会自动显示。  

若只有exec() 和 shell_exec()，可以选择将输出重定向到文件然后再读取这个文件的内容显示在浏览器中。

**exec()**

功能：

执行外部程序或命令，并返回最后一行输出。如果需要，可以通过第二个参数获取命令的所有输出行。

返回内容: 

不直接输出到浏览器。它返回最后一行输出，可以将所有输出存储在一个数组中。  

语法：

string exec(string $command[, array &$output[, int &$return_var]])

$command: 要执行的命令字符串。

$output: 可选参数，用于存储命令的所有输出行，类型为数组。

$return_var: 可选参数，用于存储命令执行后的返回状态码（0 表示成功，非0 表示错误）。



 **shell_exec()**

功能

执行外部程序或命令，并返回命令的完整输出结果。

返回内容:

 返回命令的完整输出作为字符串，但不会直接输出到浏览器  

语法

string shell_exec (string $command)  

$command: 要执行的命令字符串。 



**system()**

功能

用于执行外部程序或命令，直接将命令的输出结果输出到浏览器，同时返回命令的最后一行输出。

返回内容: 

直接将命令的输出结果发送到浏览器，同时返回最后一行输出。  

语法

string system(string$command[, int &$return_var])  

示例

$last_line = system('ls -l');   在这个例子中，ls -l 的结果会直接输出到浏览器  



### 重定向
定义：

 重定向是一种将命令的输入或输出流从默认位置（通常是终端或控制台）转移到其他位置（如文件或其他命令）的方法。这使得用户能够将命令的输出保存到文件中，或从文件中读取输入，而不是在终端中直接显示。

语法：  

 输出重定向  

`>`: 将标准输出重定向到文件。示例：command > filename.txt  

例子：

echo"Hello, World!" > hello.txt  

 这将把 "Hello, World!" 写入 `hello.txt` 文件中。如果文件已存在，内容将被覆盖。 

`>>`：将标准输出追加到文件末尾。示例：command >> filename.txt  

例子：

echo"Another line." >> hello.txt  

 这将把 "Another line." 追加到 hello.txt 文件的末尾。  



**查看重定向文件**

URL直接访问

示例：http://yourdomain.com/output.txt



### 反序列化
参考文档：

[PHP反序列化漏洞详解（万字分析、由浅入深）_php反序列化漏洞原理-CSDN博客](https://blog.csdn.net/Hardworking666/article/details/122373938)

__construct()            //类的构造函数，创建对象时触发

__destruct()             //类的析构函数，对象被销毁时触发

__call()                 //在对象上下文中调用不可访问的方法时触发

__callStatic()           //在静态上下文中调用不可访问的方法时触发

__get()                  //读取不可访问属性的值时，这里的不可访问包含私有属性或未定义

__set()                  //在给不可访问属性赋值时触发

__isset()                //当对不可访问属性调用 isset() 或 empty() 时触发

__unset()                //在不可访问的属性上使用unset()时触发 当脚本尝试将对象调用为函数时触发

__invoke()               //当尝试以调用函数的方式调用一个对象时触发---当脚本尝试将对象调用为函数时触发

__sleep()                //执行serialize()时，先会调用这个方法

__wakeup()               //执行unserialize()时，先会调用这个方法

__toString()             //当反序列化后的对象被输出在模板中的时候（转换成字符串的时候）自动调用

                               --类似 echo 



# 漏洞学习：
教学视频：

[Web安全必学知识点：文件包含、CSRF、XXE、SSRF漏洞渗透与防御零基础入门_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1xW42197vM/?spm_id_from=333.788.videopod.episode&vd_source=3d9e793b3ade52daa18c644e09cfb1b5)

## 文件上传
参考文档：

[【Web渗透入门】4.文件上传漏洞讲解_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1zk4y1b7u6/?spm_id_from=333.999.0.0&vd_source=3d9e793b3ade52daa18c644e09cfb1b5)

[太厉害了，终于有人能把文件上传漏洞讲的明明白白了-CSDN博客](https://blog.csdn.net/weixin_44519789/article/details/116570426)

[文件上传漏洞 (上传知识点、题型总结大全-upload靶场全解)_upload靶场题型-CSDN博客](https://blog.csdn.net/qq_43390703/article/details/104858705)

题目：basectf2024  week1 Upload

讲解视频 31:00[BaseCTF Week1 部分题目讲解_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV19wWEeoE1o/?spm_id_from=333.880.my_history.page.click&vd_source=3d9e793b3ade52daa18c644e09cfb1b5)

### 定义
文件上传漏洞是指由于程序员在对用户文件上传部分的控制不足或者处理缺陷，而导致的用户可以越过其本身权限向服务器上上传可执行的动态脚本文件。这里上传的文件可以是木马，病毒，恶意脚本或者WebShell等。“文件上传”本身没有问题，有问题的是文件上传后，服务器怎么处理、解释文件。如果服务器的处理逻辑做的不够安全，则会导致严重的后果。

### 原理：
在 WEB 中进行文件上传的原理是通过将表单设为 multipart/form-data，同时加入文件域，而后通过 HTTP 协议将文件内容发送到服务器，服务器端读取这个分段 (multipart) 的数据信息，并将其中的文件内容提取出来并保存的。通常，在进行文件保存的时候，服务器端会读取文件的原始文件名，并从这个原始文件名中得出文件的扩展名，而后随机为文件起一个文件名 ( 为了防止重复 )，并且加上原始文件的扩展名来保存到服务器上

程序员在开发任意文件上传功能时，并未考虑文件格式后缀的合法性校验或者是否只在前端通过js进行后缀检验。这时攻击者可以上传一个与网站脚本语言相对应的恶意代码动态脚本，例如(jsp、asp、php、aspx文件后缀)到服务器上，从而访问这些恶意脚本中包含的恶意代码，进行动态解析最终达到执行恶意代码的效果，进一步影响服务器安全。

### 产生原因：
对于上传文件的后缀名（扩展名）没有做较为严格的限制

对于上传文件的MIMETYPE(用于描述文件的类型的一种表述方法) 没有做检查

权限上没有对于上传的文件目录设置不可执行权限，（尤其是对于shebang类型的文件）

对于web server对于上传文件或者指定目录的行为没有做限制

### 漏洞利用：
多见于图片文件上传处

#### 一句话木马：
<?php eval ($_POST[0]);`

更多见参考文档：[文件上传漏洞 (上传知识点、题型总结大全-upload靶场全解)_upload靶场题型-CSDN博客](https://blog.csdn.net/qq_43390703/article/details/104858705)

## 文件包含
参考文档：

[文件包含漏洞全面详解-CSDN博客](https://blog.csdn.net/m0_46467017/article/details/126380415)

### 定义：
### 原理：
**包括文件机制**：PHP 等语言提供的 include 和 require 函数用于动态加载和执行文件中的代码。如果应用程序未对用户输入进行严格验证，攻击者可以通过提供恶意输入，操控包含的文件路径。

**路径解析**：服务器在处理 include 时，会根据输入的路径解析文件。如果攻击者能够控制路径，就可以利用路径遍历等手段访问其他文件。

### 产生原因：
### 漏洞类型：
1. **本地文件包含（LFI - Local File Inclusion）**  
定义：攻击者可以通过操控用户输入的文件路径，使服务器包含本地文件。  
攻击方式：通常通过路径遍历（如 ../../）来访问服务器上的敏感文件。  
例子：读取 /etc/passwd 或其他配置文件。
2. **远程文件包含（RFI - Remote File Inclusion）**  
定义：攻击者可以使服务器包含来自远程服务器的文件。  
条件：需要 PHP 的 allow_url_include 设置为 On。  
攻击方式：攻击者提供一个恶意的 URL，使服务器执行该 URL 指向的代码。  
例子：包含一个攻击者控制的 PHP 文件，可能导致远程代码执行。
3. **文件上传漏洞结合文件包含**  
定义：攻击者利用文件上传功能，上传恶意文件，然后通过文件包含漏洞执行。  
攻击方式：上传一个带有恶意代码的文件（如 PHP 文件），然后利用文件包含漏洞将其执行。  
例子：上传 shell.php 文件并通过包含语句执行该文件。
4. **路径遍历**  
定义：攻击者通过操控文件路径，访问不应允许的文件或目录。  
攻击方式：使用 .. 来回退到上级目录，获取敏感文件内容。  
例子：尝试访问 ../../etc/passwd 或其他不应暴露的文件。
5. **PHP 伪协议**  
定义：使用 PHP 的特定协议（如 php://filter）来读取或处理文件内容。  
攻击方式：利用 PHP 的内置过滤器绕过常规检查。  
例子：通过 php://filter/convert.base64-encode/resource=flag.php 获取文件的 Base64 编码

## SSRF(Server-Side Request Forgery 服务器端请求伪造)
参考文档：

[从一文中了解SSRF的各种绕过姿势及攻击思路-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/2288231)



题目：basectf week1 Aura 酱的礼物



### 定义：
 SSRF（Server-Side Request Forgery）是一种安全漏洞，攻击者通过操控服务器发起请求，而不是直接与应用程序交互。攻击者通常利用这一漏洞诱使服务器向内部或外部资源发起请求，从而获取敏感信息或执行未授权操作。  

### 原理：
**请求代理**：SSRF 利用服务器的请求能力，通常服务器被视为可信来源，攻击者可以通过构造请求操控服务器。

**内部服务访问**：许多网络架构将内部服务与外部网络隔离。SSRF 攻击可以突破这种隔离，允许攻击者访问本不应对外公开的内部资源。

### 产生原因：
**用户输入未验证**：应用程序直接接受用户提供的 URL 或其他网络地址，且没有进行适当的验证和过滤。攻击者可以利用这一点，输入恶意的内部地址。

### 漏洞利用：
**URL 构造**：攻击者会在用户输入中注入一个恶意 URL，通常是服务器能够解析和访问的地址。例如，如果应用程序允许用户输入 URL 进行数据抓取，攻击者可以输入内部地址。

示例： http://localhost/admin  

**协议利用**：利用特定的协议（如 `http://`、`ftp://` 或 `file://`）进行请求，可能会尝试访问敏感的内部资源。

**HTTP 请求伪造**：利用开发工具（如 Burp Suite）进行请求伪造，将请求的目标更改为内部服务或未授权的外部地址。



## RCE(远程命令执行)
参考文档：

[RCE漏洞详解及绕过总结(全面)-CSDN博客](https://blog.csdn.net/m0_73185293/article/details/131557169)



题目：

basectf week2 RCEisamazingwithspace

