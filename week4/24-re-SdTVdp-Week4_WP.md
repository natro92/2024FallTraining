<h1 id="uLhFG">一、Base64</h1>
老朋友了，先看一眼位数

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732727681703-e4747ccf-7bf3-496b-9430-80d5a18c9f9c.png)

确定64位拖进IDA就是直确接gank啊

进去shift+f12找"enter flag"类字样直接追就行，~~  追不到再找花就行  ~~

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732728163354-9a4ce276-71d2-4395-884d-f2a0fabd518e.png)

双击成功追到了我们经过base64编码后的原始字样g84Gg6m2ATtVeYqUZ9xRnaBpBvOVZYtj+Tc=

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732728243364-ec5dc13a-3059-408a-b478-d3dbc7a1312d.png)

所以直接把g84Gg6m2ATtVeYqUZ9xRnaBpBvOVZYtj+Tc=找个解码机解码就完了。。。

吧。。。。。。

吗。。？

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732728445087-a83102cc-4f2d-41b6-a75d-e82143922646.png)

那是什么环节出了问题呢，再去程序里找找看？

再看一眼base64编码的代码块

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732728821045-fb4aefcd-4ae8-4d3d-9335-0f41ffbfb5a6.png)

可见这里本来应该是ABCD的地方变成了一个“aWhydo3sthis7ab”，追进去看看

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732728890006-f495d9d4-e387-49fe-8821-343111715787.png)

这里就是经典魔改字母表的base64了

~~所以如果你不想把它复制到visualC++里再一个个改变量名的话还是网上找一个现成的实在（~~

如果支持上网查询的话有一个比较著名的轮子就是cyberchef，把字母表和对应密文打进去

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732729207783-b031fbab-5367-48d9-970b-488bf069b5f5.png)

得到flag{y0u_kn0w_base64_well}

---

<h1 id="p6QTM">二、 **Simple_encryption**</h1>
出题人说一眼秒（

祖宗之法不可变，先拽个exeinfo看看情况

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732814754376-011382d5-c370-4875-afcb-9ad923af8d3e.png)

64位无壳，直接拽x64ida看看情况

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732814811821-b4fb8f83-56eb-4737-af5c-6de9b9108658.png)

shift f12找到input flag类似语句，追进去然后f5反编译看看伪代码

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732814939577-26a80e63-7673-4433-975f-953afe6f37ba.png)

看代码这里很显然是输入flag之后存在input里面，然后和buffer逐位比较

其中，输入的flag将按照位数分组，于3、6、9。。位的将后推31位；1、4、7。。。的将前移41位；2、5、8，，，的将与0x55u（85）做一次异或

上周的作业中我们需要找到一个不变的量来还原异或的结果，现在我们直接就有了，确实一眼（

那么根据下面的buffer作为已经变化完的情况，把buffer的0、3、6、9。。位的前推31位；1、4、7。。。的后移41位；2、5、8，，，的再与0x55u（85）做一次异或便可以得到flag

追一下buffer数组的具体内容

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732815571137-1cb45759-6729-4963-ac13-ffd8211470cc.png)



还是上周的老技巧，选中buffer随后shiftE即可导出全部内容

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732815618984-3d7fee51-5f55-40cb-9b53-aea2b8c355d1.png)

即

71, 149, 52, 72, 164, 28, 53, 136, 100, 22, 

136, 7, 20, 106, 57, 18, 162, 10, 55, 92, 

7, 90, 86, 96, 18, 118, 37, 18, 142, 40（最后两个0记得舍去）

因此变化后的结果即为：

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732816523169-c68aa3ed-0857-4876-b03f-42da5149c64c.png)

运行得出结果

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732816545146-025f55bd-292a-4c7b-bfcf-70921125d791.png)

flag{IT_15_R3Al1y_V3Ry-51Mp1e}

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732816634213-8456ca34-4230-4b5d-89ed-6021b5db3e45.png)

即为本题的flag（确实一眼秒

---

<h1 id="ZpZaa">**三、ezAndroidStudy**</h1>
这个是没见过的题型OvO

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732856465009-f8c1bd36-1530-4741-a1be-0a1c9090cbdb.png)

到手的文件格式是个.apk，显然exeinfo是不管用的

