<h2 id="PK455">Begin</h2>
第一步：对文件格式进行判断

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894698651-f0c33abd-babc-4d05-8c7f-3f4a9d3c74eb.png)

第二步：用IDA打开文档并找到main函数

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894713164-56210f86-6524-4e1d-822c-994aa0b2031e.png)

第三步：进行伪代码

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894727815-288d3b0b-5a07-43c0-a635-a00c0aa4bb6f.png)

第四步：找到flag的Part1

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894742188-08266754-fb89-49af-93c5-7ac0314d61ed.png)

第五步：用string找到Part2

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894761481-3e76f091-386e-411b-8643-7f57db4d499f.png)

第六步：点击变量

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894781326-adb30a12-b540-43e2-8b86-b81bb2996b90.png)

第七步：找到伪代码

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894792882-56493890-3722-45ca-91fe-ff4cc678ad8d.png)

第八步：找到索引函数即Part3

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894806889-99ea46a5-5385-45a2-8c22-bd1174667d77.png)

最终得出

flag{Mak3_aN_3Ff0rt_tO_5eArcH_F0r_th3_f14g_C0Rpse}



<h2 id="WK7xZ">Base64</h2>
  
第一步：用IDA处理文件  
第二步：由于没有主函数，查找字符串

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894911004-cd22ebfc-fd9b-4aa7-b9b3-6d4c5c3fbd65.png)

第三步：发现Enter the flag并查找相关函数

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894920035-1f563e7c-e6e2-404e-9f1c-780c7a227ca2.png)

第四步：进行反编译

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894931593-8478db61-4c81-4c4a-a694-41b1fbcbb988.png)

第五步：点击加密函数sub_1400014E0并发现是Base64加密（点击aWhydo3sthis7ab）

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894945660-628456b1-2499-4c9f-ac69-fae87d7dce11.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894961544-f57b581a-e4b1-4337-81b0-c45a20076b9c.png)

第六步：找到被修改后的密码表  
WHydo3sThiS7ABLElO0k5trange+CZfVIGRvup81NKQbjmPzU4MDc9Y6q2XwFxJ/  
第七步：编写Python文件进行解密  
import base64  
s1="ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"  
s2="WHydo3sThiS7ABLElO0k5trange+CZfVIGRvup81NKQbjmPzU4MDc9Y6q2XwFxJ/"  
en_text="g84Gg6m2ATtVeYqUZ9xRnaBpBvOVZYtj+Tc="

map_text=''  
for i in en_text:  
    if(i!='='):  
        idx=s2.index(i)  
        map_text+=s1[idx]  
    else: 

map_text+=i  
print(map_text)  
print(base64.b64decode(map_text))

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894984856-caa8d2b8-55b3-4d33-9b09-3df98c7ff349.png)

得到返回结果b'flag{y0u_kn0w_base64_well}'

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1732894998557-3ed4dc87-2c81-4e1b-8b47-9e9bebe50d79.png)



<h2 id="GeYeu">Simple_encryption</h2>
第一步：确认是64位，并且无壳

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733016166526-a2066166-d918-4735-80fe-b9a00b2d4db1.png)

第二步：用IDA反编译：找到main函数，按TAB建

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733016281125-8f55a978-bb83-4f5f-9681-876c57fa932a.png)

第三步：分析加密算法

这边采用逆向分析：最后buffer和input输入的字符串相等

返回最初输入input

首先点击len，发现数值为1110b即是十进制30![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733016617060-de483739-647e-483b-abfe-541e3cda1baf.png)

然后对每一位进行加密

如果是3的倍数，从输入值减去31；

如果整除3余1，就加上41；

如果整除3余2，就和0x55进行异或

点击进入Buffer数组，提取数据

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733016911054-e45ceb22-f4b8-4744-b70a-819d65bf3398.png)

第四步：编写逆向脚本

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733017037286-40c9a81a-fc81-4188-a093-c3add023ba7f.png)

最后运行得到flag{IT_15_R3Al1y_V3Ry-51Mp1e}



<h2 id="MycYL">ez_debug</h2>
这里使用IDA处理

第一步：IDA反汇编后直接进入主函数![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733017833976-61342ed5-7875-4c72-aadb-825730144e5b.png)

