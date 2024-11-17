#### Linux终端
在使用LInux时，我们并不是直接与底层系统交互的，而是通过一个名为“Shell”的中间程序来完成的。除此以外，Linux系统还提供了一个模拟终端的程序“Terminal”（终端模拟器）。

终端本质上是对应着Linux上的/dev/tty设备，Linux的多用户登录就是通过不同的/dev/tty设备来完成的。Linux系统默认提供了6个纯命令行界面的“terminal”来让用户登录，通过`Ctrl + Alt + F1 ~ F6`来切换。如果想回到图形界面可以按下`Ctel + Alt + F7`。

#### Shell(壳)
“壳”--“核”  ~~  “Shell”--“UNIX/LInux内核”。

<font style="color:#000000;background-color:#D9EAFC;">Shell是</font>指“提供给使用者使用界面”的<font style="background-color:#D9EAFC;">命令解释器</font>。（之所以被称作是Shell是因为它隐藏了操作系统的底层细节）

#### Linux命令
##### 常用快捷键
| <font style="color:#DF2A3F;">Ctrl + c</font> | 强行终止当前程序 | |
| :---: | :---: | --- |
| <font style="color:#DF2A3F;">Ctrl + d</font> | 键盘输入结束或退出终端 | |
| <font style="color:#DF2A3F;">Ctrl + s</font> | 暂停当前程序，暂停后按任意键恢复运行 | |
| <font style="color:#DF2A3F;">Ctrl + z</font> | 将当前程序放到后台运行，恢复到前台的为命令<font style="color:#DF2A3F;">fg</font> | |
| <font style="color:#DF2A3F;">Ctrl + a</font> | 将光标移至行头，相当于<font style="color:#DF2A3F;">Home</font>键 | |
| <font style="color:#DF2A3F;">Ctrl + e</font> | 将光标移至行末，相当于<font style="color:#DF2A3F;">End</font>键 | |
| <font style="color:#DF2A3F;">Ctrl + k</font> | 删除从光标所在位置到行末 | |
| <font style="color:#DF2A3F;">Alt + Backspace</font> | 向前删除一个单词 | |
| <font style="color:#DF2A3F;">Shift + PgUp</font> | 将终端显示向上滚动 | |
| <font style="color:#DF2A3F;">Shift + PgDn</font> | 将终端显示向下滚动 | |


##### 通配符
通配符主要有星号（*）和问号（？），其<font style="background-color:#D9EAFC;">用途是</font>为了<font style="background-color:#D9EAFC;">对字符串进行模糊匹配</font>。

<font style="color:#DF2A3F;">**</font>	<u>在终端里面输入的通配符由Shell来处理，并不是由所涉及的命令语句来处理的，它只会出现在命令的“参数里”。</u>

当Shell在“参数值”中遇到通配符时，Shell会将其当做路径或文件名在磁盘上搜寻可能得匹配；如果符合要求的匹配存在，则进行代换（路径拓展）；否则将该通配符作为一个普通的字符传递给“命令”。在通配符被处理后，Shell会先完成该命令的重组，然后继续处理重组后的命令，直至执行该命令。

常见通配符

| <font style="color:#DF2A3F;">*</font> | 匹配 0 或多个字符<font style="color:#DF2A3F;">（也可以把 * 理解成“全部”的意思</font>） | |
| :---: | :---: | --- |
| <font style="color:#DF2A3F;">？</font> | 匹配任意一个字符 | |
| <font style="color:#DF2A3F;">[list]</font> | 匹配 list中的任意单一字符 | |
| <font style="color:#DF2A3F;">[^list]</font> | 匹配 除list中的任意单一字符以外的字符 | |
| <font style="color:#DF2A3F;">[c1-c2]</font> | 匹配c1-c2中的任意单一字符。如：[0-9][a-z] | |
| <font style="color:#DF2A3F;">{string1,string2,....}</font> | 匹配string1或string2其一字符串 | |
| <font style="color:#DF2A3F;">{c1..c2}</font> | 匹配c1-c2中全部字符 如下文例2中{1..10} | |


```bash
touch demo01.cpp demo02.txt
ls *.cpp
```

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731845588952-b34abfe1-d03c-4f54-b25d-6bccd040c6bd.png)

例2：同时创建多个文件，可使用`{1..n}`来同时创建n个文件

```bash
touch love_{1..10}.txt
ls *.txt
```

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731845811661-2fb760f3-45b3-40be-aebe-905ed13c9be7.png)

#####  man命令（Manual pages的缩写）
3.3.1	Manual pages 是 UNIX 或类 UNIX操作系统中在线软件文档的一种普遍形式，内容包括计算机程序（库和系统调用等）、正式的标准和惯例，甚至是抽象的概念。

3.3.2	使用格式

```bash
man <command_name>
```

手册通常被分为8个分区：

| 1 | 一般命令 | |
| :---: | :---: | --- |
| 2 | 系统调用 | |
| 3 | 库函数，（包含C标准函数库） | |
| 4 | 特殊文件（通常是/dev 中的设备）和驱动程序 | |
| 5 | 文件格式和约定 | |
| 6 | 游戏和屏保 | |
| 7 | 杂项 | |
| 8 | 系统管理命令和守护进程 | |


#### Linux用户管理
##### 查看用户
`<font style="color:#DF2A3F;">who am i</font>`<font style="color:#DF2A3F;">:用来查看打开当前伪终端的用户的用户名。（记录登录shell时的用户，你以什么用户登录就显示什么）</font>

