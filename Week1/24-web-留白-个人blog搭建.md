# 留白的个人博客搭建
---
> 参考文章
>
> <https://blog.fiveth.cc/p/bb32/>
>
> [【零成本】Hexo个人博客搭建教程 | 无需服务器_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Ju4m1c7WR/?spm_id_from=333.1007.top_right_bar_window_default_collection.content.click&vd_source=a01a62f772bf2e577e11004dfef0bde1)
我是参考了Monday的blog搭建过程，同样采用Hexo框架，
>
>参考Mondaying的md书写格式
---
## 一.准备工作

1.首先去下载了[node.js](https://nodejs.org/en)和[git](https://git-scm.com/downloads),然后检验是否下载成功

  ![](https://picture.gptkong.com/20241101/005852fb2ae31b47a7a80a6a5ea5e9da4d.png)

  运气很好，没报什么错误

2.但是在下载Hexo的时候出现问题

  先是测试下载了express，打开cmd输入npm install express -g出现报错

 ```
 npm error code ECONNRESET
npm error syscall read
npm error errno ECONNRESET
npm error network request to https://registry.npmjs.org/express failed, reason: read ECONNRESET
npm error network This is a problem related to network connectivity.
npm error network In most cases you are behind a proxy or have bad network settings.
npm error network
npm error network If you are behind a proxy, please make sure that the
npm error network 'proxy' config is set properly.  See: 'npm help config'
npm error A complete log of this run can be found in: E:\nodejs\node_cache\_logs\2024-10-31T17_24_20_852Z-debug-0.log
 ```
 分析一下应该是挂了梯子的原因，连接出现错误，再加上下载源速度不够

 把梯子关掉，下载源换成淘宝镜像源，输入npm config set registry https://registry.npmmirror.com

 输入npm install hexo-cli -g，显示

 ```
 added 53 packages in 7s

14 packages are looking for funding
  run `npm fund` for details
 ```

 ~~痛苦面具，小问题搞了几十分钟（~~

 ---

## 二.搭建仓库

 进入GitHub登录自己的账号，进入个人主页创建新仓库

![](https://img.picui.cn/free/2024/11/01/6724ceaa44777.png)
命名为

自己github用户名.github.io

勾选public和Add a README file

最后拉到最低点击create

---

## 三.配置SSH

这对我来说是及其痛苦一块部分，花了很多时间试错

正常来说我只需要打开git bash然后输入```ssh-keygen -t rsa -C "邮件地址"```连点四下enter就可以了

但是由于当初创建Windows系统用户时的疏忽，我的用户名为中文，C盘的Users文件下的用户名也为中文

导致一开始的rsa密钥没法正常创建，显示编码错误

但是问题不大，额外添加一些路径限制，使它在我的另外一个盘创建.ssh文件和rsa密钥

输入```ssh-keygen -t rsa -b 4096 -C "你的邮件地址" -f E:\.ssh\id_rsa```

打开.ssh文件夹，用笔记本打开id_rsa.pub复制ssh key

打开GitHub的settings，选择SSH and GPS keys,点击New SSH key，命名随意，把复制的ssh key粘贴进去即可

回到git bash,输入```ssh -T git@github.com```，再输入yes，结果又报错，每次报错还各不相同

```
The authenticity of host 'github.com (20.205.243.166)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/c/Users/\301\364\260\327/.ssh' (No such file or directory).
Failed to add the host to the list of known hosts (/c/Users/\301\364\260\327/.ssh/known_hosts).
git@github.com: Permission denied (publickey).
```

分析报错，应该是git bash还是默认从我的C:\Users\名字 里面找.ssh文件，继而报错中文编码以及没有任何文件

应该让它识别到我在E盘创建的.ssh文件，我作了一些尝试

 1.在.ssh文件夹里面创建known_hosts文件，里面输入

```
github.com ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOMqqnkVzrm0itCRTdd22hHhD1u5VZg2593EaO4zX3mU
```

 2.在.ssh文件夹里面创建config文件，里面输入

```
Host github.com
    HostName github.com
    User git
    IdentityFile E:/ssh/id_rsa
    UserKnownHostsFile E:/ssh/known_hosts
```

 3.git bash输入

```
eval "$(ssh-agent -s)"
ssh-add E:/.ssh/id_rsa
```

完成以上尝试，我再次输入```ssh -T git@github.com```，出现

```
The authenticity of host 'github.com (20.205.243.166)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/c/Users/\301\364\260\327/.ssh' (No such file or directory).
Failed to add the host to the list of known hosts (/c/Users/\301\364\260\327/.ssh/known_hosts).
Hi liubaiqing! You've successfully authenticated, but GitHub does not provide shell access.
```

看起来好像还是有些小问题，不过最后一句话已经success了，接下来尝试本地部署

---


## 四.本地部署

先找一个盘创建文件夹，用来存放blog的相关文件

在这个文件夹里空白处右键打开git bash

输入```hexo init```,返回

```
INFO  Cloning hexo-starter https://github.com/hexojs/hexo-starter.git
INFO  Install dependencies
INFO  Start blogging with Hexo!
```

输入```hexo install```,返回

```
INFO  Validating config
Usage: hexo <command>

Commands:
  clean     Remove generated files and cache.
  config    Get or set configurations.
  deploy    Deploy your website.
  generate  Generate static files.
  help      Get help on a command.
  init      Create a new Hexo folder.
  list      List the information of the site
  migrate   Migrate your site from other system to Hexo.
  new       Create a new post.
  publish   Moves a draft post from _drafts to _posts folder.
  render    Render files with renderer plugins.
  server    Start the server.
  version   Display version information.

Global Options:
  --config  Specify config file instead of using _config.yml
  --cwd     Specify the CWD
  --debug   Display all verbose messages in the terminal
  --draft   Display draft posts
  --safe    Disable all plugins and scripts
  --silent  Hide output on console

For more help, you can use 'hexo help [command]' for the detailed information
or you can check the docs: https://hexo.io/docs/
```

输入```hexo g```,返回

```
INFO  Validating config
INFO  Start processing
INFO  Files loaded in 276 ms
INFO  Generated: index.html
INFO  Generated: js/jquery-3.6.4.min.js
INFO  Generated: archives/index.html
INFO  Generated: css/style.css
INFO  Generated: archives/2024/11/index.html
INFO  Generated: archives/2024/index.html
INFO  Generated: fancybox/jquery.fancybox.min.js
INFO  Generated: css/images/banner.jpg
INFO  Generated: js/script.js
INFO  Generated: fancybox/jquery.fancybox.min.css
INFO  Generated: 2024/11/01/hello-world/index.html
INFO  11 files generated in 254 ms
```

输入```hexo s```,返回

```
INFO  Validating config
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/ . Press Ctrl+C to stop.
```

出现网址后就可以尝试进入了，看起来应该是这样子
![](https://img.picui.cn/free/2024/11/01/6724d8a5a5287.png)

---

## 五.上线博客

在你的博客文件夹里面找到_config.yml文件，用记事本打开
拉到底找到deploy，把它后面的全部删掉，粘贴这个

```
  type: git
  repository: 
  branch: main
```

去自己的GitHub仓库点击code，复制https链接，粘贴到_config.yml记事本里面的```repository: ```后面
保存退出

回到博客文件夹的git bash

安装自动部署发布工具

```
npm install hexo-deployer-git --save
hexo g
hexo d
```

返回

```
INFO  Validating config
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
INFO  Copying files from extend dirs...
warning: in the working copy of '2024/11/01/hello-world/index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'archives/2024/11/index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'archives/2024/index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'archives/index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'css/style.css', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'fancybox/jquery.fancybox.min.js', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'js/jquery-3.6.4.min.js', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'js/script.js', LF will be replaced by CRLF the next time Git touches it
Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got '留白@LAPTOP-QJ1JI6GM.(none)')
error: src refspec HEAD does not match any
error: failed to push some refs to 'https://github.com/liubaiqing/liubai.github.io.git'
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
Error: Spawn failed
    at ChildProcess.<anonymous> (E:\my box\my blog\node_modules\hexo-deployer-git\node_modules\hexo-util\lib\spawn.js:51:21)
    at ChildProcess.emit (node:events:518:28)
    at cp.emit (E:\my box\my blog\node_modules\cross-spawn\lib\enoent.js:34:29)
    at ChildProcess._handle.onexit (node:internal/child_process:293:12)
```

因为是第一次配置，提示我们输入邮箱和用户名，已经有了示例

```
 git config --global user.email "you@example.com"
 git config --global user.name "Your Name"
```

根据格式修改输入即可

再输入```hexo d```上传，可能会出现

```
INFO  Validating config
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
INFO  Copying files from extend dirs...
warning: in the working copy of '2024/11/01/hello-world/index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'archives/2024/11/index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'archives/2024/index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'archives/index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'css/style.css', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'fancybox/jquery.fancybox.min.js', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'js/jquery-3.6.4.min.js', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'js/script.js', LF will be replaced by CRLF the next time Git touches it
On branch master
nothing to commit, working tree clean
fatal: unable to access 'https://github.com/liubaiqing/liubai.github.io.git/': Failed to connect to github.com port 443 after 21179 ms: Could not connect to server
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
Error: Spawn failed
    at ChildProcess.<anonymous> (E:\my box\my blog\node_modules\hexo-deployer-git\node_modules\hexo-util\lib\spawn.js:51:21)
    at ChildProcess.emit (node:events:518:28)
    at cp.emit (E:\my box\my blog\node_modules\cross-spawn\lib\enoent.js:34:29)
    at ChildProcess._handle.onexit (node:internal/child_process:293:12)
```

这种情况似乎是因为你的网络连接问题，可以尝试挂个梯子或者反复尝试输入```hexo d```来解决这个问题

之后就会弹出GitHub账户登陆的弹窗，成功后显示这个
```
INFO  Validating config
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
INFO  Copying files from extend dirs...
warning: in the working copy of '2024/11/01/hello-world/index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'archives/2024/11/index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'archives/2024/index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'archives/index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'css/style.css', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'fancybox/jquery.fancybox.min.js', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'index.html', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'js/jquery-3.6.4.min.js', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'js/script.js', LF will be replaced by CRLF the next time Git touches it
On branch master
nothing to commit, working tree clean
info: please complete authentication in your browser...
Enumerating objects: 24, done.
Counting objects: 100% (24/24), done.
Delta compression using up to 24 threads
Compressing objects: 100% (18/18), done.
Writing objects: 100% (24/24), 278.64 KiB | 17.41 MiB/s, done.
Total 24 (delta 3), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (3/3), done.
To https://github.com/liubaiqing/liubai.github.io.git
 + 676aa38...c1a17b6 HEAD -> main (forced update)
branch 'master' set up to track 'https://github.com/liubaiqing/liubai.github.io.git/main'.
INFO  Deploy done: git
```

之后就可以打开浏览器地址输入你的GitHub仓库名，即可进入你的网站

我的blog

[https://liubai.github.io](https://liubai.github.io)

---

## 六.总结感想
一个博客搭建花了我差不多一天，大大小小的问题接连不断，
很多次都想放弃，不过好在网上都有相关资源可以参考，
最后还是勉强搭建了一个不像样的blog
不过确实也学了很多东西，比如markdown文档语言和git的基本使用，以前都是听说过它们，
但是一直找不到时间也没有什么动力去学习这些，看到HnuSec-Star的秋季训练，
决定还是去尝试做以前没做过的事情

---



