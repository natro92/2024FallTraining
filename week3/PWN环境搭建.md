---



---

[本文参考](https://blog.csdn.net/j284886202/article/details/134931709)：[★pwn 22.04环境搭建保姆级教程★_pwn环境搭建-CSDN博客](https://blog.csdn.net/j284886202/article/details/134931709)

---

#### 一、系统环境
##### 1.VM虚拟机
我去VM官网看了看，emmm，看了半天愣是没看出来该怎么下载。。。

对于VMware这个软件的安装直接参考这篇文章吧，[VMware虚拟机安装Linux教程(超详细)](https://blog.csdn.net/weixin_52799373/article/details/124324077?ops_request_misc=%257B%2522request%255Fid%2522%253A%25224EFDDA87-6EB8-4C1C-88AC-39D930DEF2CD%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=4EFDDA87-6EB8-4C1C-88AC-39D930DEF2CD&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-124324077-null-null.142^v100^pc_search_result_base5&utm_term=vmware%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B&spm=1018.2226.3001.4187)

##### 2.配置Ubuntu映像

###### 2.1 Ubuntu官网

[Ubuntu](https://ubuntu.com/download/desktop)

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481608071-30eb096b-f828-4bbc-8be2-64d6ebd9ac86.png)

选择一个你想要的版本，直接下载。

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481628690-1d64c331-a435-4a9c-ad6e-ebb46d407d0c.png)

然后会自动跳转到这个界面，如果没有下载，点击“download now”，下载。

###### 2.2 创建虚拟机

​	点击 + 号创建新的虚拟机

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481640625-cf3b686c-5b75-4138-ac78-e83077cba0d6.png)

选择自定义，下一步。

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481648928-90d2275a-e431-4def-8183-a830fbdde98b.png)

选择下载好的映像文件，下一步。

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481659321-13219017-0bf6-463c-b4ea-c60a99bd37f6.png)

设置用户名和密码。

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481665418-c1a2b94c-e0c0-423d-a455-87c77b217694.png)

选择安装位置

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481680747-515fea58-a47f-4ac1-98d1-23dff964a988.png)

处理器，按照默认的来就行

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481690569-10346659-95a0-4aec-9aa1-96d4941e6fc1.png)

默认即可

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481698871-60e4a414-0073-41f4-af41-d8881193b344.png)

网络使用NAT地址



![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481707036-16be26fa-4dac-4bb8-9465-8b71abe08c2b.png)

按照推荐的就行

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482657322-1e1f4d8d-5f40-48cc-9927-a5fac3a8aaf2.png)

固态硬盘选择NVMe，机械硬盘选择SATA

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482636737-cb7aad6c-64c0-4609-9ec6-bd306b1068a1.png)

注：可按照以下步骤查看个人电脑硬盘类型

1.以管理员身份运行命令提示符

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482702142-2763bce1-1542-4d7d-b8cf-0b59b8efc7f2.png)

2. 依次输入下图中的四条命令

```plain
diskpart
list disk
select disk 0  //数字是想查看的硬盘的编号
detail disk
```

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482247754-971c674b-dc5c-4411-b2c1-806f4ae2c56b.png)

创建新磁盘

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482257466-707e1970-b981-49c2-9f67-981aeeba2806.png)

最大磁盘大小尽可能选大一些（那位师傅说如果选的太小了，以后扩容会很麻烦，而且这只是预留空间大小，并不是一下子就分出这么多空间给虚拟机，而是随着你添加的文件的增多而变大的）

存储为单个或多个文件都可以，单个文件更好管理一些（个人感觉）

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482268810-5a8ab018-9be1-43f2-a52a-0a9e5af59056.png)

下一步

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482273997-6dbc9e97-b9c7-4b12-ab8a-974cbe52d398.png)

取消勾选“创建后开启此虚拟机”，点击完成

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482278016-c35d134d-cdf7-4af2-ad18-a2c4909c6af4.png)



编辑设置

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482306656-a0617106-cd05-426c-a897-7ee6cf0435f5.png)

将 软盘 和 带数字的CD移除

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482312751-960667f4-99b5-4217-a92a-b2f83a5885e5.png)

然后在不带数字的CD里面配置安装好的Ubuntu映像文件

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482332343-7ffceae7-be66-4bda-bdb3-5eeac8706285.png)

开启虚拟机，出现以下内容，按Enter进入

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482346847-d099ae14-6add-4729-8a27-9cea88892e89.png)