直接拖进IDA看看（？

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732856661162-eace32e0-c93a-44a9-8ea7-16281c9fda51.png)

也看不出什么。。。

芝士因为.apk更多涉及到的是java逆向的方面，所以需要应用到[Jadx](https://github.com/skylot/jadx)甚至是apk更常用的[Jeb](https://share.weiyun.com/UlbVLOlP)

不过既然题目都写了study了那就拿模拟器打开看看情况（？

打开是个类似教程的文件

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732896871029-c2ccf71a-d754-4e58-8165-d17aaa02fa7d.png)

点击右下角可以看见官方给的教程

所以本质是个教程类的文档

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732856643964-e947e121-4d53-440e-8285-c780efd8b447.png)

第一页给了提示说要在AndriodManifest中的Activity中寻找第一段flag

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732897291904-8aeac126-c08f-4728-9d52-ee7a635f958f.png)

在这里找到AndriodManifest，然后根据提示查找Activity标签确认一个软件有哪些Acticity，随后定位第一段flag

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732897450625-990d47a3-7815-4dcc-a166-4228036b421f.png)

点击activity使得词条高亮，不难注意到 ~~  注意力不惊人  ~~ 这里有Homo和Mainactivity

Mainactivity看上去挺正经的，去看看最高占据分子轨道（Highest Occupied Molecular Orbital，HOMO）啥情况（（（，地址已经给出来了

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732897641197-0ba399b8-6976-49a4-9c01-6a8f5a0924e6.png)

由此可得第一段flag：flag{Y0u

翻页看看情况（

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732898029945-cff272b8-d780-4855-b20c-17fb73880a1a.png)

照着他给的地址找找看（

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732898438353-3abb6c9e-8fb7-4ccd-ac65-005660a2ca8d.png)

根据地址找到了对应文件resources.arsc（在资源文件下面），翻到底看到了下一步提示

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732898599757-c909f580-ee13-43a4-9078-1e18963631f5.png)

在中段也是找到了flag2:  _@r4 以及一些其他tips

根据layout里的提示找到一个含有"activity"字样的文件



![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732898863759-e4d1ff06-ae9d-48a3-9565-43c9d89a6705.png)

找到第三个即可在偏下的位置找到flag3: _900d

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732898994990-aea389f9-6a74-454f-90d9-b6aa9518ec46.png)

第四部分flag和第三部分一样，都在第二部分的flag寻找中有所提及，进去找就行

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732899094384-63d012aa-714a-4080-8207-0251184d530b.png)

这次是盐豆不带盐の直接给文件命名成flag4了，点开看看

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732899161523-4030bbc4-38df-404b-ad5a-ccec8a5046a8.png)

易得flag4为  _andr01d

第五段是个不明所以的提示。。。

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732899300614-463b4eed-6b9b-442d-80b9-9836f7f1ebfb.png)

好的，告诉了我们应该用IDA反编译~~  回归初心说是  ~~

但是。。。这个so文件在哪里呢。。。这个apk根本没在电脑上运行过，所以首先排除在programdata之类的文件来存储这个动态链接库

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732900317100-78885b1a-2ff3-436f-bfa6-41941bf9ea2a.png)

欸。。。直接解压会发生什么事情呢

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732900416744-015e1b12-d6b1-40c3-a07b-0f04e8d7b871.png)

成功把这个动态链接库文件解压出来了

继续经典找函数f5反编译

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732900550984-16dac7fc-1ebf-46d3-bff9-5ebbf9446ad8.png)

看见了这个右括号成功锁定最后一部分flag

最终的flag即为flag{Y0u_@r4_900d _andr01d_r4V4rs4r}

---

<h1 id="rtmIq">四、**begin**</h1>
![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732900792898-a04f1bb0-564e-480e-bb87-c6c1ac8aacf4.png)

用IDA打开后出现了一些话，疑似IDA教程

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732900852940-91b82c0f-ed20-4cbd-a98b-7d8ad68ee33f.png)

确实是IDA教程，记录了f5和shift f12两个常用的快捷键

双击flag1然后按a即可转换为string格式（不过我还是觉得shiftE更常用一点，不只是能转换string

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732901031916-c28dbe65-d457-4920-845c-1cbaff0ccd93.png)

