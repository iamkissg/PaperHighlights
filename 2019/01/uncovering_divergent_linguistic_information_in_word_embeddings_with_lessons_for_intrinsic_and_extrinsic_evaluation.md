### Uncovering divergent linguistic information in word embeddings with lessons for intrinsic and extrinsic evaluation

论文地址: [https://aclweb.org/anthology/K18-1028](https://aclweb.org/anthology/K18-1028)

##### 要点

本文的观点简单, 方法也简单, 不是 simple/easy 的简单, 而是**以简驭繁**的简. 观点就一个句话: **word embeddings 蕴含的信息比表面(测试结果)上要多**.

文章通过一个非常简单的 post-process 来发掘 word embeddings 的潜力.

首先了解一下本文所谓的N阶相似性(n-th order similarity)的概念. 和一些论文将 word-context similarity 称为一阶相似性(first order similarity), 将 word-word similarity 和 context-context similarity 称为二阶相似性(second order similarity)不同, 本文将 word-word similarity 称作一阶相似性. 用数学语言来描述就是, X 是 word embedidng 的矩阵, $sim(i,j)=X_{i*} \cdot X_{j*}$. 将 $M(X)=XX^{T}$ 定义为相似性矩阵(similarity matrix), 那么$sim(i,j)=M(X)_{ij}$. 而所谓的二阶相似性, 就是来度量一阶相似性矩阵中, 元素的相似性. 用类似的方式来表示二阶相似性矩阵的话, 就是 $M_{2}(X)=XX^{T}XX^{T}$, 而 $sim_{2}(i,j)=M_{2}(X)_{ij}$. 依此类推, 可以得到N阶相似性.

根据文章的说法, 二阶相似性不直接度量单词的相似性(显然), 但度量了它们与第三方单词的相似性的相似程度. GloVe 举了一个很好的例子(不同它统计的是 co-occurrence), 可以稍微体验一下: ice 与 steam, 根据相似性的不同, 可以分成 4 类: 更接近 ice(solid), 更接近 steam(gas), 与两者都接近(water), 与两者都不接近(fashion).

以上我们已经了解了可以将相似性矩阵一直推到N阶, 即 $M_{n}(X)=(XX^{T})^{n}$. 更可怕的是, 0阶相似性(根据定义来)也行, 负数阶也行, 甚至非整数阶都行, 只要你愿意.

SVD 是一种常用的用于从 count-based word representation 得到 distributional representation 的方法. 本文没用 SVD, 而是用了奇异值分解(eigendecomposition)来从相似性矩阵中得到 word embeddings. 具体而言: $X^{T}X=Q\LambdaQ^{T}$, $\Lambda$是正定对角阵, 对角元素为$X^{T}X$是奇异值, Q 是以奇异值向量为列向量的正交矩阵. 作者将 $W=Q\sqrt(\Lambda)$ 提取出来, 与 $X$ 相乘得到 $X^{'}=XW$, 它捕获了 word embedding X 的二阶相似性, 因为 $M(X^{'}=M_{2}(X)$.

从$W=Q\Lambda$又可以引伸出$W_{\alpha}=Q\Lambda^{\alpha}$, $\alpha=(n-1)/2$. $XW_{\alpha}$ 将分别捕获对应的N阶相似性. 文章就针对这些 embeddings 进行了实验, 发现通过调整 $\alpha$ 的值, 可以提高 word representation.

以上的推导与计算看起来有一丢丢复杂的样子, 其实在代码中就是一行的事. 比起来, 一个最简单的神经网络也比这要复杂得多.

在 intrinsic evaluation 上, 调整 $\alpha$ 能带来明显的性能提升, 在 downstream tasks 上使用无监督方法同样如此. 但是当模型是可学习时, 性能提升变得不明显, 因为 $XW_{\alpha}$ 只是对原 word embedding 进行了一次线性变换, 对它再进行一次线性变换的结果, 可以由对原 word embedding 只进行一次线性变换得到.

##### 备注

CoNLL2018 最佳论文.
