title: 深度学习-CNN
date: 2016-03-23 21:38:23
tags: [DL]
categories: DL
---

from [liulina603][1]，[zouxy09][2] , [http://deeplearning.net/][3]

 1. 输入图像和网络的拓扑结构能很好的吻合；
 2. 特征提取和模式分类同时进行，避免了显式的特征抽取，而隐式地从训练数据中进行学习；
 3. 权重共享可以减少网络的训练参数，通过结构重组和减少权值将特征提取功能融合进多层感知器。

## 架构

![cnn][4]
### 滤波器组合层
包含：
1. 卷积滤波器：
2. 非线性变换函数：
3. 可训练的增益函数。
$$y=gtanh(\sum{k x})$$

### 非线性变换层

### pooling/subsampling
1. 减少计算量
2. 旋转不变性 



## feature map
利用不同的卷积核来对图像进行特征提取便可以得到不同特征的放映。
![CNN][5]




## 流程
### 第一阶段，向前传播阶段：
1. 从样本中取一个样本(X,Yp)，将X输入网络；
2. 计算相应的实际输出Op。
在此阶段，信息从输入层经过逐级的变换，传送到输出层。这个过程也是网络在完成训练后正常运行时执行的过程。在此过程中，网络执行的是计算（实际上就是输入与每层的权值矩阵相点乘，得到最后的输出结果）。

### 第二阶段，向后传播阶段
3. 计算实际输出Op与相应的理想输出Yp的差；
4. 按极小化误差的方法反向传播调整权矩阵。
﻿﻿


  [1]: http://blog.csdn.net/liulina603/article/details/44915905
  [2]: http://blog.csdn.net/zouxy09/article/details/8781543
  [3]: http://deeplearning.net/tutorial/lenet.html#the-convolution-operator
  [4]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_mylenet.png
  [5]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_cnn_explained.png