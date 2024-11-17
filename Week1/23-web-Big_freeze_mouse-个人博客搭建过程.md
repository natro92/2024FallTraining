<h3 id="mwsvX">1. 安装Git和NodeJS</h3>
在Windows上使用Git，可以从Git官网直接 [链接](https://nodejs.org/en/download/)，然后按默认选项安装即可。安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！

<font style="color:rgb(77, 77, 77);">在Git中绑定</font>[Github](https://so.csdn.net/so/search?q=Github&spm=1001.2101.3001.7020)<font style="color:rgb(77, 77, 77);">账号，打开“Git Bash”，在命令框中依次输入两行命令：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730122176480-947a8af1-1f8f-40a6-843d-7245a1af0951.png)

<font style="color:rgb(77, 77, 77);">由于 Hexo 是基于 </font><font style="color:rgb(78, 161, 219) !important;">Node.js</font><font style="color:rgb(77, 77, 77);"> 驱动的一款博客框架，所以安装NodeJS</font>

<font style="color:rgb(77, 77, 77);">安装链接 </font>[**<font style="color:rgb(77, 77, 77);">链接</font>**](https://nodejs.org/en/download/)<font style="color:rgb(77, 77, 77);">  </font>

<font style="color:rgb(77, 77, 77);">安装之后可以输入以下命令查看是否安装成功</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730122594305-bc902a94-dc86-4732-91b6-799a315eadc4.png)

<h3 id="saDpL">2、创建仓库</h3>
<font style="color:rgb(77, 77, 77);">在Github上创建一个新的代码仓库用于保存我们的网页。</font>

<font style="color:rgb(77, 77, 77);">点击your repositories，进入仓库页面。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730190147327-0bc66877-0d78-48cc-a740-d2bb7d18f13b.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730190183954-4e313937-7c9f-4044-901f-f1d55928a3c8.png)

<font style="color:rgb(77, 77, 77);">填写仓库名，格式必须为<用户名>.github.io，然后点击Create repository。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730190254911-a63f9c5c-ac26-45aa-a3dd-13c21bb3a736.png)

<font style="color:rgb(77, 77, 77);">新文件的名字必须为index.html，内容先随便写一个简单的，内容示例如下，填写之后点击commit new file 提交。</font>

```plain
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>yaorongke</title>
</head>
<body>
    <h1>big_freeze_mouse的个人主页</h1>
    <h1>Hello ~</h1>
</body>
</html>

```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730190377399-e825e496-7828-4522-be0a-6e5c342964c2.png)

<font style="color:rgb(77, 77, 77);">GitHub Pages中找到我的主页的地址为 https://icemouse094.github.io/</font>

<h3 id="UIOS5"><font style="color:rgb(79, 79, 79);">3、安装Hexo</font></h3>
<font style="color:rgb(77, 77, 77);">准备好了就可使用 npm 开始安装 Hexo 了，在命令行输入执行如下命令：</font>

```plain
npm install -g hexo-cli
```

如果出现报错如图所示

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730186452554-fc2aa372-a5d2-4089-ab06-5329de5cb522.png)

可能是网络问题导致，可删除代理 输入下行代码可删除代理

```plain
npm config delete proxy
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730186646972-c3351174-cb1b-4a33-89f3-fadefef14a35.png)

输入代码

```plain
hexo -v
```

查看是否安装成功

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730186367731-f4a0a6b7-5ecd-4da0-bc33-50ca0662d2cb.png)

安装好后输入代码创建一个hexo-blog

```plain
hexo init hexo-blog
cd hexo-blog
npm install
```

本地启动

```plain
hexo g
hexo server
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730191968474-2a543708-b7b4-4b20-acfd-80e46da93df0.png)

<font style="color:rgb(77, 77, 77);">浏览器访问 http://localhost:4000，页面默认主图风格如下</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730191839764-fc8805cf-56ce-4ad2-9a2f-1f5444664a91.png)

<h3 id="lgDcL">4、更换主题</h3>
<h5 id="a40a0e93"><font style="color:rgb(79, 79, 79);">Fluid主题</font></h5>
<font style="color:rgb(77, 77, 77);">下载</font>[<font style="color:rgb(77, 77, 77);">链接</font>](https://github.com/fluid-dev/hexo-theme-fluid/releases)<font style="color:rgb(77, 77, 77);">解压到 themes目录，并将解压出的文件夹重命名为 fluid。</font>

**<font style="color:rgb(77, 77, 77);">指定主题</font>**

<font style="color:rgb(77, 77, 77);">如下修改 hexo 博客目录中的 _config.ymi：</font>

```plain
theme: fluid  # 指定主题
language: zh-CN  # 指定语言，会影响主题显示的语言，按需修改
```

**<font style="color:rgb(77, 77, 77);">创建「关于页」</font>**

<font style="color:rgb(77, 77, 77);">首次使用主题的「关于页」需要手动创建：</font>

```plain
hexo new page about
```

<font style="color:rgb(77, 77, 77);">创建成功后，编辑博客目录下 /source/about/index.md,添加 layout属性。</font>

<font style="color:rgb(77, 77, 77);">修改后的文件示例如下：</font>

```plain
---
title: about
date: 2020-02-23 19:20:33
layout: about
---

这里写关于页的正文，支持 Markdown, HTML
```

本地启动

```plain
hexo g -d
hexo s
```

<font style="color:rgb(77, 77, 77);">浏览器访问 http://localhost:4000，fluid主题风格页面如下</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730252562919-b1ef5656-7d08-4b79-ac8d-ed0055b148dd.png)

<h3 id="u3CvN">5、创建文章</h3>
<font style="color:rgb(77, 77, 77);">如下修改 Hexo 博客目录中的 _config.yml，打开这个配置是为了在生成文章的时候生成一个同名的资源目录用于存放图片文件</font>

```plain
post_asset_folder: true
```

<font style="color:rgb(77, 77, 77);">执行如下命令创建一篇新文章，名为《测试文章》</font>

```plain
hexo new post 巨龙是个蛋
```

 ![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730274152080-2ec9b341-276a-4213-bf48-b249aedbb0e9.png)

打开'巨龙是个蛋.md'编辑

 ![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730274287787-326f7cfc-5843-43d9-bef7-b9155e33a99e.png)

并且找到D:\IT tools\hexo-blog\themes\fluid\source\img目录下放入图片

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730274361536-17ad2953-da4d-44f1-a31a-2dd12ae732e9.png)

