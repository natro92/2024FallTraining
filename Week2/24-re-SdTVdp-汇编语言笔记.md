## 一、汇编语言的产生
汇编语言为解决机械码难以阅读、难以查错纠错的问题而生。

是一种机器指令便于记忆的书写格式。

### 1.计算机の基本结构
#### (1).CPU
计算机的核心部件，控制了整个计算机的运作并进行运算。

通过提供指令和和数据使得CPU工作

我们一般通过更改寄存器CS、IP内的内容控制cpu，CPU将从CS、IP中的内容决定执行指令的位置。

在王爽教授著的《汇编语言》中以16位的8086CPU为例~~这1978年的古董~~~~_是不是有点outdated了=.=_~~，有：

##### 1.AX 
即accumulator，累加器

##### 2.BX
即Base Register 基地址寄存器

##### 3.CX
即Count Register 计数寄存器

##### 4.DX
即Data Register 数据寄存器

以上四类寄存器均可以做通用寄存器使用；

也都可以拆分为可以两个可独立使用的8位寄存器使用，即AX可分为低八位寄存器AL和高八位寄存器AH，同理有BL和BH、CL和CH、DL和DH

##### 5.SI
即 Source Index 原变址寄存器

##### 6.DI
即 Destination Index 目标变址寄存器

##### 7.SP
即 Stack pointer 堆栈指针

##### 8.BP
即Base Pointer 基址指针

##### 9.IP
即 Instruction Pointer 指令指针寄存器，是8086cpu中最关键的寄存器之一，存放偏移地址

##### 10.CS
即Code Segment 代码段寄存器，也是8086cpu中最关键的寄存器之一，存放段地址；CS和IP共同指示CPU当前要读取指令的地址，原理类似于C语言中的行指针和列指针，CPU将始终读取M（段地址）*16+N（偏移地址）各单元开始读取指令

##### 11.SS
即Stack Segment 栈段寄存器

##### 12.DS
即 Data Segment 数据段寄存器

##### 13.ES
即 Extra Segment 附加段寄存器

以上四个统称为段寄存器，用于存放cpu中的段地址

##### 14.PSW
即 Program Status Segment 程序状态寄存器



每个寄存器中存放的具体值可以用debug指令进行查看，R命令可查看或修改寄存器的内容；D命令可查看内存内容；E命令可改写内存中的内容；U命令可将内存中的机器语言翻译为汇编语言；T命令可执行一条机械指令，如有子程序则继续逐条执行；A命令可以汇编指令的形式在内存中写入一条机械指令；P命令与T命令类似，可单步执行语句，且每次只执行一条代码语句，遇到子程序调用的时候直接执行完子程序代码，不会进入子程序逐条执行。

是的，非常显然在书中的示范下debug在查看内存/寄存器内容方面非常有用。。。嘛（？

だがしかし只要我们一上手就会发现。。。。。。

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731692562634-8e86ed96-6dab-4976-9951-923da692b467.png)

debug最多支持到win8，对于现在流行的win11和win10并不支持

~~_我从看到8086cpu开始就感觉不对劲，甚至给的例子还是比我还老的windows2000_~~

那要看寄存器上的具体内容，该怎么办呢？

书中说了debug是一种可以在dos环境下运行的程序，那么我们只需要[随手一搜](https://blog.csdn.net/sjjsbsbbs/article/details/120663590?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522428BC23F-9836-44C7-A6B0-B1C60F52A0FD%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=428BC23F-9836-44C7-A6B0-B1C60F52A0FD&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-120663590-null-null.142^v100^pc_search_result_base9&utm_term=dosbox&spm=1018.2226.3001.4187)就可以搜到dosbox作为dos模拟机的下载链接

根据指引下载完毕后，就可以打开dosbox的界面

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731694022356-2a101afd-c1a8-4a92-9459-dcf42f7aa1c9.png)

