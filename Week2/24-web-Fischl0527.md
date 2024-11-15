本人大一新生，专业是信息安全，第一次注册账号打开BaseCTF2024新生赛，看了一下里面的题目，对于一个连C语言学的都有些吃力的大一学生来说确实有些晦涩难懂。但是这些知识早晚都得学，所以我也想借这个机会好好去学习一下，了解一下互联网。



![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664348074-3f45e05e-b3e5-497a-9969-70595fd4ff1a.png)

- [ ] 

![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664366340-b477d68d-77bc-4709-9a93-a8916364ccbe.png)



这些题目的答案的提交都需要flag，我查了一下flag的含义：

“flag用于标记某种状态、属性或情况等。比如设置一个“flag”来表明某个用户是否已经登录（登录状态设为一种“flag”值，未登录设为另一种），或者标记某个任务是否完成（完成设为一个特定“flag”值，未完成是另一个）等，方便程序在运行过程中根据这些“flag”的值来进行不同的处理操作。”



能力有限，所以复现题目为web中week1的内容：**A dark room、HTTP是什么呀和喵喵喵´•ﻌ•`**



复现这些题目时我是跟着这个视频一步一步来的:

[<u><font style="color:rgb(0,0,255);">https://www.bilibili.com/video/BV19wWEeoE1o/spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=88ab8d2adf5ab07f0674cb316b7c960a#</font></u>](https://www.bilibili.com/video/BV19wWEeoE1o/spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=88ab8d2adf5ab07f0674cb316b7c960a#)<u><font style="color:rgb(0,0,255);"> </font></u>

# 一、A dark room
![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664456471-de3a21a2-99be-454d-9d25-3c64233191db.png)

这种小游戏的flag一般是藏在这个源代码里面的，可以用F12看源代码看元素找flag，其他的可以看网络请求，通常在这种游戏过关的题里面，通常查看看js代码，里面可能藏flag。其他的题可能藏在javascript里面。

![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664463478-95ced820-0453-40e9-aaaa-a12e9278f0f4.png)



![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664467380-c1664807-98ce-46af-bbd1-6f3516d30bf0.png)



![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664515149-d4b26abd-f9a5-4398-9a20-bf3ca42568aa.png)

# 二、HTTP是什么呀
![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664522869-f6bf8c52-8271-4f99-98ff-8749f31fa359.png)



关于**HTTP**：超文本传输协议（HTTP）是一个用于传输超媒体文档（例如 HTML）的应用层协议。它是为Web 浏览器与 Web 服务器之间的通信而设计的，但也可以用于其他目的。HTTP 遵循经典的客户端—服务端模型，客户端打开一个连接以发出请求，然后等待直到收到服务器端响应。HTTP 是无状态协议，这意味着服务器不会在两个请求之间保留任何数据（状态）。

参考这篇文章：[https://developer.mozilla.org/zh-CN/docs/Web/HTTP](https://developer.mozilla.org/zh-CN/docs/Web/HTTP)

## 1.GET参数
![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664538073-4396a9c3-c4a5-472a-b46e-439e95ee739f.png)



查看新手指引和视频内容，解这道题需要下载**Hackbar**



![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664576470-eb867020-0478-4764-96d8-9b8ff8bff8dc.png)



打开下载链接发现响应时间太长了，看攻略需要科学上网一下，之前我是没用过ladder的，花了一上午从B站、QQ、Github等平台上找资料学习，最后下载好了。



![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664571506-4bf097b4-20b7-46cc-ab41-5c792b175a43.png)



![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664710965-487f7056-a560-4825-bf43-a0f47183a807.png)



关于**HackBar**：

HackBar是一个用于浏览器的扩展插件，主要用于进行网络渗透测试和安全评估。它提供了一系列方便的工具和功能，可以帮助用户执行各种网络攻击和测试，包括 XSS、SQL 注入、CSRF、路径穿越等。以下是 HackBar插件的一些主要特点和功能：



自定义请求发送：HackBar允许用户自定义 HTTP 请求，并可以通过插件直接发送这些请求。用户可以手动构造 GET、POST、PUT、DELETE 等类型的请求，并添加自定义的 HTTP 头部、参数等信息。



编码/解码工具：HackBar提供了各种编码和解码工具，包括 URL 编码、Base64 编码、MD5 加密等。这些工具可用于在渗透测试中对数据进行编码或解码，以绕过一些安全机制或进行数据处理。



漏洞检测：HackBar可以帮助用户检测网站中常见的漏洞，例如 XSS、SQL 注入、CSRF 等。用户可以通过插件发送特定的测试请求，然后分析响应来确定目标网站是否存在漏洞。



Cookie 管理：HackBar允许用户管理浏览器中的 Cookie，包括添加、编辑、删除 Cookie 等操作。这对于进行身份验证、绕过登录限制等方面的渗透测试非常有用。



参数注入：HackBar提供了一个参数注入工具，可以帮助用户在 URL 中注入自定义的参数。用户可以使用这个工具测试网站的安全性，尝试发现潜在的漏洞。



总的来说，HackBar是一款功能丰富、易于使用的浏览器插件，适用于进行各种网络渗透测试和安全评估任务。它提供了许多有用的工具和功能，可以帮助安全专家快速有效地发现和利用网站中的漏洞。



参考资料：[<u><font style="color:rgb(0,0,255);">https://blog.csdn.net/2302_82189125/article/details/137762983</font></u>](https://blog.csdn.net/2302_82189125/article/details/137762983)



关于**URL**:

Internet上的每一个网页都具有一个唯一的名称标识，通常称之为URL（Uniform Resource Locator, 统一资源定位器）。它是www的统一资源定位标志，简单地说URL就是web地址，俗称“网址”。URL是对互联网上得到的资源的位置和访问方法的一种简洁表示，是互联网上标准资源的地址。URL它具有全球唯一性，正确的URL应该是可以通过浏览器打开此网页的，但如果您访问外网，会提示网页无法打开，这并不能说明这个URL是错误的。只不过在国内不能访问而已。



参考资料：[<u><font style="color:rgb(0,0,255);">https://blog.csdn.net/chen1415886044/article/details/103914255</font></u>](https://blog.csdn.net/chen1415886044/article/details/103914255)



百分号编码的内容查了一下：<font style="color:rgb(77,77,77);">URL编码（也称为百分号编码）是一种在URL中表示特殊字符的方法。它将非字母数字字符替换为</font>%<font style="color:rgb(77,77,77);">后跟两个表示字符</font><font style="color:rgb(77,77,77);">ASCII值的</font>十六进制<font style="color:rgb(77,77,77);">数字。这种编码用于确保这些字符可以安全地包含在</font><font style="color:rgb(77,77,77);">URL中，在传输和解析过程中不会引起错误</font><font style="color:rgb(77,77,77);">。</font>

#### 
<font style="color:rgb(79,79,79);">特殊字符及其编码意义：</font>

空格（Space）：%20  引号（"）：%22  百分号（%）：%25  加号（+）：%2B  斜杠（/）：%2F

冒号（:）：%3A  问号（?）：%3F@   符号（@）：%40  链接符号（#）：%23



解这道题首先要GET参数basectf，GET参数我们需要问号来传递。

![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664780527-79e08454-952d-4ac8-becf-ca9e32c23cc6.png)



我当时直接传递成这样了,发现问题是我这个问号是中文的，并且%的内容没有转译过去。



![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664793104-ed6333d9-6441-472e-ae98-42208482cd74.png)



在HackBar中用ENCODING中修正后就成功了

![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664847079-c10feb7a-18f7-4452-8b3e-9b59b21884eb.png)



![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664805727-627f227f-fee6-4aba-88c4-020da1bfa4e8.png)



## 2.POST参数
<font style="color:rgb(24,24,24);">HTTP四种请求：POST、GET、DELETE、PUT。</font>



<font style="color:rgb(36,41,46);">POST</font><font style="color:rgb(36,41,46);">请求用于向指定资源提交数据，通常会导致服务器端的状态发生变化。例如，在</font><font style="color:rgb(36,41,46);">Web 表单中填写用户信息并提交时，就是使用</font><font style="color:rgb(36,41,46);">POST</font><font style="color:rgb(36,41,46);">请求方式将表单数据提交到服务器存储。使用</font><font style="color:rgb(36,41,46);">POST</font><font style="color:rgb(36,41,46);">请求方式提交的数据会被包含在请求体中，而不像</font><font style="color:rgb(36,41,46);">GET</font><font style="color:rgb(36,41,46);">请求方式那样包含在</font><font style="color:rgb(36,41,46);">URL 中。因此，</font><font style="color:rgb(36,41,46);">POST</font><font style="color:rgb(36,41,46);">请求可以提交比</font><font style="color:rgb(36,41,46);">GET</font><font style="color:rgb(36,41,46);">更大的数据量，并且相对更安全。</font>



<font style="color:rgb(36,41,46);">GET</font><font style="color:rgb(36,41,46);">请求用于向指定资源发出请求，请求中包含了资源的</font><font style="color:rgb(36,41,46);">URL 和请求参数。服务器端通过解析请求参数来返回相应的资源，不会修改服务器端的状态。使用</font><font style="color:rgb(36,41,46);">GET</font><font style="color:rgb(36,41,46);">请求方式提交的数据会被包含在</font><font style="color:rgb(36,41,46);">URL中，因此易于被缓存和浏览器保存，但也因此不适合用于提交敏感数据。</font>



<font style="color:rgb(36,41,46);">DELETE</font><font style="color:rgb(36,41,46);">请求用于请求服务器删除指定的资源，可以理解为对服务器上的资源进行删除操作。使用</font><font style="color:rgb(36,41,46);"> </font><font style="color:rgb(36,41,46);">DELETE</font><font style="color:rgb(36,41,46);"> 方式请求会导致指定的资源被永久删除，因此需要谨慎使用。</font>



<font style="color:rgb(36,41,46);">PUT</font><font style="color:rgb(36,41,46);">请求用于向服务器更新指定资源，可以理解为对服务器上的资源进行修改操作。使用</font><font style="color:rgb(36,41,46);">PUT</font><font style="color:rgb(36,41,46);">请求方式会覆盖原有的资源内容，因此需要谨慎使用。</font>



参考资料：[<font style="color:rgb(0,0,255);">https://developer.aliyun.com/article/1476820</font>](https://developer.aliyun.com/article/1476820)



这道题打开下方的use POST method 输入Base=fl@g再传递就行了。



![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664839492-9c44b17b-6eca-4337-be4b-54896494a493.png)

## 3.Cookie
Cookie主要是存储用户登录网站时的数据，这样的话每一次访问网站的时候都会把Cookie带上，这样的话就是一个有状态的HTTP。

Get 和post都是拿&符号分隔的，cookie是分号来分隔的。

这道题在右下角Name一栏中输入Cookie，Value一栏中输入c00k13=i can't eat it就行了。





![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664891335-53844eab-1d23-458e-985b-38b7ca2320c3.png)



## 4.用户代理(User-Agent)
User Agent中文名为用户代理，简称 UA，它是一个特殊字符串头，使得服务器能够识别客户使用的操作系统及版本、CPU 类型、浏览器及版本、浏览器渲染引擎、浏览器语言、浏览器插件等简单来说，UA是浏览器的身份证，让服务器知道你是谁？服务器通过识别UA，来响应适合你电脑、手机...的网络页面                      

原文链接：[https://blog.csdn.net/qq_42680471/article/details/120706248](https://blog.csdn.net/qq_42680471/article/details/120706248)



这道题也是相同的思路，在Name一栏中填入User-Agent,Value一栏中填入Base再传递即可。





![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664902742-e4e845d4-0e3e-429b-a183-5aa0aa846e93.png)

## 5.来源(Referer)
Referer 请求头包含了当前请求页面的来源页面的地址，即表示当前页面是通过此来源页面里的链接进入的。服务端一般使用 Referer 请求头识别访问来源，可能会以此进行统计分析、日志记录以及缓存优化等。Referer的正确英语拼法是referrer 。由于早期HTTP规范的拼写错误，为了保持向后兼容就将错就错了。其它网络技术的规范企图修正此问题，使用正确拼法，所以目前拼法不统一。



referer的作用：



1.防盗链：防盗链是一种通过服务器端编程防止资源被盗用的技术，主要通过URL过滤和检查HTTP协议中的referer字段来实现。图片服务器每次取到Referer来判断一下是不是我自己的域名，如果是就继续访问，不是就拦截。



2.防止恶意请求：静态请求是*.html结尾的，动态请求是*.shtml，那么由此可以这么用，所有的*.shtml请求，必须 Referer为自己的网站。



这道题基本思路和上面的差不多，在Name一栏中填入Referer,Value一栏中填入Base再传递即可。

![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664925831-7d2b101b-dae9-4ece-8cf9-e2478e163a3a.png)



## 6.你的IP
IP地址（Internet Protocol Address）是指互联网协议地址，又译为网际协议地址。IP地址是IP协议提供的一种统一的地址格式，它为互联网上的每一个网络和每一台主机分配一个逻辑地址，以此来屏蔽物理地址的差异。

同时写网站的时候做好X-Forward-For可以防止有对它内部进行的一些的攻击，也可以伪造IP。

X-Forwarded-For（XFF）：是用来识别通过HTTP代理或负载均衡方式连接到Web服务器的客户端最原始的IP地址的HTTP请求头字段。

这道题的思路同上。在Name一栏中填入X-Forward-For,Value一栏中填入127.0.0.1再传递即可。



![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664940636-928926f3-6bc9-429d-baea-91442666bcf9.png)

## 7.结束
结束的时候会出现这个页面,提醒我们在一个页面转到另一个页面中途会经历什么



![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664954005-7afe702a-d0e6-4f11-ac46-c3793f3f815d.png)



这时候我们看一下这个网络这个选项卡里面发现每一次都经历了重定向，重定向(Redirect)就是通过各种方法将各种网络请求重新定个方向转到其它位置（如：网页重定向、域名的重定向、路由选择的变化也是对数据报文经由路径的一种重定向） ，通常是通过302的代码表示重定向，还有响应表头的location表示要把我重定向到哪里。



![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731664983230-288fc1e5-f4f8-4c41-bddb-a13a3a04d074.png)

这里的flag就重定向了一次，当访问到success时，又重定向到Thank you,然后把这个flag的信息抹去了，所以最后一句话我理解的意思是不要忘了flag。这里直接拿flag还不行，需要Base64解码来拿flag。



![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731665000874-999b5c12-4fb8-4057-a21c-10ba04e2eccb.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731665010605-17355475-5a40-4355-aa71-52422bf43c5e.png)

# 三、喵喵喵´•ﻌ•`
![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731665032335-02ca1b89-f436-49f2-b449-cc270d56ee5f.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731665053491-4ac28d63-3b25-47bc-96e3-619665aa1892.png)