`<font style="color:#DF2A3F;">whoami</font>`<font style="color:#DF2A3F;">：查看当前登录的用户的用户名（当前系统的有效用户）</font>

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731847508663-8867349f-a533-42b1-8458-fb0c62a25649.png)

第一列便是相应的用户名了。而第二列的`pst/0`中`pst`表示伪终端、后面的数字便是伪终端的序号。使用/dev/tty7时每打开一个终端，就会产生一个伪终端。（说白了就是，在桌面右击打开的终端，都是伪终端，每次打开，都是在创建新的伪终端。）

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731847863755-06f86696-99ee-40d4-82cb-b1bb0510b1b7.png)

who命令的其它常用参数

| 参数 | 说明 | |
| :---: | :---: | --- |
| <font style="color:#DF2A3F;">-a（all）</font> | 打印能打印的全部 | |
| <font style="color:#DF2A3F;">-d(died)</font> | 打印死掉的进程 | |
| <font style="color:#DF2A3F;">-m</font> | 同`am i`,`mom likes` | |
| <font style="color:#DF2A3F;">-q</font> | 打印当前登录用户数及用户名 | |
| <font style="color:#DF2A3F;">-u</font> | 打印当前登录用户登录信息 | |
| <font style="color:#DF2A3F;">-r(rank)</font> | 打印运行等级 | |


##### 创建用户
###### 在Linux系统中，<font style="color:#DF2A3F;">root</font>账户拥有整个系统至高无上的权限。
![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731848572958-697021b1-152f-4dd9-835b-8b2aec861ec4.png)

######  sudo命令（Super User Do ）
`<u>su <user></u>`<u>:切换到用户user，执行时需要输入目标用户的密码</u>

`<u>su - <user></u>`<u>:切换到目标用户，但是同时用户的环境变量和工作目录也会跟着改变成目标用户所对应的。</u>

`<u>sudo <cmd></u>`<u>:可以以特权级别运行cmd命令，需要当前用户属于sudo组，且输入当前账户的密码。</u>



例：1.创建一个叫 lilie 的用户，键入以下指令

```bash
sudo adduser lilei		//这个命令不仅可以添加用户到系统，
                      //同时也默认 为新用户在/home目录下创建一个工作目录
```

然后就是给 lilei 用户设置密码。然后就可以一路 enter 键直到创建用户结束。

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731849347003-5cd1ca10-b573-4173-b76e-76c359ab5e1c.png)

依次键入以下命令

```bash
su -l lilei		//切换到 lilei 用户
who am i			//查看登录shell用
whoami				//查看当前用户
pwd						//查看工作路径（Print Working Directory）
```

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731850327902-5fbe0886-d8fc-4a68-9229-3f112fce3f33.png)

(这里可以很明显地看出来`who am i`和`whoami`的区别)

2.用`exit`命令或快捷键`Ctrl + D`退出当前用户

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731850461785-db7b25e4-ac96-44f2-98a5-70a10c9aa890.png)



##### 用户组
###### 
Linux里面，每个用户都有一个归属（用户组），简单地说就是用户组是一组用户的集合。一个用户可以属于多个用户组。

######  查看当前用户归属于那个用户组
法一：使用 groups 命令查看`groups <user>`

```bash
groups lilei
```

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731851111034-ddb44676-43fa-45ee-bbad-ddb0bbd853ce.png)

<font style="color:#DF2A3F;">冒号之前表示用户，冒号之后便是该用户所属用户组。每次新建用户，如果不指定用户组，系统会自动创建一个与用户名相同的用户组。</font>

法二：查看/etc/group文件

```bash
cat /etc/group  | sort		// | sort  表示将读取的文本进行一个字典排序再输出。
```

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731851473539-82d88bc3-fd0f-4f03-a04d-7697be3bd5e5.png)

但是，这样得到的内容太杂乱，不方便找到目标用户组。

可以使用 <font style="color:#DF2A3F;">grep </font>命令，过滤掉不需要的结果

```bash
cat /etc/group | grep -E "lilei"
```

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731851593771-ec406da0-0657-4eb9-ae0a-6c3635006880.png)

补：/etc/group文件格式说明

/etc/group 的内容包括 用户组（Group）、用户组口令 、GID（组ID）、及该用户组所包含的用户（User），每个用户组一条记录，格式如下：

```bash
group_name : password : GID : user_list
```

###### 将其他用户添加到 sudo 用户组
默认情况下，新创建的用户是不具有 root 权限的，也不再sudo用户组，可以让其加入sudo用户组从而获得相应权限。

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731852019780-a8815e76-2ced-44db-bebf-189b6309d59b.png)



使用 usermod 命令为用户添加用户组，<u>使用该命令必须具有root权限</u>

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731852278068-51add412-c7fb-41b9-9209-64edc47e21a0.png)



##### 删除用户和用户组
######  使用`deluser`命令删除用户
```bash
sudo deluser <user_name> --remove-home
```

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731852510325-cdc64c1b-1fca-45b8-824d-1c02e800001d.png)

使用`--remove-home`参数在删除用户时会一并将该用户的工作目录删除。如果不使用那么系统会自动在 /home 目录为该用户保留工作目录

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1731852660651-b6636cb7-6dce-496e-9952-555822c51918.png)

######  使用`groupdel`命令删除用户组。必须确保群组中不包含任何用户，才能将群组删除。


