### Learning word embeddings from dictionary definitions only

论文地址: [http://metalearning.ml/2017/papers/metalearn17_bosc.pdf](http://metalearning.ml/2017/papers/metalearn17_bosc.pdf)

##### 要点

* 本文提出了一种仅使用词典来学习 word embeddings 的方法. 具体的, 使用了 autoencoder, encoder 将单词的定义编码成单词的向量表示, 然后再将这个向量输入 decoder, 还原定义.
* 以 LSTM 作为 encoder, last hidden state 作为 definition embedding, 即 word embedding
* Decoder 是一个简单的单层 MLP, 且还原的定义的词序被忽略了. 其实这个时候, 和 skip-gram 很像了, 只是多了 bias, 少了 negative sampling.
* 考虑到单词与定义的递归关系, 引入了 encoder 的 embedding layer 与 definition embedding 之间的一致性损失.
* 对于一词多义的情况, 由多个定义编码得到多个 definition embeddings, 使用 cosine similarities 的平均 (另一种策略是使用最大相似值).
* 由于数据量实在太小了 (几十兆封顶了), 在 word similarity/relatedness 上的表现基本不如基线. 不过有时候会比基线 (GloVe, word2vec, word2vecf) 要好.