再输入Mount C 存放代码的位置 以确定汇编代码存储的位置，汇编语言貌似并不区分大小写。请注意，路径中不能出现中文~~_就是头铁出现了你也_~~~~_**打不进去**_~~，很多程序都需要注意这个问题。

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731694098650-7fde1bd4-309f-4175-8774-d9bbac4298e7.png)

然后再输入C:就可以进入虚拟的C盘

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731694627157-a961daae-3102-4041-8ba1-6a2e045b4c2f.png)

把一些编辑用的文件黏贴进来（有现在用到的debug，也有之后要用的masm等汇编语言的编辑工具）

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731694694080-8dcaf226-05c6-41ba-89a4-e3d30e9f1444.png)

就可以成功在win11下查看各寄存器内的值了，也可以继续跟着书看接下来的内容了

此处CS=073F，IP=0100，此处存放的机器码为0000，对应的汇编指令是ADD [BH+SI],AL. 

在寄存器中，一个字用两个地址连续的内存单元存放，低位字节存在低地址单元（AL），高位字节存在高地址单元（AH）

#### (2).存储器
分为内存和硬盘，用于存放指令和数据。

CPU能直接通过地址总线寻找内存的指定存储器单元并通过数据总线调取内存的数据。

磁盘的数据不能被直接调用。

在内存/磁盘中的数据和指令在存放时都为二进制信息的形式，依据需要选择以数据或指令的方式读取。

#### (3) 主板
用于放置核心部件和部分主要器件，通过总线相连，可在扩展插槽上承载RAM内存条和各类接口卡（显卡、声卡、网卡（现在也有部分高端主板整合了网卡和声卡）、etc.）

## 二、汇编语言的组成
### 1.伪指令
计算机不执行但是编译器执行的指令，**没有对应的机械码。**

### 2.其他符号
+-*/等，编译器识别，但是**没有对应的机械码。**

### 3.汇编指令
机器码的注记符，**有对应机械码**，是汇编语言的核心。

以NSSCTF网站上找的一段无壳新手题直接拽进IDA的形式为例~~_其实是时间有限看不完书了先写个ida食用手册（_~~

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731781426985-cdb2d6cb-99be-4acc-9fa1-0b5675812f83.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731781562091-34e5894f-399f-46bd-b151-9553b4979e3e.png)

#### (1).mov&movzx
食用方法： mov ax,bx  

实战example：![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731771062358-49823906-500b-4244-971c-71b509f846ac.png)    

_即：mov 存放对象（寄存器/内存单元/段寄存器）,寄存器/数据/段寄存器_

执行操作：寄存器bx的内容/某段具体信息送到ax中，但是不能设置CS、IP的值。

movzx是movの拓展形式，意为<font style="color:rgb(26, 32, 41);">将一个较小的源操作数（字节或字）移动到一个较大的目的操作数（字或双字），并在移动过程中将高位扩展为0。</font>

#### (2). jmp
食用方法：jmp CS地址:IP地址  

即：jmp段地址:偏移地址（用于同时更改CS和IP寄存器）

实战example：![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731771094523-beb85728-f2a5-462b-8d68-9440e3164869.png)

或：          jmp ax  即：jmp 某一合法寄存器的地址（用于单独更改IP寄存器）

执行操作： 将CS、IP寄存器的内容更改为输入内容，某种意义上是mov的填充形式。

#### (3).add（加法）&sub（减法）
食用方法： add ax,bx      

即：_add 存放对象,数据/数据地址_

实战example：![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731771209456-6c5abce9-3b2f-46af-95da-3b8cfa3fd850.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731771125077-bc6586ed-d40d-40c8-a4a6-7865871a92e5.png)，

执行操作：寄存器bx的内容/某段具体数据加到ax中，结果存在ax中。

#### (4).push（入栈）&pop（出栈）
食用方法：pop ax   

即：push/pop 寄存器

实战example：![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731771685582-f3bd93e0-f3d9-4435-a7c4-3dfdafbc799a.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731819664660-6b0f7be8-e441-4dc3-93c4-61c88c4cd9ac.png)

