# 个人博客搭建
> 参考文章
>
> [https://blog.fiveth.cc/p/bb32/](https://blog.fiveth.cc/p/bb32/)
>
> [【零成本】Hexo个人博客搭建教程 | 无需服务器_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Ju4m1c7WR/?spm_id_from=333.1007.top_right_bar_window_default_collection.content.click&vd_source=a01a62f772bf2e577e11004dfef0bde1)
>

记录一下个人博客的搭建过程，我的是Hexo框架，参考的就是上面的视频和博客，中间有一点问题，然后我看了另外一位大佬的博客解决的，等到具体步骤具体说，然后我搭好的结果就是[mondaying.cn](https://mondaying.cn)（就是简单搞出来，主题什么的还是没搞懂），下面就简单讲一下过程。

## 一. 准备工作
> 参考文章
>
> [https://www.cnblogs.com/liuqiyun/p/8133904.html](https://www.cnblogs.com/liuqiyun/p/8133904.html)
>
> [https://www.cnblogs.com/xueweisuoyong/p/11914045.html](https://www.cnblogs.com/xueweisuoyong/p/11914045.html)
>

1. 首先就是要下载node.js  
[node链接](https://nodejs.org/en)  
就到官网下载就行，我是下载的挺顺的，没什么差池，第一篇文章讲的挺详细的，照着做就行



2. 然后下载git  
[git链接](https://git-scm.com/downloads)  
也是官网下载，我遇到的问题就是下载很慢，得有快半个小时多才下载好，其他没什么问题，可以参考上面放的第二篇文章



下载完上面两个后可以打开cmd检查一下是不是安装成功（输入下图三行代码），成功的话就是下面这个界面

![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1730260613488-26cebe87-d4f0-4a1c-9cd4-8e1a9cb4bd68.png)



3. 然后就是下载hexo  
得用管理员身份打开cmd  
![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1730260734784-72d18db3-7c13-46d9-8a47-1cf5cfb9a0c2.png)  
然后输入下面这串代码下载hexo，这里我做完忘记截屏了，不过没报错应该就是成功了（确信）

```git
npm install hexo-cli -g
```



4. 接着就是注册一个GitHub账号，这个我上个假期搞的，也就没截屏了，好像也挺顺的，就是取名字那里花了挺久（为什么想取的名字全被注册啊，恼）  


## 二. 搭建仓库
这里就要用到我们之前注册的GitHub账号。主页的左边就有Create repository，点后就是下图  
![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1730262710074-a072ec7b-ab05-4936-bb98-6b3cbe57b849.png)

这里就得注意了，仓库名必须按照格式填写，即：**用户名.github.io**，比如我的用户名是Mondaying（这里的name和user name好像是有区别的，得用user name），那么仓库名必须填Mondaying.github.io

仓库也必须是public的，然后那个Add a README file也点上（这个我也不知道为什么，反正跟着点就对了）

然后就创建好了，放着备用



## 三. 配置ssh key
1. 在桌面空白处右键，点显示更多选项，打开git bash，粘贴下面这串代码，记得把邮箱改成自己的，然后敲四下键盘

```git
ssh-keygen -t rsa -C "邮件地址"
```



2. 之后就去C盘用户文件夹里的某个文件夹里（因为好像每个人的都不一样，反正我和那个up主的就不一样，得找一下）去找下面这个文件夹  
![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1730263737700-ac2180c4-8bd6-41f6-bc1d-99cb4e1f62ee.png)  
点开里面找到id_rsa.pub打开，全选复制



3. 然后我们打开GitHub，打开Settings

![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1730264452254-9a4f4209-202a-4001-a148-02e26c07dffd.png)

点进去后在左边找到SSH and GPG keys，然后点击new ssh key，title那里随便取名，key那里粘贴刚刚复制的东西，创建就可以了  
![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1730264596585-f4a79952-4151-4467-9825-9ba0870d06bd.png)



4. 回到桌面，打开git bash，输入下面这串代码，验证一下有没有添加成功

```git
ssh -T git@github.com
```

回车后记得输入yes，这里也忘记结果截屏了，还是没报错就成功（确信）



## 四. 本地部署
1. 首先选择一个合适的位置创建一个文件夹，用来放置博客文件  
![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1730265201600-39609800-8918-4b0f-9836-193245214bd8.png)  
打开文件夹，在文件夹里右键打开git bash，输入hexo init进行初始化（如果不行就输入npx hexo init）注意，输入这行代码后文件夹里应该得冒出来东西才对，就是得有一些文件的，但是像我就是输入后文件夹里没有东西，虽然没报错（我记得当时是没报错），但是是有问题的，我的解决方法也挺简单的，就是退出去，删除文件夹，然后重新弄，如果显示文件夹被占用无法删除那就重启电脑。  
反正后面几步也一样，一旦有问题就删除文件夹重新来，多来几次就可以了  

2. 输入npx hexo install安装  

3. 输入npx hexo g生成  
我记得这里我卡了挺久，就是输入后它就是没反应，但是多等一会就有了，如果过了很久都不行就按上面方法，删除文档重新来  

4. 输入npx hexo s本地部署  
这之后我们就会得到一个链接，打开就可以看到我们的博客在本地部署啦，这就是我们的博客了，这里记得回到命令行点ctrl+c停止本地服务器，然后再关闭窗口，否则你就得又重启你的电脑了（恼）因为不这样后面再打开就会显示被占用



## 五. 上线博客
1. 首先用记事本打开博客文件夹里的下面这个文件  
![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1730267044848-ab487849-6676-4475-beef-3aa05005dcb9.png)  
当然用vscode打开整个文件夹再进行操作最好，我这里图个方便.  

2. 打开之后拖到最底下，把deploy以及后面的全删掉，复制粘贴下面这段

```git
deploy:
  type: git
  repository: git@github.com:user name/user name.github.io.git
  branch: main
```

记得把user name换成我们的名字



3. 然后桌面打开git bash，输入npx hexo g生成  
这里我弹了一个报错，显示没找到git，但是视频里也没讲为什么  
![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1730127721025-d9a4a9da-26b8-45fa-a33a-2532af77ec07.png)  
然后我又找到了一位大佬的博客[https://www.fomal.cc/posts/e593433d.html](https://www.fomal.cc/posts/e593433d.html)就是这篇，然后输入下面这串代码就好了

```git
npm install hexo-deployer-git --save
```



4. 然后输入npx hexo d上传  
然后这里因为是第一次使用，所以要填一下配置，按照下图输入两个代码就行，记得把邮箱和名字改成自己的（我记得代码它会提示你，你到上面复制下来改一下就行）![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1730127867283-0a8ecd95-96ea-49b3-84ed-a6e04bdb5f18.png)  

5. 配置完了以后再次输入npx hexo d上传，会弹出一个GitHub登录窗口，进行登录就可以了  
![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1730128118033-25b37a7d-ad8b-4146-bebb-58dd54e16c2e.png)  
成功后就是下图这样  
![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1730128563189-14de6c61-d288-4f12-8ca0-81a3b3406730.png)  

6. 我们回到GitHub上看到仓库那边那个黄圈圈变成绿勾后就可以输入链接访问了

![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1730128630964-3e3913b8-4c7c-43ec-8926-cb05b6ecbea5.png)

![](https://cdn.nlark.com/yuque/0/2024/png/42554774/1730128697211-575a3abb-1d12-4cf2-a2ad-5b43f796b00d.png)



## 六. vercel部署和绑定域名
> [第2期：Vercel部署并绑定自定义域名+安装Butterfly主题_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Ke4y1v7Qr/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=a01a62f772bf2e577e11004dfef0bde1)
>

这一步其实也不是必须的，就是我看视频里搞了跟着随便弄的，其实我也不是很懂，因为其实到上面第五步博客就已经搭好了，这里我就简单讲讲我干了什么，不细说

第一个vercel部署，好像是会让速度更快，而且vercel好像还可以搞图床（虽然现在还不太懂图床是什么），是可以搞一下的，跟着上面那个视频来就行，挺简单的，不难

第二个就是绑定自己的域名，因为我们这个博客得绑定一个国内的域名才能在国内访问（好像是），然后就是买一个，绑一下就行，也挺简单的，上面视频讲得也很详细



**这样我们的博客就搭建好啦，之后就是美化什么的，希望大家都能顺利搭建！**