得到了第一部分flag：flag{Mak3_aN_

按照上面的提示，第二部分需要追文档中所有的字符串信息来获取，

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732901230369-f5a93df0-2fb5-4595-813f-9302386ac05d.png)

容易看出第二段为3Ff0rt_tO_5eArcH_

第三段需要看右侧函数名

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732901308544-0183b605-c29a-463e-98a9-c12bd901ce13.png)

不难看出就是这个

第三段即为F0r_th3_f14g_C0Rpse}

综上，flag即为flag{Mak3_aN_3Ff0rt_tO_5eArcH_F0r_th3_f14g_C0Rpse}

这道题也没什么好说的，咱都用IDA这么久了

不过这道题怎么是第三题，第一题反而对IDA使用要求蛮高的，你俩是不是应该换个位置（（

---

<h1 id="y7Es3">**五、ez_debug**</h1>
望名生意，这道题应该就是要考动态调试了

先拖进exeinfo看一眼，x32/64dbg是中途报错还挺麻烦的

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732901590847-e802b8bc-b1e8-449f-8258-561ccf21d589.png)

64位无壳，直接x64dbg看看情况

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732902936472-2bb71d4c-f8ec-4712-af55-1cc6c2c819c1.png)

这里既然给出了decrypted flag就没必要和加密算法硬刚，直接追这个即可

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732903041128-26926194-5744-4425-a9a8-81cce45ac9e3.png)

在这条前面打个断点，然后f9让程序执行一下

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732903128564-dfaf314c-34b7-443a-aae2-3cdbf5c8253b.png)

这里就会出现类似跑飞的状况（如果没有就再按一下F9，实际就只是开始跑了），然后让程序执行

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732903285669-5426146c-c658-4698-bf69-071d7deb9e06.png)

可以看见原本下面空着的地方出现了flag

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732903325052-c6cd9699-018c-4977-980b-b1f60db2b699.png)

双击一下就可以复制了flag{y0u_ar3_g0od_@_Debu9}

动态调试在上周第三题中的剥壳里面也有涉及，也算是老朋友了

~~上周那个迷宫题做的到现在都有阴影~~

ok，至此week1的题目就做完了，整体并没有很难

接下来施工week2

---

<h1 id="j1xGQ">六、pangbai泰拉记</h1>
![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732955687793-4f011f17-9a24-44d7-8813-f6a0f67491f0.png)

经典打不开报错（

不过这次有网站验证正误打不开也就还好，IDA能打开就行

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732955774176-8bd52121-0790-4b34-b576-14e26da67ec9.png)

提示说要用动态调试打开，那就x64dbg试试看

坏了报错没法动态调试。。。只能IDA硬做了

把key展开一下看看情况

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732956474706-6b73a72b-54c7-4256-9bb0-0ef596f27734.png)

多了一个CheckRemoteDebuggerPresent函数，经过stfw后得出结论，这是个检测反调试的函数

> `kernel32`的`CheckRemoteDebuggerPresent()`函数用于检测指定进程是否正在被调试. `Remote`在单词里是指同一个机器中的不同进程.
>
> 如果调试器存在 (通常是检测自己是否正在被调试), 该函数会将`pbDebuggerPresent`指向的值设为`0xffffffff`.
>
> 我们可以直接修改`isDebuggerPresent`的值或修改跳转条件来绕过 (注意不是`CheckRemoteDebuggerPresent`的 izhi, 它的返回值是用于表示函数是否正确执行).
>
> 但如果要针对`CheckRemoteDebuggerPresent`这个 api 函数进行修改的话. 首先要知道`CheckRemoteDebuggerPresent`内部其实是通过调用`NtQueryInformationProcess`来完成功能的. 而我们就需要对`NtQueryInformationProcess`的返回值进行修改.
>

由此可得，在这个反调试函数存在的时候，我们在上面看到的那一串东西是假的。。。

不过既然知道了这个东西是通过设置pbDebuggerPresent的值来标记是否被调试器侦测的，因此我们也可以观察前段逻辑来更改原有逻辑以达成跳转到我们想要的结果的效果

