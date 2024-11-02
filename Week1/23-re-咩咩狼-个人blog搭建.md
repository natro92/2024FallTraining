# 一、下载git
<font style="color:rgb(85, 85, 85);">git下载网址：</font>[Git官网](https://git-scm.com/)

<font style="color:rgb(85, 85, 85);">配置环境变量信息，打开系统环境变量中的Path变量，添加本地环境安装目录</font>

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730516560307-c03e8d62-4198-4147-96fc-93d168295b47.png)

# 二、下载node.js
node.js下载网址：[Node.js — Run JavaScript Everywhere](https://nodejs.org/en/)

下载过程一路next就好了

## 遇到的问题：
在安装hexo的过程中总是报错，尝试过使用管理员权限、清除缓存、强制执行，但始终没有办法安装hexo，以下是两个典型报错图片

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730516227706-b299932d-441b-46ec-9f50-8edd4aa75855.png)

### ![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730516287760-64c8aa12-371b-48f0-a849-fc10360ce776.png)
## 解决方法
最后发现有可能是初始node.js版本过高，在node.js官网中下载更换node-v20.18.0-x64版本后，问题得以解决

# Hexo安装
## 安装hexo环境
```plain
npm install -g hexo-cli
```

直接在桌面打开git bash here，输入以上代码，安装hexo环境

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730516903300-917ab8bb-dc72-4f1f-85c3-5b682d6eabf0.png)

## 初始化
在本地文件夹处右键git bash here，输入以下指令：

```plain
hexo init
```

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730516996578-c9ce55da-1e33-47f5-bec6-9e87f048d71b.png)

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730517019496-2b859c75-aad9-46f2-9d33-13b53fd124e1.png)

可以看到，本地blog初始化已经完成

## hexo文件介绍
+ public 最终所见网页的所有内容
+ node_modules 插件以及hexo所需node.js模块
+ _config.yml 站点配置文件，设定一些公开信息等
+ package.json 应用程序信息，配置hexo运行所需js包
+ scaffolds 模板文件夹，新建文章，会默认包含对应模板内容
+ themes 存放主题文件，hexo根据主题生成静态网页
+ source 用于存放用户资源

用hexo生成的时候报错，显示没有权限，在网上查找方法之后提权了一下

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730522937774-12b2a3f4-e62c-4bd0-9ec6-3ff1ad11a811.png)

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730522950300-90d53e86-c7d5-4f22-a77e-fc263c75770d.png)

然后设置主题，安装hexo插件

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730523663093-e0a94108-b8cf-4006-9ed7-5d35f70f33c2.png)

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730523638927-218c84a7-c551-4219-b0d0-76be71bfc4e1.png)

配置git的SSH密钥

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730524779633-5d94c527-acb4-4ccd-a44f-c0b831690ea1.png)

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730524686925-d7ef2a69-d73e-4a32-b6f5-bcd9c60e6365.png)

远程连接成功

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730524973039-09e5db46-8c08-4248-bdd6-b59136608d07.png)

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730532697677-f3ac3295-84f3-4cf3-86bc-b0af4a1255aa.png)

然后初始化本地blog，将本地与GitHub page进行远程连接

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730526467851-91a02bb3-d974-4b86-a6da-0da825a463ca.png)

跟着教程上传到GitHub

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730527344226-85cab46f-5889-4ef3-8276-0b523277569d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730527411733-5ca55629-f225-4ff3-a1f9-aa9e685deabf.png)

GitHub page就配置完成了

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730527798853-d4e887dc-049a-4506-9957-f7a0be22981a.png)

![](https://cdn.nlark.com/yuque/0/2024/png/48895444/1730527883408-964bff1c-ab76-4900-84cb-3431968b17cf.png)



git bash 的操作

新建文档：hexo new "文章标题"

	地址位于source --> posts

生成网页：hexo g

运行博客：hexo s



git bash 与 github之间的操作

添加文件：git add .

提交本地仓库：git commit -m "commit-messages"

提交远程仓库：git push origin main

拉取远程文件：git pull

创建本地分支：git branch temp

强制推送：git push origin main --force

# 参考链接
[https://pdpeng.github.io/2022/01/19/setup-personal-blog/](https://pdpeng.github.io/2022/01/19/setup-personal-blog/)

### <font style="color:rgb(85, 85, 85);"></font>
