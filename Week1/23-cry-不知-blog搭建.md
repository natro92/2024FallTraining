<h3 id="fcd8d520"><font style="color:rgb(31, 35, 40);">本次搭建过程，参考此B站视频</font></h3>
[<font style="color:rgb(31, 35, 40);">https://www.bilibili.com/video/BV1Ju4m1c7WR/?spm_id_from=333.337.search-card.all.click</font>](https://www.bilibili.com/video/BV1Ju4m1c7WR/?spm_id_from=333.337.search-card.all.click)

<h3 id="4caccaf0"><font style="color:rgb(31, 35, 40);">一、Nodejs 与 Git 下载与安装</font></h3>
**<font style="color:rgb(31, 35, 40);">1.Nodejs下载</font>**

<font style="color:rgb(31, 35, 40);">官网链接：</font>[<font style="color:rgb(31, 35, 40);">nodejs官网</font>](https://nodejs.org/en/)

<font style="color:rgb(31, 35, 40);">（如果不想安装在自己的C盘，可以参考这篇修改nodejs缓存路径的文章：</font>[<font style="color:rgb(31, 35, 40);">参考文章</font>](https://www.cnblogs.com/liuqiyun/p/8133904.html)<font style="color:rgb(31, 35, 40);">）</font>

**<font style="color:rgb(31, 35, 40);">2.Git下载</font>**

<font style="color:rgb(31, 35, 40);">官网链接：</font>[<font style="color:rgb(31, 35, 40);">Git官网</font>](https://git-scm.com/downloads)

<font style="color:rgb(31, 35, 40);">一路默认点到底就行了。</font>

**<font style="color:rgb(31, 35, 40);">3.测试是否都安装成功</font>**

<font style="color:rgb(31, 35, 40);">管理员运行cmd，输入以下指令</font>

```plain
node -v 
npm -v （这个是node附带的）
git -v
```

<font style="color:rgb(31, 35, 40);">4.下载hexo</font>

```plain
npm install hexo-cli -g
```

<h3 id="ba3fad02"><font style="color:rgb(31, 35, 40);">二、仓库的搭建</font></h3>
<font style="color:rgb(31, 35, 40);">成功下载好工具后，接下来便可以着手搭建GitHub存储静态页面的仓库</font>

**<font style="color:rgb(31, 35, 40);"></font>**<font style="color:rgb(31, 35, 40);">测试是否成功</font>

<font style="color:rgb(31, 35, 40);">在gitbash中输入</font>

`<font style="color:rgb(31, 35, 40);">ssh -T git@github.com</font>`

<h3 id="4e13034e"><font style="color:rgb(31, 35, 40);">三、本地生成博客内容</font></h3>
<font style="color:rgb(31, 35, 40);">1.新建Blog文件夹，进入Blog文件夹，右击空白处然后点击Git bash Here，依次输入</font>

```plain
hexo init
hexo install
hexo g
hexo s
```

<h3 id="d4af7a21"><font style="color:rgb(31, 35, 40);">四、上线Blog</font></h3>
<font style="color:rgb(31, 35, 40);">1.进入Blog根目录文件，打开_config.yml文件</font>

<font style="color:rgb(31, 35, 40);">拉到最下面将deploy：后面的全删掉，复制粘贴以下内容：</font>

```plain
type: git
  repository: 
  branch: main
```

```plain
npm install hexo-deployer-git --save
hexo g(生成)
hexo d(上传)
```

<font style="color:rgb(31, 35, 40);">:::color1 如果是第一次使用git的话需要配置</font>

<font style="color:rgb(31, 35, 40);">1.</font>`<font style="color:rgb(31, 35, 40);">git config --global user.email "你的邮箱"</font>`

<font style="color:rgb(31, 35, 40);">2.</font>`<font style="color:rgb(31, 35, 40);">git config --global user.name "你的名字"</font>`

<font style="color:rgb(31, 35, 40);">配置完后，再</font>`<font style="color:rgb(31, 35, 40);">hexo g</font>`<font style="color:rgb(31, 35, 40);">上传</font>

<font style="color:rgb(31, 35, 40);">我们只需打开Blog根目录下的_config.yml文件，</font>

<font style="color:rgb(31, 35, 40);">将#Site下面按自己的需求填上。</font>

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

<font style="color:rgb(31, 35, 40);">  
</font><font style="color:rgb(31, 35, 40);"> </font>

<font style="color:rgb(31, 35, 40);">然后保存，打开git bash 生成、上传</font>

