### Improving Distributional Similarity with Lessons Learned from Word Embeddings

论文地址: [http://www.aclweb.org/anthology/Q15-1016](http://www.aclweb.org/anthology/Q15-1016)

##### 要点

虽然只是 4 年前的论文, 但是在这个论文层出不穷的年代, 感觉已经好古老了.

我觉得这篇论文的一个优点在于: 质疑与祛魅. 在 word representations 领域的新兴势力 word embeddings 崛起并展示出其无与伦比的优势之时, 尤其是有论文刚刚"系统"地证明了 word2vec 明显优于传统方法, 作者们本着科学的怀疑态度进行了更严格的实验, 给 word embeddings 的拥趸们来了一记当头棒喝. 明显的优势? 没有的事儿, 只是它们的算法使用了一些奇技淫巧而已, 将这些方法应用到传统方法上, 那结果完全不差. 这也正是本文标题的由来: lesssons learned from word embeddings.(老年人也要赶时髦, 潮起来连年轻人都害怕.)

本文选择了两个传统方法 PPMI 和 SVD, 两个当时风光一时无三的 SGNS(Skipgram Negative Sampling) 和 GloVe.

然后, 作者很细致地挑出了 word embeddings 算法中可以应用于传统方法的超参数, 分为一下三类:

1. 语料预处理相关的:
    1. Dynamic Context Window: 这算是 word2vec 的一个隐藏设置, 它根本就没有在 word2vec 的两篇论文中提到过 (由于看过源码, 我对这一点印象太深刻了, 变量名是 b, 这么重要的设置, 怎么在论文里完全没提起). 当我们设置 window size == 5 时, 训练时, window size 实际在 [1, 5] 随机动态变化. 不过本文提到的一点我原先没注意到: word2vec 根据到 target word 的距离为单词分配了不同的权值, 以 window size 为分母, 越近的单词权越高 (5/5, 4/5, 3/5, 2/5, 1/5). GloVe 的加权规则是: 1/distance. 为不同距离的单词赋不同的权比较好理解, 也许可以用"局部性原理"解释, 越近的单词, 一般就越相关嘛;
    2. Subsampling: 这个在 word2vec 的论文提到过, 对词频 f 超过阈值 t 的单词, 以概率 p 从语料中剔除. 不过根据本文的意思, 代码与论文并不一致呀: 论文中是 $p=1-sqrt(t/f)$, 代码中是 $p=(f-t)/f-sqrt(t/f)$. 降采样在形成 word-context pairs 之前, 所以, window size 不变的情况下, target word 可以触及到更远的单词;
    3. Deleting Rare Words: 这一点在 word2vec 的论文中也提到了, 使用时也有这个 option. 同 subsampling 一样, 在 word-context pairs 之前发生, 让 target word 的鞭更长了;
2. word-context 交互相关的:
    1. Shifted PMI: 名字取得挺陌生的, 其实对 word2vec 就是 negative samples 的数量, 但是到了传统这边就变成这个奇怪的名字了 (在另一篇论文中, 作者用数学语言证明了 SGNS 写成 PMI 的形式);
    2. Context Distribution Smoothing: 在 word2vec 中, negative samples 并非均匀采样的, 而是才一个平滑过的均匀分布中采样.
3. 词向量的后处理相关的:
    1. Add Context Vectors: 这一点是从 GloVe 启发来的, 作者同时导出了 word2vec 的 context embeddings. 增加 context vector 的好处是, 向二阶相似函数(WW, CC 的 cosine similarity)中加入了一阶相似性(WC, CW 的 cosine similarity). 称前者为二阶相似性是因为它度量了两个单词(或 contexts)在所有 contexts 中的情况得到可替换程度; 而后者的度量, 从矩阵来看, 只是一个单元的值.
    2. Eigenvalue Weighting: 这一点受启发于 SGNS 的 word embeddings 也好, context embeddings 也好, 看起来更"对称", 都不正交. 此前人们在使用 SVD 时, 普遍用 $U sqrt(Sigma)$ 来表示 word embedding, 用 $V$ 来表示 context embedding, 结果 word emebdding 是非正交的, context embedding 是正交的, 两个矩阵明显不"对称".
    3. Vetor Normalization: 文中总共实验了 4 种 normalization 方法, 1) 按行; 2) 按列 (这一点来自 GloVe); 3) 按行又按列; 4) 不进行.

以上这些参数, 是 word embeddings 算法的设计过程中容易考虑到, 比如 context window, 它们就是基于局部信息来学习词向量的, 与此相关的一些设置容易想到. 如果没有这些算法, 传统方法可能是很难想到这些奇技淫巧的, 比如它们就没有 context window 的概念.

文章的结论反而比较简单, 为传统方法插上 word embeddings 的翅膀之后, 它们也腾飞了, 增益明显. 分点来说的话:

1. 调整超参数有时候比调整(切换)模型的增益更大;
2. 超参数的作用有时候比数据量还关键;
3. 在本文的实验中, GloVe 被 SGNS 完爆 (在 GloVe 的论文中情况刚好相反);
4. SVD 的常规用法还不如反常规的使用 (word embedding 与 context embedding 的"对称") 来得好;
5. context distribution smoothing 是万金油;
6. SGNS 的可扩展性非比寻常 (大数据的情况下, SGNS 只要半天, GloVe 要好几天, 当然传统方法玩不起来);
7. word embedding 与 context embedding 的双剑合璧效果更好.

##### 备注

本文权且当作对 word embedding 超参数设置的一个心得来看吧.
