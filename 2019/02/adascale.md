# AdaScale: Towards Real-time Video Object Detection Using Adaptive Scaling

论文地址: [https://arxiv.org/abs/1902.02910](https://arxiv.org/abs/1902.02910)

## 要点

> 本文我单纯地想了解下方法, 没细看, 没看完.

通常, 我们都认为_速度_和_准确度_是鱼和熊掌不可兼得的, 只能取其一. 本文就是来打脸, 它告诉我们, 涉及到 image scaling, 将图片 rescale 到更低的分辨率, 不仅训练更快了, 有时候准确率还会上去.

文章的应用场景是视频中的目标检测, 从一帧帧的图像开始. 如何在对图片降采样的时候又提高分类的准确度呢? 这就是本文要解决的问题. 两个策略:

1. 减少假正例 \(false positive\), 它们可能是由于对不必要的细节过分关注引入的; \(这也是过拟合的一个原因吧\);
2. 增加真正例 \(true positive\), 通过对图片中目标的 scale 实现.

从标题可以看出, 文章应该是选择了后者, 并提出了一个自适应的方法. 具体的流程就是: 训练目标检测器 \(object detector\), 并用它来生成最佳的 scale labels, 然后用生成的 scale labels 来训练一个 scale regressor, 最后就是用得到的 scale regressor 来实时地 scale 视频的每一帧中的物体了.

