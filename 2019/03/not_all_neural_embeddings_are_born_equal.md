# Not All Neural Embeddings are Born Equal

论文地址: [https://arxiv.org/abs/1410.0718](https://arxiv.org/abs/1410.0718)

## 要点

这篇小短文主要验证了两类完全不同的方法习得的 word embeddings 具有非常不同的属性.

一类是用单语言, 基于 language model 及其变种学习的词向量, GloVe/CW/Skipgram 都属于此类; 另一类是从翻译模型中习得的 encoder 的 word embeddings.

结果表明, 基于翻译的 word emeddings 在 word similarity 上表现得好得多, 而基于单语言的 word embeddings 在 relatedness 上迎头赶上. 而且即使加大单语言的语料, 不能改变现状.

而 word analogy 的结果现实, 基于翻译的 word embeddings 在 semantic 型任务上被按在地上摩擦, 但是在 syntactic 任务上, 反过来将其他 word embeddings 完爆了.

综上, 基于翻译的模型对于 word similarity \(相似性\) 和 syntactic \(句法\) 的学习更有效. 而单语模型更善于捕获相关性和语义属性.

翻译时, 目标语言中一个单词对应原语言中多个单词, 会鼓励这些单词相互靠近; 因为原语言中的词基本不会被翻译成目标语言中的反义词 \(虽然在单语环境下, 反义词可能会有很多相似的上下文\). 所以基于翻译的 word embeddings 会强化这类属性, 使得向量空间更分明.

## 备注

就本文的一个启示是, 用预训练的模型作为 NMT 的 embedding layer, 固定该层是绝对不恰当的做法. 这个结论应该可以推广到其他 downstream tasks.

