# 个人blog搭建过程（全过程记录）
> 参考视频及blog  
[【零成本】Hexo个人博客搭建教程 | 无需服务器 ](https://www.bilibili.com/video/BV1Ju4m1c7WR/?spm_id_from=333.1007.top_right_bar_window_default_collection.content.click&vd_source=a01a62f772bf2e577e11004dfef0bde1)  
[上面那个教程的博主发的blog版教程 ~~_(可以ctrl C V步骤）_~~)](https://blog.fiveth.cc/p/bb32/)   
>

## 一、hexo法
### （1）.下载nodejs与git
#### 1.点击上面的blog教程链接
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-1.png)

#### 2.下载nodejs：
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image.png)

#### 3.下载git：
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-2.png)  
此处根据自己的操作系统选择32位或64位下载即可  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-3.png)

> 鉴于[Monday的模范教程](https://github.com/natro92/2024FallTraining/blob/main/Week1/23-Cry-Monday-%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA.md) 中提到了git下载速度慢的问题，实测使用直连网线+IDM的配置可以将模范教程中半个小时的下载时间压缩到2分钟不到。  
~~_是的这也是为什么我的经济危险到先尝试这种方式（指盈余生活费购买IDM终生码）_~~
>

#### 4.下载hexo
使用**管理员**打开cmd  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-4.png)  
在[hexo官网](https://hexo.io/zh-cn/docs/#%E5%AE%89%E8%A3%85-Hexo)找到npm install指令并输入管理员权限的cmd并等待以下载hexo  
` npm install -g hexo-cli `  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-6.png)

#### 5.注册一个github账号
~~_都能看到这篇文档了应该也没有上不来github的问题了，过程略_~~  

### （2）搭仓库
点击左上角菜单  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-7.png)

#### 1.回到home
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-9.png)

#### 2.点击左侧“创建仓库”
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-10.png)

#### 3.填写具体内容
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-11.png)

#### 4.注意事项
为搭建blog而搭的仓库的蓝框的两处须相同（即仓库名的格式为"**用户名**.github.io"的形式），若非为了搭blog则无所谓  
描述选择性添加，不添加也无所谓  
勾选上红框以添加README.md的文件  
最后点击右下角create repository即可创建仓库辣  
如图示，跳出这个界面即为仓库搭建成功（图为我的仓库）  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-12.png)

### （3）配置ssh key
~~_为啥要配置ssh key？因为可以不用每次输入账密_~~   

#### 1.进入任意文件夹，右键空白处然后点Git bash here
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-13.png)  
输入  
`ssh-keygen -t rsa -C "邮件地址"`  
邮箱地址可替换为你的任意一个邮箱（qq、netease、gmail,etc.）  

#### 2.找到.ssh文件夹内的公钥
点一次回车并记住红框这个地址，.ssh保存在那里（如果没找到也可以再生成一次）  
随后连点三次回车即可  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-14.png)  
从该地址打开.ssh的文件夹  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-15.png)  
用记事本打开id_rsa.pub这个文件  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-16.png)   
如果装了office全家桶的可能显示为publisher文件  
但是这里的pub是指公钥的意思（可公开的），因此需要你保证上方私钥的安全  
因此请无视并继续用记事本打开  
~~_我试过了publisher打不开，不用好奇了_~~    

#### 3.上传至github
重新进入github并点击右上角，进入settings  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-17.png)  
直奔主题（  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-18.png)  
新建一个ssh key  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-19.png)  
标题可任意填写；  
key type不用变；  
下方key栏粘贴你的id_rsa.pub内的所有内容；  
点击"Add SSH key",按提示输入密码即可添加成功  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-20.png)  
可以输入  
`ssh -T git@github.com`   
再按一次回车后，输入yes并回车以验证是否成功  
注：如果你此时正在通过科学方式连接github，请在执行此步骤时关闭科学工具以免DNS污染影响判断结果~~别问我是怎么知道的~~  

### （4）、blogの面世
#### 1.本地部署
在本地创建用于存放blog的文件夹  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-21.png)  
在文件夹内再次打开Git Bash Here  

#### 2.输入hexo init
随后分别输入  
`hexo install`  
`hexo g`  
`hexo s`  
如果遇到问题可以尝试  
`rm -rf node_modules && npm install --force`  
等，具体可以查看报错信息  
后续过程可以看[教程](https://blog.fiveth.cc/p/bb32/)  
~~由于我的npm出现问题因此转移阵地至服务器端~~  
_优点：维护便捷，不需要经济支撑_  

## 二、服务器法
> 1.[参考视频](https://www.bilibili.com/video/BV1ob411W7Dc/?spm_id_from=333.337.search-card.all.click&vd_source=e303c0ea389085181a0f330ef3d148ba)  
2.阿里巴巴服务器自带的教程  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-29.png)
>

### 1.准备工作
既然准备做一个自己的网页，那就得有服务器和域名，域名（相当于门牌号）须要自己实名登记注册并租赁，而服务器（本质上是一台24h开机的电脑，是你存储网站的地方）同样需要租赁（~~当然房子够大的富哥也可以自己组一台，内网穿透完也就是自己的服务器了，就是很耗电且很吵~~）   

#### 1.购买域名
登录[西部数据](https://www.west.cn/)官网寻找一个自己喜欢的域名并注册  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-22.png)  
购买完成后实名注册  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-23.png)  
于红框处进行实名登记  
当且仅当实名登记通过后才可对域名进行解析   

#### 2.购买服务器
基于阿里云的大学生政策在登陆后进行大学生认证可以白嫖一年服务器(注意选Linux的，不要选windows的)  
登入[阿里云](https://www.west.cn/)  
认证完成后点击红框领取300元优惠券  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-25.png)  
此处红框处服务器一年284元，可以通过大学生优惠直接白嫖一年。  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-26.png)  
购买此服务器后根据跳转页面进入控制中心  
选择右侧实例栏，并于下图红框处复制自己购买的服务器的ip地址  
按箭头顺序操作  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-28.png)

