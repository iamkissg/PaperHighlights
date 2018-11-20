### Frustratingly Easy Meta-Embedding – Computing Meta-Embeddings by Averaging Source Word Embeddings

论文地址: [http://aclweb.org/anthology/N18-2031](http://aclweb.org/anthology/N18-2031)

##### 要点

本文就一点: 可以对同一个单词的不同 embeddings 求平均, 作为它的 meta-embedding. 就结果而言, 它的表现和 CONC 相似, 集成方法更好的时候, 都更好; 更差的时候, 都更差, 只是绝大多数情况下, AV 都稍微不如 CONC.

本文的论证挺有意思的, 如下:

以两个预训练 embeddings 为例, CONC 可以看作对其中一个前补零, 对另一个后补零, 然后相加. 补零后的两个向量正交. 文中没有细说, 我的理解是: 对于一个单词, 可以认为它在不同预训练模型中的向量相同, 极端地, 记作 [1], 补零之后, 得到 [0 1] 和 [1 0], 两者正交, 就这样. 经过下式的转换, 就得到了两个单词在 CONC 后的欧式距离是它们在两个预训练 embeddings 空间的欧式距离的平方和的开方. (cos(theta)=0 的关键就是正交)

![E conc](../../img/11/wme_e_conc.png)

使用平均法代替拼接, 两个向量在新空间的欧式距离计算如下. 此时 theta 似乎无解了.

![E avg](../../img/11/wme_e_avg.png)

好在 [Distributions of angles in random packing on spheres](https://orfe.princeton.edu/~jqfan/papers/13/packing.pdf) 提出的以下理论完美解决了这个问题:

> N 维空间, 随着空间中的点趋向于无穷多, 任意两点间的夹角成高斯分布, 分布的均值就是 pi/2, 即 90 度.

Word embeddings 基本满足上述要求: 维数算挺高了, vocab 也挺大. 于是, 平均法得到的欧式距离的期望正比于拼接法的欧式距离, 从而平均法有效.

![angle distribution](../../img/11/avg_angle_distribution.png)

#### 备注

不知道该使用哪个预训练的 word embeddings? 平均法试一下先.
