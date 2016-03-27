title: python学习-UnicodeDecodeError
date: 2015-09-16 15:52:01
tags: python
---

------

>UnicodeDecodeError
>SyntaxError

------

# UnicodeDecodeError
在Windows平台下，安装python模块时，报如下错误：
UnicodeDecodeError: 'ascii' codec can't decode byte 0xc9 in position 7: ordinal not in range(128)
解决方法：

修改mimetypes.py文件，路径位于python的安装路径下的Lib\mimetypes.py文件。在import下添加如下几行：

```
if sys.getdefaultencoding() != 'gbk': 
reload(sys) 
sys.setdefaultencoding('gbk')
```

# SyntaxError

python下出现错误：

`
SyntaxError: Non-ASCII character '\xd6' in file F:/F/documents/python/Spider/lab2.py on line 17, but no encoding declared
`
`解决方法：源代码文件第一行添加：`#coding:utf-8`
