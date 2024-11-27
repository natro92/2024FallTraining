<h1 id="A7TIf">Xor</h1>
<h2 id="xIWUJ">1.首先用Exeinfo分析文件</h2>
![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732411621856-33eb9581-7dc6-40d6-a3d5-d7b56737bcb0.png)

得到这是一个64bit的文件且无壳

<h2 id="kydrN">2.直接用ida64打开，轻易找到main函数并进行反汇编</h2>
![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732411800057-de5ef207-082f-4861-8e02-4457af2f3351.png)

<h2 id="nMPWD">3.分析加密原理：</h2>
首先字符串V5的长度应为33，其次对字符数组V5进行了异或加密^,最后用strncmp进行判断.

知识点补充：

（1）异或的理解

在逻辑学中，逻辑算符异或（exclusive or）是对两个运算元的一种逻辑析取类型，符号为 XOR 或 EOR 或 ⊕（编程语言中常用^）。但与一般的逻辑或不同，异或算符的值为真仅当两个运算元中恰有一个的值为真，而另外一个的值为非真。转化为命题，就是：“两者的值不同。”或“有且仅有一个为真。”            

可以参考：原文链接：[https://blog.csdn.net/weixin_43899069/article/details/121048025](https://blog.csdn.net/weixin_43899069/article/details/121048025)

（2）strncmp函数

选择性比较字符串长度，可以参考[https://www.runoob.com/cprogramming/c-function-strncmp.html](https://www.runoob.com/cprogramming/c-function-strncmp.html)

<h2 id="WYb0V">4.具体分析（加密算法）</h2>
for循环可以先简化：假设V5的长度为2

加密是V5[1]^=V5[0]  , V5[2]^=V5[1].

那么解密则需要反序，即V5[2]^=V5[1], V5[1]^=V5[0] 也就是进行逆向处理

<h2 id="dceKA">5.具体解法</h2>
![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732412775225-65d403c2-6f72-4628-aebb-ef82d684fb7d.png)

（1）要使得if判断为真，则要使得条件为！0；即v5和global在前0x21（十进制33）字符数组相同

（2）分析global，点击后

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732413110028-9edfec7c-d3cb-4627-98c8-920a483ded8d.png)

继续点击 aFKWOXZUPFVMDGH

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732413169210-7166f7c9-82cc-4c8d-bbca-14244ffe8339.png)

得到具体数值（注意：在编译器中后缀为h表示16进制数，但在编写C语言脚本时应改成0x//而且在应用字符数组时，需要将字符串拆解，如：'G2O'应改成'G','2','O')

(3)编写脚本

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732414039720-1dc39bb4-5bb7-413c-9ccc-c30ed17675d1.png)

得到结果

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732414079455-b7afb2bc-080d-4327-afce-f480e6d593aa.png)





<h1 id="hJ9rp">Base64</h1>
<h2 id="GXZGg">1.首先用Exeinfo分析文件</h2>
![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732414407118-def4c84b-2dbe-4d20-82dd-3bbb0d4d0350.png)

得到这是一个32位的文件，且无壳。

<h2 id="hWrav">2.直接用ida打开，轻易找到main函数并进行反汇编</h2>
![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732414621194-a51a9b6d-6c01-4c59-b9f6-424a79f14813.png)

<h2 id="tnR0E">3.分析加密原理</h2>
（1）此处函数sub_41132F应该表示printf, 函数sub_411375表示scanf

首先将Destination数组的前100个元素赋值为0；

则我们先输入一串字符串，Str这个字符数组接受前20个元素，

核心是对V4的sub_4110BE加密

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732415251843-ac29ae0c-734f-4d6a-b53c-697cdb195659.png)

继续点击 sub_411AB0函数

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732415318126-c7438047-e058-4f90-a071-67e3f09eb8c6.png)





![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732415447637-ac1338af-c1da-4a10-b64b-d763ca54b511.png)

通过典型的aAbcdefghijklmn字符判断是base64加密

（2）base64加密后还有一层for循环的处理



![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732415781114-9cbd9d47-20dd-410d-b839-05beec986d69.png)

（3）点击str2得到密文

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732415788106-80c0d8f2-bdc1-4b95-b189-b69e9b784d52.png)

（4）先编写C语言处理for循环加密

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732415853107-e2dfc995-850a-4cdf-8863-03b14ff9c9e3.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732415868256-0b5d97b6-1c2c-4ce3-a182-b31093f38051.png)

