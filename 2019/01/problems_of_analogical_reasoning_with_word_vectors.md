### The (Too Many) Problems of Analogical Reasoning with Word Vectors

论文地址: [http://www.aclweb.org/anthology/S17-1017](http://www.aclweb.org/anthology/S17-1017)

##### 要点

这是一篇批判性的论文, 在我看来, 它将 word analogy 批了个体无完肤, 希望看过本文的同学, 慎重地考虑 word analogy 这项测试.

你一定听过"king is to man as queen is to woman (king-man+woman=queen)"的传说. 本文首先告诉你, 这叫类比(analogy)? 这就是关系相似性(relational similarity). 哲学与逻辑学上的类比推理(analogical reasoning)大概是这样的: **X 和 Y 有相同的属性 a, b, c, 那么他们还可能具有相同的属性 d**. 举个例子: 地球和火星都围绕着太阳公转, 都有至少一个卫星, 都会自转, 都受重力影响, 既然地球上有生命, 火星上理应也有. 在 NLP 领域, 最早将 relational similarity 定义为 analogy 的是 Peter Turney, 这位大佬将单词间的相似性与单词对之间的关系相似性进行了区分, 于是两个具有高度关系相似性的单词对就成了 analogous. "a is to a' as b is to b'" 就一直延用至今.

在 word2vec 的论文中, 3CosAdd(a-a'+b=b')被用来推理 b'. 但是有一个大家都不知道的内幕: **a, a', b 三个词从 b' 的候选项中被剔除了**. 这实际上限制了一些关系的推理, 比如"snow is to white as sugar is to ?", 就永远得不到正确答案了. 更要命的是, 另一篇论文发现, 如果将 a, a', b 包含在候选项中, Google set 上的准确率将急剧下降, 其中 9/15 的准确率直达 0. 本文在另一个测试集 BATS 上进行了这项实验, 结果如下:

![3cosadd_on_bats.png](../../201901/3cosadd_on_bats.png)

可以看到, 推理得到的 b' 最可能是 b, 其次是 a'. 这表明, a-a' 对 b 的影响极小, 小到最邻近的结果依然是 b. 另一篇论文中提到的一个数据, Google set 中的单复数关系推理, 直接选最邻近 b 的词作为答案, 准确率高达 70%, 而 3CosAdd 只能提升 10% 的准确率.

文章还对单词之间的相似性对准确性的影响进行了实验, 直接上图吧:

![3cosadd_accuracy_vs_similarity.png](../../201901/3cosadd_accuracy_vs_similarity.png)

可以看到, 基本上 a 与 a', a' 与 b', b 与 b', a 与 b' 的相似性越高, 准确性就越高.

这些结论, 不只限于 3CosAdd, 其他方法如 3CosMul 和 LRCos 同样如此.

##### 备注

文章内容不止于此, 只是我暂时无从写起, 太多了. 建议看原文, 或者记住结论: word analogy 问题很多, 慎用.