选择 Install Ubuntu

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482363154-e79d2686-aa07-4ef3-9c29-13a3147d9050.png)

语言都选择“English（US）”，然后一路无脑按continue

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482370914-9e70790c-501a-4613-ab10-393d0b17437d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482376879-bd669597-c4e3-40f1-b161-c920395d58a7.png)

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482381217-4a8beaa3-2741-4be9-b478-3cb091e32b07.png)

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482386572-519997f4-9382-4927-813a-0bb13a62780c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482393447-27db1a09-d402-46a4-910e-a23289fefd66.png)

设置一下用户名和登录密码，继续continue

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482399571-a24bee9e-41de-4dfd-9b85-6b57d09f0887.png)

输入刚才设置的登录密码

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482404177-9e931065-b189-469d-863f-d49d28df340f.png)

点击右上角skip

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482410077-38e63b24-bc3b-4b43-aa1b-55c604dcd6cd.png)

会弹出一个下图这样的小窗口，点击“Install now”

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482415481-088803a6-a87d-4463-ab33-4750c7c4ec16.png)

关机后，来到设置里面，在CD/DVD 里面改为使用物理引擎（我这里不知道为什么是灰色的，点击不了，可能是因为我第一个虚拟机设置过使用物理引擎了。。。）

点击确定

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482759458-5684a7c1-7fac-458a-a68b-4463d4a5d97b.png)

重新启动，会弹出如下内容，直接点击否就行（因为我上一步没能设置，所以重启并不会有弹窗，就直接借用了另一位师傅的图片）

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482742233-1ca9c3ca-d2e4-4cad-9414-945d68ba6556.png)

拍摄一下快照保存一下状态，防止以后系统崩溃，导致现在的努力付诸东流。

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482190880-e3a31d36-8786-4fdd-8def-da000a0a6a61.png)

至此，Ubuntu操作系统安装完成。

#### 二、PWN工具安装
##### 右击桌面，打开terminal
![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482174868-4defca81-3bb4-44c3-a325-74a58f96a404.png)

输入

```plain
sudo apt install pipx
pipx ensurepath
```

(按照那位师傅的说法，在Ubuntu23.以上的版本可能会出现pip（externally-managed-environment）错误，因为Ubuntu23.使用Python包来实现增强功能，以避免包管理工具之间的冲突)

##### vim
```plain
sudo apt install vim
```

##### gcc
```plain
sudo apt install gcc
```

##### python3-pipx
```plain
sudo apt install python3-pipx
```

##### pwntools
```plain
sudo pipx install pwntools
```

##### checksec
```plain
sudo apt install checksec
```

##### git
```plain
sudo apt install git
```

##### pwndbg
```plain
cd ~/
git clone https://github.com/pwndbg/pwndbg
cd ~/pwndbg
./setup.sh
cd ..
```

##### ipython
```plain
sudo pipx install ipython
```

##### LibcSearcher
```plain
cd ~/
git clone https://github.com/lieanu/LibcSearcher.git
cd LibcSearcher
sudo python3 setup.py develop
cd ..
```

##### ROPgadget
```plain
sudo pip install capstone
sudo pip install ropgadget
```

##### ropper
```plain
sudo pipx install ropper
```

##### one_gadget
```plain
sudo apt install ruby
sudo apt install ruby-dev
sudo gem install one_gadget
```

##### seccomp-tools
```plain
sudo gem install seccomp-tools
```

##### glibc-all-in-one
```plain
cd ~/
git clone https://github.com/matrix1001/glibc-all-in-one
cd ~/glibc-all-in-one
sudo python3 update_list
cd ..
```

##### patchelf
```plain
sudo apt install patchelf
```

##### clibc
```plain
cd ~/
git clone https://github.com/dsyzy/free-libc
cd ~/free-libc
sudo sh ./install.sh
cd ..
```

工具安装结束，保存一个快照。

#### 三、Linux系统美化
##### 3.1 VIM
进入vimrc

```plain
vim ~/.vimrc
```

将以下代码复制粘贴进去（）

