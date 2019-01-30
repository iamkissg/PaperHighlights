### Dict2vec: Learning Word Embeddings using Lexical Dictionaries

论文地址: [http://aclweb.org/anthology/D17-1024](http://aclweb.org/anthology/D17-1024)

##### 要点

本文改进了 word2vec 算法, 方法上有点像 Airbnb 对 skip-gram 的改造: **在 sampling 上做文章**.

对 word embeddings 的改进, 一些方法在训练完毕之后, 使用 WordNet 等数据包含的语义信息来进行 post-processing, 本文则直接对学习过程进行了改造, 而且用的是从在线词典(牛津/剑桥/柯林斯/美国在线词典)爬取的定义数据.

对于词典类数据的使用, 假设或者说共识都是, 它们包含了丰富的语义信息(semantic information). 本文也不外如是, 通过对词典的利用, 在无监督地学习过程中, 添加了一些学习规则, 起到一些监督的作用. 具体而言, 文章将互相出现在对方定义中的单词对称为 strong pairs, 将只有一方出现在另一方定义中的单词对称为 weak pairs, 并为它们分配了不同的权值(作为模型的超参数).

**Strong pairs 与 weak pairs 控制着 sampling 过程: 每次都从 target word 的 strong pairs 和 weak pairs 中采样作为 postive samples, 而进行 negative sampling 时, 避免从以上两个集合中采样.** 该过程, 强化了相似/相关单词间的联系, 也避免了随机 negative sampling 的误采样(和 Airbnb 将 booked listing 作为 global context, 增加同一市场的 negative samples 等等的做法, 有异曲同工之妙).

根据以上描述, 显然, 目标函数由 3 部分组成, 第一部分是 target word 与 context words, 第二部分是 postive samples, 第三部分是 negative samples. 各司其职.

文章的实验发现用在线词典的效果居然比 WordNet 还要更好一些, 比起不用 strong/weak pairs 要好得更多. Dict2vec 在训练时间上, 是 word2vec 的约 1/4, 是 fasttext 的约 1/3, 这让人挺惊讶的.

##### 备注

本文提供的爬虫是一个很方便的工具.
