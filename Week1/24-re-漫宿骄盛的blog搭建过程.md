<h1 id="dLMth">漫宿骄盛的blog搭建过程</h1>
<h2 id="USmDZ">结果：</h2>
www.msjs.fun

刚搭好，还没装饰，现在还是工地没法看。

<h2 id="j4UsV">正文:</h2>
我是按照[https://www.bilibili.com/video/BV1ac411B7Li/](https://www.bilibili.com/video/BV1ac411B7Li/)这个视频操作的，基本是傻瓜式操作。

<h4 id="UF2t2">第一步：软件准备</h4>
下载wordpress

<font style="color:rgb(24, 25, 28);">官网：https://cn.wordpress.org/</font>

<h4 id="PLmzu"><font style="color:rgb(24, 25, 28);">第二步：</font>域名购买</h4>
我的是去腾讯云买的，花了10块，续费65。

我想要的要么被注册了，要么就是溢价域名买不了，于是就买了这个。PS：华为云和腾讯云都有活动，可以1块钱买一年域名。

<h4 id="MK3R5">第三步:服务器购买</h4>
我的服务是用<font style="color:rgb(24, 25, 28);">彩虹云</font>

<font style="color:rgb(24, 25, 28);">官网：https://www.cccyun.net/</font>

<font style="color:rgb(24, 25, 28);">服务器位于香港，不用备案，8块钱一个月，一次性买一年的有优惠，按10月计算价格。</font>

<h4 id="ockX2"><font style="color:rgb(24, 25, 28);">第四步：解析域名</font></h4>
点开你买域名的平台的控制台。

我在腾讯云买的，所以以腾讯云为例。

找到这个界面

![](https://cdn.nlark.com/yuque/0/2024/png/49936689/1730188866094-729e6965-fb1a-4c7d-970b-e71280550120.png)

点击解析

![](https://cdn.nlark.com/yuque/0/2024/png/49936689/1730188981432-9cdf66cb-b05f-49cb-afb1-5bd5a045666a.png)

然后点击添加记录就会得到我这张图片

主机记录和记录类型就按我这个配置，权重，优先级，TTL都不用管。

现在要配置记录值

打开你买服务器的控制面板

![](https://cdn.nlark.com/yuque/0/2024/png/49936689/1730189171145-0455c6fd-19fe-4806-b368-9970649835fc.png)

点击基本功能，然后点域名绑定

![](https://cdn.nlark.com/yuque/0/2024/png/49936689/1730189239684-6bb634bd-b7d3-4eff-9c5b-da6dc9bc0fa2.png)

将我打码且框住的东西复制到记录值那里，然后点确定就可以。

<h4 id="CXjOJ">第五步：安装wordpress到你的服务器</h4>
来到这里

![](https://cdn.nlark.com/yuque/0/2024/png/49936689/1730189963155-8c9787f6-9b7c-4663-ac18-047ef54248af.png)

将你下载的wordpress文件上传到www.root的文件里（这个过程可能有些漫长），然后解压——》点击反选——》剪切——》回到上级目录——》粘贴——》现在就可以把压缩包和wordpress文件删掉了。

<h4 id="OezQw">第六步：配置服务器</h4>
回到这里

![](https://cdn.nlark.com/yuque/0/2024/png/49936689/1730189239684-6bb634bd-b7d3-4eff-9c5b-da6dc9bc0fa2.png?x-oss-process=image%2Fformat%2Cwebp)

将你的域名按www.xxxx.xx的格式输入到域名里，然后点击确定。

然后在浏览器地址框里去输入你的网址

![](https://cdn.nlark.com/yuque/0/2024/png/49936689/1730189582386-e9b5586c-991e-4597-9c04-a8a3e41df3d4.png)

出现这个，基本上就要配置好了。

![](https://cdn.nlark.com/yuque/0/2024/png/49936689/1730189636650-14a61b24-023f-4688-b0ea-4052d1fe795a.png)

这里面的东西都按这里的一个一个输进去

![](https://cdn.nlark.com/yuque/0/2024/png/49936689/1730189750987-677192cd-168a-4dd1-84e3-9431ee99b26f.png)

输入好之后执行安装程序

![](https://cdn.nlark.com/yuque/0/2024/png/49936689/1730189836463-9890c546-a3f5-48ff-8eb7-5eb7f3ed5202.png)

配置好你的管理员账号

然后，你会进入这个界面

![](https://cdn.nlark.com/yuque/0/2024/png/49936689/1730190293695-9e479232-729b-4378-bdeb-2cd984668b82.png)

点击外观，点击编辑，就可以设计你的blog了！

<h2 id="ijbs0">拓展：</h2>
你可能注意到，你的网站在左上角是显示不安全，这时，你就需要配置SSL证书，来使用HTTPS.

![](https://cdn.nlark.com/yuque/0/2024/png/49936689/1730190471364-651079d0-8f4e-435a-9fe8-87ab7447c391.png)

来到这里

拉到最下面，有个帮助文档

![](https://cdn.nlark.com/yuque/0/2024/png/49936689/1730190501683-ee09e633-032d-4324-a206-d74880df0945.png)

按他的一步一步来就可以，配置好后，你的博客就显示安全了。

<h1 id="Qf7uR">祝你配置顺利！</h1>
