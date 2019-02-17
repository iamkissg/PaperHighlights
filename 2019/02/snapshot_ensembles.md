### Snapshot Ensembles: Train 1, get M for free

论文地址: [https://openreview.net/pdf?id=BJYwwY9ll](https://openreview.net/pdf?id=BJYwwY9ll)

##### 要点

本文利用了神经网络多局部最优点的特点, 在承认局部最优点价值的基础上, 保存每一次到达局部最优点时的模型(称为 snapshot), 测试阶段通过集成多个局部最优模型来取得更好的结果.

该方法能成功的原因有以下几点:

* 神经网络多凸, 多局部最优点的特性
* SGD 具有跳出局部最优困境的能力

为确保模型能跳出局部最优困境, 文章使用了 SGDR, 即时不时地突然放大学习率; 此外, 为了尽快探索更多的局部最优点, 使用了 Cyclic Annealing, 在每个 cycle (由多个 epoch 组成) 中, 快速地减小学习率, 以达到快速收敛至局部最优点的目的. 两种技术的结合, 使得训练时, loss 成下图中红色曲线般变化:

![snapshot loss](../../img/201902/snapshot_lr.png)