```plain
syntax on "自动语法高亮
winpos 5 5          " 设定窗口位置 
set lines=40 columns=155    " 设定窗口大小 
set nu              " 显示行号 
"set go=             " 不要图形按钮 
"color asmanian2     " 设置背景主题 
set guifont=Courier_New:h10:cANSI   " 设置字体 
"syntax on           " 语法高亮 
autocmd InsertLeave * se nocul  " 用浅色高亮当前行 
autocmd InsertEnter * se cul    " 用浅色高亮当前行 
set ruler           " 显示标尺 
set showcmd         " 输入的命令显示出来，看的清楚些 
"set cmdheight=1     " 命令行（在状态行下）的高度，设置为1 
"set whichwrap+=<,>,h,l   " 允许backspace和光标键跨越行边界(不建议) 
"set scrolloff=3     " 光标移动到buffer的顶部和底部时保持3行距离 
set novisualbell    " 不要闪烁(不明白) 
set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ %{strftime(\"%d/%m/%y\ -\ %H:%M\")}   "状态行显示的内容 
set laststatus=1    " 启动显示状态行(1),总是显示状态行(2) 
set foldenable      " 允许折叠 
set foldmethod=manual   " 手动折叠 
"set background=dark "背景使用黑色
set nocompatible  "去掉讨厌的有关vi一致性模式，避免以前版本的一些bug和局限 

" 显示中文帮助
if version >= 603
    set helplang=cn
    set encoding=utf-8
endif

" 设置配色方案
"colorscheme murphy

"字体
"if (has("gui_running"))
"   set guifont=Bitstream\ Vera\ Sans\ Mono\ 10
"endif

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"""""新文件标题""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"新建.c,.h,.sh,.java文件，自动插入文件头 
autocmd BufNewFile *.cpp,*.[ch],*.sh,*.java exec ":call SetTitle()" 

""定义函数SetTitle，自动插入文件头 
func SetTitle() 
    "如果文件类型为.sh文件 
    if &filetype == 'sh' 
        call setline(1,"\#########################################################################") 
        call append(line("."), "\# File Name: ".expand("%")) 
        call append(line(".")+1, "\# Author: zll") 
        call append(line(".")+2, "\# mail: zhnlion@126.com") 
        call append(line(".")+3, "\# Created Time: ".strftime("%c")) 
        call append(line(".")+4, "\#########################################################################") 
        call append(line(".")+5, "\#!/bin/bash") 
        call append(line(".")+6, "") 
    else 
        call setline(1, "/*************************************************************************") 
        call append(line("."), "    > File Name: ".expand("%")) 
        call append(line(".")+1, "    > Author: zll") 
        call append(line(".")+2, "    > Mail: zhnllion@126.com ") 
        call append(line(".")+3, "    > Created Time: ".strftime("%c")) 
        call append(line(".")+4, " ************************************************************************/") 
        call append(line(".")+5, "")
    endif
    if &filetype == 'cpp'
        call append(line(".")+6, "#include<iostream>")
        call append(line(".")+7, "using namespace std;")
        call append(line(".")+8, "")
    endif
    if &filetype == 'c'
        call append(line(".")+6, "#include<stdio.h>")
        call append(line(".")+7, "")
    endif

    "新建文件后，自动定位到文件末尾
    autocmd BufNewFile * normal G
endfunc 


""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"键盘命令
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
nmap <leader>w :w!<cr>
nmap <leader>f :find<cr>

" 映射全选+复制 ctrl+a
map <C-A> ggVGY
map! <C-A> <Esc>ggVGY
map <F12> gg=G

" 选中状态下 Ctrl+c 复制
vmap <C-c> "+y

"去空行  
nnoremap <F2> :g/^\s*$/d<CR> 

"比较文件  
nnoremap <C-F2> :vert diffsplit 

"新建标签  
map <M-F2> :tabnew<CR>  

"列出当前目录文件  
map <F3> :tabnew .<CR>  

"打开树状文件目录  
map <C-F3> \be  

"C，C++ 按F5编译运行
map <F5> :call CompileRunGcc()<CR>
func! CompileRunGcc()
    exec "w"
    if &filetype == 'c'
        exec "!g++ % -o %<"
        exec "! ./%<"
    elseif &filetype == 'cpp'
        exec "!g++ % -o %<"
        exec "! ./%<"
    elseif &filetype == 'java' 
        exec "!javac %" 
        exec "!java %<"
    elseif &filetype == 'sh'
        :!./%
    endif
endfunc

"C,C++的调试
map <F8> :call Rungdb()<CR>
func! Rungdb()
    exec "w"
    exec "!g++ % -g -o %<"
    exec "!gdb ./%<"
endfunc


""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
""实用设置
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

" 设置当文件被改动时自动载入
set autoread

" quickfix模式
autocmd FileType c,cpp map <buffer> <leader><space> :w<cr>:make<cr>

"代码补全 
set completeopt=preview,menu 

"允许插件  
filetype plugin on

"共享剪贴板  
set clipboard+=unnamed 

"从不备份  
set nobackup

"make 运行
:set makeprg=g++\ -Wall\ \ %

"自动保存
set autowrite
set ruler                   " 打开状态栏标尺
set cursorline              " 突出显示当前行
set magic                   " 设置魔术
set guioptions-=T           " 隐藏工具栏
set guioptions-=m           " 隐藏菜单栏

"set statusline=\ %<%F[%1*%M%*%n%R%H]%=\ %y\ %0(%{&fileformat}\ %{&encoding}\ %c:%l/%L%)\
" 设置在状态行显示的信息
set foldcolumn=0
set foldmethod=indent 
set foldlevel=3 
set foldenable              " 开始折叠

" 不要使用vi的键盘模式，而是vim自己的
set nocompatible

" 语法高亮
set syntax=on

" 去掉输入错误的提示声音
set noeb

" 在处理未保存或只读文件的时候，弹出确认
set confirm

" 自动缩进
set autoindent
set cindent

" Tab键的宽度
set tabstop=4

" 统一缩进为4
set softtabstop=4
set shiftwidth=4

" 不要用空格代替制表符
set noexpandtab

" 在行和段开始处使用制表符
set smarttab

" 显示行号
set number

" 历史记录数
set history=1000

"禁止生成临时文件
set nobackup
set noswapfile

"搜索忽略大小写
set ignorecase

"搜索逐字符高亮
set hlsearch
set incsearch

"行内替换
set gdefault

"编码设置
set enc=utf-8
set fencs=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936

"语言设置
set langmenu=zh_CN.UTF-8
set helplang=cn

" 我的状态行显示的内容（包括文件类型和解码）
"set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ %{strftime(\"%d/%m/%y\ -\ %H:%M\")}
"set statusline=[%F]%y%r%m%*%=[Line:%l/%L,Column:%c][%p%%]

" 总是显示状态行
set laststatus=2

" 命令行（在状态行下）的高度，默认为1，这里是2
set cmdheight=2

" 侦测文件类型
filetype on

" 载入文件类型插件
filetype plugin on

" 为特定文件类型载入相关缩进文件
filetype indent on

" 保存全局变量
set viminfo+=!

" 带有如下符号的单词不要被换行分割
set iskeyword+=_,$,@,%,#,-

" 字符间插入的像素行数目
set linespace=0

" 增强模式中的命令行自动完成操作
set wildmenu

" 使回格键（backspace）正常处理indent, eol, start等
set backspace=2

" 允许backspace和光标键跨越行边界
set whichwrap+=<,>,h,l

" 可以在buffer的任何地方使用鼠标（类似office中在工作区双击鼠标定位）
set mouse=a
set selection=exclusive
set selectmode=mouse,key

" 通过使用: commands命令，告诉我们文件的哪一行被改变过
set report=0

" 在被分割的窗口间显示空白，便于阅读
set fillchars=vert:\ ,stl:\ ,stlnc:\

" 高亮显示匹配的括号
set showmatch

" 匹配括号高亮的时间（单位是十分之一秒）
set matchtime=1

" 光标移动到buffer的顶部和底部时保持3行距离
set scrolloff=3

" 为C程序提供自动缩进
set smartindent

" 高亮显示普通txt文件（需要txt.vim脚本）
au BufRead,BufNewFile *  setfiletype txt

"自动补全
:inoremap ( ()<ESC>i
:inoremap ) <c-r>=ClosePair(')')<CR>
:inoremap { {<CR>}<ESC>O
:inoremap } <c-r>=ClosePair('}')<CR>
:inoremap [ []<ESC>i
:inoremap ] <c-r>=ClosePair(']')<CR>
:inoremap " ""<ESC>i
:inoremap ' ''<ESC>i

function! ClosePair(char)
    if getline('.')[col('.') - 1] == a:char
        return "\<Right>"
    else
        return a:char
    endif
endfunction

filetype plugin indent on 
"打开文件类型检测, 加了这句才可以用智能补全
set completeopt=longest,menu


"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" CTags的设定  
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
let Tlist_Sort_Type = "name"    " 按照名称排序  
let Tlist_Use_Right_Window = 1  " 在右侧显示窗口  
let Tlist_Compart_Format = 1    " 压缩方式  
let Tlist_Exist_OnlyWindow = 1  " 如果只有一个buffer，kill窗口也kill掉buffer  
let Tlist_File_Fold_Auto_Close = 0  " 不要关闭其他文件的tags  
let Tlist_Enable_Fold_Column = 0    " 不要显示折叠树  
autocmd FileType java set tags+=D:\tools\java\tags  
"autocmd FileType h,cpp,cc,c set tags+=D:\tools\cpp\tags  
"let Tlist_Show_One_File=1            "不同时显示多个文件的tag，只显示当前文件的
"设置tags  
set tags=tags  
"set autochdir 


""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"其他东东
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"默认打开Taglist 
let Tlist_Auto_Open=1 


"""""""""""""""""""""""""""""" 
" Tag list (ctags) 
"""""""""""""""""""""""""""""""" 
let Tlist_Ctags_Cmd = '/usr/bin/ctags' 
let Tlist_Show_One_File = 1 "不同时显示多个文件的tag，只显示当前文件的 
let Tlist_Exit_OnlyWindow = 1 "如果taglist窗口是最后一个窗口，则退出vim 
let Tlist_Use_Right_Window = 1 "在右侧窗口中显示taglist窗口
" minibufexpl插件的一般设置
let g:miniBufExplMapWindowNavVim = 1
let g:miniBufExplMapWindowNavArrows = 1
let g:miniBufExplMapCTabSwitchBufs = 1 
let g:miniBufExplModSelTarget = 1

```

