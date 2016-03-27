title: Caffe学习-layer层
date: 2016-03-21 20:37:41
tags: [caffe]
categories: caffe
---

# Layer


from[楼燚(yì)航][1],[Deep_Learner][2]
![Layer思维导图][3][shwley][4]
`Layer`类是抽象出来的基类，其他都是在其基础上的继承。
## 构造函数
构造函数会尝试从protobuf文件读取参数，在初始化列表初始化LayerParameter,之后blobs_这里存放的是一个指向blob类的shared_ptr指针的一个vector，在这里是申请空间，然后将传入的layer_param中的blob拷贝过来。
```
explicit Layer(const LayerParameter& param)
    : layer_param_(param), is_shared_(false) {
      // Set phase and copy blobs (if there are any).
      phase_ = param.phase();
      if (layer_param_.blobs_size() > 0) {
        blobs_.resize(layer_param_.blobs_size());
        for (int i = 0; i < layer_param_.blobs_size(); ++i) {
          blobs_[i].reset(new Blob<Dtype>());
          blobs_[i]->FromProto(layer_param_.blobs(i));
        }
      }
    }//每个网络层需要自己定义它的setup而不需要构造函数
```
## SetUp
``bottom`网络层的输入，`top`网络层的输出，需要被reshape
```
void SetUp(const vector<Blob<Dtype>*>& bottom, const vector<Blob<Dtype>*>& top) 
{
    InitMutex();
    CheckBlobCounts(bottom, top);
    LayerSetUp(bottom, top);
    Reshape(bottom, top);
    SetLossWeights(top);
}
```
## Forward
给定输入的`blob`，计算输出`blob`并返回`loss`:
```
inline Dtype Forward(const vector<Blob<Dtype>*>& bottom, const vector<Blob<Dtype>*>& top);
```
## Backward
给定输出的`blob`，据此计算输入误差梯度：
```
inline void Backward(const vector<Blob<Dtype>*>& top,
      const vector<bool>& propagate_down,
      const vector<Blob<Dtype>*>& bottom);
```
## loss
```
//返回指定索引的损失值
inline Dtype loss(const int top_index) const
//设置网络层制定索引位置的loss
inline void set_loss(const int top_index, const Dtype value)
```
## layer type
```
   //返回网络层名字，字符串描述
virtual inline const char* type() const { return ""; }
   //Bottom的blob的确切数目
virtual inline int ExactNumBottomBlobs() const { return -1; }
   //Bottom blob的最小数目
virtual inline int MinBottomBlobs() const { return -1; }
   //Botttom的确切数目
virtual inline int MaxBottomBlobs() const { return -1; }
   //Top Blob的确切数目
virtual inline int ExactNumTopBlobs() const { return -1; }
   //最小的blob的数目
virtual inline int MinTopBlobs() const { return -1; }
   // 最大的blob的数目
virtual inline int MaxTopBlobs() const { return -1; }
   // 是否bottom 和top的数目相同
virtual inline bool EqualNumBottomTopBlobs() const { return false; }
   // 是否自动Top blob
virtual inline bool AutoTopBlobs() const { return false; }
```
## propagate
AllowforceBackward用来设置是否强制梯度返回，因为有些层其实不需要梯度信息，后面两个函数分别查看以及设置是是否需要计算梯度。
```
  /**
   * @brief Return whether to allow force_backward for a given bottom blob
   *        index.
   *
   * If AllowForceBackward(i) == false, we will ignore the force_backward
   * setting and backpropagate to blob i only if it needs gradient information
   * (as is done when force_backward == false).
   */
  virtual inline bool AllowForceBackward(const int bottom_index) const {
    return true;
  }

  /**
   * @brief Specifies whether the layer should compute gradients w.r.t. a
   *        parameter at a particular index given by param_id.
   *
   * You can safely ignore false values and always compute gradients
   * for all parameters, but possibly with wasteful computation.
   */
  inline bool param_propagate_down(const int param_id) {
    return (param_propagate_down_.size() > param_id) ?
        param_propagate_down_[param_id] : false;
  }
  /**
   * @brief Sets whether the layer should compute gradients w.r.t. a
   *        parameter at a particular index given by param_id.
   */
  inline void set_param_propagate_down(const int param_id, const bool value) {
    if (param_propagate_down_.size() <= param_id) {
      param_propagate_down_.resize(param_id + 1, true);
    }
    param_propagate_down_[param_id] = value;
  }
```
## SetLossWeights
初始化top bottom的weights，并且在diff blob中存储非零的loss weights.
```
inline void SetLossWeights(const vector<Blob<Dtype>*>& top)
```


  [1]: http://www.cnblogs.com/louyihang-loves-baiyan/p/5152653.html
  [2]: http://blog.csdn.net/chenriwei2/article/details/46417293
  [3]: http://7xlbd9.com1.z0.glb.clouddn.com/hsh_blog_3383918621.png
  [4]: http://www.shwley.com/index.php/archives/68/