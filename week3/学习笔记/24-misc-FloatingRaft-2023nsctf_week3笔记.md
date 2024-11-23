### 1.base100编码（emoji编码）与AES（高级加密标准）
首先先了解一下base家族编码，base家族编码有base16、base32、base58、base64、base85、base91、base100这几种编码

base100编码是其中一种最好识别的编码，因为他全部由emoji组成

AES，中文名叫高级加密标准<font style="color:rgb(51, 51, 51);">（Advanced Encryption Standard，AES），这是最常见的对称加密算法。加密者设置一个密钥K，以密钥K与明文P为参数代入AES加密函数，最后输出密文C。而解密者需要知道密文C与密钥K，以密文C与密钥K为参数代入AES解密函数，从而得到明文P</font>

<font style="color:rgb(51, 51, 51);">而在“阳光开朗大男孩”一题中，flag采取的是emoji-aes编码，就是一种以emoji表情为密文、并且需要拥有私钥K才能破解密文得到明文的加密手段</font>

### 2.二维码
<font style="color:rgb(51, 51, 51);">二维码在日常生活中经常提及也经常用到，不过二维码是一个统称，下面还有许多分类，而人们最常提到的二维码其实是是QR Code，在“大怨种”一题中用来记录数据的是Han Xin Code（汉信码）。</font>

<font style="color:rgb(51, 51, 51);">二维码是根据某种特定的几何图形和规律，在二维平面上利用黑白相间的图形来记录数据信息。二维码在水平和垂直方向都可以存储信息，并且能存储汉字、数字和图片等信息。其在代码编制上巧妙地利用构成计算机内部逻辑基础的“0”“1”比特流的概念，使用若干个与二进制相对应的几何形体来表示文字数值信息，通过图像输入设备或光电扫描设备自动识读，以实现信息自动处理。同时每种码制有其特定的字符集，每个字符占有一定的宽度，具有一定的校验功能，同时还具有对不同行的信息自动识别功能及处理图形旋转变化等特点。由于二维条形码能够在横向和纵向两个方位同时表达信息，因此能在很小的面积内表达大量的信息。</font>

<font style="color:rgb(51, 51, 51);">二维码按不同码制的编码原理通常分以下类型：堆叠式（行排式）、矩阵式和邮政码。堆叠式（行排式）二维码形态上是由多行短截的一维条码堆叠而成；矩阵式二维码以矩阵的形式组成，在矩阵相应元素位置上用“点”表示二进制“1”，用“空”表示二进制“0”，由“点”和“空”的排列组成代码。</font>

**<font style="color:rgb(51, 51, 51);">堆叠式/行排式</font>**

<font style="color:rgb(51, 51, 51);">堆叠式（行排式）二维码在码制设计、校验原理、识读方式等方面继承了一维码的诸多特点，识读设备与条码印刷与一维码兼容。但由于行数的增加，要对行进行判定，译码算法与软件也不同于一维码。代表性堆叠式(行排式)二维码有:Code16K、Code 49、PDF417等。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732372736857-c7eecdd8-fd2b-4317-a81c-6fab84663a43.png)<font style="color:rgb(85, 85, 85);"></font>

**<font style="color:rgb(51, 51, 51);">矩阵式二维码</font>**

