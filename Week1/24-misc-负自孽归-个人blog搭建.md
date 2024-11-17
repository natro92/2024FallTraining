# 个人博客搭建



## 零. 作者的小声bb

​		简单记录一下博客的搭建过程作为博客的第一篇文章, ~~毕竟都有这么美的博客了可就不能被认为成杂鱼(bushi)~~

​		一开始考虑过Wordpress,其出名度可谓家喻户晓, 因此主题很多, 搭建也比较方便, 但是作为搭建练手可不能只图方便, 因此我选择了最元老级的框架: Hexo!就目前能力来看, Hexo已经足够, 毕竟不可能一夜之间变出来上百篇文章塞进我的博客, 等真到那时候再考虑换Worpress吧, ~~谁还没有一个大佬梦嘛~~.

​		具体思路是用Hexo框架搭建好后先放在github仓库, 之后进行转移到我的云服务器, 本期先完成前一部分.

​		欢迎大家来[我的博客](https://fuziniegui.github.io/)坐坐. 那么,废话不多说,咱们这就开始.

​		参考文献已经在下面单列出来,感谢他们的指导.

***

> 参考文献
>
> [Git配置](https://www.cnblogs.com/xueweisuoyong/p/11914045.html)
>
> [NodeJS配置](https://www.cnblogs.com/liuqiyun/p/8133904.html)
>
> [【零成本】Hexo个人博客搭建教程 | 无需服务器_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Ju4m1c7WR/?spm_id_from=333.1007.top_right_bar_window_default_collection.content.click&vd_source=a01a62f772bf2e577e11004dfef0bde1)
>
> [[教程]Hexo & Github搭建自己的专属博客](https://www.bilibili.com/video/BV1Eg41157tL/?spm_id_from=333.337.search-card.all.click&vd_source=ae21a115fb3cf089964d95a3f08708ac)
>
> 以及众多报错代码讲解



## 一. 准备工作

###  1. 安装Git和NodeJS

​		**在安装Hexo框架之前, 首先需要在本地配置好环境.**

* 官网下载[Git](https://git-scm.com/downloads/), 选择适合自己电脑系统的版本, 我用的Windows, 所以下载的Windows版本.

  下载完成后点击exe文件按默认选项安装即可. 

  安装完成后,会自动弹出Git命令框, 或者在开始菜单里找到“Git”->“Git Bash”, 说明Git安装成功！

* 在Git中绑定Github账号

  继续在命令框中依次输入两行命令：

  ```shell
  git config --global user.name “Your Name”
  git config --global user.email email@example.com
  # 其中Your Name和email@example.com替换成上面注册时的账户名和邮箱
  ```

* Hexo 是基于 Node.js 驱动的一款博客框架

  因此从官网下载[NodeJS](https://nodejs.org/en/download/package-manager)安装并配置环境变量.

  安装之后可以输入以下命令查看是否安装成功：

  ```shell
   git version
   node -v
   npm -v
  ```

![git等安装成功](https://fuziniegui.oss-cn-guangzhou.aliyuncs.com/img/image-20241107131830554.png "安装成功后显示")

### 2. 创建github仓库以及配置ssh key

搭建github仓库前提是有一个github账户, 由于我的账户好几年前就已经创建过了, 此部分内容省略.

* 创建github仓库

  **进入github网站, 新建一个仓库**![github首页](https://fuziniegui.oss-cn-guangzhou.aliyuncs.com/img/image-20241107184327451.png "左上角绿色方框创建仓库")

  **仓库名按照 <u>固定格式</u> : user.github.io ( user为你github的账号名称 ), 依次勾选"Public","Add a README file", 最后在最下方点击绿框创建仓库**

  ![创建github仓库](https://fuziniegui.oss-cn-guangzhou.aliyuncs.com/img/image-20241107185054108.png)

* 配置ssh key

  回到桌面, 右键打开Git Bash here调出Git命令窗

  在弹出的命令窗中输入命令后按4次回车

  ```shell
  ssh-keygen -t rsa -C "email@example.com"
  # 其中email@example.com替换成github注册时的邮箱
  ```

  ![配置ssh显示](https://fuziniegui.oss-cn-guangzhou.aliyuncs.com/img/image-20241107191322499.png "此时应该显示这些")

  之后打开文件资源管理器, 依次找 **c盘, 用户, (你的用户名称), .ssh**

  ![ssh路径](https://fuziniegui.oss-cn-guangzhou.aliyuncs.com/img/image-20241107191844757.png)

  用任意文本编辑器打开 **id_rsa.pub** 文件, 全选复制文件中的所有内容

  回到github官网, 点击右上角头像里的 **settings**, 找到 **SSH and GPG keys**, 新建一个 **SSH key**

  ![新建ssh](https://fuziniegui.oss-cn-guangzhou.aliyuncs.com/img/image-20241107192326343.png)

  title任意, 在key的框内输入刚复制的key

  ![输入key](https://fuziniegui.oss-cn-guangzhou.aliyuncs.com/img/image-20241107192626391.png)

  点击绿框 **Add SSH key** 添加此key

  完成后, 回到桌面, 再次打开Git命令框, 验证是否添加成功, 输入命令:

  ```shell
  ssh -T git@github.com
  ```

![验证key](https://fuziniegui.oss-cn-guangzhou.aliyuncs.com/img/image-20241107193129884.png)

输入 **yes**

![验证key输入yes](https://fuziniegui.oss-cn-guangzhou.aliyuncs.com/img/image-20241107193233790.png "这样就成功了") 



## 二. 安装Hexo框架

* 以上环境准备好了就可使用 npm 开始安装 Hexo 了, 在命令行输入执行如下命令：

  ```shell
  npm install -g hexo-cli
  ```

* 安装 Hexo 完成后, 新建一个文件夹用来存放你的blog数据, 在文件夹下打开“Git Bash”, 再依次执行下列命令, Hexo 将会初始化(在指定文件夹中新建所须要的文件)：

  ```shell
  npx hexo init
  npx hexo install
  npx hexo g    # 生成
  npx hexo s    # 本地部署
  ```

* 完成后, 获得一个本地链接: http://localhost:4000/ , 打开此链接, 显示如下页面即成功

![hexo本地部署页面](https://fuziniegui.oss-cn-guangzhou.aliyuncs.com/img/image-20241107200359164.png "恭喜你准备工作本地部署完成")

回到命令窗, 键入 **Ctrl+C** 停止本地服务器



## 三. 上线博客

### 1. 链接gihub仓库

* 进入之前新建的Blog文件夹, 打开 **_config.yml** , 拉到最下面将 **deploy** 后面的全删掉，复制这些

  ```
    type: git
    repository: 
    branch: main
  ```

  <u>**注意缩进格式：每行前面都有两个空格不要删，每个冒号后面都有个空格也不要删！**</u>

​	   然后不要关 **_config.yml** , 回到github仓库, 点击绿框 **code** , 复制你的仓库链接, 粘贴到刚才复制到**_config.yml** 中的  `repository：` 后面, 保存退出

* 回到blog文件夹, 打开Git bash, 安装自动部署发布工具, 输入命令:

  ```shell
  npm install hexo-deployer-git --save
  ```

![自动部署发布工具](https://fuziniegui.oss-cn-guangzhou.aliyuncs.com/img/image-20241107203054389.png)

* 再输入命令:

  ```shell
  npx hexo g # 生成
  npx hexo d # 上传
  ```

---

**至此, 本地博客部署到github仓库成功**

**网址是之前设的仓库名：用户名.github.io**

---

## 四. 自定义修改blog

### 1. 博客标题与页面等修改

* 打开blog文件夹中的 **_config.yml** 文件

  将 **#Site** 下面按自己的需求填上, 然后保存

  ```
  ## Site
  title: 标题
  subtitle: 副标题
  description: 描述
  keywords: 关键词
  author: 站主
  language: 语言（可以填写zh-CN）
  timezone: 时区（可以填写Asia/Shanghai）
  ```

### 2. 美化主题

* 在博客文件下打开Git, 依次输入命令:

```
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

* 依然打开 **_config.yml** 文件, 拖到最下面, 找到theme, 把后面内容改为 **butterfly** , 然后保存

* 然后把themes文件夹中的 **config.yml** 重命名为 **_config.butterfly.yml**，复制到博客的根目录下与 **config.yml** 同级

  ![butterfly](https://fuziniegui.oss-cn-guangzhou.aliyuncs.com/img/image-20241107205111627.png)

  

### 3.应用blog本地修改

* 在博客文件下打开Git, 依次输入命令:

  ```shell
  hexo cl
  hexo g
  hexo d
  ```

成功修改

## 五. 结束语

**至此, 基础的blog创建完成**

**(不定期更新ing)**

*end*
