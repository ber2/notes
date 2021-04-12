---
aliases: [CBOW]
---

The __continuous bag-of-words__ is one of the methods available to obtain [[word vectors]] in the [[Word2Vec]] model.

The CBOW is setting up a [[Neural Network]] that fits to the problem of predicting the token following a given sentence.

The [[word embeddings]] are then given by the values of each word in the model in the last [[dense layer]] of the [[Neural Network]].