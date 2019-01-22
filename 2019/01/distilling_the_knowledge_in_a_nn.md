### Distilling the Knowledge in a Neural Network

论文地址: [https://arxiv.org/abs/1503.02531](https://arxiv.org/abs/1503.02531)

##### 要点

本文介绍了一种很实用的迁移学习方法: 将集成的/大的模型的知识压缩进一个单个/小的模型.

简单地说, 是训练小模型达到集成模型的泛化能力, 换言之, 将集成模型的泛化能力迁移到小模型.

稍微具体以西点, 是用集成模型(集成之后)输出的概率, 文中称为"soft target", 作为小模型的 target 进行学习. 这就好像"对着参考答案写作业, 抄多了, 以后不管是对是错, 写出来就是参考答案的味道了". 文中的说法是, **当 soft target 的熵很高时, 它提供比 hard target (即 label) 更多的信息, 而且样本之间的方差会更小, 于是只需要少得多的数据就能完成小模型的训练, 学习率也可以设得更高**. 还是以写作业为例, "在不会写的情况下, 标准答案就给出了数字, 过程略, 这不过给了往答案凑的方向; 如果是参考十位同学的作业, 那么他们答案的出入能带来更多的解题思路". 

再具体一些就是, 为小模型生成 soft targets 时, 对 softmax 升温, 直到得到合适的 soft targets. 然后用 soft targets 和同样的高温来训练小模型. 就"升温"而言, 本文用的"蒸馏(distillation)"一词可以说很形象了. (撇开这个, 我还是觉得用得精妙, 有一种化学实验提纯的味道, 只是这次提纯的是知识)

那么什么叫"对 softmax 升温"呢? 大多数同学熟悉的 softmax 函数应该是这样的:

![softmax_normal](../../img/201901/softmax_normal.png)

不过, 有时候它也会被写成如下形式:

![softmax_temperature](../../img/201901/softmax_temperature.png)

式中的 T 就是 softmax 的温度, 前边的 softmax 是 T=1 的一个特例 (请忽略字符的不统一). 使用更大的 T 值, 即升温, 会得到更软的概率分布. 体会一下:

```ipython
In [5]: np.exp(1/1)/(np.exp(1/1)+np.exp(10/1))
Out[5]: 0.0001233945759862317

In [7]: np.exp(10/1)/(np.exp(1/1)+np.exp(10/1))
Out[7]: 0.9998766054240137

In [8]: np.exp(1/10)/(np.exp(1/10)+np.exp(10/10))
Out[8]: 0.28905049737499605

In [9]: np.exp(10/10)/(np.exp(1/10)+np.exp(10/10))
Out[9]: 0.7109495026250039
```

根据熵的计算公式: ![entropy](../../img/201901/entropy.png), 更软的 target, 带来了更高的熵, 根据熵的定义, 信息量更大了.

```ipython
In [10]: -(0.0001233945759862317*np.log2(0.0001233945759862317)+0.9998766054240137*np.log2(0.9998766054240137))
Out[10]: 0.0017802184127800975

In [11]: -(0.28905049737499605*np.log2(0.28905049737499605)+0.7109495026250039*np.log2(0.7109495026250039))
Out[11]: 0.8674915504724934
```

如此大费周章的目的何在呢? 文中的例子很直观, 照搬了: 在 MNIST 上, 给定不同的 2, 有些是更像 3, 有些更像 7, 其他数字就不例举. 这些信息对于小模型模拟集成模型是极有用的, 所谓丝丝入扣. 但是鉴于集成模型优越的性能, 以上相似的概率很小很小, 比如说 0.000001 更像 3, 0.000000001 更像 7, 从量级上看, 差了 1000 倍耶, 但是对于交叉熵的影响还是太小太小了, 可以说连鸡肋都不如. 以上对 softmax 升温, 并使用 soft target 来训练小模型的目的正在与此, 为了让小模型快速获得这类信息. 事实上, 小模型的 softmax 用了相同的高温. 这有点像电影"无双"里的造假, 要放大了观察, 也要同比例地模仿, 不过造出来的假钞还得和原本真钞一般大小. 所以, 最后使用小模型的时候, 温度 T 恢复到 1.

文中提供了两种迁移的方法, 一种就使用带 soft targets 的数据, 另一种混合使用了 soft targets 和 targets, 就好像一遍参考同学的作业, 一遍参考标准答案. 对于后者, 文章建议使用两个目标函数, 分别针对 soft targets 和 targets, 然后求加权平均, 而不是混合 soft targets 和 targets.

文中提到了一种集成模型的思路, 个人觉得挺受启发的: 先在整个训练集上训练一个通用模型, 然后针对通用模型易混淆的数据, 用这些数据去训练专家模型. 专家模型用通用模型的参数初始化, 在各自对应的小数据集上进行专门地训练, 调整参数. 至于数据的选择, 作者对通用模型的预测结果的协方差矩阵进行了聚类操作. 所有专家模型不在乎的类, 都归为它的垃圾类.

文章还证明 soft targets 具有正则化的作用, 能缓解模型在小数据集上的过拟合.


##### 备注

这篇文章的思想和 meta-embedding (将多个 word embeddings 的知识压缩进一个 word embedding) 很接近.