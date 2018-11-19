### [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385)

**TL;DR** 本文提出了大名鼎鼎的*残差网络 ResNet*, 意在解决深度神经网络的退化问题. 将要学习的变换 H(x) 变形为 F(x)+x 的形式, 使得残差单元具有模拟恒等映射的能力, 从而缓解了深度带来的退化问题.

#### Key Points

* 首先提出了一个问题: Is learning better networks as easy as stacking more layers? 全文就是在寻找这个问题的解.
* 指出深度的增加带来了两个问题:
    1. 梯度消失与爆炸问题. Normalized initalization 与 intermediate normalization layers (比如 BN) 等就是解决该问题的;
    2. Degradation problem, 即深度增加, 但准确度开始饱和, 然后急剧下降. 本文提出的 ResNet 正是解决整个问题的.
* 本文的一个观点是: 基于浅层模型来构成深层模型, 增加的层全部使用*恒等映射 identity mapping*. 此时, 深层模型不应该比浅层模型具有更高的训练误差. Degradation problem 的现实表明模型通过多层非线性变换来学习 identity mapping 有困难.
* 本文的部分灵感来自*残差表示 residual representation*. 相关工作表明: 良好*变形 reformulation* 或*预处理 preconditioning* 能简化优化.
* 本文的另一部分灵感来自于 *shortcut connection*, 即某层的输出可不经过一些中间层的处理而直接作为后续某层的输入.
* 在以上两点的基础上, ResNet 将要学习的映射 H(x) 变形为 F(x)+x. 其中 F(x) 是残差函数; 因为 x 既然 F(x) 的输入, 又与 F(x) 相加作为下一层的输入, 不经过 $F(\cdot)$ 的那条路径就是 shortcut connection.
* ResNet 能解决 degradation problem, 因为如果 identity mapping 是最优解的情况, 模型可以将更多的权值分配到 shortcut connection 上, 使 F(x) 的权值趋向 0, 从而更加近似 identity mapping.
* Shortcut connection 并不引入额外的参数, 也就不会带来计算复杂性. 这保证了 ResNet 的简单高效. 不过也可以在 shortcut connection 上加一些变换, 文章在图像尺寸发生变化时, 就采用了这种思路, 提出了两种参考方法:
    1. shortcut 上是 zero padded 过的 x;
    2. 通过 1x1 的卷积改变 x 的尺寸.
* 文中对学习率的处理很有意思. 在误差停滞时, 将学习率下调一个数量级 (10x); 在另一个实验中, 初始学习率稍大了点, 先用小一个数量级 (10x) 的学习率作 warm up, 直到训练误差小于初始误差的 80%, 恢复到原来的学习率, 后续的学习率更新如前.
* 为了实现更深的网络, 文章调整了残差单元, 使用 3 层网络来学习残差函数, 分别实现 1x1, 3x3, 1x1 的卷积 (原来是两个 3x3 的卷积层). 其中 1x1 的卷积层作降维与恢复维数用. 由于 3x3 的卷积层的输入输出图像的尺寸更小, 像"瓶颈"一般, 因此这种设计称为 *bottleneck design*.
* 文章指出使用 x 作为 shortcut connnection, 即不引入额外的变换参数对 bottleneck 架构很关键. 当在 shortcut connection 上使用某种变换, 由于 shortcut 连接在两个高维图像 (两个1x1卷积层) 的两端, 时间复杂度和模型尺寸将翻倍.
* 实验分析表明: 残差函数通常比非残差函数函接近于 0, 这使得残差单元的表现更倾向于 identity mapping. 而随着网络的加深, ResNet 的单独一层对信号/数据的修改变得更小.

![residual_function.png](../../img/residual_function.png)

#### Notes/Questions

* 在搜索论文的时候, 我刷新了 3 次 Google 的页面, 第一次显示引用量 8200+, 第二次, 8300+, 第三次 8400+, 间隔几分钟. 这...
* 在看本文之前, 我对 ResNet 一直抱着"敬而远之"的心态, 我知道它很牛, 但又担心太复杂而不敢去了解. 看过本文之后的感觉是: 人间有大美, 于简单处成. 原来 ResNet 不复杂嘛, 简洁到通用.
* 又一篇模范论文, 逻辑很清晰, 实验很完整, 壮哉我凯明大神.
