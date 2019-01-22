### Think Globally, Embed Locally — Locally Linear Meta-embedding of Words

论文地址: [https://www.ijcai.org/proceedings/2018/0552.pdf](https://www.ijcai.org/proceedings/2018/0552.pdf)

##### 要点

本文的核心思想是, 对一个单词的语义影响最大的是它的近邻. 之前的 Meta-Emebdding (ME) 是在整个 vocab 上学全局映射, 对近邻的局部变化不敏感. 举个例子: glove 学到的 bank 是*银行*的意思, 从它的最近邻点是 credit/financial/cash 等可看出, 而另一个 WE 算法 (Huang et al 2012) 学到的是*岸*的意思, 因为其最近邻点是 river/valley/marsh 等.

因此本文将 ME 的学习分成了两步: neighbourhood reconstruction step & projection step. 第一步的工作实际就是用一个单词的最近邻点 (embeddings) 的线性加权来表示它, 而且学到的 weights 在各个 source embeddings (几篇论文看过来, 用来学习 ME 的 pretrained embeddings 都被这样称呼) 间共享. 在前一步的基础上, 再学习 ME, 一定程度上能保证单词在 SE 中的最近邻点在 ME 中也相互靠近.

上文提到 weights 在各 SE 中共享, 具体而言就是将单词在不同 SE 中的最近邻点的并集作为最终的 kNN, 每一个最近邻点有对应的非零权, 不在 kNN 中的点, 权为零.

##### 备注

