参考文章：[https://blog.csdn.net/yaorongke/article/details/119089190](https://blog.csdn.net/yaorongke/article/details/119089190)

## 一、准备工作
##### 1. GitHub账号
需要有一个`GitHub`账号，没有的话到 [官网](https://github.com/) 申请一个。  
注册很简单，不懂的话可以参考 [GitHub申请账号](https://blog.csdn.net/yaorongke/article/details/119086305)

##### 2. 安装Git
在自己电脑上安装好`Git`，hexo部署到`GitHub`时要用。  
网上找篇教程或者参考 [Git安装(Windows)](https://blog.csdn.net/yaorongke/article/details/119085413)

##### 3. 安装NodeJS
在自己电脑上安装好`NodeJS`，`Hexo`是基于`NodeJS`编写的，所以需要安装`NodeJS`和`npm`工具。  
网上找篇教程或者参考 [NodeJS安装及配置(Windows)](https://blog.csdn.net/yaorongke/article/details/119084295)

## 二、创建仓库
进入github主页，创建仓库

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730385918068-d1f6a341-aa93-42fa-9266-d0e1bda08f40.png)

<font style="color:rgb(51, 51, 51);">填写仓库名，格式必须为<用户名>.github.io，然后点击Create Repository</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730385938041-e1b6f756-4408-4849-8309-6e551fa1c346.png)

<font style="color:rgb(51, 51, 51);">点击creating a new file创建一个新文件，作为我们网站的主页。 </font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730385953554-cf56128c-5b76-4caa-a4c5-4de4b664bcdb.png)

<font style="color:rgb(51, 51, 51);">新文件的名字必须为index.html，内容先随便写一个简单的，内容示例如下，填写之后点击Commit changes提交。</font>

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>Dre4m1nd</title>
</head>
<body>
    <h1>Dre4m1nd的个人主页</h1>
    <h1>Hello ~</h1>
</body>
</html>
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730386012700-b3d0b937-8a8f-47aa-a110-3792921a83ec.png)

<font style="color:rgb(51, 51, 51);">GitHub Pages中找到我们主页的地址为</font>[<font style="color:rgb(51, 51, 51);">https://dre4m1nd.github.io/</font>](https://dre4m1nd.github.io/)

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730386030058-f8426db0-4298-4327-bf04-a8e005528b8a.png)

<font style="color:rgb(51, 51, 51);">浏览器中访问，展示成功。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730386077402-3ad421eb-d8b2-401c-8c34-2bae03e65b07.png)

## 三、安装Hexo
我们采用`Hexo`来创建我们的博客网站，`Hexo` 是一个基于`NodeJS`的静态博客网站生成器，使用`Hexo`不需开发，只要进行一些必要的配置即可生成一个个性化的博客网站，非常方便。点击进入 [官网](https://hexo.io/zh-cn/)。

安装 `Hexo`

选定一个文件，之后空白处右键

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730386410402-1bf656a0-cca9-4bed-bae9-83a0d99cabac.png)

```bash
npm install -g hexo-cli
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730386427769-6c07b4c2-a605-4e5c-aae3-8511e5dffaab.png)

<font style="color:rgb(51, 51, 51);">查看版本</font>

```bash
hexo -v
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730386452258-87dc11be-7c3b-4361-9dfb-c1fe54964eee.png)

<font style="color:rgb(51, 51, 51);">创建一个项目 Blog-Hexo 并初始化</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730386467345-6eb7e3dc-0638-428b-8a03-693be7b4ed86.png)

<font style="color:rgb(51, 51, 51);">本地启动</font>

```bash
hexo g
hexo server
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730386506295-41e63916-be51-4f07-bddf-52eedc494c79.png)

<font style="color:rgb(51, 51, 51);">浏览器访问 </font>[<font style="color:rgb(51, 51, 51);">http://localhost:4000</font>](http://localhost:4000/)<font style="color:rgb(51, 51, 51);"> ，页面默认主图风格如下</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730386528505-8caf46e6-c7ff-4904-8221-08717c247130.png)

## 四、更换主题
### **Fluid主题**
**安装主题**

载 [最新 release 版本](https://github.com/fluid-dev/hexo-theme-fluid/releases) 解压到 `themes` 目录，并将解压出的文件夹重命名为 `Fluid`

**指定主题**

```bash
theme: Fluid  # 指定主题
language: zh-CN  # 指定语言，会影响主题显示的语言，按需修改
```

如下修改 `Hexo` 博客目录中的 `_config.yml`：

**<font style="color:rgb(51, 51, 51);">创建「关于页」</font>**

<font style="color:rgb(51, 51, 51);">首次使用主题的「关于页」需要手动创建：</font>

```bash
hexo new page about
```

<font style="color:rgb(51, 51, 51);">创建成功后，编辑博客目录下 /source/about/index.md，添加 layout属性。</font>

<font style="color:rgb(51, 51, 51);">修改后的文件示例如下：</font>

```markdown
---
title: about
date: 2024-10-28 21:09:02
layout: about
---

