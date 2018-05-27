### Distance-based Self-Attention Network for Natural Language Inference

论文地址: [https://arxiv.org/abs/1712.02047](https://arxiv.org/abs/1712.02047)

#### TL;DR

本文在 DiSAN 上做了微创新, 使用 distance mask, 对相对位置进行了建模, 相距越远的单词, mask matrix 中对应位置将是一个越大的负值, 从而一定程度上抑制了远距离单词间的依赖, 换言之, 强调了单词对邻近单词的依赖, 从而更好地分配 attention.

#### Key Points

* 文章将提出的模型应用于自然语言推理 NLI, 沿用了传统框架 (如下). 创新点体现在 sentence encoder 上

![overall_architecture_distance_self_attention_network.png](../img/overall_architecture_distance_self_attention_network.png)

* sentence encoder 基于 self-attention 对句子进行编码 (如下), 可以看到中间那一部分像极了 Transformer 的 encoder. 不同点在于, mutl-head attention 带上了 mask, 后一层的 add 变成了 gate.

![sentence_encoder_in_distance_self_attention_network.png](../img/sentence_encoder_in_distance_self_attention_network.png)

* 可以看到, 模型从 forward 和 backward 两个方向分别进行了学习, 因此, 即使使用了 distance mask, 也没有抛弃 DiSAN 中提出的 directional mask. Masked Attention 的计算如下: $Masked(Q, K, V)=softmax(\frac{QK^T}{\sqrt{d_k}} + M_{dir} + \alpha M_{dis})V$. 式中 $\alpha$ 起到调控作用.
* Distance mask 中每个元素代表句中两个单词间绝对距离的负. 由于 $exp(-\inf)=0$, 因此距离越远, 负值越大, 单词间的依赖程度越低.

![distance_mask_distance_self_attention_network.png](../img/distance_mask_distance_self_attention_network.png)

* Distance mask 强化单词对邻近单词的依赖, 作用类似于 CNN 的 filter提取局部特征. 不同点在于, 前者是对整个句子的 mask, 而后者仅仅局部像素的 mask.
* Masked multi-head attention 之后是一个 Fusion gate, 控制 attention 输出和 word embedding 的比例: $Gate(S, H)=F\bigodot S^F+(1-F)\bigodot H^F, where F=sigmoid(S^F+H^F+b^F)$ (S 是 word embedding 的矩阵, H 是 attention 的矩阵).
* 最后使用 MaxPooling 或 Multi-dimensional attention 或两者一起, 将拼接结果矩阵压缩为向量.
* 实验证明:
    * 相对位置很重要, 使用 distance mask 的实验组比不使用 distance mask 的实验组对句子长度具有更强的鲁棒性;
    * distance mask 强化了单词对邻近单词的依赖, 但真正具有强依赖关系的单词, 在远距离的情况下也能保持依赖. 换言之, 在保证局部依赖的同时, 又不是全局依赖.
    * Fusion gate 具有调节输出的作用, 关键词将更多地从 attention 输出, 非关键词更多地走 shortcut connection, 保持 word embedding.
    * Multi-dimensional attention 与 max pooling 的行为很相似, 都更关注关键词.

#### Notes/Questions

* 本文对 DiSAN 做了一个很小的改动, 站在巨人的肩膀上, 在 SNLI 上取得了 SOTA 的测试结果
