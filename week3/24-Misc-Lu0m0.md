24-Misc-Lu0m0

# NewStar CTF 2024  Misc
## 1.AmazingGame
提示：提示:出题人们都好狠啊，题做累了不如来玩玩游戏吧，还有就是，安卓软件私有数据会放在哪？(游戏至少通过一个关卡才会有存档)	

1.开始尝试jadx来逆向apk，但根据提示搜索良久没有发现有用的线索，于是换IDA进行逆向。

2.开始选择了这个AndroidManifest.xml进入ida，结果没有什么东西

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731995015187-c4eb6d84-e4c4-4c37-b049-00646ed4325c.png)



3.然后又重新选择，浏览文件的时候发现了classes这个敏感文件名，个人不太懂安卓逆向，刚刚入门，但对class比较熟悉，是编程语言中常用的类，觉得这个class应该是应用的一个重要文件。后面查资料得知 classes.dex (** **dex全称：Dalvik Executable )：包含应用的所有编译后的 Java 类（包括方法、属性、常量等），是 Android 应用的可执行字节码文件。所以我认为里面肯定藏有与flag相关的线索。

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731995027788-51816140-e79e-4347-8d23-f85f6a594eba.png)

4.进入ida后，浏览了一遍函数名称，发现没有main函数，然后尝试F12+shitf查看字符串，看看能得到什么线索不。然后看到了Zmxh这个敏感关键词，于是拿去base64解码得到flag.

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731995212310-5d5e7d5c-6877-4629-9cdc-adb644a8107b.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731995255436-af7d4b9c-168c-421c-91d5-c4cc3640571a.png)

flag{U_W1n!!_7he_g@m4}

## <font style="color:rgb(31, 9, 9);">2.BGM坏了吗？</font>
提示：刚想听一篇推文，但是bgm吵死了，我忍了又忍，摈除杂音仔细听，up难道在拨号吗？

1.<font style="color:rgb(31, 9, 9);">听这个音乐听到后面听到了拨号声音，猜想是拨号声转数字，然后到网上查找资料，发现有响应的解码软件，但直接拿</font><font style="color:rgb(77, 77, 77);">dtmf2num软件</font><font style="color:rgb(31, 9, 9);">去解码，得到了乱七八糟的结果，卡住了。后面又查资料，发现可以通过使用Audacity查看音频的频率，然后双轨频率拨号编码图去一个个手动破解。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731993368065-9da8fe11-23f2-4dfc-aa05-0e6b0cd53602.png)

2.用Audacity打开wav文件，然后右键文件名字，选择多视图查看，就可以看见音频的双轨频率了，但是由于尺度较大，辨识不了，所以我们选中频谱图设置把频率范围调成697-1633

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731993601176-acf8d8b3-22cc-4210-a684-5fbc74a0c34a.png)![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731993737879-44ca76cd-189b-4c66-a220-070099f71341.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731994034014-73d671e5-bbff-4e86-9999-9c467401e986.png)  


 3.放大频谱图对照，翻译，得到：2024093020241103

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731994308716-e18e60d0-2fbd-481d-a0ac-de194ed55420.png)	

写完后看官方解，和我不一样，于是复现官方给的步骤：

1.用Audacity打开文件，然后右键文件名<font style="color:rgb(63, 63, 70);">选择</font>**分离立体音到单声道 » 关闭左声道 » 导出wav文件**

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731994735575-57c3828e-fab1-41c4-80bb-be48650f3edd.png)

2.选择<font style="color:rgb(63, 63, 70);">按键音（即DTMF）解密网站上传导出wav进行解密：</font>[https://dtmf.netlify.app/](https://dtmf.netlify.app/)



![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1731994535800-889469b6-0679-4a91-87f9-715c05c1d64a.png)

## 3.OSINT-MASTER
1.根据给的图片信息知道了飞机的类型是：B2419，查看图片属性得知：2024/8/18 14:30拍的

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732106312956-60ee298d-fa75-49af-b950-4a1c39d26c86.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732106374616-e82848bd-fd8e-4e50-86ae-56a6aae2046d.png)

2.在百度上搜索B2419得知了FlightAware软件可以查询历史航班信息，由于复现的时候距离8/18已经过去3个月了看不到8/18的，但根据经验航班航线是会有重复的，可以找其他时间同样的航班代替。找到8/25号时间。

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732107106393-b24912fe-430c-4c2f-bd13-f967c1b4773a.png)



