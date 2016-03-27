title: 深度学习-ReLU
date: 2016-03-22 21:21:30
tags: [DL]
categories: DL
---

# ReLu(Rectified Linear Units)


from [贾伟][1]， [Physcalの大魔導書][2]，[liulina603][3]

## 为何引入非线性激励函数

在不用非线性激励函数时，相当于$f(x)=x$，此时每一层输出都是上层输入的线性叠加，可以验证，这样的叠加都导致无论网络有多少层，输出都为输入的线性组合，只与一个隐藏层的效果相当，即多层感知机（MLP）.



## 一般非线性激励函数

### sigmiod函数

从神经科学上来看，中央区酷似神经元的兴奋态，两侧区酷似神经元的抑制态，因而在神经网络学习方面，可以将重点特征推向中央区，将非重点特征推向两侧区。
从数学上来看，非线性的Sigmoid函数对中央区的信号增益较大，对两侧区的信号增益小，在信号的特征空间映射上，有很好的效果。

1. 在反向传播求误差梯度时，求导的计算量大。
2. 对于深层网络，由于sigmiod函数接近饱和时变换过于缓慢，导数趋于零，导致其反向传播时很容易出现梯度消失的情况。



## 生物神经的稀疏激活性

神经科学家Dayan、Abott从生物学角度，模拟出了脑神经元接受信号更精确的激活模型,这个模型对比Sigmoid系主要变化有三点：
> 1. 单侧抑制 
> 2. 相对宽阔的兴奋边界 
> 3. 稀疏激活性（红框里端状态完全没有激活）

神经科学方面研究：
1. 从信号方面来看，即神经元同时只对输入信号的少部分选择性响应，大量信号被刻意的屏蔽了，这样可以提高学习的精度，更好更快地提取稀疏特征。
2. 在经验规则的初始化W之后，传统的Sigmoid系函数同时近乎有一半的神经元被激活，这不符合神经科学的研究，而且会给深度网络训练带来巨大问题。
![RS][4]
![RS2][5]




## 稀疏性(并未被证明与ReLu绝对存在关联)

### 信息分离
深度学习一个明确的目标是从数据变量中解离出关键因子。而在海量的原始数据中，通常缠绕着高度密集的特征。若能解开特征间复杂的关系，转换为稀疏特征，那么特征便具有鲁棒性。

### 线性可分性
稀疏特征有更大可能线性可分，或者对非线性映射机制有更小的依赖。因为稀疏特征处于高维的特征空间上。从流形学习观点来看，稀疏特征被移到了一个较为纯净的低维流形面上。

### 稠密分布但是稀疏
稠密缠绕分布着的特征是信息最富集的特征，从潜在性角度，往往比局部少数点携带的特征成倍的有效。而稀疏特征，正是从稠密缠绕区解离出来的，潜在价值巨大。

### 稀疏性激活函数的贡献的作用
不同的输入可能包含着大小不同关键特征，使用大小可变的数据结构去做容器，则更加灵活。
假如神经元激活具有稀疏性，那么不同激活路径上：不同数量（选择性不激活）、不同功能（分布式激活），
两种可优化的结构生成的激活路径，可以更好地从有效的数据的维度上，学习到相对稀疏的特征，起到自动化解离效果。



## ReLu

$$f(x)=max(0,x)$$
> 加速收敛
> ReLu的导数对于大于0的部分恒为1。于是ReLU确实可以在BP的时候能够将梯度很好地传到较前面的网络

### 网络的非线性部分仅来自于神经元部分选择性激活
在深度网络中，对非线性的依赖程度可以缩一缩。另外,稀疏特征并不需要网络具有很强的处理线性不可分机制。(?)
在深度学习模型中，使用简单、速度快的线性激活函数可能更为合适。(?)
![net][6]

### [ReLu缩小做和不做非监督预训练的差距][7]
![不同预训练比较][8]
ReLu减小了非监督学习和监督学习之间的差异。

### 更快的特征学习速度
![AlexNet][9]




## 参考文献
  
  * Li J, Luo W, Yang J, et al. Unsupervised Pretraining Encourages Moderate-Sparseness[J]. arXiv preprint arXiv:1312.5813, 2013.
MLA	
  * Glorot X, Bordes A, Bengio Y. Deep Sparse Rectifier Neural Networks[J]. Journal of Machine Learning Research, 2010, 15.
  * Krizhevsky A, Sutskever I, Hinton G E. Imagenet classification with deep convolutional neural networks[C]//Advances in neural information processing systems. 2012: 1097-1105.
  * He K, Zhang X, Ren S, et al. Delving deep into rectifiers: Surpassing human-level performance on imagenet classification[C]//Proceedings of the IEEE International Conference on Computer Vision. 2015: 1026-1034.



  [1]: https://www.zhihu.com/people/jia-wei-97-19
  [2]: http://www.cnblogs.com/neopenx/p/4453161.html
  [3]: http://blog.csdn.net/liulina603/article/details/44915905
  [4]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_R_S.png
  [5]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_R_S2.png
  [6]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_net.png
  [7]: https://www.researchgate.net/publication/259399568_Why_does_the_unsupervised_pretraining_encourage_moderate-sparseness
  [8]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_AlexNet.png
  [9]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_AlexNet.png