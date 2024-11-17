<meta name="referrer" content="no-referrer" />

# Q1uJu-Blog搭建过程

## Blog地址

q1uju.cc或www.q1uju.cc

## 博客搭建流程

搭建过程参照Fomalhaut大佬的b站搭建教程，使用Hexo作为博客框架。

链接：https://space.bilibili.com/220757832/channel/collectiondetail?sid=886469&spm_id_from=333.788.0.0

ps：搭建过程中忘记截图记录了，有些过程可能没有图片。

### 前置工作

本地已经安装了nodejs、git，直接在GitHub中创建一个新仓库。

![image-20241030203558852](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241030203558852.png)

用以下git命令生成ssh公钥，用于计算机与Github连接。

```shell
ssh-keygen -t rsa -C "邮箱地址"
```

执行完生成命令后在C盘用户文件夹下的.ssh文件夹看到id_rsa.pub文件，将里面内容复制并粘贴至GitHub里配置SSH KEY。

![image-20241030204054915](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241030204054915.png)

输入以下命令测试连接，出现连接到账户的信息即为完成。

```shell
ssh -T git@github.com
```

### 安装Hexo

在Git BASH输入如下命令安装Hexo。

```shell
npm install -g hexo-cli
```

安装完后输入`hexo -v`验证是否安装成功。

![image-20241030204927475](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241030204927475.png)

在目标路径执行下方命令初始化Hexo项目。

```shell
hexo init hexo
```

进入hexo，输入`npm i`安装相关依赖。安装完以来后，输入`hexo server`或者`hexo s`启动项目。打开浏览器，输入地址：http://localhost:4000/，可看到初始构建好的博客。

### 将静态博客挂载到GitHub Pages

执行如下命令安装hexo-deployer-git。

```shell
npm install hexo-deployer-git --save
```

修改_config.yml文件最后一部分。

```yaml
deploy:
  type: git
  repository: git@github.com:Q1uJu/Q1uJu.github.io.git
  branch: main
```

修改好配置后运行如下命令，将代码部署到GitHub。

```shell
hexo clean && hexo generate && hexo deploy  // Git BASH终端
hexo clean; hexo generate; hexo deploy  // VSCODE终端
```

出现`Deploy done`说明部署成功。

### Vercel部署与自定义域名

按流程使用Vercel部署完Hexo项目，在西部数码购买域名q1uju.cc，并绑定Vercel自定义域名。

![image-20241101192710963](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241101192710963.png)

### 博客界面主题及美化

在博客根目录打开Git BASH，输入下方代码安装butterfly主题。

```shell
npm i hexo-theme-butterfly
# 未安装pug和stylus的渲染器还需再安装这两个渲染器
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

在站点配置文件`_config.yml`中，把主题改为butterfly。

```yaml
theme: butterfly
```

剩余美化流程均根据Fomalhaut佬的教程更改，具体美化情况由博客界面直接展示。

## 图床搭建流程

搭建过程参照https://cloud.tencent.com/developer/article/1766197，使用PicGo插件，Gitee作为图床。

使用原有的Gitee账号登录，并在Gitee中新建一个项目。

![](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241030202224483.png)

![image-20241030202224483](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241030202224483.png)

在设置中生成一个新的私人令牌并保存token。

![image-20241030202302296](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241030202302296.png)

![image-20241030202315756](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241030202315756.png)

下载安装PicGO。

PicGo链接：https://github.com/Molunerfinn/PicGo/releases

我选择的是当前最新的稳定版2.3.1。

![image-20241030202530570](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241030202530570.png)

下载Gitee上传的插件。

![image-20241030202602864](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241030202602864.png)

打开PicGo，找到图床设置中的Gitee并填写相关信息。

- repo：用户名/项目（不用带http协议及.git）
- token：新建私人令牌中复制的token

![image-20241101192747232](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241101192747232.png)

点击[**设置默认图床**]

打开Typora，文件->偏好设置->图像。

![image-20241030202907568](https://gitee.com/Q1uJu/picture_bed/raw/master/image-20241030202907568.png)

设置好后点击[**验证图片上传选项**]，图床搭建及设置完成。
