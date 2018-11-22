### Learning linear transformations between counting-based and prediction-based word embeddings

论文地址: [https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0184544](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0184544)

##### 要点

存在两类学习 WE 的方法: counting-based (CB) 和 prediction-based (PB). 前者通过单词间的 co-occurrence 来表示单词, 因为考虑了一个单词与 vocab 中所有单词的关系, 得到一个极其稀疏的 EM (embedding matrix); 后者通过预测单词与 context 的关系来学习 WE. 可以认为 PB 习得的词向量隐含了其 context, 而 CB 的稀疏向量的一维就表示了一个 context. 本文探讨了两者间的线性关系, 通过学习 CBWE 到 PBWE 的一个线性映射.

文章给出了该线性映射的闭式解: ![CB2PB close-form](../../img/201811/CB2PB_linear_projection_close-form.png), 此处 S 表示 CBWE, T 表示 PBWE, Id 是单位矩阵. 由于 S 的稀疏性, 维数太高, 使得求解几乎不可能.

因此本文老老实实地采用了梯度下降的方法: ![CB2PB stochasticly](../../img/201811/CB2PB_stochasticly.png), 目标函数是 RMSE.

文章的实验发现, 训练过后, CBWE 经线性映射得到的 WE 在 word similarity 和 analogy 任务上表现接近 PBWE, 甚至有超过的情况出现. 得证.

该发现有什么意义呢? PBWE 对于新单词无能为力, 必须重新训练; 有了 CBWE 到 PBWE 的线性映射之后, 因为 CBWE 能很容易地表示新单词, 只需要做一个线性映射, 就能得到该新单词的近似低维表示.
