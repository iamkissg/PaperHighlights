# Dynamic Meta-Embeddings for Improved Sentence Representations

论文地址: [https://aclweb.org/anthology/D18-1176](https://aclweb.org/anthology/D18-1176)

## 要点

本文和之前的 word meta-embedding, 想法一致, 利用多个预训练的 word embeddings 来提高性能, 不同的是, 本文并没有导出一个 word embedding, 而是通过 self-attention 来调整不同 word embedding 的比例, 提高 sentence representation. 就像冷水和热水混合, 调出一个合适的水温, 而控制器把握在洗澡的人手中, 而根据他的感受\(performance\)来调整.

本文的做法有几点好处:

1. task-specific, 直接在具体任务上调整不同 word-embedding 的权值, end-to-end;
2. 直接通过具体任务的性能来反映方法的优劣, 可以说更直观;
3. 一般使用 attention 都逃不脱的权值分布图, 可以提供哪个 word-embedding 在哪个任务上更被需要等视角;

根据是否利用 context, 文章提供了两种思路: DME\(Dynamic Meta-Embedding\) 和 Contextualized DME.

前者首先将所有使用到的 word embeddings **映射到同一个向量空间**, 然后**对每一维特征都计算加权平均**, 就这样.

![dme.png](../../.gitbook/assets/dme.png)

**w'** 是映射后\(同一个单词的\)不同的 word vector 组成的矩阵, $alpha$则是对应每个位置的权值, 由 self-attention 计算得, 公式如下, 其中 **a** 是可学习的参数. 该方法被称为 self-attention 是因为, **w'** 被用了两次\(还有一次是加权平均时\).

![self\_attention\_dme.png](../../.gitbook/assets/self_attention_dme.png)

CDME 与 DME 的不同之处在于权值的计算上. CDME 将映射后的 word vector 输入了一个 BiLSTM, 然后用 BiLSTM 的状态来计算权值, 如下. 要注意的一点是, 这里的 BiLSTM 是专门用于 self-attention 的权值的, 并不是后面 DME/CDME 作为输入的 BiLSTM \(文中没有交代清楚这一点, 我一开始对 **h** 出现在此处还感到奇怪来着, 看了代码才弄明白\).

![self\_attention\_cdme.png](../../.gitbook/assets/self_attention_cdme.png)

至于$phi$, 论文配套的代码提供了两个可选的函数, 一个是常规的 softmax, 一个是用 sigmoid 来作门控\(gating\).

之后将使用 DME/CDME 得到的 $w\_j$ 当作普通的 word vector, 后面按正常的操作来就行了.