通过Decrypted提示，发现核心加密函数you

第二步：进入you函数

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733017949038-64d8c0c7-5220-42a4-9a5a-ee99953c7b63.png)

在相关位置设断点（这里采用在for循环底部）

依据最开始的v5是rax,可判断最终v5存储着最终的flag

第三步：然后点击F9进入调试

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733019352928-fb9b1764-ec29-43ad-af7e-b75e023dd690.png)

首先随意输入，让程序进入循环

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733019396309-81e90a70-2b7f-42b6-a108-3d9fc38cd516.png)

然后一直用F8进行单步调试，点击查看F5，可以发现其值与flag形式极为相近

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733019534910-65b2811f-32c7-4e41-89d0-eb427c142239.png)![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733019554597-449dfbe5-ebd3-417e-a42e-ddd349a3ca58.png)将这些字符合并![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733020012521-3a40dd97-f182-4e1c-9831-c2c9ed57b517.png)

也就得到flag{y0u_ar3_ g0od_@_Debu9}

<h2 id="sbmF7">drink_tea</h2>
第一步：查看文件的基本信息

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733020832857-f18ea043-bb4b-4b5a-b568-8e1cdb9cecf1.png)

是64位文件，并且无壳

第二步：用IDA打开找到主函数

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733020953761-1dfb261a-c8d6-4aef-b6b6-bf89beaabafc.png)

先读取32位的字符串，然后进行加密，随后进行字符串判断

点击查看加密函数

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733021200464-22e11158-73cc-4a99-9268-d2ebcc7fe9a5.png)

发现是TEA算法



第三步：编写解密脚本

这里点击aWelcometonewst得到key

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733022480499-1cb79855-1823-4d56-a34e-1ab77de5e672.png)

点击unk_140004080得到密码

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733023100225-70aa051c-5b8c-4173-86f3-90ff46296d77.png)

正式编写

#include<stdio.h>

#include<stdint.h>

//解密函数

unsigned char keys[] = "WelcomeToNewStar";

unsigned char cipher[] = { 0x78,0x20,0xF7,0xB3,0xC5,0x42,0xCE,0xDA,0x85,0x59,0x21,

0x1A,0x26,0x56,0x5A,0x59,0x29,0x02,0x0D,0xED,0x07,0xA8,0xB9,0xEE,0x36,0x59,0x11,

0x87,0xFD,0x5C,0x23,0x24 };



void decrypt(uint32_t* v,uint32_t* k){

	uint32_t v0=v[0],v1=v[1],i;

	uint32_t delta=0x9e3779b9;//这是定义的一个常数（黄金分割比的共轭）

	uint32_t sum=(32)*delta;

	uint32_t k0=k[0],k1=k[1],k2=k[2],k3=k[3];

	for(i=0;i<32;i++){//解密时将加密的算法颠倒，+=变为-= 

        v1-=((v0 << 4) + k2)^(v0 + sum)^((v0>>5) + k3);

        v0-=((v1 << 4) + k0)^(v1 + sum)^((v1>>5) + k1);

        sum -= delta;

    }

    v[0]=v0; v[1]=v1; // 解密后再重新赋值

}



int main(){

    unsigned char a;

    uint32_t *v=(uint32_t*)cipher;

    uint32_t *k=(uint32_t*)keys;

    // v 为要加密的数据是 n 个 32 位无符号整数

    // k 为加密解密密钥，为 4 个 32 位无符号整数，即密钥长度为 128 位



    for (int i=0; i<8; i+=2)

    {

        decrypt(v+i,k);

        // printf("解密后的数据：%u %u\n", v[i], v[i+1]);

    }



    for (int i=0;i<32;i++) {

        printf("%c",cipher[i]);

    }

    return 0;

}

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733024106876-561845cb-1497-4136-a08e-d1eb6a0ae692.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733024124547-2419f409-b15f-49ed-98b2-feb8ab5b5a9d.png)

得到结果flag{There_R_TEA_XTEA_and_XXTEA}

![](https://cdn.nlark.com/yuque/0/2024/png/49936975/1733024190691-043eb299-8fd8-4c45-96c1-0f64b69face3.png)





