title: Caffe学习-Blob类
date: 2016-03-16 22:45:12
tags: [caffe]
categories: caffe
---

# Blob


参考于[from][1]

### 成员变量
```
  shared_ptr<SyncedMemory> data_;//数据，正向传播时用的
  shared_ptr<SyncedMemory> diff_;//梯度
  shared_ptr<SyncedMemory> shape_data_;//存储Blob的形状
  vector<int> shape_;//存储Blob的形状,新版本
  int count_;//元素个数(个数*通道数*高度*宽度)
  int capacity_;//当前元素个数
```
其中`data_`为正向传播所用，而`diff_`为存储偏差，来更新data。

方法
--
### 构造函数
可以使用如下三种方式构造;
```
  Blob() : data_(), diff_(), count_(0), capacity_(0) {}
  explicit Blob(const int num, const int channels, const int height, const int width);
  explicit Blob(const vector<int>& shape);
```
### reshape
初始化数据成员，智能指针指向 SyncedMemory 对象:
```
  void Reshape(const vector<int>& shape);
  void Reshape(const BlobShape& shape);
  void ReshapeLike(const Blob& other);
```
注意到对输入的一个`blob`进行reshaping时立即调用`Net::Backward()`会产生错误。不管是`Net::Forward()`还是`Net::Reshape()`都需要被调用，来传递新的输入形状到更高的层中。
### offset
获得偏差，通过`((n * channels() + c) * height() + h) * width() + w`来计算。
```
  inline int offset(const int n, const int c = 0, const int h = 0,
      const int w = 0) const {
    CHECK_GE(n, 0);
    CHECK_LE(n, num());
    CHECK_GE(channels(), 0);
    CHECK_LE(c, channels());
    CHECK_GE(height(), 0);
    CHECK_LE(h, height());
    CHECK_GE(width(), 0);
    CHECK_LE(w, width());
    return ((n * channels() + c) * height() + h) * width() + w;
  }
  inline int offset(const vector<int>& indices) const {
    CHECK_LE(indices.size(), num_axes());
    int offset = 0;
    for (int i = 0; i < num_axes(); ++i) {
      offset *= shape(i);
      if (indices.size() > i) {
        CHECK_GE(indices[i], 0);
        CHECK_LT(indices[i], shape(i));
        offset += indices[i];
      }
    }
    return offset;
  }
```
### 变量获取
对于`Blob`中的4个基本变量`num`,`channel`,`height`,`width`可以直接通过`shape(0)`,`shape(1)`,`shape(2)`,`shape(3)`来访问,或者使用以下方法来实现：
```
inline int num() const { return LegacyShape(0); }
inline int channels() const { return LegacyShape(1); }
inline int height() const { return LegacyShape(2); }
inline int width() const { return LegacyShape(3); }
inline int LegacyShape(int index) const
```
变量通过以下方法来进行获得：
```
inline Dtype data_at(const int n, const int c, const int h, const int w) const 
inline Dtype diff_at(const int n, const int c, const int h, const int w) const
inline Dtype data_at(const vector<int>& index) const
inline Dtype diff_at(const vector<int>& index) const
```
### CopyFrom
`CopyFrom`复制`Blob`中的数据，可以根据参数进行梯度或数据reshape控制：
```
void CopyFrom(const Blob<Dtype>& source, bool copy_diff = false, bool reshape = false);
```
### 计算L1与L2范数
```
Dtype asum_data() const;  //L1 norm
Dtype asum_diff() const;  //L1 norm
Dtype sumsq_data() const; //L2 norm
Dtype sumsq_diff() const; //L2 norm
```
### Scale Data
利用常数`scale_factor`对`Blob`中数据进行规则化。
```
void scale_data(Dtype scale_factor);
void scale_diff(Dtype scale_factor);
```
### 计算Blob容量
```
inline int count(int start_axis, int end_axis) const//计算从start_axis到end_axiss的容量
inline int count(int start_axis) const//计算整个Blob的容量
```
### Update
`update`函数中更新 `data_` 的数据，即减去 `diff_` 的数据。其中调用了`math_functions.hpp`中的函数,这函数也是封装的`mkl`的函数：
```
void caffe_axpy(const int N, const Dtype alpha, const Dtype* X, Dtype* Y);
```
### 数据指针返回函数
函数调用 SyncedMemory 的函数，来返回数据的指针：
```
const Dtype* cpu_data() const;
void set_cpu_data(Dtype* data);
const int* gpu_shape() const;
const Dtype* gpu_data() const;
const Dtype* cpu_diff() const;
const Dtype* gpu_diff() const;
Dtype* mutable_cpu_data();
Dtype* mutable_gpu_data();
Dtype* mutable_cpu_diff();
Dtype* mutable_gpu_diff();
```
### 序列化与反序列化
```
void FromProto(const BlobProto& proto, bool reshape = true);//从 proto 读数据进来，即反序列化
void ToProto(BlobProto* proto, bool write_diff = false) const;//序列化到 proto 保存
```

  [1]: http://www.cnblogs.com/louyihang-loves-baiyan/