再编写Python进行base64解密

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732416014479-6388e4de-9424-46a6-93a1-7e155d8be3d0.png)

得到明文

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732416038318-a27b1e9e-4270-494e-bc24-ba9b306219fa.png)

最终的flag为flag{i_l0ve_you}





<h1 id="um3qk">rand</h1>
<h2 id="jSWz3">1.首先用Exeinfo分析文件</h2>
![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732416261233-eff185d5-8d44-4519-ac28-a4fb0c5d8669.png)

是一个64bit文件，且无壳。

<h2 id="eqp7Q">2.直接用ida64打开，轻易找到main函数并进行反汇编</h2>


![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732416490172-92cbe36f-0bd4-46b7-96fc-95c981302bf6.png)

点击patch_me

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732416511659-7b97a57f-85d1-4402-b1d7-4281f22ed52d.png)

初步判断这是一个奇偶判断函数，如果是偶数就进入get_flag函数

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732416641887-3de89ddd-8505-4e31-9138-616c11ef6d72.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732416675526-75482190-ced9-4056-827f-14312c9c7b4a.png)

（此处在31行处点击r 转换为字符串）

<h2 id="Hh8eE">3.加密算法分析<font style="color:#FBDE28;"></font></h2>
v6 = __readfsqword(0x28u);

  v0 = time(0LL);<font style="color:#FBDE28;"></font>

  srand(v0);<font style="color:#FBDE28;">//生成随机数</font>

  for ( i = 0; i <= 4; ++i )<font style="color:#FBDE28;">for循环5次</font>

  {

    switch ( rand() % 200 )<font style="color:#F3BB2F;">决定进入哪个case</font>

    {<font style="color:#F3BB2F;">//尤其注意电脑生成的是伪随机数，存在一定顺序</font>

      case 1:

        puts("OK, it's flag:");

        memset(&s, 0, 0x28uLL);<font style="color:#F3BB2F;">对s进行初始化</font>

        strcat((char *)&s, f1);<font style="color:#F3BB2F;">将f1拼接在s之后</font>

        strcat((char *)&s, &f2);<font style="color:#F3BB2F;">再将f2和f1拼接</font>

        printf("%s", (const char *)&s);

        break;

      case 2:

        printf("Solar not like you");

        break;

      case 3:

        printf("Solar want a girlfriend");

        break;

      case 4:

        s = '\x7Ffo`guci';<font style="color:#F3BB2F;">赋值</font>

        v5 = 0;

        strcat(&f2, (const char *)&s);<font style="color:#F3BB2F;">把s的值赋值给f2</font>

        break;

      case 5:

        for ( j = 0; j <= 7; ++j )<font style="color:#F3BB2F;">对f2数组进行加密</font>

        {<font style="color:#F3BB2F;">//运用指针</font>

          if ( j % 2 == 1 )

            *(&f2 + j) -= 2;<font style="color:#F3BB2F;">j为奇数f2-2</font>

          else

            --*(&f2 + j);<font style="color:#F3BB2F;">j为偶数f2-1</font>

        }

        break;

      default:<font style="color:#F3BB2F;">此处为没有找出flag</font>

        puts("emmm,you can't find flag 23333");

        break;

    }

  }

  return __readfsqword(0x28u) ^ v6;

}

接下来的重点是判断case的顺序

case1是将f1和f2拼接

case4是对s和f2赋值，把s的值赋给f2

case5是对f2进行处理

则得出顺序为case4->case5->case1{因为先要赋值，再进行加密，最后拼接得出flag}

<h2 id="JTmwq">4.知识扩充（小端序存储）</h2>
最低地址存放最低的字节。ELF文件常用

但是IDA会把内存中的数据自动转化成大端序存储，因而这里的s要进行反转

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732417884307-2b213181-de9d-4738-95e9-47b41f5f7eae.png)

这也是为何点击f2为？注意反转成icug`of\x7F  反斜杠后要维持原顺序！！！

<h2 id="EJfBM">5.编写脚本</h2>
首先找出f1,s，然后将s反转后的值赋给f2,再进行正向的case5操作，最终得出flag

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732418127499-badb4379-c101-48c7-bdb6-edb9570e121a.png)

点击f1获取数据

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732420218761-1a3f239b-c173-4228-80bb-5842a785fba2.png)

得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732420241336-004de1a1-3653-4e40-b7cc-c4eae47aee51.png)