#### 3.按照给定教程操作
将地址绑定你的域名后回到阿里巴巴单击控制管理台  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-31.png)  
在主页处点击教程  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-30.png)  
然后跟着教程走就好了，他的教程包比我写的好的的~~才不是写不动了~~  
下面附一下他的教程~~如果有买得起私人服务器的富哥也可以直接参考这里~~  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-33.png)  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-36.png)  
依次执行以下命令

> `sudo yum -y install httpd httpd-manual httpd-devel mod_ssl mod_perl php-mysqli`  
`sudo systemctl start httpd`  
`sudo systemctl enable httpd`  
`systemctl status httpd`
>

![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-37.png)

> `wget http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm`  
`sudo yum install -y mysql57-community-release-el7-10.noarch.rpm`  
`sudo yum install -y mysql-community-server --nogpgcheck`  
`sudo systemctl start mysqld.service`  
`systemctl status mysqld.service`  
`sudo grep "password" /var/log/mysqld.log`  
`sudo grep "password" /var/log/mysqld.log`
>

![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-38.png)

> `ALTER USER 'root'@'localhost' IDENTIFIED BY '<新密码>';`  
`create database wordpress; `  
`show databases; `  
`exit`
>

![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-39.png)

> `sudo rpm -ivh https://rpms.remirepo.net/enterprise/remi-release-8.rpm --nodeps`  
`sudo dnf update -y dnf libdnf`  
`sudo sed -i 's/PLATFORM_ID="platform:al8"/PLATFORM_ID="platform:el8"/g' /etc/os-release`  
`sudo yum -y module install php:remi-7.4`  
`sudo sed -i 's/PLATFORM_ID="platform:el8"/PLATFORM_ID="platform:al8"/g' /etc/os-release`  
`sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php'`  
`sudo systemctl restart httpd`
>

注意，如果直接租赁Alibaba Cloud Linux 3服务器，会由于系统的$releasever变量值与CentOS 8不同，导致DNF解析后的地址无效，进而导致下载RPM包失败，需要按照[教程](https://blog.csdn.net/xiangwangxiangwang/article/details/140774034)手动进入文件夹更改部分文件的地址  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-40.png)  
`wget https://cn.wordpress.org/latest-zh_CN.tar.gz`  
`sudo tar -xvf latest-zh_CN.tar.gz -C /var/www/html`  
`sudo chown -R apache:apache /var/www/html/wordpress`  
`sudo chmod -R 755 /var/www/html/wordpress`  
`sudo mv /var/www/html/wordpress/wp-config-sample.php /var/www/html/wordpress/wp-config.php`  
执行以下命令，修改wp-config.php配置文件。  

> **重要**：  
database_name_here为之前步骤中创建的数据库名称，本示例为wordpress。  
username_here为MySQL数据库的用户名，本示例为root。  
password_here为MySQL数据库的登录密码，本示例为NewPassW****。  
`sudo sed -i 's/database_name_here/wordpress/' /var/www/html/wordpress/wp-config.php`  
`sudo sed -i 's/username_here/root/' /var/www/html/wordpress/wp-config.php`  
`sudo sed -i 's/password_here/NewPassW****/' /var/www/html/wordpress/wp-config.php`  
>

查看配置文件信息是否修改成功  
`cat -n /var/www/html/wordpress/wp-config.php`  
重启Apache服务  
`sudo systemctl restart httpd`

#### 4.加油！离胜利一步之遥！
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-41.png)  
然后按照网站提示填写信息  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-43.png)  
点击下方登录  
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-44.png)  
至此，基本的blog已经搭建完成了

## 三、装点blog
![](https://for-md-usage-1331139727.cos.ap-guangzhou.myqcloud.com/image-45.png)  
wordpress本身提供了非常多的工具，在各种网站上也有很多师傅造的轮子或者做好的主题，个人还在想思路~~目前还是工地啥都没有~~  
服务器的费用是一年284，域名的价格是一年89  
最后附上个人的blog地址：[www.sdtvdp.com](http://www.sdtvdp.com)  
截至发出本篇.md文档的时候申请的域名仍然在工信部审核，因此目前只能通过[http://123.57.241.10/wordpress/](http://123.57.241.10/wordpress/)来访问了   
完结撒花

