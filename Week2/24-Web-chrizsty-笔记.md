# 学习记录

----

## *总结部分*

### 一、PHP基础

1. php基础格式，以*<?php*开始，以*?>*结束（可以省略）

   ```php
   <?php
       //php语句
   ?>
   ```

2. 变量 赋值 和 运算

   - php中，变量以$开头，且不必声明变量类型

   - 赋值和复合赋值

     | 运算符 | 等同于 | 描述                          |
     | ------ | ------ | ----------------------------- |
     | x=y    | x=y    | 将右侧表达式的值赋值给左侧    |
     | x?=y   | x=x?y  | ?可以替换为+=，-=，/=，%=，.= |
     
   - 逻辑运算与c语言类似
   
   - 比较
   
     * 松散比较（==）只比较值，不比较类型
     * 严格比较（===），两者都较
     
   - 输出
   
     * `echo`-可以输出一个或多个字符串
   
     * 魔术常量
       形如**`__FILE__`这样的定义常量
   
       ```php
       __FILE__ //返回文件的完整路径和文件名
       
       highlight_file(__FILE__); //代码高亮的显示当前文件内容
       ```
   
   （***以下参照[Hello-CTF页面](https://hello-ctf.com/HC_Web/php_basic/#_4)***）
   
   - 表单数据
   
     * **$_GET** —— 接受 GET 请求传递的参数。
       **示例**：`example.com/index.php?book=HELLOCTF`，你可以使用 `$_GET['book']` 来获取相应的值。
     * **$_POST** —— 接受 POST 请求传递的参数。
       **示例**：对 `example.com/index.php` 进行 POST 传参，参数名为 `book` 内容为 `HelloCTF`，你可以使用 `$_POST['book']` 来获取相应的值。
     * **$_REQUEST** —— 接受 GET 和 POST 以及 Cookie 请求传递的参数。
     * **示例**：
       - 如果你通过 URL 传递了一个参数 `example.com/index.php?key=value_from_get`，你可以通过 `$_REQUEST['key']` 获取这个值。
       - 如果你通过 POST 方法提交了一个表单，其中有一个名为 `key` 的字段且其值为 `value_from_post`，你也可以通过 `$_REQUEST['key']` 获取这个值。
       - 同时，如果你设置了一个名为 `key` 的 cookie，其值为 `value_from_cookie`，你还是可以使用 `$_REQUEST['key']` 来获取这个值。
     
   - 内建函数
   
     * **文件操作函数**：
   
       - `include()`: 导入并执行指定的 PHP 文件。例如：`include('config.php');` 会导入并执行 `config.php` 文件中的代码。
       - `require()`: 类似于 `include()`，但如果文件不存在，则会产生致命错误。
       - `include_once()`, `require_once()`: 与 `include` 和 `require` 类似，但只导入文件一次。
       - `fopen()`: 打开一个文件或 URL。例如：`$file = fopen("test.txt", "r");` 会以只读模式打开 `test.txt`。
       - `file_get_contents()`: 读取文件的全部内容到一个字符串。例如：`$content = file_get_contents("test.txt");`
       - `file_put_contents()`: 将一个字符串写入文件。例如：`file_put_contents("test.txt", "Hello World!");`
   
       **代码执行函数**：
   
       - `eval()`: 执行字符串中的 PHP 代码。例如：`eval('$x = 5;');` 会设置变量 `$x` 的值为 5。
       - `assert()`: 用于调试，检查一个条件是否为 true。
       - `system()`, `shell_exec()`, `exec()`, `passthru()`: 执行外部程序或系统命令。例如：`system("ls");` 会执行 `ls` 命令并显示输出。
   
       **反序列化函数**：
   
       - `unserialize()`: 将一个已序列化的字符串转换回 PHP 的值。例如：`$array = unserialize($serializedStr);` 可以将一个序列化的数组字符串转换为数组。
   
       **数据库操作函数**：
   
       - `mysql_query()`, `mysqli_query()`: 发送一个 MySQL 查询。例如：`$result = mysql_query("SELECT * FROM users");`
   
       **其他函数**：
   
       - `preg_replace()`: 执行正则表达式搜索和替换。例如：`$newStr = preg_replace("/apple/i", "orange", $str);` 会将 `$str` 中的 "apple" 替换为 "orange"。
       - `create_function()`: 创建匿名的 lambda 函数。例如：`$func = create_function('$x', 'return $x + 1;');`

### 二、HackBar的使用

![HackBar示例界面](http://120.55.62.144/wp-content/uploads/2024/11/8ffe0322-cfc2-48c8-90a6-14a8e33c635f.png)

**【1】**

* `LOAD`：拉取网址；`EXEUTE`：发包；`SQLI`：实现简易的sql注入；`ENCODING`：解、编码

**【2】**

* 可以实现GET传参`?bianliang=zhi`

**【3】**

* 可以实现POST传参

**【4】**

* 可以增加Header的值，比如cookie，Referer等

### 三、Burpsuite入门

* ~~没啥好写的~~，没学多少，就挂一个代理拦截拓展，可以用burp拦截后分析、改包
* [Burp官方文档](https://www.bookstack.cn/read/Burp_Suite_Doc_zh_cn/Burp_Suite_Documentation-Getting_Started-Startup_Wizard-Select_a_Project.md)

### 四、PHP中常见的执行系统指令的函数

1. eval()函数
   - 定义：‌**eval()函数在PHP中的作用是将字符串作为PHP代码来执行，返回执行结果。**
   - 常见的使用场景：**动态执行PHP代码**
   - 通常可以用来组成一句话木马和恶意输入，与system()函数配合使用，可以执行系统shell命令
2. shell_exec()
   * 这种函数为上述eval()和system()的集合体，但不会输出回显，通常需要写入其他文件来读取
     cat_here > 1.txt
3. 后续待补充

### 五、伪协议

* 参考文章[伪协议csdn](https://blog.csdn.net/qq_50963064/article/details/121343925)

  * PHP伪协议：

    需要开启allow_url_fopen的：php://input, php://stdin, php://memory, php://temp。

    不需要开启allow_url_fopen的：php://filter。

    php://filter用于读取源码，php://input用于执行php代码。

    php://input需要请求提交数据。

    php://filter可以get提交?a=php://filter/read=convert.base64-encode/resource=xxx.php 以base64编码读取xxx.php内容。
### 六、SSRF
* 参考文章[SSRF](https://blog.csdn.net/shangguanliubei/article/details/136834347)

  * 在PHP中的curl()，file_get_contents()，fsockopen()等函数是几个主要产生ssrf漏洞的函数

  * 常用URL伪协议
    ```
    file:///  -- 本地文件传输协议，主要用于访问本地计算机中的文件
    dict://   -- 字典服务器协议，dict是基于查询相应的TCP协议，服务器监听端口2628
    sftp://   -- SSH文件传输协议（SSH File Transfer Protocol），或安全文件传输协议（Secure File Transfer Protocol）
    ldap://   -- 轻量级目录访问协议。它是IP网络上的一种用于管理和访问分布式目录信息服务的应用程序协议
    tftp://   -- 基于lockstep机制的文件传输协议，允许客户端从远程主机获取文件或将文件上传至远程主机
    gopher:// -- 互联网上使用的分布型的文件搜集获取网络协议，出现在http协议之前
    ```
### 七、正则表达式
* https://regexlearn.com/zh-cn/learn/regex101（感谢unjoke大佬提供）

----

## 题目复现过程

*（已经能自行做出来的就没有列举）*

#### week1

* **MD5绕过欸&所以你说你懂MD5**
  
  * **MD5是什么**：**‌[MD5](https://www.baidu.com/s?wd=MD5&rsv_idx=2&tn=baiduhome_pg&usm=2&ie=utf-8&rsv_pq=a34e1a75006fc450&oq=md5定义&rsv_t=55c6G26AObidyoBhFBk%2BUbasfsRGlMcDnWHjk1szqCfrTI5lFgBOj4efGah6X1QaPWYO&sa=re_dqa_generate)（Message-Digest Algorithm 5）是一种广泛使用的散列算法，用于确保信息传输的完整性和一致性。**‌ 它可以将任意长度的数据（如汉字）运算为另一固定长度的值，这种算法的特点是输出长度固定，且对原数据的任何微小改变都会导致输出值的巨大变化。MD5算法的前身包括‌[MD2](https://www.baidu.com/s?wd=MD2&rsv_idx=2&tn=baiduhome_pg&usm=2&ie=utf-8&rsv_pq=a34e1a75006fc450&oq=md5定义&rsv_t=771f0Hqnn%2BkE9M6nj74m3UZ1RfNAR2zF3PhLVk8cvfD0ICVih0C3JzFCmTsjs5aXRaS0&sa=re_dqa_generate)、‌[MD3](https://www.baidu.com/s?wd=MD3&rsv_idx=2&tn=baiduhome_pg&usm=2&ie=utf-8&rsv_pq=a34e1a75006fc450&oq=md5定义&rsv_t=e8f9eTWi4VT96XzplXC6NX20TqwBisB2PVwPqlBZpEBHt24jjtKxFD85paNFA7bwKP%2Fg&sa=re_dqa_generate)和‌[MD4](https://www.baidu.com/s?wd=MD4&rsv_idx=2&tn=baiduhome_pg&usm=2&ie=utf-8&rsv_pq=a34e1a75006fc450&oq=md5定义&rsv_t=e8f9eTWi4VT96XzplXC6NX20TqwBisB2PVwPqlBZpEBHt24jjtKxFD85paNFA7bwKP%2Fg&sa=re_dqa_generate)，它在MD4的基础上增加了“安全带”的概念，使其更加安全。‌
  
  * ```php
    $apple = $_POST['apple'];
    $banana = $_POST['banana'];
    if (!($apple !== $banana && md5($apple) === md5($banana))) {
        die('加强难度就不会了?');
    }
    ```
  
    这里将`$apple`和`$banana`的值进行了弱否比较而对其md5值进行强比较。
  
    采用数组绕过：两数组值不一样可以符合第一条，并且md5加密数组后相同
  
  * ```php
    $apple = (string)$_POST['appple'];
    $banana = (string)$_POST['bananana'];
    if (!((string)$apple !== (string)$banana && md5((string)$apple) == md5((string)$banana))) {
        die('难吗?不难!');
    }`
	  ```
	  $apple`和`$banana`在弱比较中被强制转换为string型，此时使用数组都会变为Arrary
	
    此时可以用一些被md5加密后为0e开头的值，在php弱比较里会被当做0
  
  * ```php
    $apple = (string)$_POST['apppple'];
    $banana = (string)$_POST['banananana'];
    if (!((string)$apple !== (string)$banana && md5((string)$apple) === md5((string)$banana))) {
      die('嘻嘻, 不会了? 没看直播回放?');
    }
    ```
	
    可以通过md5碰撞生成器生成一对值不同但md5值相同的值

* Aura酱的礼物

  * ```
    $pen = $_POST['pen'];
    if (file_get_contents($pen) !== 'Aura')
    {
        die('这是 Aura 的礼物，你不是 Aura！');
    }
    ```
    
    由file_get_contents()函数知此处可以使用data://协议`pen=data://text/plain,Aura`
    
  * ```
    $challenge = $_POST['challenge'];
    if (strpos($challenge, 'http://jasmineaura.github.io') !== 0)
    {
        die('这不是 Aura 的博客！');
    }
    ```

    `strpos()函数用于在一个字符串中查找指定字符（或字符串）第一次出现的位置。`所以可以拼接

    payload：`challenge=http://jasmineaura.github.io@127.0.0.1`

  * ```
    $blog_content = file_get_contents($challenge);
    if (strpos($blog_content, '已经收到Kengwang的礼物啦') === false)
    {
        die('请去博客里面写下感想哦~');
    }
    
    // 嘿嘿，接下来要拆开礼物啦，悄悄告诉你，礼物在 flag.php 里面哦~
    $gift = $_POST['gift'];
    include($gift);
    ```

    最后使用一个include()函数并且有提示flag在flag.php文件里，但是用访问flag.php没有信息，可以猜测flag在注释里，故需要过滤器将里面的代码base64加密显示出来

    payload：`gift=php://filter/convert.base64-encode/resource=flag.php`

#### week2

* #### 数学大师

  * 题上说要5s解一题，还要50道，包不是人写的，得要脚本，直接上代码了

    ```python
    //加载模组
    import requests
    import re
    
    req = requests.session()	//题目有说明要检查session，所以这一步用来固定session
    url = "http://gz.imxbt.cn:20522/"	//网站链接
    
    answer = 0	//初始化answer
    while True:
        response = req.post(url , data={"answer": answer})	//以POST的方式传参数answer
        print(response.text)	//输出传参后的值
        if "BaseCTF" in response.text:
            print(response.text)
            break						//检测flag是否输出
        regex = r" (\d*?)(.)(\d*)\?"
        match = re.search(regex, response.text)		//正则表达式读取内容
        if match.group(2) == "+":
            answer = int(match.group(1)) + int(match.group(3))
        elif match.group(2) == "-":
            answer = int(match.group(1)) - int(match.group(3))
        elif match.group(2) == "×":
            answer = int(match.group(1)) * int(match.group(3))
        elif match.group(2) == "÷":
            answer = int(match.group(1)) // int(match.group(3))
    ```

* #### RCEisamazingwithspace

  * `preg_match()`函数会检查变量里是否有空格，可以进行空格绕过如：

    ```shell
    ${IFS},$IFS,$IFS$9
    ```

  * ~~先学这么多~~，学不动了