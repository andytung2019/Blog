title: 计算机视觉学习-投影变换与F矩阵
date: 2016-03-18 22:06:51
tags: CV
---

> 投影变换
> F矩阵

# 投影变换

## 仿射变换
需要保证转换矩阵最后一行为（0,0,1） ，其包含了线性变换和平移。主要性质：
1.	原点并不需要被映射到原点；
2.	直线会映射到直线；
3.	平行线依旧为平行线；
4.	比率被保存；
5.	Closed under composition

## 投影变换
包含放射变换与投影扭曲。
1.	原点不需要被映射到原点；
2.	直线映射为直线；
3.	平行线不再一定为平行线；
4.	比率关系不复存在；
5.	Closed under composition
![投影变换][1]
![投影变换2][2]
![投影变换3][3]

# F矩阵
## 相机模型
![相机模型][4]
![相机模型2][5]
将世界坐标系中的点转换为图像上的坐标：
![转换][6]
![CAMERA][9]
对于物理上同一点，在不同的角度对其观察时产生不同的世界坐标分别为$P_{w},P_{c}$ ，那么两者之间的关系为：其中`R`存在正交性：
$$ {P_c} = R({P_w} - C) = R{P_w} - RC = R{P_w} + T $$
![极平面][7]
 `F`将图像1中的其次点映射为图像2中的极线`Fp`。图像1中对应于图像2中的极线均相交于极点。
$$ {q^T}Fp = 0 $$
## 外极几何
$$ \lambda m = K[I|0]M $$
$$ \lambda 'm' = K'[I|t]M $$
$$ M' = RM + t $$
$$ t \times M' = t \times RM + t \times t $$ 
$$ t \times M' = t \times RM $$
$$ M' \cdot (t \times M') = M' \cdot (t \times RM) $$
$$ 0 = M' \cdot (t \times RM) $$
$$ M' ( {[t]_ \times }R )M = 0 $$
其中![A][8]
本质矩阵为：
$$ E = {[t]_ \times }R $$

基本矩阵为： 
$$ F = K'^{ - T}{[t]_ \times }R K^{ - T} $$ 

综上有: 
$$ m_{'T}Fm = 0 $$
由于实际中存在噪声的影响，导致基本矩阵`F` 非奇异，则需要对基本矩阵`F`进行奇异值分解，得到最小特征值对应的特征矩阵。




  [1]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_bianhuan.png
  [2]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_bianhuan2.png
  [3]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_bianhuan3.png
  [4]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_camera.png
  [5]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_camera2.png
  [6]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_zhuanhuan.png
  [7]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_jixian.png
  [8]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_a.jpg
  [9]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_camera3.png