##### 3.2 窗口（实现窗口的亚克力效果）
打开终端执行

```plain
sudo apt install chrome-gnome-shell
sudo apt install gnome-shell-extensions
sudo apt install gnome-tweaks
```

打开火狐浏览器，访问[https://extensions.gnome.org](https://extensions.gnome.org)

点击下图标注位置，安装插件

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482145116-019d7a66-3ac9-4ba1-94de-10e18b230c33.png)

continue

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482138921-7d580393-066a-4460-abc9-d4c878acde34.png)

点击Add

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482133635-01d763e9-a2fd-4675-a904-cb3c0ae3dc7f.png)

点击Allow

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482128622-20ed3322-b18e-4291-a555-c0b91c1afa68.png)

搜索Blur my Shell，进入

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482112896-cf3dea40-f940-441e-96ef-2f478c5bfff0.png)

点进来后，如果安装成功，右上角会有个off，把它调为on开始安装，然后弹窗，点击install.

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482099782-d7d38d45-4ef8-4f7c-a33e-ad948686b20a.png)

进入终端，键入以下命令，启动Blur my Shell

```plain
gnome-extensions prefs blur-my-shell@aunetx
```

点击Application，再点击Add，就可以选定窗口，然后将那个窗口毛玻璃化，如果出现无法选定的情况，把那个窗口拖着晃几下，多Add几次。

