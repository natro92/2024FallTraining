> 参考教程：
>
> [【零成本】Hexo个人博客搭建教程 | 无需服务器_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Ju4m1c7WR/?vd_source=72840f1407bd8d539300fcc6c32c4fc5)
>
> [如何用Hexo搭建个人博客? (fiveth.cc)](https://blog.fiveth.cc/p/bb32/)   
>

博客地址https://floatingraft.github.io/
### 一、工具
##### 1.Github账号
首先需要有一个Github账号，没有的在Github官网上注册一个[GitHub · Build and ship software on a single, collaborative platform · GitHub](https://github.com/)

（ps：Github有时候可能会打不开或者加载速度慢，可以用Watt Toolkit加速）

##### 2.NodeJS
NodeJS官方下载链接[Node.js — 下载 Node.js® (nodejs.org)](https://nodejs.org/zh-cn/download/prebuilt-installer)

NodeJS的安装过程可以查看这篇文章[Node.js安装及环境配置之Windows篇 - 刘奇云 - 博客园 (cnblogs.com)](https://www.cnblogs.com/liuqiyun/p/8133904.html)，最好是安装最新版的NodeJS

##### 3.Git
Git官方下载链接[Git - Downloads (git-scm.com)](https://git-scm.com/downloads)

后续我们对hexo进行部署需要用到Git，安装过程可以参考这篇[Windows系统Git安装教程（详解Git安装过程） - 学为所用 - 博客园 (cnblogs.com)](https://www.cnblogs.com/xueweisuoyong/p/11914045.html)，同样也是安装最新版的

##### 4.Hexo
在安装hexo之前，我们先检测一下是否全部下载安装成功

管理员身份运行cmd，输入以下指令

```plain
node -v
npm -v #这是nodejs自带的
git -v
```

若正常输出版本号则代表安装成功

接下来我们要下载安装hexo，在cmd中输入

```plain
npm install hexo-cli g
```

如果无法安装，则改为输入

```plain
npx install hexo
```

后续若还有npm报错的，一律改为npx

至此，已经完成了工具的准备

### 二、搭建仓库
接下来准备在Github中搭建用于存放静态页面的仓库

首先登录Github，在右上角点击加号，选择New repository

仓库名输入“用户名.github.io”

勾选Public，勾选add a readme file，然后Create创建

### 三、生成ssh key
电脑任意位置右键空白处，选择git bash here，输入

```plain
ssh-keygen -t rsa -C "你的邮箱"
```

敲4下enter

然后进入C:\Users\用户名，找到.ssh文件夹

:::color3
**<font style="color:#DF2A3F;">注意，用户文件夹名称必须是英文，不能包含中文！！</font>**

**<font style="color:#DF2A3F;">若用户文件夹名为中文，删除.ssh文件夹，修改文件夹名后再重复上面生成.ssh的步骤</font>**

**<font style="color:#DF2A3F;">具体修改用户文件夹名称的方法笔者没找到，更简便的方法是新建一个用户</font>**

:::

然后用记事本打开id_rsa.pub文件，全选文本内容复制

然后打开github，右上角点击个人头像，选择“Settings”

在左侧边栏找到“SSH and GPG keys”，新建一个ssh key，名称随意，在“Key”栏粘贴刚刚的文本内容

然后点击“Add SSH key”

接着我们测试一下是否创建成功

如果有开Watt Toolkit一类加速器先关闭，避免报错

在git bash中输入

```plain
ssh -t git@github.com
```

### 四、本地生成Blog内容
自选位置新建Blog文件夹，在文件夹中打开git bash，输入

```plain
hexo init
```

如果报错command not found，则改为输入

```plain
npx hexo init
```

然后输入

```plain
hexo install
```

接着依次输入

```plain
hexo g
hexo s
```

现在我们就可以按住Ctrl键单击输出的链接访问我们的本地服务器了

关闭网站后记得Ctrl+C关闭本地服务器

### 五、上线Blog
进入Blog文件夹，用记事本打开_config_yml

拉到最底部，把deploy后面的全部删掉，复制粘贴这段

```plain
  type: git
  repository: 
  branch: main
```

:::color3
**<font style="color:#DF2A3F;">注意缩进格式，每行前面两个空格不要删，冒号后的一个空格也不要删</font>**

:::

<font style="color:#000000;">打开github，打开我们新建的仓库“用户名.github.io”，右上角点击Code，复制https链接</font>

<font style="color:#000000;">将其粘贴至我们的_config.yml文件中</font><font style="color:#000000;background-color:#FBDE28;">repository: </font><font style="color:#000000;">后面（</font>**<font style="color:#DF2A3F;">冒号后面的空格不要删！！！</font>**<font style="color:#000000;">）</font>

<font style="color:#000000;">保存后退出，在Blog文件夹打开git bash，输入以下指令安装自动部署发布工具</font>

```plain
npm install hexo-deployer-git --save
```

<font style="color:#000000;">然后依次输入</font>

```plain
hexo g
hexo d
```

:::color3
如果是第一次使用git的话需要配置

git config --global user.email "你的邮箱"  
git config --global user.name "用户名"

配置完后再hexo d上传

在跳出来的窗口进行登录

:::

至此，个人Blog已初步搭建完成

在浏览器内输入“用户名.github.io”，就可以访问我们的个人Blog主页了！

