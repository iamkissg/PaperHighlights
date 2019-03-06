### Contextual Word Representations: A Contextual Introduction

论文地址: [https://arxiv.org/abs/1902.06006](https://arxiv.org/abs/1902.06006)

##### 要点

如标题所示, 本文要讲的无非 contextual word representations, 但是它并不从 context 讲起, 而是换了个说法, 很清奇.

文章将我们通常意义上所谓的 word, 分成了两类: word token 和 word type. Word token 就是 tokenization 得到的一个个 token; 而 word type, 就是字面的意思, 是一个类型, 是一个更抽象的概念. 但是, 我们所知道的 token 被归到了 word type, 而 word token 是 tokenization 得到的那一个个与 context 息息相关的, 可以说独一无二的 token. 这一点不难理解, 按照这个说法, 就是按字形分类了呗, 所有长得一样的是一个 word type. 就像我们习惯把随单复数变化的单词认为是一个单词, 现在只是换了一个分类准则.

于是, 过去所谓的种种 word representations, 不管是 one-hot, 还是 count-based, 还是 word embeddings, 都是 word type representations. 不管对应的 word 出现在哪个句子中的哪个位置, 它的 representations 不变. 怎么被作者说通了呢- -.

而 word token representations, 或者换个更耳熟能详的名字, contextual word representations, 一个 word token 在不同 context 中含义不同, 取决于 context. 对于一词多义的情况, 尤其如此, 比如 bank, 一作银行一作岸, 联系上下文就很清楚是什么意思了.

我和本文的作者一样, 硬是就一个很清晰的概念扯了不少, 但是以本文提供的 word type 和 word token 的视角来看, 当念及一句话中的一个单词, 你可能会首先想到 word token, 自然就强化了上下文的作用, 而且也不必转念去思考什么 context; 而念及一个孤零零的单词, 可能就首先想到 word type. 本文带给我这种思维上的转换, 就像打通了任督二脉一样, 让我感觉很舒服. 如果读者没有这种感受, 恕我笔拙, 请看原文.

文章的最后有点口号式, 抄一下:

* Language is a lot more than words.
* Natural language processing is not a single problem.

说白了, 就是任重而道远.
