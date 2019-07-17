### Graph Convolutional Networks for Text Classification

论文地址: [https://arxiv.org/abs/1809.05679](https://arxiv.org/abs/1809.05679)

##### 要点

本文的创新点是: 提出了 Text Graph Convolutional Network, 将 word 和 document *一同*作为图的节点, 学习 text classification 的同时, 也学到了 word embedding 和 document embedding.

作者的出发点是: 像 CNN/RNN 的深度学习模型只在 document-level 学习文本分类, 捕获了局部连续的信息, 但忽略了全局 word-word occurrence, 而它包含了非连续的长距离的意义 (这个东西, 原先只在 word embedding 中看到过).

##### 备注


