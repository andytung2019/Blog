title: Caffe学习-lmdb型数据库的建立
date: 2016-03-27 22:03:41
tags: [caffe]
categories: caffe
---

# 为train和test建立图片位置和标签文本
以下使用python制作的脚本，将原始数据与脚本放置在同一个目录下，即可生成`train.txt`和`val.txt`文件：
```
#!/usr/bin/python  
# -*- coding:utf8 -*-  
import os

def findPath():
	path="."
	dirList=[]
	fileList=[]
	files=os.listdir(path)
	for f in files:
		if(os.path.isdir(path+'/'+f)):
			dirList.append(f)
		if(os.path.isfile(path+'/'+f)):
			fileList.append(f)
	train=open('train.txt','w')
	val=open('val.txt','w')
	classfile=open('classfile.txt','w')
	TRAINSIZE=25
	TESTSIZE=10
	classIndex=0
	for dir in dirList:
		trainInd=0
		testInd=0
		subfiles=os.listdir(path+'/'+dir)
		for subfile in subfiles:
			if trainInd<TRAINSIZE:
				train.write('/'+dir+'/'+subfile+' '+str(classIndex)+'\n')
				trainInd=trainInd+1
			else:
				if testInd<TESTSIZE:
					val.write('/'+dir+'/'+subfile+' '+str(classIndex)+'\n')
					testInd=testInd+1
		classfile.write(dir+'\t'+str(classIndex)+'\n')
		classIndex=classIndex+1
	train.close()
	val.close()
	classfile.close()

if __name__ == '__main__':
	findPath()
```
# 生成lmdb数据库和均值文件
在windows下生成lmdb数据库和均值文件：
```
echo off
echo "creating training lmdb..."

"../Build/x64/Release/convert_imageset.exe" --resize_height=128 --resize_width=128 --shuffle "Caltech101" "Caltech101/train.txt" "Caltech101/train_lmdb"  

echo "creating testing lmdb..."

"../Build/x64/Release/convert_imageset.exe" --resize_height=128 --resize_width=128 --shuffle "Caltech101" "Caltech101/val.txt" "Caltech101/val_lmdb"  

echo "generating mean file..."

"../Build/x64/Release/compute_image_mean.exe" "Caltech101/train_lmdb" "Caltech101/imagenet_mean.binaryproto"
```
# solver文件
```
net: "train_val.prototxt"
# 这里是test的批次，设为100，迭代次数10次，这样，就覆盖了1000
test_iter: 10
# 每迭代100次测试一次
test_interval: 100
base_lr: 0.01
lr_policy: "step"
gamma: 0.1
# drop the learning rate every 100 iterations  
stepsize: 10
display: 10
max_iter: 1000
momentum: 0.9
weight_decay: 0.0005
## 每100次迭代存储一次数据到电脑，名字是caffe_alexnet_train
snapshot: 100
snapshot_prefix: "caffe_alexnet_train"
solver_mode: CPU
```