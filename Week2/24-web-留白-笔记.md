---
title: BaseCTF2024的赛题review和study log
date: 2024-11-16 22:54:15
comments: true
description: 学习记录
categories:
- baseCTF复现
tags:
- 网络安全
- php
- 抄作业答案
---
# BaseCTF2024的赛题review和study log

## 一.A Dark Room(入门)

F12直接打开源码看，得到flag

## 二.flag直接读取不就行了?(简单，但是对我来说不简单)

获取容器网址后进入显示

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
作为一个零基础小白，自然是不会做的，直接去搜wp，然后根据wp反推考察的知识和技巧

跟着网上的wp复现的时候还是遇到了很多问题，不能够马上获得flag

---

分析如下：

现在许多web服务应用框架使用PHP作为后端编程语言，该容器提供了一部分PHP代码，来说明一些基本规则和定义

```php
$J1ng = $_POST['J'];
$Hong = $_POST['H'];
$Keng = $_GET['K'];
$Wang = $_GET['W'];
$dir = new $Keng($Wang);
```
先是定义了四种变量类型，分别对应获取POST或GET请求的参数

再定义了一个新的对象，使用$Keng作为类名，并将$Wang的值作为构造函数的传参

```php
foreach($dir as $f) {
    echo($f . '<br>');
}
```

采用了循环去遍历$dir对象中的每个文件条目并使用`echo`换行输出

```php
echo new $J1ng($Hong);
```

还是创建了一个新的对象，去输出该对象里面的内容

再结合注释的提示，我们需要使用php的一个内置类来把这个容器的后端系统的所有文件列出来，然后找到secret文件进行下一步操作

采用的是GET请求，直接在原来的容器网址后面添加

`?K=DirectoryIterator&W=secret`

确实，大部分wp是这样子做的，但是我按照这样子发送请求并没有得到任何文件列表

在查询大量的wp和询问ai后，我后来发送请求

`?K=GlobIterator&W=glob:///secret/*`

列出了secret文件夹下的f11444g.php文件

解释这个GET请求的意思

`?`用于分隔路径部分和查询参数部分

`K=GlobIterator`第一个查询参数，K 是参数名，GlobIterator 是参数值

`&`用于分隔不同的查询参数

`W=glob:///secret/*`第二个查询参数，`W`是参数名

`glob:///secret/*`是参数值，其中`glob://`这是一个特殊的前缀，用于指示PHP使用GlobIterator类；`/secret/`是要遍历的目录路径；`*`通配符，表示匹配目录中的所有文件和子目录

### 为什么会有这样子的差别?

1.php内置了丰富的类，在这里列出一些常用的类

#### 文件系统相关类

SplFileObject

用于读取和写入文件。

示例：
```php
$file = new SplFileObject("example.txt");
while (!$file->eof()) {
    echo $file->fgets();
}
```

DirectoryIterator

用于遍历目录中的文件和子目录。

示例：

```php
$dir = new DirectoryIterator('/path/to/directory');
foreach ($dir as $file) {
    if ($file->isFile()) {
        echo $file->getFilename() . "\n";
    }
}
```
GlobIterator

用于遍历符合特定模式的文件。

示例：
```php
$dir = new GlobIterator('/path/to/directory/*.php');
foreach ($dir as $file) {
    echo $file->getFilename() . "\n";
}
```
#### 错误处理相关类

Exception

基础异常类，所有异常都继承自它。

示例：

```php
try {
    throw new Exception("An error occurred");
} catch (Exception $e) {
    echo $e->getMessage();
}
```

ErrorException

用于将 PHP 错误转换为异常。

示例：
```php
set_error_handler(function($severity, $message, $file, $line) {
    throw new ErrorException($message, 0, $severity, $file, $line);
});
```
#### 数据结构相关类

SplStack

实现栈数据结构。

示例：
```php
$stack = new SplStack();
$stack->push('A');
$stack->push('B');
echo $stack->pop(); 
```
SplQueue

实现队列数据结构。

示例：
```php
$queue = new SplQueue();
$queue->enqueue('A');
$queue->enqueue('B');
echo $queue->dequeue(); 
```
SplFixedArray

固定大小的数组。

示例：
```php
$array = new SplFixedArray(3);
$array[0] = 'A';
$array[1] = 'B';
$array[2] = 'C';
echo $array[1]; 
```
#### 其他常用类

DateTime

处理日期和时间。

示例：
```php
$date = new DateTime();
echo $date->format('Y-m-d H:i:s');
```
DateInterval

表示两个日期之间的间隔。

示例：
```php
$interval = new DateInterval('P1Y2M3D');
```
DatePeriod

表示日期范围。

示例：
```php
$start = new DateTime('2023-01-01');
$end = new DateTime('2023-01-31');
$period = new DatePeriod($start, new DateInterval('P1D'), $end);
foreach ($period as $date) {
    echo $date->format('Y-m-d') . "\n";
}
```
DOMDocument

处理 XML 和 HTML 文档。

示例：
```php
$doc = new DOMDocument();
$doc->loadHTML('<html><body><h1>Hello World</h1></body></html>');
echo $doc->saveHTML();
```
SimpleXMLElement

简化 XML 文档的处理。

示例：
```php
$xml = new SimpleXMLElement('<root><item>Value</item></root>');
echo $xml->item;
```
PDO

数据库访问抽象层。

示例：

```php
$pdo = new PDO('mysql:host=localhost;dbname=test', 'username', 'password');
$stmt = $pdo->query('SELECT * FROM users');
while ($row = $stmt->fetch()) {
    print_r($row);
}
```

