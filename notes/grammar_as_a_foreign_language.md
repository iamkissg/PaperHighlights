### [Grammar as a Foreign Language](https://papers.nips.cc/paper/5635-grammar-as-a-foreign-language.pdf)

本文将 *Syntactic consistency parsing* 任务转化为 Seq2Seq 问题, 并用 LSTM+Attention 模型来求解.

本文更像是一篇实验指导: 从 LSTM, attention mechanism, 线性化解析树, 模型参数与初始化, 现有实验数据以及自建数据, 实验分析与参数影响, 一应俱全. 欲了解 Google 是如何进行实验的, 可观看本文. (笔者并不熟悉 syntactix consistency parsing, 并没有细看实验过程)

实验结果验证了一些既有的认知:

* Beam sise 过大过小都使性能下降;
* Pre-trained word vector 不如随任务训练 word embedding 好;
* Attention mechanism 很有效, 尤其在小数据集上.

本文带给我的启发是: 问题转化: 有时候换一种角度思考问题, 或者将问题转化为另一种形式, 使用不一样的求解方式也许能获得意想不到的结果 (最近学到的一个认知是: 实现十倍的性能提升比 10% 更容易, 原因在于 10% 可能是想尽办法在现有方法上进行改进, 而 10x 通常是跳出当前的思维框架. 与本文提倡的方法别无二致);