这里写关于页的正文，支持 Markdown, HTML
```

**<font style="color:rgb(51, 51, 51);">本地启动</font>**

```bash
hexo g -d
hexo s
```

<font style="color:rgb(51, 51, 51);">浏览器访问 </font>[<font style="color:rgb(51, 51, 51);">http://localhost:4000</font>](http://localhost:4000/)<font style="color:rgb(51, 51, 51);">，Fluid主题风格页面如下 </font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730387413937-51807e80-3a2b-4b1f-9934-1ec80f687057.png)

## 五、创建文章
如下修改 Hexo博客目录中的 _config.yml，打开这个配置是为了在生成文章的时候生成一个同名的资源目录用于存放图片文件。

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730387432001-6b462351-3118-48e4-973c-8e898c0602e5.png)

<font style="color:rgb(51, 51, 51);">执行如下命令创建一篇新文章，名为《测试文章》</font>

```bash
hexo new post 测试文章
```

**<font style="color:rgb(51, 51, 51);">本地启动</font>**

```bash
hexo g -d
hexo s
```

<font style="color:rgb(51, 51, 51);">浏览器访问 </font>[<font style="color:rgb(51, 51, 51);">http://localhost:4000</font>](http://localhost:4000/)<font style="color:rgb(51, 51, 51);">，页面如下，文章添加成功</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730387489186-1d91101a-843a-491e-a7e9-ba78380cd3a6.png)

## 六、个性化页面展示
页面的标题等位置显示默认的文字，可以改下显示一些个性化的信息。

##### 1. 浏览器tab页名称
修改根目录下 _config.yml 中的 title 字段。

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730387606869-a924e4f8-d641-4ca2-a180-fd006fc0d7a5.png)

<font style="color:rgb(51, 51, 51);">主题目录 themes\Fluid 下 _config.yml 中的 blog_title 字段。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730387643547-4242e807-6ef6-45e4-83ca-a4219c65f7d3.png)

<font style="color:rgb(51, 51, 51);">主题目录 themes\Fluid 下 _config.yml 中的 text 字段。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730387677379-124a3ae2-d207-4d27-b9b4-535d242d98a3.png)

<font style="color:rgb(51, 51, 51);">修改好配置后，页面效果如下，可以看到现在显示的内容变成了我们的个人信息。</font>

## <font style="color:rgb(51, 51, 51);">七、发布到GitHub Pages</font>
##### <font style="color:rgb(51, 51, 51);">方式一</font>
<font style="color:rgb(51, 51, 51);">安装hexo-deployer-git</font>

```bash
npm install hexo-deployer-git --save
```

<font style="color:rgb(51, 51, 51);">修改根目录下的 _config.yml，配置 GitHub 相关信息</font>

```bash
deploy:
  type: git
  repo: https://github.com/Dre4m1nd/dre4m1nd.github.io.git
  branch: main
  token: 输入自己的token
```

<font style="color:rgb(51, 51, 51);">其中 token 为 GitHub的 Personal access tokens，获取方式如下图</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730387769789-4efcdb14-b63b-40ce-a0b3-14a4ddecb0b9.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730387776316-05b08fd2-4a40-4d0b-b135-e2b9e25dec23.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730387783519-a1ed5dbf-b43e-4155-88c8-0235bc1de7cc.png)

第一次部署可能出现如下问题

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730387881586-cab83622-6c7f-4506-834a-97c3fd2d3053.png)

浏览器登录最后一个链接，会出现，登录页面，之后登录并允许就好了

参考文章:[https://blog.csdn.net/crisschan/article/details/136866137](https://blog.csdn.net/crisschan/article/details/136866137)



<font style="color:rgb(51, 51, 51);">浏览器访问 </font>[<font style="color:rgb(51, 51, 51);">https://dre4m1nd.github.io/</font>](https://dre4m1nd.github.io/)<font style="color:rgb(51, 51, 51);">，部署成功</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730387811475-cf4fe5c1-9e20-4531-a525-2281153b36a8.png)

##### 方式二
直接将 public 目录中的文件和目录推送至 GitHub仓库和分支中。

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730387929227-a0c157e9-36a2-422d-aa77-8799a36fd231.png)