#### SPL（Standard PHP Library）类

SPL 提供了许多标准接口和类，用于处理常见的编程任务。

SplObserver 和 SplSubject

观察者模式的接口。

示例：

```php
class MyObserver implements SplObserver {
    public function update(SplSubject $subject) {
        echo "Subject state is now: " . $subject->getState() . "\n";
    }
}

class MySubject extends SplSubject {
    private $state;

    public function setState($state) {
        $this->state = $state;
        $this->notify();
    }

    public function getState() {
        return $this->state;
    }
}

$subject = new MySubject();
$observer = new MyObserver();
$subject->attach($observer);
$subject->setState('new state');
```
SplHeap

堆数据结构。

示例：
```php
class MyHeap extends SplMaxHeap {
    protected function compare($value1, $value2) {
        return $value1 - $value2;
    }
}

$heap = new MyHeap();
$heap->insert(10);
$heap->insert(20);
$heap->insert(5);
echo $heap->top(); 
```
2.DirectoryIterator和GlobIterator的区别

在使用DirectoryIterator时，它会尝试遍历指定目录中的所有文件和子目录
但是，如果目录路径不正确或存在权限问题，DirectoryIterator可能无法列出任何文件

也许官方的容器系统出现了什么奇奇怪怪的问题又或是什么其它原因造成的，导致DirectoryIterator发挥不了作用

在使用GlobIterator时，它会使用glob模式(允许你使用一些特殊的字符来匹配一组文件名或路径的模式)查找符合特定要求的文件，也许这使得它更加灵活泛用

---
在第一步操作中，我通过GET请求得到了secret里面的文件

现在第二步，现在我需要通过POST请求读取文件里面的内容

打开火绒浏览器，找到插件商店下载hackbar

![](https://img.picui.cn/free/2024/11/17/6739cc5a3374d.png)

在进入了secret的文件夹的容器网址中F12打开开发者面板，在面板上面的工具栏中点击hackbar，再点击Lpad URL，右边的空白地址栏就会把你的地址载入

再点击Post date，下方出现空白栏等待输入POST请求

在前面的PHP类的学习，我们知道了读取文件所需要的类为SplFileObject

那么输入

`J=SplFileObject&H=/secret/f11444g.php`

点击execute发送Post

![](https://img.picui.cn/free/2024/11/17/6739d29bee5b0.png)

似乎页面没有发生变化，我们再回到查看器查看页面源代码，我们发现多出来一行注释，flag得到

事已至此，我确实应该深入学习一下Http的请求的知识

### Http的几种请求方式

1.GET

请求参数通过 URL 传递，请求指定的页面信息，并返回实体主体，这种方法提交的数据会直接填充在目标URL上，所以我们可以通过修改URL实现GET提交

2.POST

请求参数通过请求体传递，向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改

例如页面正文内容或者源代码发生变化

3.HEAD

请求参数通过 URL 传递，和GET类似，请求指定的资源，但只返回响应头的元数据，不返回响应体

4.PUT

请求参数通过请求体传递，从客户端向服务器传送的数据取代指定的文档的内容

即不仅仅上传数据，而是修改已有的数据内容

5.DELETE

请求参数通过 URL 传递，顾名思义，请求服务器删除Request-URL所标识的资源

6.CONNECT

请求参数通过 URL 传递，HTTP/1.1协议中预留给能够将连接改为隧道方式的代理服务器，用于通过代理服务器建立到另一台服务器的连接

7.OPTIONS

请求参数通过 URL 传递，返回服务器针对特定资源所支持的HTTP请求方法，查看服务器的性能

8.TRACE

请求参数通过 URL 传递，请求服务器回送收到的请求，主要用于测试或诊断，追踪请求路径，帮助调试

## 三.HTTP是什么呀

![](https://img.picui.cn/free/2024/11/17/6739ec6fbfc0c.png)

很好的http基本知识学习教程，使我的hackbar旋转

前面几个直接使用hackbar提交上传即可，如下

![](https://img.picui.cn/free/2024/11/17/6739f1c3ca115.png)

卡我最久的是最后一个修改ip地址，总是因为奇奇怪怪的原因导致ip没法正常修改上传，还有一些容器本身奇怪的检测机制，没有判对

步骤是这样子的，先下载了Burp suite professional汉化版(感谢万能的吾爱pj)

使用firefox下载插件Proxy SwitchOmega配置好和Bp(上面那个软件的缩写)的代理服务器，这部分题目有引导应该怎么做

在网上下载[burp fakeip](https://github.com/TheKingOfDuck/burpFakeIP)插件，打开bp的插件设置，导入下载的fakeip

万事俱备，直接开始

点击bp的代理，打开拦截，再打开firefox在目标网页打开插件PSO，使用bp的代理实现拦截

这时候bp的拦截栏就会出现大量的URL，找到和容器对应的URL，在下面的请求栏中点击Raw,空白处右键调出功能菜单栏

移到插件，打开fakeip，点击127.0.0.1，就会迅速生成各种类型的ip，满足CTF题目容器的多种需求(搞半天真的是气炸了，全给你加上了)

再右键把这个请求文件发送到repeater(重放器)，在重放器点击发送，右边的响应一栏就会有所显示

回到代理拦截，把修改的请求放行，发生了重定向到success.php

![](https://img.picui.cn/free/2024/11/17/673a0ccdf2d00.png)

![](https://img.picui.cn/free/2024/11/17/673a0ccdf1afe.png)

第二张图红色的一长串的就是经过base64编码的flag

暂且到这，日后再刷题再更新