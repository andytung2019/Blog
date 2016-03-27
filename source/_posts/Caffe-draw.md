title: Caffe学习-绘制网络图
date: 2016-03-27 14:55:39
tags: [caffe]
categories: caffe
---

from [happynear][1]

# 一、准备
----

　　首先caffe的python接口当然是必备的了，还没有生成python接口的同学可以参照我的上一篇博客来生成。 
　　然后是需要安装protobuf的python接口，可以参照这篇博客进行安装，安装过程比较简单，就不赘述了。 
　　安装GraphViz：http://www.graphviz.org/。 
　　安装pydot，可以使用`pip install`来安装，也可以从[`Github`][2]，这里需要修改`pydot/__init__.py`文件中第596行： 
```
path = os.path.join(os.environ['PROGRAMFILES'], 'ATT', 'GraphViz', 'bin') 
```
改为： 
```
path = r"C:\Program Files (x86)\Graphviz2.37\bin" 
```
同样，下边的那个path也改为
```
r”C:\Program Files(x86)\Graphviz2.37\bin”
```
如果你将GraphViz安装在别的位置，那这个目录也要跟着改变。

# 二、生成网络结构图
---------

`draw_net.py`文件在`caffe/python`目录下。在实际中发现在 `caffe/python`目录下不能`import caffe`,然后将`draw_net.py`文件拷到caffe根目录下可以运行：
```
python draw_net.py examples/mnist/lenet_train_test.prototxt -lenet.png
```


  [1]: http://blog.csdn.net/happynear/article/details/45440709
  [2]: https://github.com/nlhepler/pydot