![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732106921914-920c0e02-b9fe-440f-9f4a-472dfbd0706a.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732106989099-de57e220-01b1-4e8a-9170-9906ca1666a7.png) 



3.根据飞机的航班线，不断尝试得到是在济宁那一块。

## 4.ez_jail
1.下载附件得到server.py 和template文件，大概看了一下，就是让你写一个c++代码，将以Hello, World!开头的字符串写到标准输出中，然后代码不能包含#include、#define 、{ 、}和长度得限定在100字节。

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732182789783-dbb0f9c5-8188-4bbb-88c6-79d55d171e9e.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732107781981-1ec7b380-3e84-4b43-827a-b6ab605e52f6.png)

2. 查找c的代替运算符资料，发现{}可以使用<% %>来替换，于是使用以下代码可以绕过检查：

 void user_code() <%write(1, "Hello, World!\n", 14);%>

代码解释：将“Hello, World!”写到标准输出中

<font style="color:rgb(6, 6, 7);">write：这是Linux系统中的一个系统调用，用于写数据。</font>

<font style="color:rgb(6, 6, 7);">1：这是文件描述符的编号。在Unix和Linux系统中，文件描述符0、1和2分别对应于标准输入（stdin）、标准输出（stdout）和标准错误（stderr）。这里1代表标准输出，也就是终端屏幕。</font>

14<font style="color:rgb(6, 6, 7);">：这是要写入的字节数。"Hello, World!"字符串的长度是13个字符（包括</font>`\n`<font style="color:rgb(6, 6, 7);">），加上字符串末尾的空字符（null terminator）总共是14个字符。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732188916731-746e942b-3734-4732-97d4-f1554b2b031b.png)

3.按照西电里面知识模块中的题目连接指南下载软件，然后在该网址:[https://eternallybored.org/misc/netcat](https://eternallybored.org/misc/netcat)下载nc连接软件。按照步骤连接到题目，之后把代码编码为base64输入得到flag

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732189335512-bd277b11-f58f-487b-9f08-371f722321d2.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732189758116-72f26f4d-5ac2-4217-bcc6-c8748bbadeae.png)



![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732189527507-8132fb0e-4dc7-4e8e-a46a-febe60305df2.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732189584449-1046ded9-aad0-482e-a7fa-431c6d73934e.png)



![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732189875422-bf9d2fa9-5814-4800-b819-991fa39884f2.png)



## 5.擅长音游的小明同学


1.题目给的附件是一个E01磁盘镜像，要进行取证分析，需要用到FTK Imager，查资料了解到：<font style="color:rgb(77, 77, 77);">FTK Imager是一款强大的数字取证工具，广泛应用于数据成像和证据收集领域，支持多种存储介质和文件系统。项目地址:</font>[https://gitcode.com/open-source-toolkit/cefbd](https://gitcode.com/open-source-toolkit/cefbd/?utm_source=tools_gitcode&index=top&type=href&)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732191208769-c0a0ba08-bcc7-4489-8c1b-34957f5b8320.png)



2.加载镜像文件，得到知该镜像是Windows7 X64。根据提示查看电脑桌面图片路径：<font style="color:rgb(60, 60, 60);"> </font>`C:\Users\Administer\AppData\Roaming\Microsoft\Windows\Themes`，发现一张奇奇怪怪的图片，没什么线索

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732265170763-40d0738d-9000-44b9-bc86-1fe26697040a.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732265484546-a9b57bdd-df05-440d-b277-d713e30e35c0.png)



3.查看桌面文件夹，发现一些文件，里面有提示说什么东西藏进了照片里面，所以我们下一步在这个镜像的picture中找到这个照片，然后右击文件导出分析，使用随波逐流的binwalk一下得到了压缩包，然后查看里面的secret.txt，得到提示：宽高、能通过桌面文件直观看到，还有dimension 1280 x 800 应该是分辨率。

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732265780772-4caf46b9-121d-4138-bb20-28c3ccf8db3a.png)



![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732265848299-e0c07616-ff85-43c7-8524-f18d40b2bee8.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732266134481-f9b49a2e-947f-4ac5-85ee-eb9de5869b0a.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732266221218-09bfc1bf-1988-449c-9633-069517ee91af.png)



