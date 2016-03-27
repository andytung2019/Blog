title: python学习-数据与语句结构
date: 2015-09-16 20:34:43
tags: [python]
---

------

>* 数据结构
>* 语句结构

------

# python更换路径

导入os模块：
```
import os
```
切换到制定路径：
```
os.chdir('制定路径')
```
获得当前路径：
```
os.getcwd()
```
列出当前目录的文件：
```
os.listdir('.')
```

# Data Structures

Python中取值方式为`A[index]`。

## List

`[]`
* list.append() 在序列结尾添加
* list.extend() 在序列结尾插入指定的另一个序列
```
>>> A=[1,2,3,4]
>>> A
[1, 2, 3, 4]
>>> B=[5,6]
>>> B
[5,6]
>>> A.extend(B)
>>> A
[1, 2, 3, 4, 5, 6]

```
* list.insert(i,x) 查入指定位置的元素
* list.remove(x) 移除序列最前面的值为x的元素，若无此值产生错误
* list.pop([i]) 弹出序列最先的元素并返回
* list.index(x) 返回第x个元素值
* list.count(x) 返回序列中值为x的元素的个数
* list.sort(cmp=None,key=None,reverse=False)对序列进行排序，默认从小到大
* list.reverse() 反转序列

### 用作栈Stack

* 进入 list.append(x)
* 弹出 list.pop()

### 用作队列Queue

由于用list作为队列效率不高，推荐使用#collecttions.deque#
```
>>> from collections import deque
>>> queue = deque(["Eric", "John", "Michael"])
>>> queue.append("Terry")           # Terry arrives
>>> queue.append("Graham")          # Graham arrives
>>> queue.popleft()                 # The first to arrive now leaves
'Eric'
>>> queue.popleft()                 # The second to arrive now leaves
'John'
>>> queue                           # Remaining queue in order of arrival
deque(['Michael', 'Terry', 'Graham'])

```
### 函数化编程工具

* filter(function,sequence)返回function(sequence)==true的*序列*
```
>>> def f(x): return x % 3 == 0 or x % 5 == 0
...
>>> filter(f, range(2, 25))
[3, 5, 6, 9, 10, 12, 15, 18, 20, 21, 24]

```

* map(function,sequence)返回序列进过函数处理后的*序列*
```
>>> def cube(x): return x*x*x
...
>>> map(cube, range(1, 11))
[1, 8, 27, 64, 125, 216, 343, 512, 729, 1000]

```
* reduce()
```
>>> def add(x,y): return x+y
...
>>> reduce(add, range(1, 11))
55

```
## Sets
`([])`
用于成员存在测试或消除重复元素等。
```
>>> basket=['apple','orange','apple','pear','orange']
>>> fruit=set(basket)
>>> fruit
set(['orange', 'pear', 'apple'])
>>> 'orange' in fruit
True
>>> 'crabgrass' in fruit
False

```

## Dictionaries
`{}`
可以将Dictionaries看为`key:value对`，strings和numbers都可以为keys
```
>>> tel={'jack':4098,'sape':3182}
>>> tel['guid']=7878
>>> tel
{'sape': 3182, 'guid': 7878, 'jack': 4098}
>>> del tel['sape']
>>> tel
{'guid': 7878, 'jack': 4098}
>>> tel.keys()
['guid', 'jack']
>>> tel.values()
[7878, 4098]

```

## tuple
`()`
tuple是一种有序列表，称之为*元祖*。与list不同的是它一旦初始化就不能修改，这样保证了代码的安全。

## 数据结构高级特性

### 切片
在list和tuple中利用*切片*来进行部分连续数据的读取：
```
>>> L = ['a','b','c','d','e']
>>> S=L[0:2]
>>> S
['a','b']

>>> S2=L[:2]
>>> S2
['a','b']

>>> S3=L[:]
>>> S3
['a','b','c','d','e']

```

### 迭代
利用`for ... in`进行快速迭代。
```
>>> d = {'a': 1, 'b': 2, 'c': 3}
>>> for key in d:
...     print key
...
a
c
b
```

对于两个变量：
```
>>> for x, y in [(1, 1), (2, 4), (3, 9)]:
...     print x, y
...
1 1
2 4
3 9
```

### 列表生成式

利用列表生成式可以快速生成制定的list：
```
>>>range(1,11)
[1,2,3,4,5,6,7,8,9,10]

>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']

```

# 结构

python中语句的缩进规则对程序有着重要的作用。

## 条件判断
 if语句从上到下判断条件：
```
age = 3
if age >= 18:
    print 'adult'
elif age >= 6:
    print 'teenager'
else:
    print 'kid'

```

## 循环

### for...in循环
将list或tuple中每个元素迭代出来，把每个元素代入变量x，然后执行缩进块的语句。
```
names=['Michael','Bob','Tracy']
for name in names:
	print name
```

### while
当条件满足时一直循环，直到条件不满足。
```
sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print sum
```