实际操作上应该是把此处jz patch成jnz或者直接nop掉上面call的反调试函数来达成完全绕过反调试代码的效果的，但是电脑不知道出什么毛病了没法这样操作=。=![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732963026985-691b718e-bbdd-4d3c-8c87-5c7bf3c09caa.png)

如上图所示

经过一个周末的探究，排除了C语言的抽风问题，目前体感上是绿色版自带的python11抽风了，一直做不到直接patch assemble

报错代码贴一下看看有没有师傅能帮我看看QwQ（特别感谢stpertag师傅帮我问了一天

> Traceback (most recent call last):
>
>   File "D:\capture the flag/IDA/plugins\patching\actions.py", line 127, in activate
>
>     wid = PatchingController(self.core, get_current_ea(ctx))
>
>           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
>
>   File "D:\capture the flag/IDA/plugins\patching\ui\preview.py", line 47, in __init__
>
>     self.refresh()
>
>   File "D:\capture the flag/IDA/plugins\patching\ui\preview.py", line 223, in refresh
>
>     self.select_address(self.address)
>
>   File "D:\capture the flag/IDA/plugins\patching\ui\preview.py", line 68, in select_address
>
>     if insn.address != ea:
>
>        ^^^^^^^^^^^^
>
> AttributeError: 'NoneType' object has no attribute 'address'
>

这个自带的汇编编辑我们是指望不上了，那么我们可以整点更低级的

从《汇编语言》中我们可以得知，0F 84是jz，0F 85是jnz

虽然汇编patch坏了但是不幸中的万幸是机械码patch能用

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733081033927-33556018-8d53-420e-b0b0-a0f453734c04.png)

手改机械码也终于是把这个jz改成jnz了

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733081977655-f106a06f-d397-40f2-9d38-964af808a992.png)

随后即可找到<font style="color:rgb(31, 35, 40);">flag{my_D3bugg3r_may_1s_banned?}</font>

---

<h1 id="hizOR">七、 **Dirty_flowers**</h1>
倒是一看到F5失效就想到了上周的花 ~~  虽然这周的反调试给了我比上周花更深的心灵创伤  ~~

还是先看一下

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732986162995-fa0a5b75-ccc5-49ec-aa59-d42a77a012b9.png)

经典jumpout啊，经过上次那个maze是一眼花

回去看看怎么个事

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732989331359-6a1c233e-61cb-4f24-811e-a9cae71dd53e.png)

经典飘红+标红啊，what can I say

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1732990305589-00d2c9b4-6184-46ad-9f80-4ec3096c277c.png)无意间shift+f12竟然有提示（

这题的花nop掉所有的call+5并rtrn这种近地tp的即可（怕咱看不懂还特地给了答案可还行（（（

不过上面在[401165,401176]处还有一处同原理的junk，记得也要nope完再声明函数

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733029323093-514dff45-b240-4d40-98d3-93c7e0b70f0b.png)

正常的代码出来了

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733082624811-c41ba9f5-b67f-4f5d-8913-b6f7af38984f.png)

稍微美化一下

不难注意到a1减掉的东西都是4的整倍数，因此合理推测这里都是变量+偏移量的形式表示变量，因此减掉相同的数即为同一变量

注意到有难以被确认的sub_4011D0，点进去看看

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733082871621-0d3aae61-b8b8-4389-9633-4dcc3c150dcd.png)

确认到是一个表列，其中有v3的各个值

再看看sub_401100里面有什么

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733083019040-dc9cb653-1e11-4af6-a85f-e69ca318a84e.png)

还是花=。=

出去修理一下

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733083137249-fefd37ba-df55-4a06-bf9f-36b90b277afe.png)

密钥如上，即使有点恶心但是经分析还是能大概看出来是在循环和dirty_flower做^

junk code out

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733083459724-0b51acd4-ab24-4363-91dd-7a6da0bebf16.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733083473443-91f1c698-1a86-4625-8851-f870c609ddca.png)

---

<h1 id="zOm31">八、Ezencypt</h1>


是安卓题

那就得启动jadx和IDA了

不过在此之前先打开模拟器看看有没有上次之类的提示（

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733048209564-49fa6627-2373-4378-b075-9244f731fbe3.png)

并没有=。=