数值条那里可以设置虚化程度，数值越小越透明。

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482082924-74ceab56-f6ad-4e71-8e51-e69f8e383701.png)

按照下图点击，

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482056521-e4f2d562-b6e1-4d70-8c3d-e996483894f0.png)

回到之前网页，搜索并安装Dash to Panel插件

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482050600-a49b9e0f-fbd8-4390-bc93-3f98b6aefc5c.png)

安装完成后来到桌面，你会发现原本的侧边栏，移到了下方。

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482044843-627c0b90-0736-414d-8a53-acc9ff70fc44.png)

保存快照。

##### 3.3 终端
必需字体下载（来自那位伟大的师傅的分享）

[百度网盘（提取码==yxxx）](https://pan.baidu.com/share/init?surl=cIiCmItu3hg6MUGn3YwN3w&pwd=yxxx)

下载完成后，复制进Ubuntu

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482025322-d3fe8579-ab86-4907-8fad-05cbadca08ae.png)

依次双击，然后点击右上角的绿色的install，下载完成后它会变为灰色（三个字体都要下载）

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482012684-3b999057-837e-4c14-b128-4f3b3aa97889.png)

打开终端，按照下图依次选择，

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732482003543-c97d7aa4-5f4d-4638-85d6-4f06d9dfdb03.png)

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481998890-f3bf29e3-c4c4-4cfb-b531-54e64672149c.png)

保存快照

打开终端，输入以下指令

zsh

```plain
sudo apt install zsh
```

oh-my-zsh

```plain
cd ~/
sudo git clone https://github.com/robbyrussell/oh-my-zsh
cd ~/oh-my-zsh/tools
sudo sh install.sh
exit
```

插件

