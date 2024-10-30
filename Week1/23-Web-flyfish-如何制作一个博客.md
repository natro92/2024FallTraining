---
title: 如何制作一个博客
---

<h2 id="lc4ZI">工具准备</h2>

<font style="color:rgb(31, 31, 31);">1.Node.js</font>

<font style="color:rgb(31, 31, 31);">2.git</font>

<font style="color:rgb(31, 31, 31);">3.Hexo</font>

<font style="color:rgb(31, 31, 31);">4.文本编辑器(推荐VSCODE)</font>

<font style="color:rgb(31, 31, 31);">5.GitHub 帐号</font>

安装hexo

在桌面右键选择git bash，输入npm install -g hexo-cli

![image-20241029210320268](https://s2.loli.net/2024/10/29/gJk2KxV8jelCowv.png)

![image-20241029210402102](https://s2.loli.net/2024/10/29/f6w7Hc2iz3FTrdQ.png)



<h2 id="KWvKs">注册仓库</h2>

1. 点击new直接创建仓库

![image-20241029210459592](https://s2.loli.net/2024/10/29/o2q7CSk5MgFdVI6.png)

2. 命名仓库：用户名+.github.io
3. <font style="color:rgb(31, 31, 31);">勾选 Initialize this repository with a README 初始化一个</font><font style="color:rgb(31, 31, 31);"> </font>[<font style="color:rgb(31, 31, 31);">README.md</font>](http://readme.md/)<font style="color:rgb(31, 31, 31);"> </font><font style="color:rgb(31, 31, 31);">文件</font>
4. <font style="color:rgb(31, 31, 31);">点击 Creat repository 进行创建</font>

![image-20241029210518364](https://s2.loli.net/2024/10/29/ivZneDKt28VOSEH.png)





<h2 id="WNBEJ">git初始化</h2>

打开git bash

```shell
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱"
```

<font style="color:rgb(31, 31, 31);">通过</font>`<font style="color:rgb(244, 116, 102);">git config -l</font>`<font style="color:rgb(31, 31, 31);"> 检查是否配置成功。</font>

<h2 id="zMo6F"><font style="color:rgb(31, 31, 31);">连接github</font></h2>

1. 生成ssh公钥

在git中执行`ssh-keygen -t rsa -C "你的邮箱"`

<font style="color:rgb(31, 31, 31);">之后打开C盘下用户文件夹下的.ssh的文件夹，会看到 id_rsa.pub</font>

![](https://s2.loli.net/2024/10/29/6C1uxO4HfUVZsaE.png)

2. 打开<font style="color:rgb(31, 31, 31);"> id_rsa.pub，复制其中的内容</font>
3. <font style="color:rgb(31, 31, 31);">将 SSH KEY 配置到 GitHub</font>

<font style="color:rgb(31, 31, 31);">  进入github，点击右上角头像 选择</font>`<font style="color:rgb(244, 116, 102);">settings</font>`<font style="color:rgb(31, 31, 31);">，进入设置页后选择 </font>`<font style="color:rgb(244, 116, 102);">SSH and GPG keys</font>`

![image-20241029210556718](https://s2.loli.net/2024/10/29/4YKDhJboGNWptRU.png)

![](https://s2.loli.net/2024/10/29/CIzHXWBY7upDsjS.png)

title可以随便填，将密钥填入<font style="color:#df2a3f;">key</font>中，点击add ssh key即可

![image-20241029210637377](https://s2.loli.net/2024/10/29/1NynTP4JIs3gX8V.png)

4.测试

在git中输入`ssh -T git@github.com`，若有账户信息返回则连接完成

<h2 id="xIcfi">初始化Hexo项目</h2>

1. 选择一个文件夹，右键打开git bash 输入hexo init

![image-20241029210647230](https://s2.loli.net/2024/10/29/PT6ivwytUDLNpal.png)

2. 再输入hexo s 则运行可以在本地浏览器输入localhost:4000打开

![image-20241029210656410](https://s2.loli.net/2024/10/29/czUji1GedVhZYQo.png)



<h2 id="HPalj">将静态博客挂载到 GitHub Pages</h2>

1. 安装<font style="color:rgb(31, 31, 31);">hexo-deployer-git</font>

`<font style="color:rgb(31, 31, 31);">npm install hexo-deployer-git --save</font>`

2. <font style="color:rgb(31, 31, 31);">修改 _config.yml 文件，将deploy后的内容修改为如下图所示（注意缩进）</font>

```shell
deploy:
 type: git
 repository: #github项目地址
 branch: main
```

![image-20241029210734351](https://s2.loli.net/2024/10/29/I91EWBPrHG4JpMA.png)

![image-20241029210745046](https://s2.loli.net/2024/10/29/ziVLK1BZg3Ux58t.png)



3.<font style="color:rgb(31, 31, 31);">修改好配置后，运行如下命令，将代码部署到 GitHub。</font>

<font style="color:rgb(31, 31, 31);">如果出现</font>`<font style="color:rgb(244, 116, 102);">Deploy done</font>`<font style="color:rgb(31, 31, 31);">，则说明部署成功了。</font>

<font style="color:rgb(31, 31, 31);">稍等两分钟，打开浏览器访问：</font>[https://](https://fomalhaut-blog.github.io/)仓库名<font style="color:rgb(31, 31, 31);"> ，这时候我们就可以看到博客内容了。</font>