执行操作：将某寄存器内的数据入/出栈并用一个寄存器接收数据。。。。。。入栈。。。。。。栈。。。。。。

欸等会栈是什么~~能吃吗2333~~

相信大家或多或少听说过程序崩溃的原因之一就是栈溢出，这个“栈”就是这里的“栈”

栈仍然是一段连续的存储空间，操作方式遵循LIFO（级Last In First Out，后进先出），类似一个纸盒子，依次放入三个东西，放置的时候只能从下往上依次叠上去，拿出来的时候也只能从上往下依次拿出，而最上面的数据便是我们最后写入的数据，我们便为最后一个写入的元素赋予一个新的名字：栈顶元素。

那么既然栈是一段连续的存储空间，那么栈在哪呢？

我们已经知道了CPU将始终读取CS（段地址）*16+IP（偏移地址）开始读取指令，那么对栈来说的寄存器是。。。。。。

bingo，自然就是名字里就有“栈”的SS栈段寄存器和Stack pointer 堆栈指针啦

**任意时刻，SS：SP都将指向栈顶元素，push和pop执行时cpu从SS：SP获取栈顶元素的位置并实行操作。**

我们亦可以手动更改SS与SP的值使得SS：SP指向我们定义的栈段来人为更换栈堆的位置，当然，至少在《汇编语言》中的8086cpu而言，栈顶超界的问题依旧只能靠程序员脑动解决~~_当然这么说也是因为我查不到目前的14900K(这个还列在这是因为285K似乎在很多方面更像E5，有些地方还不如这个），Ultra9285K，9950X和9800X3D四颗新U是否对这个情况有对应的调整措施_~~

#### (5).call
食用方法：call 标签/ax/[ax]

实战example：![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731782092321-a3f38835-2f58-4858-9110-a5751757be40.png)

执行操作：分为直接寻址和间接寻址，若写入标签则cpu将直接<font style="color:rgb(26, 32, 41);">将返回地址压栈，并跳转到对应标记的地址；ax则为其中存放的子程序的地址，若为[ax]则cpu会把ax中的内容视作一段指针，同样会将返回地址压栈，并跳转到指针指向的地址。</font>

#### (6).lea
~~_你看这个call后面很多这个显然是leave_~~实际为<font style="color:rgb(26, 32, 41);">Load Effective Address，用于计算操作数的有效地址并将其加载到指定的寄存器中，作为一种快速执行算术运算的方法或在寄存器间迁移数据。</font>

<font style="color:rgb(26, 32, 41);">食用方法：lea ax,值</font>

<font style="color:rgb(26, 32, 41);">即：lea 寄存器（用于存储计算出的有效地址），指定要计算地址的表达式（如基址寄存器ebp或rbp加一个偏移量，指针/索引寄存器esi或rdi乘比例因子加偏移量等）</font>

实战example：![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731783157025-7a64d4bc-fa8d-4a82-a798-d987e90bff3a.png)

执行操作：后者算式结果对前者寄存器赋值（不涉及内存访问只计算地址时可代替sub、add、mov且运算速度更快）

#### (7).cmp
即compare

食用方法：<font style="color:rgb(26, 32, 41);">cmp ax,bx</font>

<font style="color:rgb(26, 32, 41);">即：cmp （寄存器、内存地址或值）,（寄存器、内存地址或值）</font>

<font style="color:rgb(26, 32, 41);">实战example：</font>![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731817454060-b7017524-1ce8-4f2e-81d1-f52a5421e521.png)

执行操作：类似减法，结果存入Flag Register，并按照以下规则对各个标志进行赋值

> **<font style="color:rgb(26, 32, 41);">零标志（ZF, Zero Flag）</font>**<font style="color:rgb(26, 32, 41);">：如果结果为零，则设置为零；否则，清除为零。</font>
>
> **<font style="color:rgb(26, 32, 41);">符号标志（SF, Sign Flag）</font>**<font style="color:rgb(26, 32, 41);">：如果结果为负（最高位为1），则设置为1；否则，清除为零。</font>
>
> **<font style="color:rgb(26, 32, 41);">进位标志（CF, Carry Flag）</font>**<font style="color:rgb(26, 32, 41);">：在无符号比较中，如果  前者 小于  后者 ，则设置为1；否则，清除为零。</font>
>
> **<font style="color:rgb(26, 32, 41);">溢出标志（OF, Overflow Flag）</font>**<font style="color:rgb(26, 32, 41);">：在带符号比较中，如果比较导致溢出，则设置为1；否则，清除为零。</font>
>
> **<font style="color:rgb(26, 32, 41);">辅助进位标志（AF, Auxiliary Flag）</font>**<font style="color:rgb(26, 32, 41);">：用于二进制编码的十进制（BCD）操作。</font>
>
> **<font style="color:rgb(26, 32, 41);">奇偶标志（PF, Parity Flag）</font>**<font style="color:rgb(26, 32, 41);">：根据结果的低8位中1的个数设置。</font>
>

#### (8).cdqe
实战example：![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731817688236-69edec6a-71c4-4368-8c46-4b993765cab9.png)

食用方法：疑似直接cdqe即可

执行操作：<font style="color:rgb(26, 32, 41);">从eax寄存器中取出32位值，将这个32位值进行符号扩展到64位并把结果放入rax寄存器中。</font>

~~_<font style="color:rgb(26, 32, 41);">应该就是个32位转64位的东西没什么好说的</font>_~~

#### <font style="color:rgb(26, 32, 41);">(9).jle,jl,jg,jnz,etc.</font>
实战example：

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731818063674-7400c360-eac0-4682-91af-a3221e466343.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731818087486-8456f5dc-9016-44ec-a7b2-a2647d60f073.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731818136285-e48ef9dd-e707-4418-b211-20fc9a117cea.png)

