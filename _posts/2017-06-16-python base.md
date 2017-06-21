---
layout: post
title: python：基础使用
categories: [python]
tags: python
---

> 要学机器学习，必先学python。

## 综述

python语法很多，更多请查看完整的[官方文档](https://www.python.org/doc/)。本文专注于python常见的内置函数、模块，不包括numpy、scipy、pandas等

## 编码

```python
# -*- coding: utf-8 -*-
```
## 内存管理

```python
# -*- coding: utf-8 -*-

import gc

obj = 3
print obj
del obj
gc.collect()

print obj
```

`output:`

```python
3
Traceback (most recent call last):
  File "/Users/vista/PycharmProjects/untitled/kNN.py", line 10, in <module>
    print obj
NameError: name 'obj' is not defined
```

## 文件读取
以如下文件为例，文件名为test.txt，文本内容如下：

```
WIFIAPTag,passengerCount,timeStamp
E1-1A-1<E1-1-01>,15,2016-09-10-18-55-04
E1-1A-2<E1-1-02>,15,2016-09-10-18-55-04
E1-1A-3<E1-1-03>,38,2016-09-10-18-55-04
```

`示例1：`

```python
with open(name='test.txt',mode='r') as file:
    print file.readline()
    print file.readline()
    file.seek(0)
    print file.readline()
```

`output:`

```
WIFIAPTag,passengerCount,timeStamp

E1-1A-1<E1-1-01>,15,2016-09-10-18-55-04

WIFIAPTag,passengerCount,timeStamp
```

print在2.x版本中会自动换行，函数原型
`print(*objects, sep=’ ‘, end=’\n’, file=sys.stdout, flush=False) `
解决办法: 

```python
from __future__ import print_function

with open(name='test.txt',mode='r') as file:
    print(file.readline(),end='')
    print(file.readline(),end='')
    file.seek(0)
    print(file.readline(),end='')
```

`output`

```
WIFIAPTag,passengerCount,timeStamp
E1-1A-1<E1-1-01>,15,2016-09-10-18-55-04
WIFIAPTag,passengerCount,timeStamp
```

`示例2：`

```python
with open(name='test.txt',mode='r') as file:
    column_names =  file.readline().strip().split(',')
    print column_names
    data = []
    for line in file:
        (wifi_ap_tag,passenger_count,time_stamp) = line.strip().split(',')
        data.append([wifi_ap_tag,passenger_count,time_stamp])
    print data
```

`output:`

```
['WIFIAPTag', 'passengerCount', 'timeStamp']
[['E1-1A-1<E1-1-01>', '15', '2016-09-10-18-55-04'], ['E1-1A-2<E1-1-02>', '15', '2016-09-10-18-55-04'], ['E1-1A-3<E1-1-03>', '38', '2016-09-10-18-55-04']]
```

`文本添加：`

```python
#文本追加
with open(name='test.txt',mode='a') as file:
    file.write('\nhello python')
```

* r以读方式打开文件，可读取文件信息。 
* w以写方式打开文件，可向文件写入信息。如文件存在，则清空该文件，再写入新内容 
* a以追加模式打开文件（即一打开文件，文件指针自动移到文件末尾），如果文件不存在则创建 
* r+以读写方式打开文件，可对文件进行读和写操作。 
* w+消除文件内容，然后以读写方式打开文件。 
* a+以读写方式打开文件，并把文件指针移到文件尾。 
* b以二进制模式打开文件，而不是以文本模式。该模式只对Windows或Dos有效，类Unix的文件是用二进制模式进行操作的。

## 异常处理

```python
try:
    data = open(name='1.txt',mode='r')
except IOError as err:
    print 'File error : ' + str(err)
finally:
    if 'data' in locals():  #locals()会返回当前作用域中定义所有名的一个集合
        data.close()
```

`output:`

`File error : [Errno 2] No such file or directory: '1.txt'`

## 列表推导项、去重复项、取降序top 3

```
a = [1,2,3,4,5,4,6,8]
x = sorted(set([(item*item) for item in a if item > 3]),reverse=True)[0:3]
print x

#输出  [64, 36, 25]
#set() 表示集合
#对数据完成复制排序 data2 = sorted()
#对数据完成原地排序 data.sort()
#默认排序是升序,使用reverse=True可以降序输出
#函数原型 sorted(iterable, cmp=None, key=None, reverse=False)
```

## 内置函数lambda、filter、map、reduce、zip、enumerate

- 匿名函数lambda。原型lambda [arg1[,arg2,arg3,…,argn]] : expression

```python
abc = lambda x,y : x + y
print abc(1,2)
```

- filter(bool_func,seq)：此函数的功能相当于过滤器。调用一个布尔函数bool_func来迭代遍历每个seq中的元素；返回一个使bool_seq返回值为true的元素的序列。

```python
abc = [121,12,31,45,42,78,4,7845,78421,72412,71,121,21]
print filter( lambda x: x > 40,abc)
```

- map(func,seq1[,seq2…])：将函数func作用于给定序列的每个元素，并用一个列表来提供返回值；如果func为None，func表现为身份函数，返回一个含有每个序列中元素集合的n个元组的列表。

```python
print map(lambda x : None,[1,2,3,4])
print map(lambda x : x * 2,[1,2,3,4])
print map(lambda x : x * 2,[1,2,3,4,[5,6,7]])  
# [None, None, None, None]
# [2, 4, 6, 8]
# [2, 4, 6, 8, [5, 6, 7, 5, 6, 7]]
```

- reduce(func,seq[,init])：func为二元函数，将func作用于seq序列的元素，每次携带一对（先前的结果以及下一个序列的元素），连续的将现有的结果和下一个值作用在获得的随后的结果上，最后减少我们的序列为一个单一的返回值：如果初始值init给定，第一个比较会是init和第一个序列元素而不是序列的头两个元素

```python
print reduce(lambda x,y : x + y,[1,2,3,4])
print reduce(lambda x,y : x + y,[1,2,3,4],10) 
# 10
# 20
```

- zip(seq1 [, seq2 […]]) -> [(seq1[0], seq2[0] …), (…)]

```python
x = [1, 2, 3]
y = [4, 5, 6, 7]
print zip(x, y)
#[(1, 4), (2, 5), (3, 6)]
```

- enumerate

```python
x = [1, 2, 3]
for idx,j in enumerate(x):
    print idx,j
#0 1
#1 2
#2 3
```

## time

```python
import time
time.time()         #获取当前时间戳
time.localtime()    #当前时间的struct_time形式
time.ctime()        #当前时间的字符串形式

print time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())

#统计程序运行时间
from time import clock
t_start = clock()
t_finish = clock()
print('运行时间为：%s 秒' % str(t_finish - t_start))

#2017-06-16 16:39:47
#运行时间为：2e-06 秒
```

## pickle模块

```python
import pickle

#以保存模型参数为例
with open(name='test.pickle',mode='wb') as save_file:      #‘b’表示二进制模式
    pickle.dump({'depth':8,'ntree':100},save_file)

with open(name='test.pickle',mode='rb') as restored_file:
    para = pickle.load(restored_file)

print para['depth']
print para['ntree']
```

## os模块

```python
import os
os.getcwd()                 #当前工作目录
os.chdir('../python/code')  #注意.与..的区别

#新建文件夹
if not os.path.exists('submit'):
    os.mkdir('submit')
```

## list

list：列表（即动态数组，C++标准库的vector，但可含不同类型的元素于一个list中） 

**a = [“I”,”you”,”he”,”she”] ＃元素可为任何类型。**

`下标：`按下标读写，就当作数组处理 

以0开始，有负下标的使用，0第一个元素，-1最后一个元素，-len第一个元素，len-1最后一个元素 
取list的元素数 

`len(list)` #list的长度。实际该方法是调用了此对象的len(self)方法

创建连续的list 

```
L = range(1,5) #即 L=[1,2,3,4],不含最后一个元素
L = range(1, 10, 2) #即 L=[1, 3, 5, 7, 9]
```

list的方法 

```
L.append(var) #追加元素
L.insert(index,var)
L.pop(var) #返回最后一个元素，并从list中删除之
L.remove(var) #删除第一次出现的该元素
L.count(var) #该元素在列表中出现的个数
L.count(var) #该元素在列表中出现的个数
L.index(var) #该元素的位置,无则抛异常
L.extend(list) #追加list，即合并list到L上
L.sort() #排序
L.reverse() #倒序
```

list 操作符:,+,*，``del``

```
a[1:] #片段操作符，用于子list的提取
[1,2]+[3,4] #为[1,2,3,4]。同extend()
[2]*4 #为[2,2,2,2]
*del* L[1] #删除指定下标的元素
*del* L[1:3] #删除指定下标范围的元素
```

list的复制 

```
L1 = L #L1为L的别名，用C来说就是指针地址相同，对L1操作即对L操作。函数参数就是这样传递的
L1 = L[:] #L1为L的克隆，即另一个拷贝。
```

list comprehension（推导列表） 

`[ for k in L if ]`

list 和 tuple 的相互转化 

```
tuple(ls)
list(ls)
```

## 字典

>dictionary： 字典（即C++标准库的map）

`dict = {‘ob1’:’computer’, ‘ob2’:’mouse’, ‘ob3’:’printer’} `

每一个元素是pair，包含key、value两部分。key是Integer或string类型，value 是任意类型。 
键是唯一的，字典只认最后一个赋的键值。

dictionary的方法

```
D.get(key, 0) #同dict[key]，多了个没有则返回缺省值，0。[]没有则抛异常 
D.has_key(key) #有该键返回TRUE，否则FALSE 
D.keys() #返回字典键的列表 
D.values() 
D.items() 
D.update(dict2) #增加合并字典 
D.popitem() #得到一个pair，并从字典中删除它。已空则抛异常 
D.clear() #清空字典，同del dict 
D.copy() #拷贝字典 
D.cmp(dict1,dict2) #比较字典，(优先级为元素个数、键大小、键值大小), 第一个大返回1，小返回-1，一样返回0
```

dictionary的复制

```
dict1 = dict #别名 
dict2 = dict.copy() #克隆，即另一个拷贝
```

字典的遍历

（1）遍历字典的key（键）

```
for key in dict.keys():
    print key
```
    
（2）遍历字典的value（值）

```
for value in dict.values():
    print value
```
    
（3）遍历字典的项（元素）

```
for item in dict.items():
    print item
```
    
（4）遍历字典的key-value

```
for item，value in dict.items(): 
    print ‘key=%s, value=%s' %(item, value)  

for item，value in dict.iteritems(): 
    print ‘key=%s, value=%s' %(item, value)
```

## 字符串

> string：字符串（即不能修改的字符list）

`str = “Hello World” `

字符串是一个整体。如果你想直接修改字符串的某一部分，是不可能的。但我们能够读出字符串的某一部分。

子字符串的提取`str[:6]`

字符串包含判断操作符：`in，not in`

```
”He” in str 
“she” not in str
```

string模块方法

```
S.find(substring, [start [,end]]) #可指范围查找子串，返回索引值，否则返回-1 
S.rfind(substring,[start [,end]]) #反向查找 
S.index(substring,[start [,end]]) #同find，只是找不到产生ValueError异常 
S.rindex(substring,[start [,end]])#同上反向查找 
S.count(substring,[start [,end]]) #返回找到子串的个数 
S.lowercase() 
S.capitalize() #首字母大写 
S.lower() #转小写 
S.upper() #转大写 
S.swapcase() #大小写互换 
S.split(str, ’ ‘) #将string转list，以空格切分 
S.join(list, ’ ‘) #将list转string，以空格连接
```

处理字符串的内置函数

```
len(str) #串长度 
cmp(“my friend”, str) #字符串比较。第一个大，返回1 
max(‘abcxyz’) #寻找字符串中最大的字符 
min(‘abcxyz’) #寻找字符串中最小的字符
```

string的转换

```
oat(str) #变成浮点数，float(“1e-1”) 结果为0.1 
int(str) #变成整型， int(“12”) 结果为12 
int(str,base) #变成base进制整型数，int(“11”,2) 结果为2 
long(str) #变成长整型， 
long(str,base) #变成base进制长整型，
```

字符串的格式化（注意其转义字符，大多如C语言）

```
str_format % (参数列表) #参数列表是以tuple的形式定义的，即不可运行中改变 
print “”%s’s height is %dcm” % (“My brother”, 180) #结果显示为 My brother’s height is 180cm
```

## 引用

1. [http://blog.csdn.net/a819825294/article/details/53186306](http://blog.csdn.net/a819825294/article/details/53186306)