```plain
sudo apt install zsh-autosuggestions
sudo apt install zsh-syntax-highlighting
sudo mv /usr/share/zsh-autosuggestions/zsh-autosuggestions.zsh /usr/share/zsh-autosuggestions/zsh-autosuggestions.plugin.zsh
sudo mv /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.plugin.zsh
sudo cp -r /usr/share/zsh-autosuggestions ~/oh-my-zsh/custom/plugins
sudo cp -r /usr/share/zsh-syntax-highlighting ~/oh-my-zsh/custom/plugins
sudo git clone https://github.com/romkatv/powerlevel10k ~/oh-my-zsh/themes/powerlevel10k
sudo vim ~/.zshrc
```

输入以下指令

```plain
export ZSH="$HOME/oh-my-zsh"
ZSH_THEME="powerlevel10k/powerlevel10k"
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
source /usr/share/zsh-autosuggestions/zsh-autosuggestions.plugin.zsh
source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.plugin.zsh
source $ZSH/oh-my-zsh.sh
```

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481987613-83c2f234-2881-4ccc-83d7-b0c55f4a857f.png)

wq 保存后退出。

终端输入

```plain
sudo chmod 777 ~/.zshrc
chsh -s /bin/zsh
```

输入后重启

打开终端，就会有以下提示，这是终端的美化选择。

第一页问你能否看到钻石（diamond）图标吗？如果能看到键入y

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481945200-af078326-c8e8-4a36-83dd-3cf3bcd5942f.png)

能看到，键入y

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481940154-ee20864d-91f7-416e-b465-725e26ffb888.png)

能看到，键入y

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481929423-0d3a9dfb-8c46-47e1-b8f2-aa3c1d9538f5.png)

很紧凑，且没有符号被覆盖，键入y

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481922363-96e508ee-d0d8-44d8-ba03-7b339261081a.png)

接下来就是图标的个性化设置了，自己怎么喜欢怎么来，

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481917286-74375b39-9142-4f61-9586-94f03cea9b05.png)

选1

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481912574-6d2567aa-928a-4ee2-beec-a2de355dd3c1.png)

行末是否显示时间。若显示，是十二小时制还是二十四小时制

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481905442-fe4bac23-8825-4472-97b7-a603f4caebea.png)

选择中部风格

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481900358-6055d302-c869-4850-96fe-646e5d35b6dd.png)

选择头部风格

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481892579-fa9e43c6-1e1a-4697-956c-90a89aef976a.png)

选择尾部风格

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481888153-905f98e0-3a98-41dc-b576-3de8bd54977e.png)

选择是一行还是两行（输入的指令是和图标处在同一行，还是另起一行）

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481883917-e0bba430-9a47-4a35-ab42-8020e71b6527.png)

紧凑在一起还是分开

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481879804-748c189d-595d-4c2f-9039-062aaa62f792.png)

看个人爱好

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481874647-fb8d06f9-5afb-4773-a9c0-c8ae82a86cc6.png)

是否显示上一次输入的指令，选y

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481864517-648488e7-1dc7-4b54-a64d-816f3c4faccc.png)

选1

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481857245-eb5f0bd7-48d1-4370-b959-38af0fa4e01f.png)

选y ,完成安装

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481848928-5c908bad-2c82-4b26-a42a-0aea50cac882.png)



终端输入指令

```plain
source ~/.zshrc
```

然后调整颜色

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481832921-76ba4954-b0fb-4026-a7a4-90982a3f1a51.png)

两个数值分别设置为120 和 36

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481819846-d2037244-881f-497c-9bff-f648b747f03a.png)

按照下图依次点击，将颜色改为#FF00FF，这是终端的文本颜色

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481812462-a679f7ad-a94b-40ab-a69d-e8acdf48d297.png)

取消下面，然后勾选红框圈住的选项。

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481803242-7795659a-c38a-46b9-964d-dfd8cc1daaaf.png)

关闭，然后重新打开终端。

到这里，你就已经拥有了个性化的终端。保存快照。

效果示例

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481786568-3528a4cb-273f-402d-b120-6fbadb9944d1.png)

##### 3.4 壁纸
（我的壁纸设置很简单）

首先，把图片复制进 /home/picture 文件里面

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481772614-df6bad9a-7942-4b55-a85b-885913fec761.png)

回到桌面，右击，点击“change background”

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481757797-d0ac9360-3f47-465b-9e7b-a660337cf372.png)

点击右上角Add，选择图片。

![](https://cdn.nlark.com/yuque/0/2024/png/40792144/1732481747421-f9099a5f-947a-41ed-bbf7-f63f889faa11.png)