不过还说了本题考查So的基本加密，也基本锁定在之前三中讲述的IDA了

因此，先解压然后找到lib就i可以把so拖出来弄进IDA里面进行反编译了

不过，还说考察了java层，就先jadx进Mainactivity看看情况

往下翻到onclick发现端倪

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733049069858-636f80cc-cc7b-4b4b-86d4-d35215d81980.png)

双击这个Enc即可进去查看具体内容

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733049351079-eee9e47e-41a8-4b1c-91a7-9391cae97fb8.png)

encrypt、AES、ECB这里指向的是同一个东西：AES算法的ECB模式，是一种采用分组密码体制的加密算法；ECB模式则是指向电码本模式

既然知道了是电码本模式，这里倒也比较仁慈没让我们硬刚AES，下面也给出了密钥就是IamEzEncryptGame

并且可以从这一步得知，要得到答案的路径上必然有一步base64编码

然后用IDA打开so看看

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733072942685-f650568e-75e8-4016-9b31-fd191b2fade2.png)

成功在x86_64的libezencrypt.so中找到了这段代码

先找到了密文mm，点开看看

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733074855335-480f222e-ead3-4a9f-b902-a15e68a3db82.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733074920587-22716423-e6a6-446e-9682-eff738be6834.png)

shift+E统一改成64进制形式保存备用

点开看看那个叫enc的能更改我们的答案数组的函数怎么个事

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733073151119-958065af-de06-41c3-9705-4952dfa8e65e.png)

可以比较容易地看出本段代码的含义在于对i中的每个元素依次对xork中的元素（见下，为meow四个字）

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733073558653-b5d33604-93c0-4402-96c0-de05a8b86961.png)

按四个一组进行异或，返回值为一个套娃encc

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733073609683-2fd79b41-e71d-4af1-be39-571dbd9e1b2c.png)

首先要对上一部分的返回值meow进行一次init_box运算

在点进init_box，可以看到是一个排序相关的函数

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733078446295-54ab50d7-bdad-409c-8330-7b454aa1ab9b.png)

自此，函数已经分析完了，接下来跑代码了

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733076213367-67468a06-1f37-460b-ac20-f008fb243edf.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733076233281-111b3ae6-9f52-4314-87e7-8aa2d1b79d5c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733078529049-cd4cedda-3c2d-4b5f-8fff-9640f7d4cd82.png)

求出来结果是2BB+GQampKmsrfDG85+0A7n18M+kT2zBDiZSO28Ich4=

让赛博厨师煮一下

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733078852501-7904ded0-0bae-4367-8698-a8038909ca39.png)

得到结果flag{0hh_U_kn0w_7h15_5ki11}

---

<h1 id="TXCbw">九、UPX</h1>
这位的考点仍然是老朋友upx壳，上周就见过了

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733083655065-8f380501-180f-4d00-83ed-65836439b29b.png)

~~首先为什么这位下载下来是txt~~

那就得上010editor看看成分了

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733123110880-0c6cf75a-f3a2-4b2b-8775-fff3b1527af8.png)

显然，这是个elf文件，得改后缀（

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733123191047-8ea86b63-4caa-48a7-90e4-fbd2df299bb0.png)

拖进exeinfo看看情况，是x86_64的文件

拖进IDA看看情况

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733137997050-9a83660f-5f9f-4a90-8cf5-fb9eb36f7d06.png)

根据常识我们可以得知，RC4算法通过原文和keystream进行异或运算得到密文，也可以通过密文再和Keystream再进行一次异或运算进而得到原文（这里是异或运算的可逆性或者说结合律

点进去看看这里的RC4算法具体是什么

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733138055392-b40516ec-e303-4a54-bc36-f9ddc47644fb.png)

可见仍然是一个init_sbox的黑盒

再看看这里的init_sbox

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733138280389-81dec936-38f4-4f43-805c-667ff72109e8.png)

以及中间的swap函数

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733138340946-c2fc8872-4b43-47b6-9785-4d921e7b56a2.png)

不过鉴于我们知道了RC4需要密钥来进行解密，因此我们需要着重找找密钥的位置

也是可以在RC4的后半段中比较轻松地找到key就是aNewstar

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733138519012-65b93e30-04be-41d1-8315-f4a6173022ad.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733138563938-a0e0643a-1cce-4643-8659-90589f3731ff.png)