PHP是一种流行的通用脚本语言，尤其适用于网络开发。从博客到世界上最流行的网站，PHP 都能为其提供快速、灵活和实用的支持。



这道题是接收GET参数DT，然后把GET的参数传入到eval中去。



关于eval函数：

eval() 函数把字符串按照 PHP 代码来计算。该字符串必须是合法的 PHP 代码，且必须以分号结尾。如果没有在代码字符串中调用return 语句，则返回 NULL。如果代码中存在解析错误，则 eval() 函数返回 false。



这道题需要使用system函数



关于system函数：

在PHP中，system函数是用来执行外部程序并显示输出的。这个函数与C语言中的system函数类似，它执行指定的命令并输出结果。如果PHP在服务器模块中运行，system函数还会尝试在每行输出完毕后自动刷新web服务器的输出缓存。要使用system函数，你需要提供要执行的命令作为参数。如果你还想获取命令执行后的返回状态，可以提供一个额外的参数来存储这个状态。system函数会返回命令输出的最后一行，如果执行失败则返回false。



上面可以直接传入代码、shell指令，语法还是前面?加DT=，接函数名system和()，()里的内容为’cat /flag’是代表cat根目录下的flag，中间空格不要忘，末尾有个分号“;”不要忘。之后就拿到flag了。

![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731665119589-381f9ef6-61c1-40b6-b9e6-32c42320644f.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50616406/1731665091479-155ecf7c-4f6e-419b-bb44-141f886e46f8.png)



# 四、总结
这次在BaseCTF2024新生赛中确实了解到了很多：flag的含义、HTTP、Hackbar、ladder、URL、百分号编码、<font style="color:rgb(24,24,24);">HTTP四种请求：“POST、GET、DELETE、PUT”、</font>Cookie、来源(Referer)、IP地址（Internet Protocol Address）、重定向(Redirect)、PHP<font style="color:rgb(51,51,51);">脚本语言</font>、eval函数、system函数的基本含义。同时也让我知道了互联网的世界真的很大，我现在掌握的知识还很少，需要不断学习进步。加油！