<font style="color:rgb(79, 79, 79);">  </font>**<font style="color:rgb(77, 77, 77);">本地启动</font>**

```plain
hexo g -d
hexo s
```

<font style="color:rgb(77, 77, 77);">浏览器访问 http://localhost:4000，页面如下，文章添加成功</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730274499814-3af90422-80f2-480a-9682-e6fcf3e4e47a.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730274509461-33188dfe-2e62-4548-a688-db26f4820dc9.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730274476134-842e822f-5646-4d33-9479-614a8b7406a9.png)

<h3 id="H2pdC"><font style="color:rgb(79, 79, 79);">六、个性化页面展示</font></h3>
<font style="color:rgb(77, 77, 77);">页面的标题等位置显示默认的文字，可以改下显示一些个性化的信息。</font>

<h5 id="7c67931a"><font style="color:rgb(79, 79, 79);">1. 浏览器tab页名称</font></h5>
<font style="color:rgb(77, 77, 77);">修改根目录下 _config.yml中的 title 字段。</font>  
 ![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730253575896-91c51c43-ad97-4ad9-b044-b8dfa7d2e890.png)

<h5 id="94886889"><font style="color:rgb(79, 79, 79);">2. 博客标题</font></h5>
<font style="color:rgb(77, 77, 77);">主题目录 themes/fluid 下 _config.yml 中的 blog_title 字段。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730253745291-dbeefeb7-57a9-492e-9037-b6d3d7d9bb39.png)

<h5 id="67ba6a3b"><font style="color:rgb(79, 79, 79);">3. 主页正中间的文字</font></h5>
<font style="color:rgb(77, 77, 77);">主题目录 themes/fluid 下 _config.yml 中的 text 字段。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730253892818-596b6105-ad0b-41d6-90c3-e85a2a1ad461.png)

<font style="color:rgb(77, 77, 77);">修改好配置后，页面效果如下，可以看到现在显示的内容变成了我们的个人信息</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730253934649-89676fdf-1478-42a9-aae6-dae242b1e946.png)

<h4 id="XZPIo">4、<font style="color:rgb(44, 62, 80);">页面顶部大图</font></h4>
<font style="color:rgb(44, 62, 80);">主题配置中，每个页面都有名为 banner.img的属性，可以使用本地图片的相对路径，也可以为外站链接，比如：</font>

<font style="color:rgb(44, 62, 80);">指向本地图片：</font>

```plain
banner_img: /img/bg/example.jpg   # 对应存放在 /source/img/bg/example.jpg
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730273651176-d9072fe0-311f-450d-b755-7c4664dae123.png)

再修改对应的_config.yml即可

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730273697692-e7818c0c-3a03-464e-baf1-74a6347deab4.png)

效果如下

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730273727036-04c4a869-c70f-40dc-a24a-3d6139fbe587.png)

<h4 id="o3HIQ">5、关于页</h4>
<font style="color:rgb(44, 62, 80);">首次使用主题的「关于页」需要手动创建：</font>

```plain
$ hexo new page about
```

<font style="color:rgb(44, 62, 80);">创建成功后修改 </font><font style="color:rgb(77, 77, 77);">/source/about/index.md</font><font style="color:rgb(44, 62, 80);">，添加 layout 属性。</font>

<font style="color:rgb(44, 62, 80);">修改后的文件示例如下：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730275038306-b1090d24-468b-4b72-8f34-7a0945250d29.png)

<font style="color:rgb(44, 62, 80);">在关于页介绍自己的基础信息，可以在</font>**<font style="color:rgb(44, 62, 80);">主题配置</font>**<font style="color:rgb(44, 62, 80);">中设置：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730275400803-d4732a65-0603-4bb1-978f-e0cd5f13d6c0.png)

效果如下

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730275441214-c3f8b9d6-0030-4084-a2fd-84b330351a92.png)

<h3 id="ojT4M"><font style="color:rgb(79, 79, 79);">七、发布到GitHub Pages</font></h3>
<font style="color:rgb(77, 77, 77);">安装hexo-deployer-git</font>

```plain
npm install hexo-deployer-git --save
```

<font style="color:rgb(77, 77, 77);">修改根目录下的 _config.yml，配置 Github 相关信息</font>

```plain
deploy:
  type: git
  repo: https://github.com/icemouse094/icemouse094.github.io.git
  branch: main
  token:
```

<font style="color:rgb(77, 77, 77);">其中 token 为 Github 的 personal access tokens获取方式如下图</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730286061607-ca21af95-7ecd-4c4f-bae6-b0a3af843d0d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730388930492-982843a8-b4b4-423d-ab9a-124b29f51aeb.png)

<font style="color:rgb(77, 77, 77);">部署到GitHub</font>

```plain
hexo g -d
```

浏览器访问<font style="color:rgb(77, 77, 77);">https://icemouse094.github.io/ 部署成功</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1730286705380-26555cef-089c-451b-a153-25ff2e14c202.png)

<font style="color:rgb(77, 77, 77);"></font>



