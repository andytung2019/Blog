title: 深度学习-dropout
date: 2016-03-22 22:07:32
tags: [DL]
categories: DL
---

from [tornadomeet][1] 

`Alex`在`ImageNet Classification with Deep Convolutional Neural Networks`一文中提到利用`dropout`方法来降低过拟合。

## dropout流程

> 1. 假设训练的网络，在训练开始时，我们随机地“删除”一半的隐层单元，视它们为不存在；
> 2.  保持输入输出层不变，按照BP算法更新上图神经网络中的权值（虚线连接的单元不更新，因为它们被“临时删除”了）。
> 3. 迭代多次，每一次迭代都“随机”地去删掉一半，直至训练结束。

运用了dropout的训练过程，相当于训练了很多个只有半数隐层单元的神经网络（后面简称为“半数网络”），每一个这样的半数网络，都可以给出一个分类结果，这些结果有的是正确的，有的是错误的。随着训练的进行，大部分半数网络都可以给出正确的分类结果，那么少数的错误分类结果就不会对最终结果造成大的影响。


## 对于dropout的理解

* 由于每次用输入网络的样本进行权值更新时，隐含节点都是以一定概率随机出现，因此不能保证每2个隐含节点每次都同时出现，这样权值的更新不再依赖于有固定关系隐含节点的共同作用，阻止了某些特征仅仅在其它特定特征下才有效果的情况。
* 可以将dropout看作是模型平均的一种。对于每次输入到网络中的样本（可能是一个样本，也可能是一个batch的样本），其对应的网络结构都是不同的，但所有的这些不同的网络结构又同时share隐含节点的权值。这样不同的样本就对应不同的模型，是bagging的一种极端情况。个人感觉这个解释稍微靠谱些，和bagging，boosting理论有点像，但又不完全相同。
* native bayes是dropout的一个特例。Native bayes有个错误的前提，即假设各个特征之间相互独立，这样在训练样本比较少的情况下，单独对每个特征进行学习，测试时将所有的特征都相乘，且在实际应用时效果还不错。而Droput每次不是训练一个特征，而是一部分隐含层特征。
* Dropout类似于性别在生物进化中的角色，物种为了使适应不断变化的环境，性别的出现有效的阻止了过拟合，即避免环境改变时物种可能面临的灭亡。




![dropout1][2]
![dropout2][3]


  * Hinton G E, Srivastava N, Krizhevsky A, et al. Improving neural networks by preventing co-adaptation of feature detectors[J]. arXiv preprint arXiv:1207.0580, 2012.
  * Krizhevsky A, Sutskever I, Hinton G E. Imagenet classification with deep convolutional neural networks[C]//Advances in neural information processing systems. 2012: 1097-1105.


  [1]: http://www.cnblogs.com/tornadomeet/p/3258122.html
  [2]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_dropout1.png
  [3]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_dropout2.png