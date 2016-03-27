title: python学习-正则
date: 2015-09-30 09:19:30
tags: [python]
---


------

>* re 模式匹配模块

------

# re 模式匹配模块

包含了类似搜索、分割和替换等调用
```
>>> import re
>>> match=re.match('/(.*)/(.*)/(.*)','/usr/home/limberjack')
>>> match.groups()
('usr', 'home', 'limberjack')
>>>

```
代码中搜索子字符串，与模式中括号包含的部分与匹配的子字符串的对应部分保存为组。

[CSDN](http://blog.csdn.net/lengyue_wy/article/details/6999310)
# 元字符：`.`,`^`,`$`,`*`,`+`,`?`,`{ }`,`[ ]`,`\`,`|`,`()`