![](https://cdn.nlark.com/yuque/0/2024/png/50154293/1731818047105-7f355db8-7110-4226-ab0f-1baf4fa1b008.png)

食用方法：与cmp一起食用，均为(2)jmp的变种，带条件的jmp

执行操作：

##### 1.jle&jbe：
意为J<font style="color:rgb(26, 32, 41);">ump if Less or Equal，使用时计算机会检查ZF、SF和OF，当ZF被设置||SF与OF的值不同时{</font>

<font style="color:rgb(26, 32, 41);">同jmp;</font>

<font style="color:rgb(26, 32, 41);">}</font>

<font style="color:rgb(26, 32, 41);">适用于有符号比较，无符号比较一般使用jbe（Jump if Below or Equal），同理侦测CF和ZF</font>

##### <font style="color:rgb(26, 32, 41);">2.jl&jg:</font>
jl意为<font style="color:rgb(26, 32, 41);">Jump if Less，使用时计算机会检查SF和OF，当SF=OF时，将执行jmp；</font>

<font style="color:rgb(26, 32, 41);">jg反之意为Jump if Greater，若SF不等于OF，代表cmp的结果是大于，此时将执行jmp；</font>

<font style="color:rgb(26, 32, 41);">同样适用于有符号的比较</font>

##### <font style="color:rgb(26, 32, 41);">3.jnz&jz</font>
<font style="color:rgb(26, 32, 41);">jnz意为Jump if Not Zero，使用时计算机会检查ZF，若ZF未被设置，则执行jmp；</font>

<font style="color:rgb(26, 32, 41);">反之jz意为Jump if Zero， 若ZF内值为一，则执行jmp</font>

### <font style="color:rgb(26, 32, 41);">三、其他</font>
目前看到的汇编语言指令基本都整理出来了，后面看到别的再补吧=.=

<font style="color:rgb(26, 32, 41);">以及域名终于审核完了，blog的网站就是</font>[www.sdtvdp.com/wordpress](https://www.sdtvdp.com/wordpress)了



~~~~



#### 