函数下的'NewStar'字符串

然后再看看比较的data怎么回事

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733138935065-8e842c4c-22d3-445f-88ad-54639ea182cf.png)

在主函数中没有，但是我们可以通过点击x看看交叉引用的方式看看其他函数中有没有（毕竟后面的dataXref中明确指出了在before_main中存在这个东西

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733139025155-2709ef28-d0f4-4543-8975-74e134fab0a9.png)

可以看到在before_main中确实是有这个东西的，再进去看看

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733139087434-62f80b99-85e5-4a26-84e1-d2c3cc8b1ac7.png)

这里就可以看到具体的数值了，接下来我们就可以进行解密了

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733139806622-3c643b77-27db-4a22-9acd-e1e0b92d985d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733139825282-3d846aa8-8cc8-403a-83b2-88b3d50d175e.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733139840086-4b0d10af-01e5-41ed-88c3-6ada287bbb7f.png)

最终解出答案

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733139885451-187d2e9a-c6b0-4437-b67e-b3720aeb856d.png)

flag{Do_you_know_UPX?}

这题其实疑似有个bug，我把文件后缀改成.elf的时候壳自己就掉了,我脱壳的时候还跟我说是无壳的给我看傻了...

---

<h1 id="P3yui">十、Ptrace</h1>
~~你俩怎么下载下来又是俩txt~~

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733139990838-5a7c7874-4115-45c3-8c87-cd3909ec0a22.png)

010editor看看情况

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733140048485-0cdc50a4-32d6-4232-b6da-cb9a3b1d36ee.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733140133994-9f152a4f-7479-4177-ba7f-39c0b1f85561.png)

可以看到是俩elf

分别拖进IDA鉴赏一下了

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733140678436-9d581e47-1f7b-4f3b-ada3-f8c98242d56a.png)

这是父进程,可以看到其中再输入一个32位的答案之后fork、perror、ptrace、wait、excel都和创造并运行另一个进程相关，因此重点应该在子进程上

但是这里有一个需要注意的地方，addr处的数据被poke成了3，进去看看addr是怎么个事

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733141198772-0737b31e-5e02-4139-a358-a5c7548d0212.png)

一个很大的16进制数，目前还看不出是什么东西

接下来看看子进程是在干什么

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733141387529-884b3985-29ee-4e9c-acee-dec4446ccb69.png)

这个60004040看着有点眼熟。。。欸，是不是父进程刚改完

所以这里的数值并不重要，这个东西里的值就是3

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733142218984-265558cb-969b-4d95-87c6-3643d5149353.png)

接着追一下6004020中的值，由此就可以写代码求解了

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733143527832-a0ca991f-758e-43d6-a3d3-2a5fa982bad8.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733143542126-d44884e7-5d4e-475c-af22-9cb4d083754c.png)

跑，得出答案

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733143612711-2defa2d3-9e0e-4e5f-a847-6566013c2d3e.png)

flag{Do_you_really_know_ptrace?}

---

<h1 id="kvJBg">十一、Drink Tea</h1>
不难注意到这里大概率有tea加密啊

正常的exe就直接拖进ida看看了

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733143829409-189b2065-baea-463a-aa72-e72793f7a9f6.png)

又是经典的没有函数名系列，需要自己一个个按n手改来增加可读性

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733144325880-332a156a-a191-4f98-b06b-a88c0f93e7b8.png)

经过一番手动注释总归能看了（

总而言之就是检测你的输入是否是32位数，如果是则对它进行sub_140001180加密方法

具体加密方案：

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733144424962-3bf3dadc-e320-4ca5-897c-100dbaeb8103.png)、

中间有个unk的函数里面存的就是密文

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733144853944-5df93534-e1df-42ed-8242-142d1ef6c8d1.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733144880198-2eee8f80-026f-455d-a93a-8d1c08a6fc3c.png)

密文，密钥，移动方式都齐了，可以开始写脚本解密了

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1733145644123-b8eb4071-eda4-4549-91a3-213e9ef0c4a6.png)

flag为flag{There_R_TEA_XTEA_and_XXTEA}









