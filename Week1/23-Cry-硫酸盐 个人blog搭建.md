---

我的博客：[https://monologuue.github.io/](https://monologuue.github.io/)

搭建hexo看的视频：[https://www.bilibili.com/video/BV1Ju4m1c7WR/?spm_id_from=333.337.search-card.all.click&vd_source=802632f153a1ecb77319c61fa70dccdf](https://www.bilibili.com/video/BV1Ju4m1c7WR/?spm_id_from=333.337.search-card.all.click&vd_source=802632f153a1ecb77319c61fa70dccdf)

美化主题看的视频：[https://www.bilibili.com/video/BV1JP411P7gc/?spm_id_from=333.337.search-card.all.click&vd_source=802632f153a1ecb77319c61fa70dccdf](https://www.bilibili.com/video/BV1JP411P7gc/?spm_id_from=333.337.search-card.all.click&vd_source=802632f153a1ecb77319c61fa70dccdf)

---

# 一、搭建hexo


1、下载好 <font style="color:rgb(0, 0, 0);">nodejs、</font> git



**2、在github上搭建仓库**

![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1730270873702-28b07cb2-b2d4-4c92-a496-675528ad9f3e.png)



**3、生成SSH Keys**

在Git bash here上输入

```plain
ssh-keygen -t rsa -C "邮件地址"
```

回车，然后找到id_rsa.pub文件，复制其中内容到github账号的SSH keys上



![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1730271180861-f50fceb1-1595-4bd3-94ba-7a60fc2afab6.png)

在Git bash here上输入

```plain
ssh -T git@github.com
```

回车，然后输入yes，回车。



**4、下载hexo**

在Git bash here上依次输入

```plain
npm install hexo-cli -g
hexo init
hexo install
hexo g
hexo s
```

然后ctrl+c关闭本地服务器



**5、上线博客**

打开_config.yml文件，拖到最底下，把deploy下面的内容改成这样

```plain
deploy:
  type: git
  repository: git@github.com:Monologuue/Monologuue.github.io.git
  branch: main
```

repository后面的内容为github上的仓库页面的code

![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1730272126760-d40a08f5-5852-4ffd-9f79-7f59332f56e9.png)



**6、部署博客**

在博客文件夹中打开Git bash here，依次输入

```plain
npm install hexo-deployer-git --save
hexo g
hexo d
```

这样就搭建好了毛坯房，博客页面是默认的hexo

---

# 二、美化主题


**1、安装butterfly主题和需要用到的渲染器**

在博客文件下打开Git bash here，依次输入

```plain
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

修改_config.yml<font style="color:rgba(0, 0, 0, 0.75);">文件，拖到最下面，找到theme，把后面内容改成butterfly，记得保存</font>

![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1730273562555-244faf1a-ebab-4119-b6f5-91f4f9e40a41.png)

<font style="color:rgb(77, 77, 77);">然后把themes文件夹中的 _config.yml 重命名为 _config.butterfly.yml，复制到博客的根目录下与_config.yml同级，像这样</font>![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1730274946392-c2c423f5-47b1-41c8-8bf4-2b6998e34a07.png)

<font style="color:rgba(0, 0, 0, 0.75);"></font>

**<font style="color:rgba(0, 0, 0, 0.75);">2、然后就是跟着视频教程，我也写不出来，东西太多了，修改主题不难，就是很麻烦，如果看得懂英文就没什么太大的问题</font>**

<font style="color:rgb(77, 77, 77);">主题配置文件（最好用vscode打开） _config.butterfly.yml</font>

<font style="color:rgba(0, 0, 0, 0.75);">这其中有许多供人选择的模块，有想要的就在enable: 后加上true,相反就加false，例如</font>

![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1730274234547-710ef64a-643b-4197-b476-fe167a5b6c48.png)

我选择鼠标点击效果是出现爱心



![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1730274332545-b2626989-7a53-4bc2-ae74-6c9780eabfbe.png)

想要添加图片就在对应位置添加图片的路径



![](https://cdn.nlark.com/yuque/0/2024/png/47928994/1730274456525-c14c58fe-7b7d-4ad1-bd4e-86db08f638c7.png)

像这些如果有需要的功能可以把注释的 # 去掉



**3、应用主题**

在博客文件下打开Git bash here，依次输入

```plain
hexo cl
hexo g
hexo d
```

如果博客页面没有变换，就多等几分钟，延迟挺大的

