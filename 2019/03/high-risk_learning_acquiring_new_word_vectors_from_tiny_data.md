### High-risk learning: acquiring new word vectors from tiny data

论文地址: [http://aclweb.org/anthology/D17-1030](http://aclweb.org/anthology/D17-1030)

项目地址: [https://github.com/minimalparts/nonce2vec](https://github.com/minimalparts/nonce2vec)

##### 要点

一般训练 word embeddings, 都是用一个大型语料 (比如维基百科), 训练一次, 得到一个模型 (称 word embeddings 为模型可能不太准确, 姑且这样叫吧).

本文是我苦苦寻找的, 对预训练 word embeddings 用新数据旧算法 (即非后续的神经网络) 再训练的例子. 这样的方法的一个好处是, 为新词在旧的向量空间迅速找到合适的语义, 而不必将不同语料再结合起来重新训练. (对于稀疏矩阵表示的 word embedding, 不用训练, 新词的 context 很容易确立, 与它词意相近的单词也能马上找到, 但是现在大家不用这样的表示)

本文提出的方法叫 nonce2vec, nonce word 的有道翻译是(为特殊需要)临时造出的词语 (以下就称为"新词"了). 说白了就是要在现有的向量空间中, 快速学习一个新词的表示. 私以为这种思路和人学习新词的行为是一致的, 我们通过了了几个例句就能大致摸清一个新词的意思(比如00后的黑话).

一种快速了解一个词意的方法, 以中文为例, 就是从组成它的汉字大致推断出其意思; 英文的话, 就是从词根和前后缀等信息了. 因此就是对新词进行分解, 求各组成部分的向量和或平均. 另一种思路是对新词的 context 求和(参考CBOW的学习方式).

本文的观点是, 应该有一个架构, 它能从任意量的数据中学习词表示, 可以在之前的基础上, 继续学习词表示, 不管是旧词还是新词. 而为了快速学习新词的表示, 实验中首先使用了大学习率, 即所谓的 high-risk, 并用了一些技巧来弥补大学习率带来的损害.

Nonce2vec 是基于 word2vec 的, 主要的修改包括:

1. Initialsation: 用上面说的将上下文向量相加的方式 (限于已知的词), 初始化新词, 不过文章在这里对 context word 也进行了 subsampling;
2. 高学习率+大窗口: 目的在于, 贪心地感知相关的上下文;
3. Window resizing: 训练过程中不断缩小窗口;
4. Subsampling: 只对超高频单词进行;
5. Selective training: 只更新新词, 一个原因是防止高学习率破坏旧词表示;
6. Parameter decay: 高学习率+大窗口的学习只在新词的最早期学习中使用, 以快速定位到向量空间中的大致位置, 一旦新词的语义差不多了, 就替换掉 high-risk 学习策略, 用传统的参数来学习, 即进行精调阶段. 具体来说, 文章对学习率进行了指数衰减, 和新词的次数挂钩; 减小窗口; 提高降 subsampling rate.

文章的实验表明, parameter decay 是必要的, 保持不变是有害的.