<font style="color:rgb(51, 51, 51);">矩阵式二维条码（又称棋盘式二维条码），是在一个矩形空间通过黑、白像素在矩阵中的不同分布进行编码。在矩阵相应元素位置上，用点（方点、圆点或其他形状）的出现表示二进制“1”，点的不出现表示二进制的“0”，点的排列组合确定了矩阵式二维条码所代表的意义。矩阵式二维条码是建立在</font>[<font style="color:rgb(51, 51, 51);">计算机图像处理</font>](https://baike.baidu.com/item/%E8%AE%A1%E7%AE%97%E6%9C%BA%E5%9B%BE%E5%83%8F%E5%A4%84%E7%90%86/5687124?fromModule=lemma_inlink)<font style="color:rgb(51, 51, 51);">技术、组合编码原理等基础上的一种新型图形符号自动识读处理码制。具有代表性的矩阵式二维条码有：Code One、MaxiCode、QR Code、Data Matrix、Han Xin Code、Grid Matrix等。</font><sup><font style="color:rgb(51, 102, 204);"></font></sup>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/49267415/1732372736844-0938ebae-6a17-49a8-bb39-206dd8f39980.jpeg)<font style="color:rgb(85, 85, 85);"></font>

**<font style="color:rgb(51, 51, 51);">邮政码</font>**

<font style="color:rgb(51, 51, 51);">邮政码是通过不同长度的条进行编码，主要用于邮件编码，属于专用领域的特殊码，如：Postnet、BPO 4-</font><font style="color:rgb(51, 51, 51);">State等。</font>

### 3.HTTP协议与TCP协议的区别与联系
<font style="color:rgb(51, 51, 51);">传输控制协议（TCP，Transmission Control Protocol）是为了在不可靠的互联网络上提供可靠的端到端字节流而专门设计的一个传输协议。</font>

<font style="color:rgb(51, 51, 51);">超文本传输协议（HypertextTransfer Protocol，HTTP）是一个简单的请求-响应协议，它通常运行在TCP之上。它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应。请求和响应消息的头以ASCII形式给出；而消息内容则具有一个类似MIME的格式。超文本传输协议是一种用于分布式、协作式和超媒体信息系统的应用层协议，是万维网WWW（World Wide Web）的数据通信的基础。</font>

TCP协议<font style="color:rgb(77, 77, 77);">是传输层协议，主要解决数据如何在网络中传输，而HTTP协议是应用层协议，主要是解决如何包装数据。通俗的来说HTTP的任务是与服务器交换信息，它不管怎么连到服务器和保证数据正确的事情。而TCP的任务是保证连接的可靠，它只管连接，它不管连接后要传什么数据。HTTP协议是建立在TCP协议之上的一种应用，但是HTTP不一定要建立在TCP协议之上，</font><font style="color:rgb(17, 17, 17);">HTTP 协议中并没有规定必须使用 TCP/IP 或其支持的层。 事实上，HTTP 可以在任何互联网协议上，或其他网络上实现。 HTTP 假定其下层协议提供可靠的传输。 因此，任何能够提供这种保证的协议都可以被其使用。</font>

<font style="color:rgb(17, 17, 17);">详细内容可查看这篇文章，这篇文章讲的比较通俗比较好懂 </font>[<font style="color:rgb(17, 17, 17);">面试：HTTP协议与TCP协议的区别和联系-CSDN博客</font>](https://blog.csdn.net/weixin_44422604/article/details/108435009)

### <font style="color:rgb(17, 17, 17);">4.HTTP协议的四种请求</font>
<font style="color:rgb(36, 41, 46);">在 Web 服务的开发中，HTTP 协议是最常使用的协议。其中，常见的 HTTP 请求方式有四种：POST（添加）、GET（查询）、DELETE（删除）和PUT（修改）</font>

**<font style="color:rgb(36, 41, 46);">POST请求</font>**

<font style="color:rgb(36, 41, 46);">POST请求用于向指定资源提交数据，通常会导致服务器端的状态发生变化。例如，在 Web 表单中填写用户信息并提交时，就是使用POST请求方式将表单数据提交到服务器存储。</font>

<font style="color:rgb(36, 41, 46);">使用POST请求方式提交的数据会被包含在请求体中，而不像GET请求方式那样包含在 URL 中。因此，POST请求可以提交比GET更大的数据量，并且相对更安全。</font>

<font style="color:rgb(36, 41, 46);">一般应用于</font>

+ <font style="color:rgb(36, 41, 46);">向服务器提交表单数据</font>
+ <font style="color:rgb(36, 41, 46);">向服务器上传文件</font>
+ <font style="color:rgb(36, 41, 46);">创建资源或提交数据到服务器</font>

**<font style="color:rgb(51, 51, 51);">GET请求</font>**

<font style="color:rgb(36, 41, 46);">GET请求用于向指定资源发出请求，请求中包含了资源的 URL 和请求参数。服务器端通过解析请求参数来返回相应的资源，不会修改服务器端的状态。</font>

<font style="color:rgb(36, 41, 46);">使用GET请求方式提交的数据会被包含在 URL 中，因此易于被缓存和浏览器保存，但也因此不适合用于提交敏感数据。</font>

<font style="color:rgb(36, 41, 46);">一般应用于</font>

+ <font style="color:rgb(36, 41, 46);">获取资源信息</font>
+ <font style="color:rgb(36, 41, 46);">对资源进行查询操作</font>

**DELETE请求**

<font style="color:rgb(36, 41, 46);">DELETE请求用于请求服务器删除指定的资源，可以理解为对服务器上的资源进行删除操作。使用DELETE方式请求会导致指定的资源被永久删除，因此需要谨慎使用。</font>

<font style="color:rgb(36, 41, 46);">一般应用于</font>

+ <font style="color:rgb(36, 41, 46);">删除指定的资源</font>
+ <font style="color:rgb(36, 41, 46);">按照条件删除一组资源</font>

**PUT请求**

<font style="color:rgb(36, 41, 46);">PUT请求用于向服务器更新指定资源，可以理解为对服务器上的资源进行修改操作。使用PUT请求方式会覆盖原有的资源内容，因此需要谨慎使用。</font>

<font style="color:rgb(36, 41, 46);">一般应用于</font>

+ <font style="color:rgb(36, 41, 46);">更新指定的资源</font>
+ <font style="color:rgb(36, 41, 46);">按照条件更新一组资源</font>

### 5.DTMF编码
拨号音用来通知主叫用户可以拨号。拨号音采用频率为450±25Hz的交流电源，发送电平为-10±3dBm，是连续的信号音，拨号音的编码方式是DTMF。

<font style="color:rgb(51, 51, 51);">双音多频DTMF（Dual Tone Multi Frequency），双音多频，由高频群和低频群组成，高低频群各包含4个频率。一个高频信号和一个低频信号叠加组成一个组合信号，代表一个数字。DTMF信号有16个编码。利用DTMF信令可选择呼叫相应的对讲机。</font>

<font style="color:rgb(51, 51, 51);">DTMF编码器基于两个二阶数字正弦波振荡器，一个用于产生行频，一个用于产生列频。向DSP装入相应的系数和初始条件，就可以只用两个振荡器产生所需的八个音频信号。典型的DTMF信号频率范围是700~1700Hz，选取8000Hz作为采样频率，即可满足Nyquist条件。DTMF双音频信号由两个二阶数字正弦振荡器产生，一个用来产生行音频信号，另一个产生列音频信号。</font>

<font style="color:rgb(51, 51, 51);">DTMF信号包含两组</font><font style="color:rgb(51, 51, 51);">音频信号</font><font style="color:rgb(51, 51, 51);">，解码器的任务是通过数学变换把它从时域转化到频域，然后得出对应的数字信息。由于芯片处理的是数字信号，所以必须把输入信号数字化，再用DSP芯片处理。频率检测时，检测出DTMF信号的基波及二次谐波，DTMF信号只在基波上有较高的能量，而话音信号则是在基波上叠加有较强的二次谐波，检测二次谐波的作用是用来区分DTMF信号与语言和音乐信号。</font>

<font style="color:rgb(51, 51, 51);">DTMF信号最先用于程控电话交换系统来代替号盘脉冲信号，主叫用户摘机按键拨号后，电话号码所对应的DTMF信号通过电话线传到程控交换机中的DTMF接受电路，交换机中的微机识别被叫电话号码后，接通主被叫用户实现双方通话。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49267415/1732375025856-24a5ddad-fbc0-4ecc-b612-f4179399b3c6.png)