4.了解到：<font style="color:rgb(6, 6, 7);">e01镜像文件是一种磁盘映像文件格式，主要用于数字取证领域。有些工具如xmount可以将E01格式的镜像文件转化为VDI或者VMDK格式，以便使用VirtualBox和VMware软件打开文件进行取证操作。在FTK中选择文件-》Image Mount，然后挂载方式选writable。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732266847565-f1eb0761-229a-4859-a169-5cf9edb58b4a.png)

5.安装虚拟机（一般选择推荐，下面是要注意的点）

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732347391409-6d11af2f-2621-44c1-883b-0723dfa992ff.png)+

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732347431708-896fd08c-7c64-4848-a38b-a7c3f61a931d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732347454675-f4313ab7-cf58-47f6-a2a0-738396444932.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732347467624-361ce190-6323-4499-ac2a-63358212d98f.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732347481836-7f913903-5aa2-466f-afb9-88df7c849922.png)



6.启动windows7 x64,出现物理磁盘被占用情况（查资料得知是：<font style="color:rgb(77, 77, 77);">他的意思大概是该磁盘是有虚拟内存在里面的，不能乱来。那不行，我就要对你进行操作…咱挪一挪，让别的磁盘来承担虚拟内存的任务。</font>）

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732347552382-343ca2b4-3e91-46e9-8f5d-546cbfb04560.png)

解决方案：把c的托管系统移到E盘即可

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732347687364-698872b4-f00a-4824-96d4-fb4de6f2388e.png)

7.进入到windows7 x64中，<font style="color:rgb(60, 60, 60);">切换到 Guest 调整窗口到1280x800分辨率再切换到 Admin </font>得到flag: wowgoodfzforensics



![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732347364059-bf51a5f3-c699-4101-acd7-eba555a8f762.png)



![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732347129583-11cdcc93-c05c-46c3-9fb6-2dedde2e6c82.png)

## 6.	扫码领取flag


1.根据提示，查看图片属性得到了base64然后解码得到：Aztec Civilization

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732348538497-d7aa6420-7732-47ef-ae7e-bdbbc7644adb.png)

2.根据压缩包的名字CRC线索，猜想是宽高被改变了，然后遍历寻找宽高，计算crc是否正确，若正确则找到真正的宽高代码如下：最后得到宽高为:500

```plain
import zlib
import struct
import  os
#crc正确，宽高未知
def a1():
    with open(r'C:\Users\chenz\Downloads\CRC\flag.png', 'rb') as image_data:#rb 以二进制读取，但最后显示是hex
        bin_data = image_data.read()
    print(bin_data[:50])
    data = bytearray(bin_data[12:29])  # 获取IHDR数据头相关数据,并转化为可变字节数组对象
    print(data[0])#打印的为10进制的数
    hex_data = bin_data[29:33].hex()
    crc32key = int(hex_data, 16)
    print(crc32key)
    #理论上0xffffffff,但考虑到屏幕实际，0x0fff就差不多了
    n = 1200
    #高和宽一起爆破
    for w in range(n):
        # q为8字节，i为4字节，h为2字节
        # >i 表示大端存储+有符号整数。struct.pack将整数w打包成一个二进制有符号整数并转化为可变字节数组对象
        width = bytearray(struct.pack('>i', w))
        for h in range(n):
            height = bytearray(struct.pack('>i', h))
            #把宽高赋给data数组中，用于计算crc
            for x in range(4):
                data[x+4] = width[x]
                data[x+8] = height[x]
            crc32result = zlib.crc32(data)#计算 crc
            if crc32result == crc32key:
                print("width:%s  height:%s" % (bytearray(width).hex(),
                                               bytearray(height).hex()))
                exit()
```

3.修改宽高得到一个类似二维码的东西，根据上面hint：Aztec Civilization，得知这个是Aztec码。

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732348628631-703fad73-a562-4c4d-b965-acd3312a307b.png)

4.在线扫码得到flag ( [https://tuzim.net/hxdecode/](https://tuzim.net/hxdecode/) ):

![](https://cdn.nlark.com/yuque/0/2024/png/49967934/1732348948263-5d8e467b-d0d5-41b9-bba6-6100f26e0fb1.png)

