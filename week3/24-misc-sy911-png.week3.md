<h2 id="HNv7p">AmazingGame</h2>
下在文件后缀是apk，安卓的软件包。传到手机上，无视风险继续安装。先玩一把，好玩。20关通关即可。

![](https://cdn.nlark.com/yuque/0/2024/jpeg/50077463/1732419615677-66c349da-c809-41ea-a8d1-38145a01f546.jpeg)

手机下载在本地的文件管理打不开，下载MT资源管理器。

搜索amazinggame，找到文件夹![](https://cdn.nlark.com/yuque/0/2024/jpeg/50077463/1732382547487-9174e47a-e16b-48b1-b746-8a71b920e8ed.jpeg)![](https://cdn.nlark.com/yuque/0/2024/jpeg/50077463/1732382568735-6321b76d-f37d-433f-9900-45ab44fe6fc9.jpeg)

好了非常容易就找到了，ZmxhZ3tVX1cxbiEhXzdoZV9nQG00fQ==

非常明显base64解码

flag{U_W1n!!_7he_g@m4}

<h2 id="qII92">BGM坏了吗</h2>
打开文件，听歌。听到最后有一段杂音，戴着耳机听的，杂音还只有左声道有。

有点子像无线电的声音，当时MMsstv试过没有用。

再拿au打开，发现左右声道确实不一样。把左声道的音频，删了或者是把左耳机摘了。

有点像电话拨号的声音。电话拨号隐写。

没有找到在线工具，然后自己对着频率图，一个一个翻译。

 flag{2024093020241103}  

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732427444687-e24550b6-b7a0-4bf4-91da-3eecbc970640.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732427701277-64d60d2a-5347-4004-97cf-1fc7de2db60a.png)

<h2 id="KXC5R">OSINT-MASTER</h2>
打开图片能看到机翼上编号B-2419

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732420349748-3e32fb70-d83c-4931-80a3-91c628109ad5.png)

查看照片属性，8月18日，打开航班管家搜索

![](https://cdn.nlark.com/yuque/0/2024/jpeg/50077463/1732421761390-1f096504-ed87-495f-964a-32cdccbed7c5.jpeg)

东方航空MU5156,济宁，注意txt有的字体不显示下划线，建议提前更换字体

flag{MU5156_济宁市}

<h2 id="lvMB6">ez_jail</h2>
搜索题目是python jail沙盒逃逸，不太清楚。对于python的题一般都得看别人的思路。没办法。

不过好像没啥关系。

打开文件竟然是我亲切的C语言，额，也看不懂，原来是C++。再运行文件，让提交base64码

询问ai

<font style="color:rgb(36, 41, 47);">该程序通过严格的代码检查和安全措施来防止用户编写和执行恶意代码。虽然用户无法使用库引用、宏定义和大括号，但这种限制也限制了用户的功能。程序通过</font>`<font style="color:rgb(36, 41, 47);">subprocess.run()</font>`<font style="color:rgb(36, 41, 47);">函数编译并执行生成的C++文件，并使用</font>`<font style="color:rgb(36, 41, 47);">-lseccomp</font>`<font style="color:rgb(36, 41, 47);">选项来限制程序的系统调用权限，从而提高安全性。</font>

<details class="lake-collapse"><summary id="u8b40b7f8"></summary><p id="u4b8dc5e9" class="ne-p"><span class="ne-text">def cpp_code_checker(code):</span></p><p id="u8da94511" class="ne-p"><span class="ne-text">    if &quot;#include&quot; in code:</span></p><p id="ud9db09ae" class="ne-p"><span class="ne-text">        return False, &quot;Code is not allowed to include libraries&quot;</span></p><p id="uffbede01" class="ne-p"><span class="ne-text">    if &quot;#define&quot; in code:</span></p><p id="u7472ec85" class="ne-p"><span class="ne-text">        return False, &quot;Code is not allowed to use macros&quot;</span></p><p id="u27bf9b00" class="ne-p"><span class="ne-text">    if &quot;{&quot; in code or &quot;}&quot; in code:</span></p><p id="uba110c37" class="ne-p"><span class="ne-text">        return (</span></p><p id="u8b905d48" class="ne-p"><span class="ne-text">            False,</span></p><p id="u2c4c9f7b" class="ne-p"><span class="ne-text">            &quot;Code is not allowed to use `{` or `}`,but it needs to be a single function&quot;,</span></p><p id="u5f44271e" class="ne-p"><span class="ne-text">        )</span></p><p id="ua4515dbc" class="ne-p"><span class="ne-text">    if len(code) &gt; 100:</span></p><p id="ub2b59b87" class="ne-p"><span class="ne-text">        return False, &quot;Code is too long&quot;</span></p><p id="ubb54dbed" class="ne-p"><span class="ne-text">    return True, &quot;Code is valid&quot;</span></p></details>
这段代码定义了一个名为 `cpp_code_checker` 的函数，用于检查 C++ 代码是否符合一些特定的规则。总的来说，这个函数的目的是确保提交的 C++ 代码是一个简单的、没有使用库文件和宏定义的、长度不超过 100 个字符的单一函数。

说明提交的base不能太长，且没有使用库文件和宏定义的。

<font style="color:rgb(60, 60, 60);">这里引用大佬原话“这段代码看似过滤了 </font>`#include`<font style="color:rgb(60, 60, 60);"> </font>`#define`<font style="color:rgb(60, 60, 60);"> 等，但不知道同学们有没有意识到 </font>`#`<font style="color:rgb(60, 60, 60);"> 后加空格就能绕过这里，也就是说可以通过宏定义来做到编译前预处理”</font>

<font style="color:rgb(36, 41, 47);">能够绕过代码检查或找到其他方式来执行恶意代码,即通过# 加空格的方式</font>

<font style="color:rgb(36, 41, 47);"># define user_code() write(STDOUT_FILENO, "Hello, World!", 13);</font>

<font style="color:rgb(60, 60, 60);">然后使用 </font>`<% %>`<font style="color:rgb(60, 60, 60);"> 替换</font>`{}`

<font style="color:rgb(36, 41, 47);">void user_code()<%write(1, "Hello, World!\n", 14);%></font>

[https://mainvooid.github.io/posts/C++%E5%A5%87%E6%B7%AB%E5%B7%A7%E8%AE%A1.html](https://mainvooid.github.io/posts/C++%E5%A5%87%E6%B7%AB%E5%B7%A7%E8%AE%A1.html)

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732445599880-2dd54355-6c90-4db3-9dc8-048b9eb1d4e6.png)

或者是另一位大佬的方法

<font style="color:rgb(77, 77, 77);">采用内敛汇编，主要是还做了长度限制</font>

extern"C" int user_code();asm("user_code:lea m(%rip),%rdi;jmp puts;ret;m:.asciz \"Hello, World!\"");

ZXh0ZXJuIkMiIGludCB1c2VyX2NvZGUoKTthc20oInVzZXJfY29kZTpsZWEgbSglcmlwKSwlcmRpO2ptcCBwdXRzO3JldDttOi5hc2NpeiBcIkhlbGxvLCBXb3JsZCFcIiIpOw==

![](https://cdn.nlark.com/yuque/0/2024/png/50077463/1732445721253-646330d2-d08a-4d29-86ec-728b99346aa5.png)





