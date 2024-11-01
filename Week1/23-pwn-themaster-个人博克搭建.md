### 本次搭建过程，参考此B站视频。
[【零成本】Hexo个人博客搭建教程 | 无需服务器](【【零成本】Hexo个人博客搭建教程%20|%20无需服务器】https://www.bilibili.com/video/BV1Ju4m1c7WR?vd_source=44d516fa899c3efd5e14f2d6c8a13d12)

---

### 一、Nodejs 与 Git 下载与安装
**1.Nodejs下载**

官网链接：[nodejs官网](https://nodejs.org/en/) 

（如果不想安装在自己的C盘，可以参考这篇修改nodejs缓存路径的文章：[参考文章](https://www.cnblogs.com/liuqiyun/p/8133904.html)）

**2.Git下载**

官网链接：[Git官网](https://git-scm.com/downloads)

一路默认点到底就行了。

**<u><font style="color:#000000;">3.测试是否都安装成功</font></u>**

<font style="color:#000000;">管理员运行cmd，输入以下指令</font>

```plain
node -v 
npm -v （这个是node附带的）
git -v
```

如果见到以下输出，那么说明安装没有问题。

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1730377002063-dce00e00-d44a-4c6e-859b-08674d2d0fcc.png)

4.下载hexo

```plain
npm install hexo-cli -g
```



### 二、仓库的搭建
成功下载好工具后，接下来便可以着手搭建GitHub存储静态页面的仓库

**1.注册/登录**[GitHub](https://github.com/)

点击Create a new repository进入新建仓库页面

仓库名输入：

```plain
用户名.github.io
```

勾选Public

勾选Add a README file

点击Create创建

**2.生成SSH Keys**

进入任意文件夹，右键空白处然后点击Git bash here，输入

```plain
ssh-keygen -t rsa -C "邮箱地址"
```

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1730379132240-7a966b49-70d4-42b3-b850-af41e2acda47.png)

然后进入C:/用户/Lenovo/.ssh（我电脑上的路径是这样的，在生成密钥的时候，git bash 列表会有密钥路径的，可自行查找）

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1730433544124-a5f62f84-5537-48d0-b065-73a07ae00bfa.png)

用记事本打开id_rsa.pub，复制里面的全部代码

在GitHub中进入用户设置，

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1730379169481-c28912f2-40fd-4586-bb30-adc65839358a.png)

找到SSH Keys，新建SSH Keys，将刚才的代码粘贴到下方框框里，点击创建。

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1730379373279-83bd5c84-3757-4358-abce-038ce8804341.png)

最后测试是否成功

在gitbash中输入

`ssh -T git@github.com`

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1730386303008-2d733e6b-4c1d-4088-9fca-75e7d2af2c5d.png)

### 三、本地生成博客内容
1.新建Blog文件夹，进入Blog文件夹，右击空白处然后点击Git bash Here，依次输入

```plain
hexo init
hexo install
hexo g
hexo s
```


**第一步如果出现：command not find，试着输入npx hexo init**

一切没问题的话，就可以复制生成的链接进入浏览器看到我们生成的本地服务器了。

回到命令行，输入Ctrl + c 结束进程。

### 四、上线Blog
1.进入Blog根目录文件，打开_config.yml文件

拉到最下面将deploy：后面的全删掉，复制粘贴以下内容：

```plain
  type: git
  repository: 
  branch: main
```

:::color4
一定要注意缩进格式，每一行前都有两个空格，每个冒号后也都有一个空格！！！

:::

	2.前往github之前生成的仓库页面，点code，复制https链接，将其复制到`repository: `后面

 然后保存退出，![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1730387181458-6c72989e-4d47-486c-a817-89bd61e876dd.png)

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1730387201708-3f8208a8-1204-4db2-88c2-135262f3a419.png)

3.回到Blog文件夹，git bash，安装自动部署发布工具

```plain
npm install hexo-deployer-git --save
hexo g(生成)
hexo d(上传)
```

:::color1
如果是第一次使用git的话需要配置

1.`git config --global user.email "你的邮箱"`

2.`git config --global user.name "你的名字"`

配置完后，再`hexo g`上传

（参考文章说会跳出来github登录界面，但不知道为什么我配置的时候没有跳出登录界面，但影响不大）

:::

4.这样，我们就成功把本地内容上传到github了，上传成功以后就算搭建好了！

可以自行前往对应网址查看(网址：`用户名.github.io`）



### 五：网站资料的修改
我们的博克标题默认是hexo，整个页面是初始默认的。

我们只需打开Blog根目录下的_config.yml文件，

将#Site下面按自己的需求填上。

```plain
## Site
title: 标题
subtitle: 副标题
description: 描述
keywords: 关键词
author: 站主
language: 语言（可以填写zh-CN）
timezone: 时区（可以填写Asia/Shanghai）

```

然后保存，打开git bash 生成、上传。



### <font style="color:#ED740C;">Congratulations！！!</font>
### <font style="color:#ED740C;">至此，我们已经成功搭建好基本的博客了，剩下的就是对博客的优化和个性化了:)</font>
