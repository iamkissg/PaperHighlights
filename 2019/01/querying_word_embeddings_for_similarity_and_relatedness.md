# Querying Word Embeddings for Similarity and Relatedness

论文地址: [http://www.aclweb.org/anthology/N18-1062](http://www.aclweb.org/anthology/N18-1062)

## 要点

本文的引言很长很值得一读 \(部分内容权且当作额外的知识吧\). 首先抛出一个观点: **相似性\(similarity\)通常被认为是无向的\(对称的\), 而相关性\(relatedness\)是有方向的\(非对称的\)**. 对于后者, 举个例子的话, "咖啡"在"杯子"里, 但"杯子"不能在"咖啡"里. 但是人类的行为数据显示, 其实相似性也是非对称的, 人们认为豹子更像老虎, 而老虎则不那么像豹子.囧.

心理语言学的实验证明: 在 semantic priming paradigms 里, 在先给定相关词的前提下, 目标词的处理会更高效, 相对于中性的词或不相关的词而言 \(比如大脑对蜜蜂和椅子蜂的处理\); 当单词对具有纯粹的相似关系\(律师和外科医生\), 或者纯粹的相关关系\(手术刀和外科医生\)时, 单词对的关系会被简化处理, 当单词对同时具有这两种关系, 大脑需要额外处理; 非对称性是 semantic priming data 的标准; 自由联想\(free association\)也受到非对称性的影响.

自由联想也是心理学的概念, 它的任务是, 给参与者一个提示词, 要求迅速反应脑海中出现的第一个词. 自己试试能体会到这其中的非对称性.

Counting-based word embeddings 的词向量的一维\(个\)特征体现了该单词与上下文单词/文档/主题的_一阶相关性_; 对其降维之后, 一维\(个\)特征则体现了单词间的_二阶相关性_.

引言介绍完毕.

如果看过 word2vec 的源码的话, 会发现实际有两个矩阵, 最后只输出了一个, 就是我们所熟知的 word embedding \(我去年研究过, 忘得差不多了\). 根据本文的说法, 另一个矩阵, 也就是隐层到输出层权重, 就是 context embedding. 本文将 context embedding 也导了出来, 结合 word embedding, 提出了 5 种计算相似度的组合: WW, WC, CW, CC, AA. WW 和 CC 比较直观, 直接计算单词对应的 word embedding 或 context embedding 的 cosine similarity; WC 和 CW 则一半一半, 属于对非对称性的发掘, 比如 WC 反应了第二个单词出现在第一个单词的 context 中的似然; AA 比较特殊, 同时使用了两个单词对应的 word embedding 和 context embedding, 用如下公式来处理:

![cosine\_aa.png](../../.gitbook/assets/cosine_aa.png)

针对 similarity 和 relatedness, 文章使用了 SimLex-999 和 ProNorm 数据集, 前者比较常见, 后者采用自由联想的方式获得, 更多细节请搜索看原文.

基于相似性的对称性, 而相关性\(自由联想\)的非对称性, 尤其是第二个词是在给定第一个词的情况下得到, 文章实验之前有两个假设:

1. WW 最适合 similarity;
2. WC 最适合 relatedness.

实验证明确实如此. CW 在 relatedness 上的表现远不如 WC, 根据定义, 它度量的是第一个词出现在第二个词的 context 中的似然, 这可以解释成人在自由联想时, 直觉是在提示词的上下文思考. 一开始也许有同学会以为 AA 会是最好的方法, 但并不是.

不过文章还在 wordsim-353 上进行了实验, 这一次, AA 翻盘了. 文章对此的解释是, 当标注人员没有受到明确的提示时 \(比如 SimLex-999 的要求是相似但不相关的单词对才能打高分\), 倾向于同时进行对称性与非对称性思考, 于是 WW 和 WC 都不那么管用了, 因为数据不再按照这样的思路生成.

文章还分别用 WW 和 WC 来模拟了自由联想的过程: 给定提示词 \(第一个词向量\), 搜索最近邻的单词作为联想的词 \(分别在 W 空间和 C 空间\), 结果发现 WC 的结果与人类联想的结果更接近.

结论: 考虑 similarity 时, 对称关系, 用 WW; 考虑 relatedness 时, 非对称关系, 用 WC; 混合关系, 考虑 AA.

