# SQL注入笔记

----

* ***理解：往往服务器需要用户提交表单来查询数据库里的信息，SQL注入就是这样通过可控输入点达到非预期执行数据库语句，非预期即通过拼接语句来执行数据库命令拿到其他数据。***

## SQL数据库基础

* **一个服务器里会有许多库，每个库里会有许多表，而表中有许多列的层级排列**

  ```
  +数据库 ( database )
  + - 表_user ( table_user )
  + - 表_users ( table_users )
  + + - 列_id (column_id)
  + + - 列_username (column_username)
  + + - 列_password (column_password)
  + + + - 数据
  + + + - 数据
  ```

  ![数据库基础](https://blog.chrizsty.cn/wp-content/uploads/2024/11/202304261646646.png)
  
* **在数据库里有自带数据库，常见的有user，mysql.db，information_schema。这些表可用于爆库时查询**

  * 用户表（user）

    用户表(mysql.user)存储MySQL的用户信息，包括用户名、密码等重要信息。以下是一些常用字段的含义：

    ​	User：用户名
    ​	Host：允许登录的主机（IP地址或主机名）
    ​	Password：加密后的用户密码
    ​	Select_priv：是否具有查询权限
    ​	Insert_priv：是否具有插入权限
    ​	Update_priv：是否具有更新权限
    示例`SELECT * FROM mysql.user;`
  
    ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/c4fafab7-330b-40d1-8fdf-2410984a8a9e.png)
  
  * **数据库表（db）**
  
    数据库表(mysql.db)存储了MySQL实例中的数据库信息，包括数据库名、数据库所属用户等。以下是一些常用字段的含义：
  
    ​	Host：允许访问该数据库的主机
    ​	Db：数据库名
    ​	User：数据库所属用户
    ​	Select_priv：是否具有查询权限
    ​	Insert_priv：是否具有插入权限
    ​	Update_priv：是否具有更新权限
  
    ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/676a1f9a-c353-4f77-8aec-d5ab2a625282.png)
  
    示例`SELECT * FROM mysql.db;`
  
  * #### information_schema数据库
  
    数据库表(information_schema.SCHEMATA)存储了MySQL实例中的所有数据库信息，包括数据库名、创建时间等。以下是一些常用字段的含义：
  
    CATALOG_NAME：数据库名
    SCHEMA_NAME：数据库名
    DEFAULT_CHARACTER_SET_NAME：默认字符集
    DEFAULT_COLLATION_NAME：默认排序规则
    CREATE_TIME：创建时间
    
    TABLE_NAME：表名
    
    COLUMN_NAME：列名
    
    示例`SELECT * FROM information_schema.SCHEMATA;`
    
    ![数据库实例](https://blog.chrizsty.cn/wp-content/uploads/2024/11/b327d72c-9db8-47ef-b9be-bf42543bfb62.png)
  
  ## 数据库基础语法
  
  
  - SELECT
  
    `SELECT 列名1, 列名2, ... FROM 表名 WHERE 条件`
  
  - UNION
  
    ```mysql
    SELECT 列名 FROM 表名
    UNION
    SELECT 列名_1 FROM 表名_1;
    ```
  
    **使用UNION的前提是两表的列的数量相同**
  
  - LIMIT
    ```mysql
    SELECT 列名1, 列名2, ... FROM 表名 WHERE 条件 LIMIT number //返回number行的数据
    SELECT 列名1, 列名2, ... FROM 表名 WHERE 条件 LIMIT x,y //从第x+1行开始返回y行数据
    ```
  
    简单而言是限制显示数据的列数。
  
  - 注释
    ```mysql
    SELECT 列名1, 列名2, ... FROM 表名 WHERE 条件； -- ‘ limit 0,1后面的所欲都会被注释
    SELECT 列名1, 列名2, ... FROM 表名 WHERE 条件；# 后面的所有内容都会被注释
    ```
  
    在输入时# 和(空格)都要url编码后才能提交表单，空格可以用+代替
  
  - Order by
    ```mysql
    SELECT column1, column2, ... FROM table_name [WHERE condition] ORDER BY column_name [ASC|DESC];
    ```
  
    其中，column1、column2 等表示要查询的列名，table_name 表示要查询的表名，condition 表示查询条件，column_name 表示要按照哪一列进行排序，ASC 或 DESC 表示升序或降序排列。可以使用多个列名来进行排序，多个列名之间用逗号分隔。
    ```mysql
    # 这个语法可用于判断表里的列数
    SELECT column1, column2 FROM table_name [WHERE condition] ORDER BY 1;# 不报错
    SELECT column1, column2 FROM table_name [WHERE condition] ORDER BY 2;# 不报错
    SELECT column1, column2 FROM table_name [WHERE condition] ORDER BY 3;# 报错
    ```
  
    故可以判断这张表里有2列
  
  - 数据库的常用参数
  
    * `user()`：当前数据库用户
    * `database()`：当前数据库名
    * `version()`：当前使用的数据库版本
    * `@@datadir`：数据库存储数据路径
    * `concat()`：联合数据，用于联合两条数据结果。如 `concat(username,0x3a,password)`
    * `group_concat()`：和 `concat()` 类似，如 `group_concat(DISTINCT+user,0x3a,password)`，用于把多条数据一次注入出来
    * `concat_ws()`：用法类似
    * `hex()` 和 `unhex()`：用于 hex 编码解码
    * `ASCII()`：返回字符的 ASCII 码值
    * `CHAR()`：把整数转换为对应的字符
    * `load_file()`：以文本方式读取文件，在 Windows 中，路径设置为 `\\`
    * `select xxoo into outfile '路径'`：权限较高时可直接写文件

----

## 基础注入

* 构造闭合
  **`这一步最核心，我也花了最长时间去理解`**

  sql语句通常是``SELECT 列名1, 列名2, ... FROM 表名 WHERE id= ？``而其中的“？”可以用很多种方式表达，例如（$id）、'$id'等多种闭合方法。
  那么构造闭合，已知我们可控输入点是 ? ，我们可以自行输入与其匹配的符号‘ ，“
  比如后台是：

  `SELECT username,password FROM users WHERE id = "$id"`
  我们传入id=1”，那么执行语句就为
  `SELECT username,password FROM users WHERE id = "1" "`
  导致最后面的“不能找到匹配的从而报错。这就需要判断闭合方式来构造闭合来实现sql注入

* 数字型注入
  `$sql = "SELECT username,password FROM users WHERE id = $_GET["id"];`

  这里我们用GET方式传入id的值没有任何闭合方式，在没有做过滤的情况下，可以直接构造sql注入语句

  * 使用`Order by`来确定列数
    ```mysql
    id = 1 Order by 1;
    id = 1 Order by 2;
    id = 1 Order by 3; # 报错 确定列数为 2 
    ```

  * 使用联合查询 `union` 基于 `information_schema` 拿到数据库名（即爆库名）
  
    ```mysql
    1 union SELECT 1,schema_name FROM information_schema.schemata;
    # or
    1 union SELECT schema_name,2 FROM information_schema.schemata;
    # 注意这里的 schema_name 一定要放在会显示的列名上面 比如password不显示 但是username显示 那么就用第二种。
    # 此时后台执行为:
    SELECT username,password FROM users WHERE id = 1 union SELECT 1,schema_name FROM information_schema.schemata;
    ```
  
    当然可以直接使用`database()`来查询当前数据库名

* 字符型注入

  * 下面我们假设一个登录系统，那么他会接收两个参数 用户名和密码 后台的查询语句可能这样写

  	```mysql
  	SELECT * FROM users WHERE username='$username' AND 	password='$password';
  	```
  	
  	对于这种，开发时，预期数据收到的参数都为字符，使用字符进行查询的数据库的注入漏洞 我们称为字符型注入。
  	与数字型不同的是，我们需要先构造单引号的闭合。
  	
  	这里我们让 `$username`= `-1' or '1'='1' --`
  	
  	```mysql
  	SELECT * FROM users WHERE username='-1' or '1'='1' -- ' AND password='$password';
  	```
  	
  	然后类似数字型注入一样爆库名，爆列名
  

  ## 盲注

* 盲注是指攻击者不能直接获取数据库中的信息，需要通过一些技巧来判断或推断出数据库中的数据。盲注主要分为布尔盲注和时间盲注两种。（说白了就没报错信息，完全是一种黑盒）
  **本质上就是通过添加条件来判断语句是否**
  布尔盲注常用的函数有

  ```
  length()   //爆破数据长度 eg.SELECT username,password FROM users WHERE id = 1 AND length(username)=1;
  
  SUBSTR(string, start, length)   //函数用于截取字符串中的一部分,其中，string 表示要截取的字符串，start 表示截取的起始位置，length 表示截取的长度。SUBSTR() 函数会从字符串的 start 位置开始，截取指定长度的字符。
  
  MID()  //函数也是用于截取字符串的函数。
  
  CONCAT()   //CONCAT() 函数用于将多个字符串连接成一个字符串。eg.CONCAT(string1, string2, ...)
  ```

* 
  其实和布尔差不多，只不过是利用 SQL 语句的执行时间来判断 SQL 语句的真假，从而逐步推断出数据库中的数据。

  下面是一些常用函数 和使用技巧：

  - `IF()`

  `IF()` 函数是一种条件判断函数，它用于判断指定条件是否成立，并根据判断结果返回不同的值.

  ```
  IF(condition, value_if_true, value_if_false)
  ```

  其中，`condition` 表示要判断的条件，`value_if_true` 表示条件成立时要返回的值，`value_if_false` 表示条件不成立时要返回的值。如果条件成立，`IF()` 函数将返回 `value_if_true`，否则将返回 `value_if_false`

  - `SLEEP()`

  ```
  SLEEP()` 函数是时间盲注的核心，其语法为 `SLEEP(seconds)
  ```

  当语句被执行时，程序将会暂停指定秒数，比如下面的例子：

  通常 `IF` 和 `SLEEP` 两函数会一起使用

  ```
  SELECT * FROM users WHERE username='admin' AND IF(SLEEP(5),1,0)
  ```

  如果数据库中不存在用户名为 `admin` 的用户，那么该语句将会立即返回结束；否则，程序将会暂停 5 秒钟后再返回结果。

  同样我们使用我们的 demo 语句，`SELECT username,password FROM users WHERE id =`来演示：

  - 利用延时函数，如 `SLEEP()` 函数或者 `BENCHMARK()` 函数，来判断是否注入成功。

  ```
  SELECT username,password FROM users WHERE id = 1 AND IF(ASCII(SUBSTR(username,1,1))=97,SLEEP(5),0)
  ```

  如果用户表中的第一个用户名字符为字母 `a`，则程序会暂停 5 秒钟，否则返回 0。

  - 利用时间戳

  可以利用数据库中的时间戳函数，如 `UNIX_TIMESTAMP()` 函数来构造延时语句，如：

  ```
  SELECT username,password FROM users WHERE id = 1 AND IF(UNIX_TIMESTAMP()>1620264296,SLEEP(5),0)
  ```

  上述 SQL 语句的意思是：如果当前时间戳大于 `1620264296`，则程序会暂停 5 秒钟，否则返回 0。

  - 利用函数返回值

  可以利用函数的返回值，如 `LENGTH()` 函数、`SUBSTR()` 函数等，来判断是否注入成功。例如：

  ```
  SELECT username,password FROM users WHERE id = 1 AND IF(LENGTH(username)=4,SLEEP(5),0)
  ```

  上述 SQL 语句的意思是：如果用户名的长度为 4，则程序会暂停 5 秒钟，否则返回 0。

  - `BENCHMARK()`

  `BENCHMARK()` 函数是一种用于重复执行指定语句的函数，在 MySQL 等数据库中支持使用。`BENCHMARK()` 函数的语法通常如下：

  ```
  BENCHMARK(count,expr)
  ```

  其中，`count` 表示要重复执行的次数，`expr` 表示要重复执行的语句。

  看这个例子：

  ```
  SELECT * FROM users WHERE username='admin' AND IF(BENCHMARK(10,MD5('test')),1,0)
  ```

  如果数据库中不存在用户名为 `admin` 的用户，那么该语句将会立即返回；否则，程序将会重复执行 `MD5('test')` 函数 10 次后再返回结果

* 时间注入
* 报错注入

____

### 参考链接

* [SQL注入攻击原理，方法和类型](https://www.bilibili.com/video/BV1ZR4y1Y745)
* [Hello CTF](https://hello-ctf.com/HC_Web/sql_injection/)
* [Sqli-lab教程-史上最全详解（1-22通关）](https://blog.csdn.net/dzqxwzoe/article/details/139513787)
* [information_schema信息数据库介绍](https://blog.csdn.net/llgde/article/details/131219974)